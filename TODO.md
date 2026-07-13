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
- [x] Supabase schema + seed data · Farmer/Buyer akışları · AI özellikleri · Bildirim sistemi
- [x] Hasat MCP Server (16 tool) · Rekabet Hukuku Risk Denetimi · Kapsamlı RLS Güvenlik Denetimi
- [x] E2E-QA Dokümanı (`Build/E2E-QA`) · Tedarik Zinciri Görsel Yeniden Tasarımı
- [x] Fiyatlandırma Sistemi Tam Yeniden İnşası · 10+10 Mock Data (2 yıllık) · Alıcı Sayfaları Bug Düzeltmeleri
- [x] **Detaylı E2E Devam Turu — 6 Yeni Senaryo (S8-S13) + 4 Kritik "Sahte Veri/Kırık Akış" Bug'ı** — bkz. detaylı bölüm

### P16 — TÜM SERİ TAMAMLANDI ✅
(Detaylar önceki sürümlerde)

---

## 🔴 ŞİMDİ — Temmuz 2026

### Yasal & Altyapı
- [ ] Şahıs şirketi kur · iyzico başvurusu · Alan adı araştır · Twilio Verify service · **WhatsApp Business başvurusu → hemen başla**

### Düşük öncelikli cila
- [ ] `farmer.journal.new.tsx` "Önce bir parsel ekle" statik metni · `formatCrop()` heuristik · Landing page'de çok fazla rol-seçim butonu
- [ ] MCP yazma tool'larında aralıklı "No approval received" hatası — retry ile geçiyor
- [ ] **Lovable platform bug'ı (bilinen, düzeltilemez):** `plan_mode=false` sinyali agent'ın kod tool'larına bazen yansımıyor. Bu oturumda da (özellikle "Üretici Detay" rebuild prompt'unda) tekrar yaşandı.
- [ ] Storage bucket public/private ayarını her zaman canlı doğrula (Lovable'ın tool'u migration'a yansımayabiliyor)
- [ ] **Yeni:** `Supabase:list_tables` tool'u bu oturumda 3 kez "No approval received" hatası verdi, `execute_sql` ile `information_schema.tables` sorgusuna geçilerek atlatıldı — not edildi.
- [ ] **Yeni:** `buyer.subscription.$producerId.tsx` düzeltildi ama bu tespit süreci gösterdi ki — eski prototip döneminden kalan `useHasat` (Zustand mock store) referansları için **tüm codebase'de bir kez daha tarama** yapılmalı, benzer "gerçek görünen ama sahte veri" desenleri başka yerde de olabilir.

---

## 🏗️ Lovable Build Sırası — TAMAMLANDI ✅

> Sıradaki: **Ağustos E2E test kapsamının tamamlanması** (sertifika foto, community reply UI, AI chat kalitesi, bildirimler, referral tam akış) + `useHasat` mock store'unun codebase'de başka kullanıldığı yer var mı taraması.

---

### ✅ Detaylı E2E Devam Turu — 6 Yeni Senaryo + 4 Kritik Bug *(Tamamlandı — 2026-07-16)*

Önceki turda düzeltilen alıcı sayfaları + mock data üzerinden, Hasat MCP (Zeynep/Ahmet sabit-OTP) + Supabase MCP ile sistematik bir E2E devam turu yapıldı. 6 yeni senaryo eklendi (bkz. `Build/E2E-QA`), süreçte **4 yeni kritik bug** bulunup düzeltildi.

**Yeni E2E senaryoları (hepsi ✅):**
- **S8 — Global Fiyatlar (5 ürün):** Domates 5-çiftçi agregasyonu + 4 niş ürünün "yeterli veri yok" durumu tek seferde doğrulandı.
- **S9 — Keşfet'te gerçek üretici ismi:** 17/17 aktif ilan doğru isim/şehirle eşleşti. **Yan bulgu:** Ahmet'in `profiles.city`'si önceki bir testte yanlışlıkla "Antalya" olmuş (gerçeği Safranbolu) — düzeltildi.
- **S10 — Fotoğraf gösterimi:** Bucket-public düzeltmesi (önceki tur) canlı ekran görüntüsüyle doğrulandı, gerçekten çalışıyor.
- **S11 — Görüşmeler inbox:** 10 teklif, hepsi doğru üretici/ürün/durumla eşleşti.
- **S12 — Raporlar veri doğruluğu:** `isPaidOrder()`/`orderRowTotal()` düzeltmeleri yapısal olarak doğrulandı.
- **S13 — Abonelikler:** Gerçek bir abonelik oluşturuldu; ayrıca `harvest_subscriptions` ilişkisinin `profiles` RLS policy'sinde açıkça kapsandığı (Keşfet'teki gibi bir "Üretici" regresyonu olmayacağı) doğrulandı.

**Bu turda bulunan 4 kritik bug:**
1. **Günlük (journal) fotoğraf yükleme tamamen sahteydi.** `farmer.journal.new.tsx`'teki dosya seçici UI tam çalışıyormuş gibi görünüyordu (seçilen dosya adını gösteriyor, onay ikonu veriyor) ama `save()` her zaman `photos: []` gönderiyordu — seçilen dosya hiçbir zaman storage'a yüklenmiyordu. `useCreateEntry()`'de de upload mantığı hiç yoktu. **Düzeltme:** `uploadHarvestPhotos()` eklendi (parsel/ilan yükleyicileriyle aynı desen), gerçek `File` nesnesi mutation'a taşındı.
2. **Parti (Batch) sayfasında ilanın kapak fotoğrafı hiç gösterilmiyordu** — `ProvenanceTimeline` component'i zaten hasat kaydı fotoğraflarını doğru render ediyordu (sadece veri gelmiyordu, madde 1 düzelince otomatik çalışacak), ama ilanın kendi `listing.photos`'u sayfada hiç yoktu. **Düzeltme:** Üretici Detay sayfasındaki carousel deseniyle tutarlı bir kapak fotoğrafı galerisi eklendi.
3. **🔴 En kritik — Üretici Detay sayfası (`buyer.producer.$id.tsx`) hiç Supabase'e bağlı değildi.** `useHasat((s) => s.producers.find(...))` ile eski bir prototip Zustand store'undan **tamamen sahte/hardcoded** veri çekiyordu (rating, yorumlar, verim geçmişi, sonraki hasat, sipariş sayısı, yanıt süresi, GPS doğrulama rozeti, "0 Anlaşmazlık" — hepsi kurgusal). Gerçek bir çiftçi ID'siyle 404 veriyordu ya da yanlış bilgi gösteriyordu. **"Alıcı Yorumları" özelliğinin DB'de hiçbir karşılığı olmadığı doğrulandı** (`reviews`/`ratings` tablosu hiç yok) — kullanıcı kararıyla bu bölüm tamamen kaldırıldı, sahte veriyle doldurulmadı. **Düzeltme:** Sayfa `public_farmer_profiles`, gerçek aktif ilanlar, gerçek parseller, `harvest_entries`'ten türetilen verim/kalite istatistikleri, gerçek sipariş sayısı ve (varsa) gerçek abonelik / yoksa `crop_config` hasat penceresi tahminiyle tamamen yeniden inşa edildi.
4. **🔴 Aynı kritiklikte — Abonelik Oluştur sayfası (`buyer.subscription.$producerId.tsx`) hiçbir zaman gerçek bir abonelik oluşturmuyordu.** Aynı sahte `useHasat` store'unu kullanıyordu VE "Abonelik Oluştur" butonu, DB'de zaten var olan gerçek `useCreateSubscription()` mutasyonunu hiç çağırmıyordu — sadece local, client-only bir Zustand action'ı (`addSubscription`) çalıştırıyordu. **Yani bu sayfadan geçen hiçbir alıcı gerçekte hiçbir zaman abone olmamıştı.** **Düzeltme:** Gerçek çiftçi/ilan verisine bağlandı, gerçek mutasyonu çağırıyor, hata durumunda toast gösteriyor, sadece başarılı mutation sonrası başarı dialogu açılıyor.

**Not:** `useHasat` mock store'unun codebase'de başka kullanıldığı yer olup olmadığı taranmadı — bir sonraki oturumda yapılmalı (bkz. yukarı, düşük öncelikli cila listesi).

**tsgo:** Her 3 düzeltme turunda da temiz.

---

### ✅ Alıcı Sayfaları Bug Düzeltmeleri + Redesign, 10+10 Mock Data, Fiyatlandırma Sistemi Tam Yeniden İnşası, Tedarik Zinciri, Landing Kopya+SEO, RLS Güvenlik Denetimi, Rekabet Hukuku Denetimi, Hasat MCP Server, Landing v2

(Detaylar önceki sürümlerde — değişmedi)

---

## 🟡 AĞUSTOS 2026 — Soft Launch — ŞİMDİ BURADAYIZ

### E2E Test — devam ediyor
- [x] Farmer/Buyer onboarding · Landing page · Draft-İlan pipeline · S1-S13 (bkz. `Build/E2E-QA`) — hepsi ✅
- [x] Fiyatlandırma sistemi tam E2E · Alıcı Fiyatlar/Keşfet/Mesajlar/Raporlar/Abonelikler/Üretici Detay/Fotoğraflar — hepsi canlı test edildi, bulunan buglar düzeltildi
- [ ] Kalan kapsam: sertifika (foto upload — journal'daki gibi sahte olabilir, kontrol edilmeli), community+reply UI, AI chat kalitesi, bildirimler, referral tam akış
- [ ] `useHasat` mock store taraması (yukarıdaki not)

### Teknik Final
- [ ] hasat.lovable.app → custom domain yayını · iyzico canlı ödeme testi
- [x] P16-C tam havale e2e testi — kapandı ✅

---

## 🔵 SAFRAN SEZONU / ⬜ SONRAKI FAZLAR

(değişmedi + eklenen:)
- [ ] Faz 2 — Otomatik Resmi Fiyat Veri Servisi (TEPGE/TÜİK/HKS)
- [ ] **Gerçek bir "Alıcı Yorumları/Reviews" özelliği** — şimdilik tamamen kaldırıldı (sahte veri istemedik), ileride gerçek bir `reviews` tablosu + sipariş-sonrası puanlama akışıyla inşa edilebilir

---

## 📋 Lovable Prompt Yazma Kuralları

(1-29 önceki sürümde — devam:)
30. **"Çalışıyormuş gibi görünen ama sahte" desenine karşı özellikle dikkatli ol** — bir UI elementinin (dosya seçici, form) görsel olarak doğru tepki vermesi (dosya adını göstermesi, onay ikonu vermesi), verinin gerçekten kaydedildiği anlamına gelmez. Her "Kaydet"/"Oluştur" butonunun gerçekten DB'ye yazdığını (mutation'ın doğru çağrıldığını, hardcoded boş/sahte değer göndermediğini) ayrıca doğrula.
31. **Eski prototip/mock store'lara (örn. Zustand `useHasat`) referans veren her sayfa şüpheli kabul edilmeli** — bu oturumda 2 tam sayfa (Üretici Detay, Abonelik Oluştur) bu şekilde tamamen gerçek veriden kopuk çalışıyordu. Bir route dosyasında `useHasat`/benzer bir local-only store import edildiğini görürsen, hemen o sayfanın gerçek Supabase'e bağlı olup olmadığını sorgula.
32. **Bir özelliğin (örn. review/rating) DB şemasında karşılığı olup olmadığını asla varsaymadan kontrol et** — "Alıcı Yorumları" UI'da güzel görünüyordu ama hiçbir tabloya bağlı değildi. Sahte veriyle doldurmak yerine, gerçek altyapı yoksa özelliği dürüstçe kaldırmak (ya da açıkça "yakında" işaretlemek) doğru yaklaşım.

---

## 📌 Kararlar

(önceki tablo + eklenenler:)
| **Alıcı Yorumları/Reviews özelliği** | DB'de hiç karşılığı yoktu, tamamen kaldırıldı (sahte veri gösterilmiyor) — gerçek bir review tablosu istenirse Faz 2'de ayrı bir özellik olarak inşa edilecek |
| **Mock/sahte veri prensibi** | Bir UI elementi çalışıyormuş gibi görünse de, altındaki mutation/veri kaynağının gerçekten DB'ye bağlı olduğu ayrıca doğrulanmalı — "görsel doğruluk" ile "fonksiyonel doğruluk" farklı şeyler |

---

## 📋 Son Test Sonuçları

### Detaylı E2E Devam Turu (2026-07-16) ✅
| Senaryo/Bug | Sonuç |
|---|---|
| S8 — Global Fiyatlar (5 ürün) | ✅ |
| S9 — Keşfet üretici ismi (+ Ahmet şehir düzeltmesi) | ✅ |
| S10 — Fotoğraf gösterimi (canlı SS ile) | ✅ |
| S11 — Görüşmeler inbox | ✅ |
| S12 — Raporlar veri doğruluğu | ✅ |
| S13 — Abonelikler + RLS ilişki kapsamı | ✅ |
| Bug: Günlük foto yükleme tamamen sahte | ✅ Düzeltildi |
| Bug: Parti sayfası kapak fotoğrafı yok | ✅ Düzeltildi |
| Bug: Üretici Detay tamamen mock veri | ✅ Gerçek veriye bağlandı |
| Bug: Abonelik Oluştur hiç DB'ye yazmıyordu | ✅ Gerçek mutasyona bağlandı |
| tsgo (3 ayrı düzeltme turu) | ✅ Hepsi temiz |

### (Önceki tüm test sonuçları — Alıcı Sayfaları, Mock Data, Fiyatlandırma, Tedarik Zinciri, Landing, RLS, Rekabet Hukuku, MCP Server, P16 serisi — değişmedi, önceki sürümlerde)
