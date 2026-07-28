---
title: Hasat — Paylaşılan Mimari (Web + Mobil)
updated: 2026-07-28
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

| İçerik | Örnek |
|---|---|
| Üretilmiş DB tipleri | `supabase gen types typescript` çıktısı |
| Saf yardımcılar | `convertQuantity()`, coverage skoru, offer-status etiketleri, para/tarih formatlama |
| Supabase sorgu fonksiyonları | `fetchListings()`, `fetchRecipe()` … |
| TanStack Query hook'ları | `useListings()`, `useRecipes()` — **TanStack Query React Native'de de çalışıyor** |
| Zod şemaları | Form/RPC girdi doğrulama |
| Design token'ları | Marka renkleri, spacing, tipografi ölçeği |

### Dağıtım — kritik kısıt

`hasat-d2c-marketplace` reposunun `main` branch'i **Lovable'ın GitHub sync bot'u (`gpt-engineer-app[bot]`) tarafından yönetiliyor.** Bu repoyu pnpm workspace / monorepo yapısına çevirmek Lovable'ın build'ini ve sync'ini kırma riski taşır — yani projenin en büyük hız kaynağını riske atar.

**Bu yüzden monorepo YOK.** Bunun yerine:

```
hasat-core (ayrı repo)
   │
   ├── git subtree ──► hasat-d2c-marketplace : src/lib/core/
   └── git subtree ──► hasat-mobile          : src/lib/core/
```

- **git subtree** (submodule değil) — Lovable'ın build'i submodule init etmez; subtree düz dosya olarak iner
- `hasat-core`'da bir **GitHub Action**, değişiklikte iki repoya birden PR açar
- Her core dosyasının başında: `// hasat-core — BU DOSYAYI BURADA DÜZENLEME`
- Her iki repoda bir **hash manifest** dosyası → sapma saniyeler içinde tespit edilir

### KURAL (#105)

`src/lib/core/` altındaki dosyalar `hasat-core`'dan gelir. **Lovable dahil hiç kimse burada düzenleme yapmaz.** Değişiklik `hasat-core`'da yapılır, iki repoya PR ile iner.

> Bu kural kozmetik değil: Lovable daha önce paylaşılan bir dosyanın üzerine yazdı (P24 regresyonu). Görünür "dokunma" işareti + drift kontrolü tam bu senaryoya karşıdır.

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

| Ne zaman | İş |
|---|---|
| M1 | `hasat-core` repo + subtree + GitHub Action + drift guard + do-not-edit işaretleri + design token'lar + storage adapter |
| M1 | Küçük şema borçları (bkz. `P23-Mobile.md` M1) |
| M2 | Katman 1 RPC'leri (tarif eşleştirme, birim dönüşümü, alışveriş listesi, funnel view) |
| M2 | `device_tokens` tablosu |
| M9 | Bildirim event map konsolidasyonu (lansman sonrası) |

---

## Doğrulama kuralı

M1'in çıkış kriteri: **web uygulamasında sıfır regresyon.** Subtree kurulumu mevcut çalışan bir sistemin dosya yapısına dokunuyor — Lovable build'inin ve canlı önizlemenin hâlâ çalıştığı bağımsız olarak doğrulanmadan M2'ye geçilmez.
