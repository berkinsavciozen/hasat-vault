---
title: Hasat — Paylaşılan Mimari (Web + Mobil)
updated: 2026-07-29
tags:
  - hasat
  - architecture
  - mobile
  - shared
---

# Paylaşılan Mimari — Web + Mobil

> Onaylandı: 2026-07-28
> Amaç: Web (`hasat-d2c-marketplace`) ve mobil (`hasat-mobile`) paralel gelişirken **aynı işi iki kez yapmamak** ve daha kötüsü, **iki kopyanın birbirinden sapmasını** önlemek.

---

## Neden bu doküman var — kanıt

Bu projede "aynı mantığın iki yerde yaşaması" arıza modu **iki kez** gerçekleşti:

1. **P20 (2026-07-21):** `dispatch_sms()` (SQL) event→tercih eşlemesini biliyordu, `send-sms` (TypeScript edge function) bilmiyordu. Kullanıcı arayüzünde SMS toggle'ları vardı, açılabiliyordu, **ama hiçbir zaman çalışmıyordu.** Düzeltildi, gerçek Twilio testiyle doğrulandı.
2. **P24 (2026-07-23):** Aynı bug tekrar bulundu — `send-sms` COL map'i 10 event'ten 3'e düşmüş, dosya bir başka oturumda üzerine yazılmış. SQL tarafı hâlâ doğruydu.

**Ders:** Tek doğruluk kaynağı iki runtime'a bölündüğünde sapma kaçınılmaz, sessiz ve tekrarlayan. İkinci bir client (mobil) eklemek bu riski ikiye katlar. Mimarinin tamamı bu riski minimize etmek üzere kurgulanmıştır.

> Bkz. `TODO.md` kural #101.

---

## Katman 1 — Postgres asıl paylaşım katmanı

**KURAL (#106): İki client'ın da ihtiyaç duyduğu her mantık client'ta değil, RPC/view olarak veritabanında yaşar.**

Zemin zaten hazır: `crop_config` tek kaynak, 20 KPI view'ı, `enforce_offer_stock()`, `dispatch_sms()`, geriye dönük uyumlu RPC'ler.

### P23'e uygulanışı

| Mantık | Nerede yaşar | Neden client'ta değil |
|---|---|---|
| Tarif malzemesi ↔ aktif ilan eşleştirme | `rpc_recipe_availability(recipe_id)` | Eşleştirme kuralı değişirse iki client birden doğru davranır |
| Culinary birim dönüşümü (adet/kaşık → g) | DB fonksiyonu + `crop_culinary_meta.conversion_hints` | Dönüşüm katsayıları veri, kod değil |
| Alışveriş listesi üretimi | RPC | Porsiyon ölçekleme + min_order yuvarlaması tek yerde |
| Tarif hunisi ölçümü | `v_kpi_recipe_funnel` | Mevcut 20 KPI view'ıyla aynı desen |
| Yenilebilirlik filtresi | `crop_culinary_meta.is_edible` | Pamuk/tütün/şeker pancarı/safran soğanı tarif akışına girmemeli |

**Sonuç:** Mobil uygulama *ince* olur. İş mantığını yeniden yazmaz, RPC çağırır. Web'de bir kural değişince mobil otomatik doğru davranır.

---

## Katman 2 — `hasat-core` paylaşılan TypeScript paketi

DB'ye taşınamayan kısım:

| İçerik | Örnek | Ne zaman |
|---|---|---|
| Üretilmiş DB tipleri | `supabase gen types typescript` çıktısı | ✅ M1 |
| Design token'ları | Marka renkleri, spacing, tipografi ölçeği | ✅ M1 |
| Saf yardımcılar | `convertQuantity()` | ✅ M1 |
| Saf yardımcılar (kalan) | coverage skoru, offer-status etiketleri, para/tarih formatlama | ⬜ M5 |
| **Supabase storage adapter** | web `localStorage` ↔ mobil `expo-secure-store` | ⬜ **M5** |
| **Supabase sorgu fonksiyonları** | `fetchListings()`, `fetchRecipe()` … | ⬜ **M5** |
| **TanStack Query hook'ları** | `useListings()`, `useRecipes()` — **React Native'de de çalışıyor** | ⬜ **M5** |
| Zod şemaları | Form/RPC girdi doğrulama | ⬜ M5 |

### ⚠️ Storage adapter + query hook'ları M1'den M5'e taşındı (2026-07-29)

Bu doküman ilk yazıldığında (2026-07-28) storage adapter M1'e yazılmıştı. **Bu
karar M1 uygulanırken revize edildi:** storage adapter, sorgu fonksiyonları ve
TanStack Query hook'ları **M5'e bırakıldı**.

**Gerekçe:** üçü de auth ve veri akışı hattına dokunuyor. 25 Ağustos
lansmanından dört hafta önce, **henüz var olmayan bir client için** o hatta
dokunmanın getirisi yok — riski var. Adapter'ın tek gerçek tüketicisi mobil
uygulama; mobil iskelet M5'te doğduğunda gerçek bir kullanıcıyla birlikte
taşınacak. Bu, lansman öncesi risk kuralıyla (`Roadmap.md`) da tutarlı:
lansmandan önce yalnızca ekleyici işler.

**M1'de bu yüzden taşınan içerik bilinçli olarak küçük tutuldu:** üretilmiş DB
tipleri, design token'ları, `convertQuantity`. Boru hattının kendisi (subtree +
manifest + drift guard + Action) tam kuruldu — sonraki taşımalar artık sadece
dosya ekleme işi.

**M5'te taşınmayan diğer saf yardımcılar** (aday, ilk turu küçük tutmak için
ertelendi):

- `src/lib/hasat/format.ts` — saf ve React'e bağımsız, ama web'de **33 dosya**
  import ediyor; taşınması Lovable'ın dokunduğu 33 dosyada import değişikliği
  demek.
- `src/lib/hasat/coverage.ts`, `offer-status.ts` — saf, ama `crop-config`/`types`
  üzerinden bir tip grafiği sürüklüyorlar. `offer-status.ts` ayrıca CSS
  değişkenlerine (design token) bağlı; önce token bağlantısı netleşmeli.

### Dağıtım — kritik kısıt

`hasat-d2c-marketplace` reposunun `main` branch'i **Lovable'ın GitHub sync bot'u (`gpt-engineer-app[bot]`) tarafından yönetiliyor.** Bu repoyu pnpm workspace / monorepo yapısına çevirmek Lovable'ın build'ini ve sync'ini kırma riski taşır — yani projenin en büyük hız kaynağını riske atar.

**Bu yüzden monorepo YOK.** Bunun yerine:

```
hasat-core (ayrı repo)
   │
   ├── git subtree ──► hasat-d2c-marketplace : src/lib/core/   ✅ M1'de kuruldu
   └── git subtree ──► hasat-mobile          : src/lib/core/   ⬜ M5 (repo henüz yok)
```

- **git subtree** (submodule değil) — Lovable'ın build'i submodule init etmez; subtree düz dosya olarak iner
- `hasat-core`'da bir **GitHub Action**, değişiklikte hedef repoya PR açar
  — **M1'de tek hedefli** (sadece web); ikinci hedef `hasat-mobile` doğduğunda M5'te eklenecek
- Her core dosyasının başında: `// hasat-core — BU DOSYAYI BURADA DÜZENLEME`
- Hedef repoda bir **hash manifest** dosyası (`src/lib/core/.manifest`) → sapma saniyeler içinde tespit edilir

### Boru hattının kurulu hali (M1, 2026-07-29)

| Parça | Nerede | Ne yapar |
|---|---|---|
| Kaynak | `hasat-core/core/` | Tek doğruluk kaynağı. Build step yok, publish yok. |
| Manifest | `core/.manifest` → `src/lib/core/.manifest` | Her core dosyasının sha256'sı |
| Sapma scripti | `hasat-core` → `npm run drift -- <yol>` | **DEĞİŞTİRİLMİŞ / EKSİK / FAZLA** üç durumu yakalar, farkta exit 1 |
| Sync Action | `.github/workflows/sync-to-web.yml` | `core/**` değişince web reposuna PR açar |
| Drift Action | `.github/workflows/drift-check.yml` | Web'e inmiş kopyayı günlük doğrular |

**Gereken secret:** `hasat-core` reposunda `SYNC_TOKEN` (web reposuna push + PR
yetkisi olan PAT). ✅ Eklendi ve **iki workflow da canlıda yeşil doğrulandı**
(2026-07-29). Ayrıca sync job'ının `permissions: contents: write` izni var —
`core-dist` dalını kendi reposuna geri itiyor.

> ⚠️ `hasat-core` şu an **public**. İçinde sır yok, ama üretilmiş DB tipleri
> tüm tablo/kolon/enum/view adlarını açıkça gösteriyor. Bilinçliyse sorun yok;
> değilse Settings → General → Change visibility ile private yapılabilir
> (subtree ve Action'lar private'da da aynen çalışır).

### ⚠️ Drift kontrolünün kör noktası — "bayat ama tutarlı" hali (P23-M2-ek'te bulundu)

`drift-check.yml`, web reposunun `src/lib/core/` klasörünü **o klasörün kendi
`.manifest` dosyasıyla** karşılaştırıyor:

```yaml
- name: Sapma kontrolü
  run: node scripts/check-drift.mjs target/src/lib/core
```

Yani cevapladığı soru şu: **"web kopyasını biri elle düzenledi mi?"** Bu, bekçinin
asıl amacıdır (kural #105 — Lovable P24'te paylaşılan bir dosyanın üzerine yazmıştı)
ve **doğru çalışıyor.** DEĞİŞTİRİLMİŞ / EKSİK / FAZLA üç durumu da yakalıyor.

**Sormadığı soru:** *"web kopyası `hasat-core` ile aynı sürümde mi?"*

Sebep: `.manifest` dosyası core dosyalarıyla **birlikte** subtree'ye iniyor. Bir sync
PR'ı açılıp merge edilmeden kalırsa, web'in dosyaları da `.manifest`'i de eski
sürümde kalır — ikisi birbiriyle **tutarlıdır**, dolayısıyla drift kontrolü **yeşil
yanar**. Web sessizce bayat tiplerle çalışmaya devam eder ve hiçbir alarm çalmaz.

Bu teorik bir risk değil: P23-M2 ve P23-M2-ek turlarının ikisi de `core/db/types.ts`'i
değiştirdi ve ikisi de birer sync PR'ı doğurdu. Bu PR'lardan biri merge edilmezse,
web'in tipleri canlı şemadan geri düşer — tam olarak M1-b'de `src/integrations/supabase/types.ts`
dosyasında bulunan "bayat kopya" arıza modunun aynısı, ama bu kez bekçinin içinde.

**Önerilen düzeltme (M5):** drift script'i ikinci bir karşılaştırma daha yapsın —
hedefteki `.manifest` ile **`hasat-core`'un kendi `core/.manifest`'i** birebir aynı mı?
Action zaten her iki repoyu da checkout ediyor, ek bir altyapı gerekmiyor:

```
node scripts/check-drift.mjs target/src/lib/core          # elle düzenleme (mevcut)
diff core/.manifest target/src/lib/core/.manifest         # sürüm gerisi (eksik olan)
```

Farkta uyarı ver ve bekleyen sync PR'ını işaret et.

**Bu turda bilinçli olarak DÜZELTİLMEDİ.** Gerekçe: lansmandan dört hafta önce
çalışan bir bekçiye dokunmak, kazandırdığından fazla risk getirir; ayrıca ikinci
hedef (`hasat-mobile`) M5'te doğduğunda script zaten elden geçecek. **M5 açık
maddesi.** O zamana kadar koruma insan tarafında: *sync PR'ı açıldığında merge
edilmeli, açık bırakılmamalı.*

### KURAL (#105)

`src/lib/core/` altındaki dosyalar `hasat-core`'dan gelir. **Lovable dahil hiç kimse burada düzenleme yapmaz.** Değişiklik `hasat-core`'da yapılır, iki repoya PR ile iner.

> Bu kural kozmetik değil: Lovable daha önce paylaşılan bir dosyanın üzerine yazdı (P24 regresyonu). Görünür "dokunma" işareti + drift kontrolü tam bu senaryoya karşıdır.

### ⚠️ Lovable'ın yönettiği dosyalar — kavga etme, import'u yasakla

`src/integrations/supabase/types.ts` Lovable'ın Supabase entegrasyonunun standart scaffold yoludur. M1-b'de silinip import'lar core'a çevrildi, ama Lovable dosyayı 2026-07-29'da yeniden üretti ve tekrar tekrar üretecek.

**İlke: Lovable'ın yönettiği bir dosyanın varlığıyla savaşmıyoruz — asıl tehlike dosyanın var olması değil, kodun ondan import etmesi.** Hiçbir dosya oradan import etmiyorsa bayat kopya sadece ölü ağırlıktır.

Koruma üç katmanlı:
1. Tüm uygulama kodu tipleri `@/lib/core/db/types`'tan alır
2. ESLint `no-restricted-imports` — yasaklı yol, anında geri bildirim
3. `drift-check.yml`'de yasaklı-import taraması — günlük 06:00 UTC, Lovable lint çalıştırmasa da yakalar

Reddedilen alternatifler: (a) dosyayı re-export'a çevirmek — Lovable üzerine yazar, kırılgan; (b) dosyanın varlığını yasaklamak — Lovable'ın scaffold'uyla çatışır, her gün alarm çalar, build kırma riski taşır.

---

## Katman 3 — Asla paylaşılmayan (adapter sınırları)

| Alan | Web | Mobil |
|---|---|---|
| UI bileşenleri | `div`, Tailwind | `View`, Nativewind |
| Yönlendirme | TanStack Router | Expo Router |
| Oturum saklama | `localStorage` (`hasat-store`, zustand) | `expo-secure-store` |
| Bildirim kaydı | — | `device_tokens` + APNs/FCM |

**Adapter deseni:** Supabase client bir storage adapter parametresi alır. Web'deki `/buyer` guard'ının `localStorage`'ı doğrudan okuması mobilde geçersizdir — bu sınır açıkça adapter'a çekilir.

---

## Bildirim katmanı

`notif_channel` enum'unda **`push` değeri zaten var** (2026-07-28'de doğrulandı) — şema bunu baştan öngörmüş. Yeni sistem gerekmiyor, mevcut dispatcher genişletilecek.

### Gerekenler
1. `device_tokens` tablosu (`user_id`, `token`, `platform`, `created_at`) — **ekleyici, risksiz**
2. Android FCM — **şirketten ve Apple hesabından bağımsız**
3. iOS APNs anahtarı — **yalnızca ücretli Apple Developer hesabına bağlı**
4. Event→tercih eşlemesinin tek kaynağa (DB tablosu `notif_event_map`) konsolidasyonu

### ⚠️ Risk kuralı
4. madde iki kez kırılmış SMS hattına dokunuyor. **25 Ağustos lansmanından önce YAPILMAYACAK** — M9'a bırakıldı. Lansman öncesi yalnızca ekleyici işler.

---

## Uygulanma sırası

| Ne zaman | İş | Durum |
|---|---|---|
| M1 | `hasat-core` repo + subtree + GitHub Action + drift guard + do-not-edit işaretleri + design token'lar + DB tipleri + `convertQuantity` | ✅ 2026-07-29 |
| M1 | Küçük şema borçları (bkz. `P23-Mobile.md` M1) | ⬜ |
| M2 | Katman 1 RPC'leri (tarif eşleştirme, birim dönüşümü, alışveriş listesi, funnel view) | ⬜ |
| M2 | `device_tokens` tablosu | ⬜ |
| **M5** | **Storage adapter + sorgu fonksiyonları + TanStack Query hook'ları + zod şemaları** (M1'den taşındı) | ⬜ |
| **M5** | **Drift script'ine sürüm-gerisi kontrolü** (hedef `.manifest` ↔ `hasat-core` `core/.manifest`) — bkz. "Drift kontrolünün kör noktası" | ⬜ |
| M5 | Sync Action'a ikinci hedef (`hasat-mobile`) | ⬜ |
| M9 | Bildirim event map konsolidasyonu (lansman sonrası) | ⬜ |

---

## Doğrulama kuralı

M1'in çıkış kriteri: **web uygulamasında sıfır regresyon.** Subtree kurulumu mevcut çalışan bir sistemin dosya yapısına dokunuyor — Lovable build'inin ve canlı önizlemenin hâlâ çalıştığı bağımsız olarak doğrulanmadan M2'ye geçilmez.

---

## M1'den çıkan açık riskler (Berkin'in kararına bırakıldı)

### 1. Design token'ları şu an iki yerde yazılı

`src/styles.css` Tailwind'in çalışma zamanı kaynağı olmaya devam ediyor;
`core/design/tokens.ts` aynı değerlerin JS/TS (ve M5'te Nativewind) temsili.
Bir renk değişirse **ikisi de** güncellenmeli.

Otomatik bağlanmadı, çünkü tek yol `styles.css`'in core'dan bir CSS dosyası
`@import` etmesiydi — ve `styles.css` Lovable'ın dokunduğu bir dosya. Lansman
öncesi o dosyaya dokunmak, kazandığından çok risk getiriyordu.

**Karar gerekiyor (M5):** ya `styles.css` core'dan bir `tokens.css` import
etsin, ya da drift scriptine "token'lar iki dosyada aynı mı" kontrolü eklensin.

### 2. `src/integrations/supabase/types.ts` silindi — Lovable geri üretebilir

Üretilmiş DB tipleri artık `src/lib/core/db/types.ts`'te; eski yol silindi ve 4
import core'a çevrildi. Lovable bir şema değişikliğinde tipleri **eski yola**
yeniden üretirse iki kopya oluşur (derleme kırılmaz, sessiz sapma olur).

Not: silinen dosya zaten **bayattı** — `v_routine_maintenance_status` view'ı
(P22-G, 2026-07-28) ve `buyer_profiles.company_name` nullable hali içinde yoktu.
Yani Lovable bu dosyayı her şema değişiminde otomatik üretmiyor. Yine de M2'de
şema değişikliği yapılırken `src/integrations/supabase/types.ts`'in geri gelip
gelmediğine bakılmalı.

### 3. Şema borcu bulgusu — `buyer_profiles.company_name` zaten nullable

`P23-Mobile.md` M1 listesinde "NOT NULL → nullable" işi duruyor. Canlı DB'de
kontrol edildi (2026-07-29): **zaten nullable.**

```sql
select is_nullable from information_schema.columns
where table_schema='public' and table_name='buyer_profiles'
  and column_name='company_name';
-- YES
```

Kalan üç şema borcu (`Safran Soğanı` `default_unit='adet'`, `listings` CHECK
`min_order <= quantity`, `useSetDefaultAddress`) doğrulanmadı — bu tur kapsamı
dışındaydı.
