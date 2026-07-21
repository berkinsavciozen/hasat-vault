---
title: Hasat — Master Roadmap & Build Log
updated: 2026-07-21
tags:
  - hasat
  - todo
  - roadmap
  - build
---

# Hasat — Master Roadmap & Build Log
> GTM Hedefi: **25 Ağustos 2026** · Günlük 1-2 saat · `[C]` = Claude ile · `[C Web]` = Web Claude (Lovable+Supabase+GitHub MCP+Hasat MCP)

---

## ✅ TAMAMLANDI

### Teknik Build
(Önceki maddeler değişmedi — bkz. önceki sürüm)
- [x] P18 serisi + P19 (backend+UI) tamamen tamamlandı
- [x] P19-C — İzmir tam olgun, global commodity API araştırması kapandı
- [x] P17-B — Sipariş sonrası akış sıfırdan inşa edildi, canlı doğrulandı
- [x] P17-C — Karşılıklı Değerlendirme Sistemi tamamlandı + 3 gerçek boşluk düzeltildi
- [x] P17-F — Tekrar Sipariş + Şube Adresleri tamamlandı, Abonelikler'e entegre
- [x] Alıcı Bildirim Tercihleri sayfası eklendi + `buyer.account.tsx` bayat veri bug'ı düzeltildi
- [x] **Abonelik → Sipariş bağlantısı (`subscription_id`) eklendi + escrow yanlış vaadi düzeltildi** (2026-07-21) — bkz. detay

### P16 — TÜM SERİ TAMAMLANDI ✅
(Detaylar önceki sürümlerde)

---

## 🔎 BENCHMARK RE-AUDIT (Temmuz 2026)
(Değişmedi)

---

## 🔴 ŞİMDİ — Temmuz 2026
(Değişmedi — bkz. önceki sürüm, "Şahıs şirketi kur" hâlâ işaretsiz)

### Düşük öncelikli cila
(Değişmedi — bkz. önceki sürüm)
- [ ] **[Bu turda bulundu, düşük öncelik] `useSetDefaultAddress` sadece seçilen adresi `is_default=true` yapıyor, diğerlerini `false`'a çekmiyor** — teorik olarak birden fazla "varsayılan" adres oluşabilir. Aktif bug değil (UI şu an tek seferde bir işlem yapıyor) ama küçük bir düzeltme gerekiyor: aynı işlemde `buyer_id`'nin diğer tüm adreslerini `false`'a çekmeli.

---

## 🏗️ Lovable/Supabase Build Sırası

> **P19 + P17-B + P17-C + P17-F tamamen bitti, hepsi canlı doğrulandı.** Abonelik↔sipariş bağlantısı da tamamlandı. **P17-A ve P17-D şirket kuruluşuna bağlı, bloke.** Sıradaki: **P17-E (yapılandırılmış RFQ)** veya **P17-G (KPI görünümleri)**.

---

## 🎨 P18 — ARAYÜZ YENİLEME — **SERİ TAMAMEN TAMAMLANDI ✅**
(Değişmedi)

## 🟢 P19 — BORSA DENEYİMİ — **TAMAMEN TAMAMLANDI ✅**
(Değişmedi)

## 🟠 P19-C — KAYNAK & ÜRÜN TİPİ GENİŞLETME — **KAPANDI**
(Değişmedi — bkz. önceki sürüm)

---

## 🟣 P17 — GÜVEN ÇEKİRDEĞİ *(devam ediyor — 2026-07-21)*

### Kapsam ve sıralama
| Kod | Konu | Şiddet | Durum |
|---|---|---|---|
| P17-B | Teslim/kargo/iptal/ihtilaf akışı | P0 | ✅ **TAMAMLANDI** |
| P17-C | Karşılıklı değerlendirme (rating/review) | P0 | ✅ **TAMAMLANDI** |
| P17-F | Tekrar sipariş + şube adresleri + abonelik bağlantısı | P0 (HoReCa) | ✅ **TAMAMLANDI** |
| P17-A | Gerçek bloke ödeme (escrow) | P0 | 🔴 Bloke — şirket kuruluşu + iyzico başvurusu |
| P17-D | Fatura/e-müstahsil | P0 (B2B) | 🔴 Bloke — vergi mükellefiyeti gerektiriyor |
| P17-E | Yapılandırılmış RFQ | P1 | Sırada, bağımsız |
| P17-G | KPI ölçüm görünümleri | destek | Sırada, bağımsız |

### ✅ P17-B / P17-C / P17-F — TAMAMLANDI
(Değişmedi — bkz. önceki sürüm)

### ✅ Abonelik → Sipariş Bağlantısı + Escrow Metni Düzeltmesi *(2026-07-21, kapsamlı canlı doğrulama)*

**Berkin'in sorduğu iki soru üzerine bulunan gerçek boşluklar:**
1. **Keşfedilebilirlik:** Abone olma girişi sadece `/buyer/producer/$id` (belirli bir üretici profili) üzerinden — genel bir "Abone Ol" keşif noktası yok. **Karar: şimdilik profil-önce akışı kabul edildi**, ek bir keşif noktası inşa edilmedi.
2. **Ayırt edilemezlik (gerçek bug, düzeltildi):** `offers` tablosunda abonelikle bağlantı hiç yoktu — çiftçi, abonelikten gelen bir siparişi normal siparişten ayıramıyordu.
3. **Bonus bulgu — yanlış vaat (gerçek bug, düzeltildi):** `buyer.subscription.$producerId.tsx`, henüz var olmayan bir "escrow" (bloke ödeme, P17-A) mekanizmasını vaat ediyordu ("Ödeme hasattan 2 hafta önce alınır").

**Uygulanan çözüm:**
- **Şema** ✅ — `offers` tablosuna opsiyonel `subscription_id uuid references harvest_subscriptions(id)` kolonu eklendi.
- **Uçtan uca bağlantı** ✅ — `buyer.subscriptions.tsx`'teki "Şimdi Sipariş Ver" modalı → `subscriptionId`'yi search param olarak taşıyor → `buyer.offer.$listingId.tsx`'in `validateSearch`'ü doğru parse ediyor → `submit()` → `setPendingOffer` → `useCreateOffer` → `offers.subscription_id` gerçekten yazılıyor. Dosya okumasıyla her adım tek tek doğrulandı.
- **Okuma tarafı** ✅ — `useFarmerOffers`/`useBuyerOffers` sorguları `subscription_id`'yi çekiyor, `dbToOffer`/`dbToOrder` (ORDER_SELECT genişletildi: `offer:offers(...,subscription_id,...)`) alanı taşıyor.
- **Çiftçi rozeti** ✅ — `farmer.orders.index.tsx`'teki hem `OfferCard` hem `OrderCard`'da, `subscriptionId` varsa buyer adının yanında **"🔁 Abonelik Siparişi"** rozeti (tooltip: "Bu sipariş bir Sürekli Tedarik aboneliğinden geldi"). Yönetim/akış farkı yok — sadece görsel ayırt edilebilirlik.
- **Dürüstlük düzeltmesi** ✅ — Escrow metni kaldırıldı, yerine: "ℹ️ Hasat yaklaştığında üreticiyle mevcut ödeme akışı (havale/kart) üzerinden iletişime geçilecek."

**Doğrulama:** Şema+RLS canlı SQL ile, tüm zincir (`buyer.subscriptions.tsx` → `buyer.offer.$listingId.tsx` → `queries.ts` → `farmer.orders.index.tsx`) dosya okumasıyla uçtan uca takip edildi.

---

## 🟡 AĞUSTOS 2026 — Soft Launch
(Değişmedi)

## 🔵 SAFRAN SEZONU / ⬜ SONRAKI FAZLAR
(Değişmedi)

---

## 📋 Lovable/Supabase Prompt Yazma Kuralları

(1-75 önceki sürümde — devam:)
76. **[Bu turda eklendi] Yeni bir özellik (örn. abonelik) inşa edilirken sayfadaki AÇIKLAYICI METİNLER de kod kadar dikkatli okunmalı** — `buyer.subscription.$producerId.tsx`'teki "escrow" vaadi, kod incelemesinde değil, kullanıcının ürün sorusu üzerine sayfa tekrar okunduğunda bulundu; henüz var olmayan bir backend özelliğini (P17-A) vaat eden metinler, gerçek bir bug kadar zararlı olabilir.
77. **[Bu turda eklendi] Kullanıcının "bunu nasıl yapacak/ayırt edecek" sorusu genellikle gerçek bir şema/UX boşluğuna işaret eder** — soru sorulduğunda önce ürünü/akışı savunmak yerine şemayı kontrol etmek (bu turda `offers` tablosunda ilgili kolon var mı diye bakmak) doğru ilk adımdır.

---

## 📌 Kararlar

(önceki tablo + eklenenler:)
| **P17-F tamamlandı (2026-07-21)** | Abonelikler sayfasına entegre tasarlandı, tam canlı doğrulandı. |
| **Alıcı bildirim tercihleri + bug düzeltmesi tamamlandı (2026-07-21)** | Gerçek boşluk kapatıldı, bayat veri bug'ı düzeltildi. |
| **Abonelik keşfedilebilirliği (2026-07-21)** | Şimdilik profil-önce akış kabul edildi, ek keşif noktası inşa edilmedi — ileride tekrar değerlendirilebilir. |
| **Abonelik→sipariş bağlantısı + escrow metni düzeltmesi (2026-07-21)** | `subscription_id` eklendi, çiftçi rozeti eklendi, yanlış escrow vaadi kaldırıldı. |

---

## 📋 Son Test Sonuçları

### Abonelik → Sipariş Bağlantısı (2026-07-21) ✅
| Kontrol | Sonuç |
|---|---|
| `offers.subscription_id` şeması | ✅ Canlı SQL ile doğrulandı |
| Modal → search param → offer sayfası → `setPendingOffer` → `useCreateOffer` zinciri | ✅ Dosya okumasıyla uçtan uca doğrulandı |
| `useFarmerOffers`/`useBuyerOffers` sorguları `subscription_id` çekiyor | ✅ |
| `dbToOffer`/`dbToOrder` alanı taşıyor | ✅ |
| Çiftçi tarafında "🔁 Abonelik Siparişi" rozeti (OfferCard + OrderCard) | ✅ |
| Escrow yanlış vaadi kaldırıldı, dürüst metinle değiştirildi | ✅ |

### (Önceki tüm test sonuçları — değişmedi, önceki sürümlerde)
