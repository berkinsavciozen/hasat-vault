---
title: Hasat — Master Roadmap & Build Log
updated: 2026-07-16
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
- [x] Supabase schema + seed data · Farmer/Buyer akışları (bireysel dahil) · AI özellikleri · Bildirim sistemi
- [x] P15 Teklif/Müzakere state machine + P15-Completion
- [x] Auth/profiles fix'leri · Mobil responsiveness auditi · Landing page v2/v3 (premium "güven altyapısı" + kopya/SEO)
- [x] **Hasat MCP Server** — 16 tool, OAuth 2.1 · **Rekabet Hukuku Risk Denetimi** · **Kapsamlı RLS Güvenlik Denetimi**
- [x] **E2E-QA Dokümanı** (`Build/E2E-QA`) · **Tedarik Zinciri Görsel Yeniden Tasarımı**
- [x] **Fiyatlandırma Sistemi Tam Yeniden İnşası** (price_feed kaldırma → sipariş-türevli `price_history`) — bkz. detaylı bölüm
- [x] **10 Çiftçi + 10 Alıcı Mock Data** — 2 yıllık gerçekçi sipariş/günlük geçmişi — bkz. detaylı bölüm
- [x] **Alıcı Sayfaları Bug Düzeltmeleri + Redesign** (Mesajlar/Raporlar/Abonelikler/Fiyatlar/Keşfet/Fotoğraflar) — bkz. detaylı bölüm

### P16 — TÜM SERİ TAMAMLANDI ✅
(Detaylar önceki sürümlerde)

---

## 🔴 ŞİMDİ — Temmuz 2026

### Yasal & Altyapı
- [ ] Şahıs şirketi kur · iyzico başvurusu · Alan adı araştır · Twilio Verify service · **WhatsApp Business başvurusu → hemen başla**

### Düşük öncelikli cila
- [ ] `farmer.journal.new.tsx` "Önce bir parsel ekle" statik metni · `formatCrop()` heuristik · Landing page'de çok fazla rol-seçim butonu
- [ ] MCP yazma tool'larında aralıklı "No approval received" hatası — retry ile geçiyor
- [ ] **Lovable platform bug'ı (bilinen, düzeltilemez):** `plan_mode=false` sinyali agent'ın kod tool'larına bazen yansımıyor, mesaj tekrar gönderimi gerekiyor. Bu oturumda da birkaç kez yaşandı (özellikle uzun/kritik prompt'larda).
- [ ] **Yeni:** Lovable'ın `supabase--storage_update_bucket` tool'u (bucket public/private ayarı) bazen migration'a yansımıyor — bu oturumda parcel-photos/listing-photos için tespit edildi, Claude tarafından doğrudan SQL ile (`UPDATE storage.buckets SET public=true`) düzeltildi. Gelecekte benzer bir bucket ayarı isteği verilirse, canlı doğrulamayı unutma.
- [ ] MCP yazma tool'larında (`create_parcel`, `create_offer`) aralıklı hata — bu oturumda tekrar gözlendi, retry ile geçti.

---

## 🏗️ Lovable Build Sırası — TAMAMLANDI ✅

> Fiyatlandırma sistemi yeniden inşası + mock data + alıcı sayfaları düzeltmeleri de bitti. Sıradaki: **Ağustos E2E test kapsamının tamamlanması** (sertifika foto, community reply UI, AI chat kalitesi, bildirimler, referral tam akış).

---

### ✅ Alıcı Sayfaları Bug Düzeltmeleri + Redesign *(Tamamlandı — 2026-07-16)*

Berkin'in canlı testinde 4 bulgu + 1 redesign talebi geldi, hepsi araştırılıp kanıtlanarak düzeltildi:

1. **🔴 Kritik — Fiyatlar sayfası tamamen boştu.** Kök neden: `get_price_history_summary()` RPC'si **case-sensitive** çalışıyordu (`'Domates'` ≠ `'domates'`), ama gerçek kod `listings.crop`'tan büyük harfle çağırıyordu — canlı doğrulanan, aynı kök nedenin (trigger'da daha önce düzeltilen) RPC'nin kendisinde de var olduğu bir bug. **Düzeltildi:** RPC artık önce `crop_config`'ten case-insensitive kanonik değeri çözüp onu kullanıyor.
2. **Fiyatlar sayfası tasarım kararı:** Sadece görüntüleyen çiftçinin kendi ürünlerini gösteriyordu — **rekabet hukuku analiziyle** ("kim görebilir" değil "ne kadar granüler/gerçek-zamanlı" risk yaratır) bunun sakıncalı olmadığı, aksine daha savunulabilir olduğu netleştirildi. **Düzeltildi:** `useCropsWithPriceData()` ile sistemdeki tüm aktif+resmi-kaynaklı ürünleri gösteren, hem çiftçi hem alıcıya açık (`/buyer/prices` eklendi) tek bir global sayfa.
3. **🔴 Kritik regresyon — Keşfet'te ilan sahibi ismi "Üretici" gösteriyordu.** Kök neden: Daha önceki otomatik güvenlik düzeltmesi (IBAN/telefon sızıntısını kapatan) `profiles` tablosundaki genel "herkes çiftçi profilini okuyabilir" iznini kaldırmıştı, ama `useActiveListings`/`useListing` hâlâ doğrudan `profiles` tablosuna join yapıyordu — RLS her yeni etkileşimde bu join'i engelleyip generic fallback'e düşürüyordu. **Düzeltildi:** Broad RLS policy'si geri getirilmedi (IBAN/telefon riski tekrar açılmasın), bunun yerine `attachPublicFarmerProfiles()` helper'ı ile güvenli `public_farmer_profiles` view'ından ikinci bir sorgu yapılıp client-side merge ediliyor.
4. **🔴 Kritik — Parsel/ilan fotoğrafları hiç görünmüyordu.** Kök neden: `parcel-photos` ve `listing-photos` storage bucket'ları `public=false` ayarlıydı ama kod `getPublicUrl()` kullanıyordu (private bucket'ta bu URL 400/403 döner). **Düzeltildi:** Bucket'lar public yapıldı (Lovable'ın kendi tool'u migration'a yansıtmadı, Claude doğrudan SQL ile düzeltti ve doğruladı).
5. **Alıcı Mesajlar/Raporlar/Abonelikler redesign** — plan-mode audit'te netleşti: **Mesajlar bozuk değildi, hiç yapılmamıştı** (sadece "yakında" stub'ı, gerçek mesajlaşma zaten `NegotiationThread` içinde çalışıyordu). Raporlar'da **2 gerçek veri hatası** bulundu (ödenmemiş teklifler "Toplam Harcama"ya dahil ediliyordu; pazarlık sonrası fiyat değil orijinal fiyat kullanılıyordu). Abonelikler sadece UI zenginliği eksikti.
   - **Uygulanan:** Mesajlar → gerçek "Görüşmeler" gelen kutusu (`useBuyerConversations()`, son mesaj önizlemesiyle). Raporlar → KPI'lar (sadece ödenmiş, `isPaidOrder()`/`orderRowTotal()` helper'larıyla düzeltilmiş), ürün kırılımı, "Tedarikçi güveni" listesi, CSV export. Abonelikler → hasat tarihi/miktar/kilitli fiyat detayları, persona-nötr boş durum mesajı.

**Ek olarak Claude'un kendi hatası bulunup düzeltildi:** Mock data'daki 10 ana ilanı "yayınlarken" `price_per_unit` unutulmuştu (hepsi ₺0 kalmıştı) — canlı testte fark edilip direkt SQL ile düzeltildi.

**tsgo:** Temiz. Tüm düzeltmeler kod incelemesi + canlı RPC/bucket testleriyle doğrulandı.

---

### ✅ 10 Çiftçi + 10 Alıcı Mock Data — 2 Yıllık Gerçekçi Geçmiş *(Tamamlandı — 2026-07-16)*

Fiyatlandırma sisteminin gerçek verilerle çalıştığını göstermek ve Hasat'ın "1 yıl sonraki vizyonunu" sergilemek için kapsamlı bir mock data seti üretildi (Claude tarafından doğrudan Supabase MCP ile, Lovable'a büyük bir seed script yazdırmak yerine — hem daha kontrollü hem her adımda yeni trigger/RPC'yi gerçek veriyle anında test etme imkanı sağladı).

**Kapsam:**
- **9 yeni çiftçi + 9 yeni alıcı** profili (mevcut sabit-OTP hesapları Ahmet/Zeynep korunarak, üzerine veri eklendi) — gerçek bölge/ürün eşleştirmesiyle: Domates→Antalya (5 çiftçi, agregasyon eşiği için), Patates→Niğde, Elma→Amasya, Safran→Safranbolu (2 çiftçi), Lavanta→Isparta. 10 alıcı, 5 persona tipine (restoran/otel/market/ihracatçı/bireysel) dağıtıldı.
- **110 teklif→sipariş→ödeme** döngüsü, 2 yıla yayılı gerçekçi fiyat trendiyle (araştırılan TZOB/TEPGE referanslarına dayalı) — `price_history` tablosunu otomatik dolduran trigger üzerinden.
- **400 günlük kaydı**, 9 work-type (bkz. aşağıdaki bulgu) kullanılarak ürün/bölgeye özgü gerçekçi frekansla (safran hasat penceresinde yoğun, patates/elma seyrek+mevsimsel, domates sürekli).

**Süreçte bulunan ve düzeltilen 4 gerçek bug (mock data'dan bağımsız, prod'u etkileyen):**
1. `price_feed` kaldırma işlemi, **draft-ilan otomasyon trigger'ını kırmıştı** (`create_draft_listings_for_parcel()` hâlâ silinen tabloyu referans alıyordu) — tüm gerçek kullanıcılar için parsel oluşturmayı bozan kritik bir regresyon, hemen düzeltildi.
2. `record_order_price_history()` trigger'ı **case-sensitivity** yüzünden hiçbir crop için gerçek satır yazmıyordu (`listings.crop`="Fındık" vs `crop_config.crop`="fındık") — canlı rollback-testiyle bulunup düzeltildi.
3. **Yapısal izlenebilirlik boşluğu:** `crop_config`'in zorunlu lifecycle adımlarından "ekim" (domates/patates) ve "replant"+"drying" (safran) için hiçbir journal work-type haritalanmamıştı — bu ürünler yapısal olarak "Tam İzlenebilir" seviyesine hiç ulaşamıyordu. **Düzeltme:** 3 yeni work-type eklendi (`dikim`→ekim, `kurutma`→drying, `distilasyon`→distillation), safran'ın "replant" key'i "ekim"e birleştirildi.
4. `elma`/`patates` **mevsimsel pencere olarak işaretlenmemişti** (rolling_30d kalmıştı, safran/lavanta gibi rolling_365d olmalıydı) — hasat mevsimi dışında hep "yeterli veri yok" gösteriyordu, Claude tarafından doğrudan düzeltildi.

**Fiyat tutarlılık doğrulaması:** TZOB/TEPGE gerçek verileriyle çapraz kontrol edildi (patates ₺6,13/kg Kasım 2024 referansı tam tutarlı bulundu). Domates ve elma'nın 2025-2026 fiyat noktaları, gerçek enflasyon trendini (TZOB: domates market fiyatı yıllık %77, elma %110 arttı) yansıtacak şekilde yukarı düzeltildi (domates ~₺25/kg, elma ~₺26/kg'a).

**Sonuç:** Domates kümesinde 5 çiftçiyle gerçek agregasyon çalışıyor (`insufficient_data:false`, gerçek ortalama), niş ürünlerde (safran/lavanta/elma/patates) "yeterli veri yok" durumu doğru şekilde tetikleniyor — hem "veri var" hem "veri yok" senaryosu gerçek verilerle sergileniyor.

---

### ✅ Fiyatlandırma Sistemi Tam Yeniden İnşası *(Tamamlandı — 2026-07-16)*

**Karar süreci:** `price_feed` özelliğinin (çiftçinin serbestçe fiyat girebildiği self-reported sistem) hem hukuki risk hem doğruluk sorunu yarattığı belirlendi. İki alternatif değerlendirildi (1: gerçek sipariş verisinden çıkarım, 2: resmi/dış kaynak — HKS) → **ikisi birleştirildi**, `price_feed` tamamen kaldırıldı.

**Uygulanan mimari:**
- `crop_config` genişletildi: `has_official_price_source`, `official_source_name`, `price_window_type` (`rolling_30d`/`rolling_365d`), `is_seasonal_harvest`
- Yeni tablo `crop_requests` — kullanıcıların sistemde olmayan ürün taleplerini kaydetmesi için
- Yeni tablo `price_history` (`source`: `'order'` | `'hks'`) — `farmer_id` asla public expose edilmiyor, sadece `get_price_history_summary()` SECURITY DEFINER RPC'si üzerinden agregat okunabiliyor
- Yeni trigger `record_order_price_history()` — bir teklif `payment_status='paid'` olduğunda otomatik `price_history(source='order')` satırı yazıyor
- Aynı rekabet hukuku güvenceleri korundu: agregat-only, min-5-farklı-çiftçi eşiği, mevsimsel/geciktirilmiş pencere, AI hiçbir zaman spesifik sayı önermiyor

**HKS (Hal Kayıt Sistemi) segmentasyonu:** Domates/Patates/Elma `has_official_price_source=true` işaretlendi (HKS kapsamı sadece sebze/meyve — doğrulandı). Safran/Lavanta/Gül'de resmi kaynak yok, manuel kalıyor.

**Faz 2 (roadmap'e not edildi):** Şu an sadece 5 ürün için manuel araştırılan veriyle mock dolduruldu. İleride TEPGE/TÜİK/HKS'den **tüm** `crop_config` satırları için otomatik veri çeken bir servis (Supabase Edge Function + `pg_cron`) kurulacak.

---

### ✅ Tedarik Zinciri Görsel Yeniden Tasarımı, Landing Kopya+SEO, RLS Güvenlik Denetimi, Rekabet Hukuku Denetimi, Hasat MCP Server, Landing v2

(Detaylar önceki sürümlerde — değişmedi)

---

## 🟡 AĞUSTOS 2026 — Soft Launch — ŞİMDİ BURADAYIZ

### E2E Test — devam ediyor
- [x] Farmer/Buyer onboarding E2E · Landing page E2E · Draft-İlan pipeline E2E · S1,S2,S3,S5,S7 (bkz. `Build/E2E-QA`)
- [x] **Fiyatlandırma sistemi tam E2E** (mock data ile gerçek agregasyon + insufficient-data senaryoları doğrulandı)
- [x] **Alıcı Fiyatlar/Keşfet/Mesajlar/Raporlar/Abonelikler** canlı test edildi, bulunan buglar düzeltildi
- [ ] Kalan kapsam: sertifika (foto upload), community+reply UI, AI chat kalitesi, bildirimler, referral tam akış

### Teknik Final
- [ ] hasat.lovable.app → custom domain yayını · iyzico canlı ödeme testi
- [x] P16-C tam havale e2e testi — kapandı ✅

---

## 🔵 SAFRAN SEZONU / ⬜ SONRAKI FAZLAR

(değişmedi + eklenen:)
- [ ] **Faz 2 — Otomatik Resmi Fiyat Veri Servisi:** TEPGE/TÜİK/HKS'den tüm crop_config ürünleri için otomatik veri çeken pg_cron tabanlı servis

---

## 📋 Lovable Prompt Yazma Kuralları

(1-24 önceki sürümde — devam:)
25. **Bir tabloyu kaldırırken, ona referans veren TÜM trigger/fonksiyonları tara** — `price_feed` kaldırılırken `create_draft_listings_for_parcel()` gözden kaçmıştı, gerçek kullanıcı akışını kırdı. `grep`/kod taraması yeterli değil, canlı bir test (örn. yeni parsel oluşturma) ile doğrulanmalı.
26. **Case-sensitivity kontrolü artık standart bir kontrol maddesi olmalı** — bu oturumda AYNI hata 3 farklı yerde (trigger, RPC, ve dolaylı olarak crop eşleştirmesi gereken her fonksiyonda) tekrarlandı. Bir crop/kategori metnini DB fonksiyonunda eşleştiren her yeni kod, `lower()` normalizasyonunu en baştan içermeli.
27. **Otomatik güvenlik düzeltmeleri (Lovable'ın kendi tarayıcısı) sonrası, o tabloya bağımlı TÜM public-facing sorguları yeniden test et** — IBAN/telefon sızıntısını kapatan doğru düzeltme, farklı bir yerde (Keşfet'te üretici ismi) gerçek bir regresyon yarattı. Güvenlik düzeltmesi + fonksiyonel regresyon testi birlikte gitmeli.
28. **Storage bucket public/private ayarını her zaman canlı doğrula** — Lovable'ın bucket-güncelleme tool'u migration'a yansımayabilir, `SELECT public FROM storage.buckets` ile teyit şart.
29. **Mock/test verisi üretirken birim tutarlılığına dikkat** — bir ürünün fiyatını yanlış birimde (kg yerine g bazında) hesaplamak, sonradan fark edilmesi zor, mantıksız görünen sayılara yol açar (bkz. safran ₺14.000/gram hatası).

---

## 📌 Kararlar

(önceki tablo + eklenenler:)
| **Fiyatlandırma kaynağı** | `price_feed` (self-reported) tamamen kaldırıldı → sipariş-türevli `price_history` + opsiyonel resmi (HKS) kaynak, ikisi ayrı segment olarak gösteriliyor, asla birleştirilmiyor |
| **Fiyatlar sayfası kapsamı** | Çiftçiye özel değil, **sistemdeki tüm ürünler için, hem çiftçi hem alıcıya açık, global** — rekabet hukuku açısından bu daha savunulabilir (şeffaflık pro-rekabetçi) |
| **HKS kapsamı** | Sadece sebze/meyve (5957 sayılı Kanun) — domates/patates/elma'da var, safran/lavanta/baharat/tıbbi bitkide yok |
| **Mock data üretim yöntemi** | Claude, Lovable'a seed script yazdırmak yerine doğrudan Supabase MCP ile üretiyor — daha kontrollü, her adımda gerçek trigger/RPC testi yapılabiliyor |
| **Güvenli public join deseni** | Public-facing sorgular asla `profiles` tablosuna doğrudan FK-embed join yapmamalı, `public_farmer_profiles` view'ından ikinci sorgu + client-side merge kullanılmalı |

---

## 📋 Son Test Sonuçları

### Alıcı Sayfaları Bug Düzeltmeleri + Redesign (2026-07-16) ✅
| Test | Sonuç |
|---|---|
| RPC case-sensitivity fix (`'Domates'` vs `'domates'`) | ✅ Birebir aynı doğru sonuç |
| Global Fiyatlar sayfası (`/buyer/prices` + `/farmer/prices`) | ✅ |
| "Üretici" isim bug'ı → `attachPublicFarmerProfiles()` | ✅ |
| Storage bucket public fix | ✅ Claude tarafından doğrudan doğrulandı |
| Mesajlar/Raporlar/Abonelikler redesign + 2 veri düzeltmesi | ✅ |
| tsgo | ✅ |

### 10 Çiftçi + 10 Alıcı Mock Data (2026-07-16) ✅
| Test | Sonuç |
|---|---|
| Profiller/parseller/draft-ilanlar | ✅ |
| 4 regresyon/bug bulundu ve düzeltildi (draft-ilan trigger, case-sensitivity, izlenebilirlik boşluğu, mevsimsel pencere) | ✅ |
| 110 sipariş, gerçekçi 2 yıllık fiyat trendi | ✅ TZOB/TEPGE ile çapraz doğrulandı |
| 400 günlük kaydı, 9 work-type | ✅ |
| Domates agregasyonu (5 çiftçi) | ✅ Gerçek ortalama gösteriyor |

### Fiyatlandırma Sistemi Tam Yeniden İnşası (2026-07-16) ✅
| Test | Sonuç |
|---|---|
| price_feed kaldırma + price_history/crop_requests/crop_config genişletmesi | ✅ |
| get_price_history_summary RPC | ✅ (sonradan case-sensitivity düzeltmesiyle tam çalışır hale geldi) |
| Rekabet hukuku güvenceleri korundu | ✅ |

### (Önceki tüm test sonuçları — Tedarik Zinciri, Landing Kopya+SEO, RLS Güvenlik, Rekabet Hukuku, MCP Server, Landing v2, P16 serisi — değişmedi, önceki sürümlerde)
