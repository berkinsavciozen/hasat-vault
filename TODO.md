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
- [x] Detaylı E2E Devam Turu — 6 Yeni Senaryo (S8-S13) + 4 Kritik "Sahte Veri/Kırık Akış" Bug'ı
- [x] **Tam Konuşma Denetimi (Audit)** — bu oturumun tamamı TODO ile karşılaştırılıp 6 eksik madde bulunup eklendi (bkz. aşağıdaki yeni maddeler)

### P16 — TÜM SERİ TAMAMLANDI ✅
(Detaylar önceki sürümlerde)

---

## 🔴 ŞİMDİ — Temmuz 2026

### Yasal & Altyapı
- [ ] Şahıs şirketi kur · iyzico başvurusu · Alan adı araştır · Twilio Verify service · **WhatsApp Business başvurusu → hemen başla**
- [ ] **[Audit'te bulundu] Rekabet hukuku uzmanına gerçek danışma** — Rekabet Kurumu risk denetiminin asıl önerisiydi ("fiyat uyarı sistemi + AI fiyat tavsiyeleri özelliklerini bir avukata göster"). Teknik önlemler (band-based AI, RLS, moderasyon trigger'ı) uygulandı ama **gerçek hukuki onay hâlâ alınmadı** — Ekim safran sezonu / gerçek GMV başlamadan önce mutlaka yapılmalı.
- [ ] **[Audit'te bulundu — kontrol edilmeli] Çerez onay banner'ı** — `/privacy` sayfası "çerez politikası" bölümü içeriyor ama gerçek bir çerez onay mekanizması (banner) olup olmadığı hiç konuşulmadı/kontrol edilmedi. KVKK/GDPR açısından potansiyel bir boşluk olabilir — önce mevcut durumu kontrol et, gerekirse ekle.

### Düşük öncelikli cila
- [ ] `farmer.journal.new.tsx` "Önce bir parsel ekle" statik metni · `formatCrop()` heuristik · Landing page'de çok fazla rol-seçim butonu
- [ ] MCP yazma tool'larında aralıklı "No approval received" hatası — retry ile geçiyor
- [ ] **Lovable platform bug'ı (bilinen, düzeltilemez):** `plan_mode=false` sinyali agent'ın kod tool'larına bazen yansımıyor.
- [ ] Storage bucket public/private ayarını her zaman canlı doğrula (Lovable'ın tool'u migration'a yansımayabiliyor)
- [ ] `Supabase:list_tables` tool'u aralıklı "No approval received" veriyor, `execute_sql` ile `information_schema.tables` sorgusuna geçilerek atlatılabiliyor
- [ ] `useHasat` (Zustand mock store) referansları için **tüm codebase'de bir kez daha tarama** yapılmalı — Üretici Detay ve Abonelik Oluştur sayfalarında bulundu, başka yerde de olabilir
- [ ] **[Audit'te bulundu] MCP tool setine rate limiting eklenmesi** — orijinal MCP güvenlik tasarımında not edilmişti ("düşük öncelik ama not edilmeli"), henüz uygulanmadı. Özellikle `create_offer`/`respond_to_offer` gibi yazma tool'larının kötüye kullanım potansiyeli var.
- [ ] **[Audit'te bulundu] `crop_requests` tablosu için admin inceleme arayüzü** — tablo + kullanıcı-tarafı form var ama bilinçli olarak "bu turda gerek yok" denip ertelenmişti, hiçbir zaman ileriye dönük bir görev olarak işaretlenmedi. Kullanıcılar ürün talebi bıraktıkça bunları görüp `crop_config`'e taşıyacak bir arayüz gerekiyor.
- [ ] **[Audit'te bulundu] MCP tool setini genişletme** — topluluk paylaşımı, sertifika yükleme, **abonelik oluşturma** (UI/mutation düzeltildi ama hâlâ MCP tool'u yok) için hiç tool yok. Tam E2E otomasyon kapsamı için gerekli.
- [ ] **[Audit'te bulundu] Berkin'in eski test hesabındaki (`d040fc98`, "Berkin Savcıözen") tek başına kalan "Domates" ilanı** — S9'da zararsız bulundu ama temizlik listesine hiç eklenmemişti, temizlenmeli.

---

## 🏗️ Lovable Build Sırası — TAMAMLANDI ✅

> Sıradaki: **Ağustos E2E test kapsamının tamamlanması** (sertifika foto, community reply UI, AI chat kalitesi, bildirimler, referral tam akış) + yukarıdaki audit maddeleri.

---

### ✅ Detaylı E2E Devam Turu — 6 Yeni Senaryo + 4 Kritik Bug *(Tamamlandı — 2026-07-16)*

Önceki turda düzeltilen alıcı sayfaları + mock data üzerinden, Hasat MCP (Zeynep/Ahmet sabit-OTP) + Supabase MCP ile sistematik bir E2E devam turu yapıldı. 6 yeni senaryo eklendi (bkz. `Build/E2E-QA`), süreçte **4 yeni kritik bug** bulunup düzeltildi.

**Yeni E2E senaryoları (hepsi ✅):** S8 Global Fiyatlar, S9 Keşfet üretici ismi (+ Ahmet şehir düzeltmesi), S10 Fotoğraf gösterimi (canlı SS ile), S11 Görüşmeler inbox, S12 Raporlar veri doğruluğu, S13 Abonelikler.

**Bu turda bulunan 4 kritik bug (hepsi düzeltildi):**
1. Günlük (journal) fotoğraf yükleme tamamen sahteydi — dosya seçici UI çalışıyormuş gibi görünüyordu ama `photos: []` hardcoded gönderiliyordu.
2. Parti (Batch) sayfasında ilanın kapak fotoğrafı hiç gösterilmiyordu.
3. **🔴 Üretici Detay sayfası hiç Supabase'e bağlı değildi** — eski prototip Zustand store'undan (`useHasat`) tamamen sahte veri (rating, yorumlar, verim geçmişi vb.) çekiyordu. "Alıcı Yorumları" özelliğinin DB'de hiç karşılığı olmadığı doğrulandı, kullanıcı kararıyla tamamen kaldırıldı.
4. **🔴 Abonelik Oluştur sayfası hiçbir zaman gerçek bir abonelik oluşturmuyordu** — aynı sahte store + gerçek `useCreateSubscription()` mutasyonu hiç çağrılmıyordu.

**tsgo:** Her düzeltme turunda temiz.

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
- [ ] **Gerçek bir "Alıcı Yorumları/Reviews" özelliği** — şimdilik tamamen kaldırıldı, ileride gerçek bir `reviews` tablosu + sipariş-sonrası puanlama akışıyla inşa edilebilir

---

## 📋 Lovable Prompt Yazma Kuralları

(1-32 önceki sürümde — devam:)
33. **[Audit'te eklendi] Bir özellik "bu turda gerek yok" diye bilinçli olarak ertelendiğinde, mutlaka bir TODO maddesi olarak işaretle** — `crop_requests` admin arayüzü tam olarak bu şekilde kayboldu: doğru bir karardı (kapsamı şişirmemek), ama hiçbir yerde "gelecekte yapılacak" olarak yazılmadığı için oturumlar arasında unutuldu.
34. **[Audit'te eklendi] Bir teknik/güvenlik denetiminin "önerilen aksiyon" kısmı, teknik düzeltme kadar önemlidir ve ayrıca takip edilmeli** — Rekabet Hukuku denetiminin en kritik önerisi ("bir avukata danış") teknik mitigasyonlar tamamlanınca gözden kayboldu. Bir denetim hem "biz ne yaptık" hem "kullanıcının hâlâ yapması gereken" olarak iki ayrı listede tutulmalı.

---

## 📌 Kararlar

(önceki tablo + eklenenler:)
| **Alıcı Yorumları/Reviews özelliği** | DB'de hiç karşılığı yoktu, tamamen kaldırıldı — gerçek bir review tablosu istenirse Faz 2'de ayrı bir özellik olarak inşa edilecek |
| **Mock/sahte veri prensibi** | Bir UI elementi çalışıyormuş gibi görünse de, altındaki mutation/veri kaynağının gerçekten DB'ye bağlı olduğu ayrıca doğrulanmalı |
| **Tam konuşma denetimi (2026-07-16)** | Ayrı geçmiş konuşma bulunamadı (proje kapsamında tek uzun oturum) — denetim, bu oturumun sıkıştırılmış özeti dahil tam içeriğinin TODO ile karşılaştırılmasıyla yapıldı, 6 eksik madde bulundu ve eklendi |

---

## 📋 Son Test Sonuçları

### Detaylı E2E Devam Turu (2026-07-16) ✅
| Senaryo/Bug | Sonuç |
|---|---|
| S8-S13 (Fiyatlar, Keşfet, Fotoğraf, Görüşmeler, Raporlar, Abonelikler) | ✅ Hepsi geçti |
| Bug: Günlük foto yükleme, Parti kapak fotoğrafı, Üretici Detay mock veri, Abonelik Oluştur hiç yazmıyordu | ✅ Hepsi düzeltildi |
| tsgo (3 ayrı düzeltme turu) | ✅ Hepsi temiz |

### Tam Konuşma Denetimi (2026-07-16) ✅
| Kontrol | Sonuç |
|---|---|
| `conversation_search`/`recent_chats` ile ayrı geçmiş konuşma arandı | Bulunamadı — tek oturum |
| Sıkıştırılmış özet + tüm oturum içeriği TODO ile karşılaştırıldı | ✅ 6 eksik madde bulundu, kullanıcı onayıyla eklendi |

### (Önceki tüm test sonuçları — Alıcı Sayfaları, Mock Data, Fiyatlandırma, Tedarik Zinciri, Landing, RLS, Rekabet Hukuku, MCP Server, P16 serisi — değişmedi, önceki sürümlerde)
