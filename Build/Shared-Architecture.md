---
title: Hasat — Paylaşılan Mimari (Web + Mobil)
updated: 2026-07-30
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
| Teklif oluşturma (çoklu-parti) | `rpc_create_offer(p_farmer_id, p_items, p_delivery, p_delivery_date, p_note, p_subscription_id, p_source_recipe_id)` | Kural #106'nın uygulanması — bkz. aşağıda |

**Sonuç:** Mobil uygulama *ince* olur. İş mantığını yeniden yazmaz, RPC çağırır. Web'de bir kural değişince mobil otomatik doğru davranır.

### `rpc_create_offer` (P23-M7-a, 2026-08-04)

**Neden:** Canlı şemada teklif oluşturmak için RPC yoktu — web `offers` INSERT +
`offer_items` INSERT'i client'ta iki ayrı adımda yapıyordu (`insertOfferWithItems`,
`hasat-d2c-marketplace/src/lib/hasat/queries.ts`), ikinci adım başarısız olursa
JS tarafında best-effort bir "rollback" (offer'ı sil) deniyordu — atomik değildi.
Mobilde aynı akışı yeniden yazmak kural #106'nın tam uyardığı durumdu (bkz.
`dispatch_sms`/`send-sms` sapması, iki kez yaşandı). RPC bu orkestrasyonu tek
transaction'a taşıdı; her iki client da artık aynı fonksiyonu çağırıyor.

**SECURITY INVOKER yeterli** — kontrol edildi, DEFINER'a gerek yok:
`buyer_id` parametre olarak alınmıyor, `auth.uid()`'den okunuyor; mevcut RLS
politikaları (`Buyers insert offers` → `auth.uid() = buyer_id`, `Buyer inserts
own offer items` → `offers.buyer_id = auth.uid()` üzerinden) invoker'ın kimliğiyle
zaten doğru çalışıyor. `auth.uid()` NULL ise (anon) fonksiyon RLS'e varmadan
kendi `RAISE EXCEPTION 'Oturum bulunamadı'`'ı fırlatıyor.

**Mevcut trigger'ları bozmadan, üstünde çalışıyor:**
- `trg_offer_received` (`notify_offer_received`, AFTER INSERT) — RPC gerçek bir
  INSERT yaptığı için otomatik tetikleniyor, hiç dokunulmadı.
- `trg_enforce_offer_stock` (`enforce_offer_stock`) — yalnızca `status→'accepted'`
  geçişinde (BEFORE UPDATE) çalışıyor, oluşturma anında hiç kontrol yoktu. RPC
  oluşturma anı için AYNI stok hesaplama mantığını (batch_total>0 ise
  `listing_harvest_entries` toplamı, yoksa `listings.quantity` fallback'i,
  `accepted` teklifler rezerve) kendi içinde tekrarlıyor + `min_order` kontrolü
  ekliyor. Accept-time trigger hâlâ ikinci savunma hattı, kaldırılmadı.
- `enforce_offer_transitions`, `enforce_offer_accept_turn` — RPC yalnızca INSERT
  yapıyor, bu trigger'lar UPDATE'e bağlı, etkilenmiyor.

**Web geçişi ayrı, revert edilebilir commit:** `insertOfferWithItems`'ın iki
ayrı `.insert()` çağrısı `(supabase as any).rpc("rpc_create_offer", {...})`'e
çevrildi; public arayüz (`OfferInput`/`MultiBatchOfferInput`, `useCreateOffer`,
`useCreateMultiBatchOffer`) değişmedi — çağıran taraf (`buyer.payment.tsx`)
hiç dokunulmadı. Web'in üretilmiş tip dosyası (`src/lib/core/db/types.ts`)
yeni RPC'yi henüz tanımıyor — mevcut kod tabanının (`get_price_history_summary`,
`dispatch_sms`, `get_farmer_rating_summary` gibi) zaten kullandığı
`(supabase as any).rpc(...)` deseni izlendi, `hasat-core`'a dokunulmadı (sync
PR riski yok — bkz. `TODO.md` → P23-M7-a build log).

**Doğrulama (kural #96, gerçek veri + transaction/ROLLBACK):** tek parti ✅ ·
çoklu parti (2 farklı ilan, ağırlıklı ortalama fiyat doğru: 32 birim, ₺348.13
ort.) ✅ · min_order altı → `Minimum sipariş miktarının altında` ile reddedildi ✅ ·
stoktan fazla → `Stok yetersiz (batch)` ile reddedildi ✅ · gerçek insert +
`notify_offer_received` zinciri (in-app bildirim + `dispatch_sms` →
`net.http_request_queue`'ya 1 satır kuyruklandı) çalıştığı kanıtlandı, ROLLBACK
sonrası kuyruk 0'a döndü (gerçek SMS gitmedi) ✅ · anon → `Oturum bulunamadı`
ile reddedildi ✅ · RLS zaten `buyer_id`'nin parametre olmaması sayesinde
başkası adına oluşturmayı yapısal olarak imkansız kılıyor.

⚠️ **Bilinen, önceden var olan sınır (düzeltilmedi — kapsam dışı):**
`offers.quantity`/`offers.price_per_unit` (geriye dönük uyumluluk için
tutulan agregat alanlar) çoklu-parti tekliflerde item'ların RAW miktarını
topluyor, birim dönüşümü yapmıyor — aynı crop'un farklı birimde partileri
(ör. safran'ın 15g/500g/100kg partileri) varsa bu toplam anlamsızlaşır.
Bu, RPC'nin yeni bir davranışı DEĞİL: web'in eski `insertOfferWithItems`'ı
da (kural gereği aynen taşındı) hep böyleydi. P21-A'nın "mixed-unit
toplama riski" düzeltmesi yalnızca DISPLAY katmanını (Keşfet grup kartı,
ürün detay toplamı) kapsamıştı, bu agregat alanı değil. Asıl doğruluk
kaynağı zaten `offer_items` (her satır kendi birim/fiyatıyla doğru) —
sorun yalnızca legacy özet alanların okunmasında. Mobil ürün ekranının
kendi TOPLAM göstergesi bu turda `convertQuantity` ile düzeltildi (bkz.
`hasat-mobile/src/lib/hasat/offers.ts` → `useCropCanonicalUnit`), ama
`offers.quantity` kolonunun kendisine dokunulmadı — anlamını değiştirmek
(agregat alanın semantiğini kanonik birime çevirmek) ayrı bir karar,
Berkin'e bırakıldı.

⚠️ **Web'in gerçek tarayıcı/click-through testi bu oturumda yapılamadı** — bu
ortamın ağ politikası `efuqpiaavrzimvstpdpm.supabase.co`'ya (REST API) doğrudan
erişimi engelliyor (kural #103, P24/M4-a/M5-a'da da aynı kısıt yaşanmıştı,
`curl` ile yeniden doğrulandı: bağlantı kurulamadı). Kanıt SQL seviyesinde
(yukarıdaki tablo) — web'in çağıracağı ile birebir aynı parametrelerle,
gerçek trigger zinciriyle. Berkin'in kendi tarayıcısında bir teklif oluşturup
doğrulaması gerekiyor.

---

## Katman 2 — `hasat-core` paylaşılan TypeScript paketi

DB'ye taşınamayan kısım:

| İçerik | Örnek | Ne zaman |
|---|---|---|
| Üretilmiş DB tipleri | `supabase gen types typescript` çıktısı | ✅ M1 |
| Design token'ları | Marka renkleri, spacing, tipografi ölçeği | ✅ M1 |
| Saf yardımcılar | `convertQuantity()` | ✅ M1 |
| Saf yardımcılar (kalan) | coverage skoru, offer-status etiketleri, para/tarih formatlama | ⬜ M5-b/M9 |
| **Supabase storage adapter** | `core/supabase/client.ts` — `createHasatSupabaseClient()`, storage parametreli | ✅ **M5-a (2026-07-30)** |
| **Supabase sorgu fonksiyonları** | `fetchListings()`, `fetchRecipe()` … | ⬜ **M5-b** |
| **TanStack Query hook'ları** | Mobilde `@tanstack/react-query` kuruldu (M5-a); ortak hook'lar (`useListings()`, `useRecipes()`) henüz core'a taşınmadı | ⬜ **M5-b** |
| Zod şemaları | Form/RPC girdi doğrulama | ⬜ M5-b |

### ✅ Storage adapter M5-a'da taşındı (2026-07-30) — M1'den revize geçmişi

Bu doküman ilk yazıldığında (2026-07-28) storage adapter M1'e yazılmıştı, sonra
M1 uygulanırken M5'e ertelendi (2026-07-29, aşağıdaki gerekçe o zaman
yazılmıştı, tarihi kayıt olarak korunuyor). **M5-a'da taşındı:**
`hasat-core/core/supabase/client.ts` → `createHasatSupabaseClient(url, key,
{ storage })` — `persistSession`/`autoRefreshToken` sabit, `storage` platform
başına parametre (web: `localStorage`, mobil: `expo-secure-store` tabanlı
`LargeSecureStore` — AES + AsyncStorage + SecureStore anahtarı, Supabase'in
resmi Expo deseni; SecureStore'un ~2048 byte/değer sınırı Supabase'in oturum
payload'unu aşıyor, bu yüzden ham SecureStore yetmiyordu). Web tarafında
`src/integrations/supabase/client.ts` bu factory'yi kullanacak şekilde minimal
değiştirildi (ayrı PR — canlı auth'a dokunan tek nokta); `storage` opsiyonel
yapıldı ki web'in SSR yolu (`typeof window === 'undefined'` → `storage:
undefined`) davranışı birebir korunsun. `npm run typecheck` + `npm run build`
temiz; gerçek tarayıcıda OTP girişi bu oturumun ağ politikası Supabase host'unu
engellediği için doğrulanamadı (P24/M4-a'da da aynı kısıt yaşanmıştı) —
Berkin'in kendi tarayıcısında doğrulaması gerekiyor.

#### Orijinal erteleme gerekçesi (2026-07-29, tarihi kayıt)

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
   └── git subtree ──► hasat-mobile          : src/lib/core/   ✅ M5-a'da kuruldu (2026-07-30)
```

- **git subtree** (submodule değil) — Lovable'ın build'i submodule init etmez; subtree düz dosya olarak iner
- `hasat-core`'da bir **GitHub Action**, değişiklikte **her iki hedef repoya birden** (matrix) PR açar
  — M1'de tek hedefliydi (sadece web); **ikinci hedef `hasat-mobile` M5-a'da eklendi**
- Her core dosyasının başında: `// hasat-core — BU DOSYAYI BURADA DÜZENLEME`
- Hedef repoda bir **hash manifest** dosyası (`src/lib/core/.manifest`) → sapma saniyeler içinde tespit edilir

### Boru hattının kurulu hali (M1: 2026-07-29 · ikinci hedef + sürüm-gerisi kontrolü: M5-a, 2026-07-30)

| Parça | Nerede | Ne yapar |
|---|---|---|
| Kaynak | `hasat-core/core/` | Tek doğruluk kaynağı. Build step yok, publish yok. |
| Manifest | `core/.manifest` → `src/lib/core/.manifest` (her iki hedefte) | Her core dosyasının sha256'sı |
| Sapma scripti | `hasat-core` → `npm run drift -- <yol>` | **DEĞİŞTİRİLMİŞ / EKSİK / FAZLA** üç durumu yakalar, farkta exit 1 |
| **Sürüm-gerisi scripti** (**M5-a'da eklendi**) | `hasat-core` → `npm run drift:freshness -- <yol>` | Hedefin manifest'i `core/.manifest`'ten farklıysa (bekleyen sync PR'ı) exit 1 — bkz. aşağıdaki "kör nokta" bölümü, artık kapalı |
| Sync Action | `.github/workflows/sync-to-web.yml` | `core/**` değişince **her iki hedefe de** (matrix) PR açar |
| Drift Action | `.github/workflows/drift-check.yml` | Her iki hedefte de günlük hem sapma hem sürüm-gerisi kontrolü yapar |

**Gereken secret:** `hasat-core` reposunda `SYNC_TOKEN` (her iki hedef repoya
push + PR yetkisi olan PAT). ✅ Web için 2026-07-29'da eklendi ve iki workflow
canlıda yeşil doğrulandı; **kapsamına `hasat-mobile` M5-a'da Berkin tarafından
eklendi.** Ayrıca sync job'ının `permissions: contents: write` izni var —
`core-dist` dalını kendi reposuna geri itiyor.

**M5-a doğrulaması (statik, bu oturumda):** `hasat-core`'a `git subtree add
--prefix=src/lib/core <hasat-core> core-dist --squash` ile `hasat-mobile`'a
indirildi; `diff core/.manifest src/lib/core/.manifest` → birebir aynı.
`check-drift.mjs` ve `check-manifest-freshness.mjs` her ikisi de kasten
bozulup (bir core dosyasını hedefte elle düzenleyip / `hasat-core`'da yeni bir
değişiklik yapıp hedefe hiç sync etmeyerek) exit 1 verdiği, sonra geri alınıp
tekrar exit 0'a döndüğü doğrulandı. GitHub Action'ların kendisi (matrix,
gerçek CI koşusu) bu oturumda tetiklenmedi — sadece PR merge edildiğinde
çalışır; canlı doğrulaması Berkin'e kalıyor (M1'in Action'ları da aynı şekilde
merge sonrası doğrulanmıştı).

> ⚠️ `hasat-core` şu an **public**. İçinde sır yok, ama üretilmiş DB tipleri
> tüm tablo/kolon/enum/view adlarını açıkça gösteriyor. Bilinçliyse sorun yok;
> değilse Settings → General → Change visibility ile private yapılabilir
> (subtree ve Action'lar private'da da aynen çalışır).

### ✅ Drift kontrolünün kör noktası — KAPANDI (M5-a, 2026-07-30)

> Bu bölüm P23-M2-ek'te bulunan bir açık maddeydi (M5'e bırakılmıştı). Tarihi
> kayıt olarak aşağıda korunuyor; "Önerilen düzeltme" artık uygulandı.

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

**M5-a'da uygulandı:** `scripts/check-manifest-freshness.mjs` (yeni script,
`diff`'in kendisi değil ama aynı işi görüyor + Türkçe, aksiyon işaret eden bir
hata mesajı) `drift-check.yml`'e üçüncü adım olarak eklendi (matrix'teki her
iki hedef için de çalışıyor). Artık cevaplanan soru: hem "hedef kopya elle mi
değiştirilmiş?" (`check-drift.mjs`) hem "hedef `hasat-core`'un GÜNCEL
sürümünde mi?" (`check-manifest-freshness.mjs`) — ikisi birden yeşil olmadan
bekçi susmuyor. **Kasten bozup exit 1 verdiği, sonra geri alındığı bu oturumda
doğrulandı** (bkz. yukarıdaki "Boru hattının kurulu hali" → "M5-a
doğrulaması").

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
| **M5-a** | **Storage adapter** (`core/supabase/client.ts`) — web'in davranışı değişmeden | ✅ 2026-07-30 |
| M5-b | Sorgu fonksiyonları + TanStack Query hook'ları + zod şemaları (mobilde kütüphane kuruldu, ortak hook'lar henüz core'a taşınmadı) | ⬜ |
| **M5-a** | **Drift script'ine sürüm-gerisi kontrolü** (hedef `.manifest` ↔ `hasat-core` `core/.manifest`) — bkz. "Drift kontrolünün kör noktası" | ✅ 2026-07-30 |
| **M5-a** | Sync Action'a ikinci hedef (`hasat-mobile`) | ✅ 2026-07-30 |
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

**M5-a notu:** Mobil taraf bu riski büyütmedi — `hasat-mobile/tailwind.config.js`
üçüncü bir kopya yazmak yerine `core/design/tokens.ts`'i doğrudan `require`
ediyor (Node'un TS dosyalarını doğrudan çalıştırabilmesi sayesinde, sabit bir
yedekle). Yani hâlâ iki yer var (`styles.css` ↔ `tokens.ts`), üç değil —
mobil `tokens.ts`'e bağlı, `styles.css`'e değil. Yukarıdaki karar hâlâ açık.

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
