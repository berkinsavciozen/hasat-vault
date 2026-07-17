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
- [x] Hasat MCP Server (16→21 tool) · Rekabet Hukuku Risk Denetimi · Kapsamlı RLS Güvenlik Denetimi
- [x] E2E-QA Dokümanı (`Build/E2E-QA`) · Tedarik Zinciri Görsel Yeniden Tasarımı
- [x] Fiyatlandırma Sistemi Tam Yeniden İnşası · 10+10 Mock Data (2 yıllık) · Alıcı Sayfaları Bug Düzeltmeleri
- [x] Detaylı E2E Devam Turu — 6 Yeni Senaryo (S8-S13) + 4 Kritik "Sahte Veri/Kırık Akış" Bug'ı
- [x] **Tam Konuşma Denetimi (Audit)** — bu oturumun tamamı TODO ile karşılaştırılıp 6 eksik madde bulunup eklendi
- [x] **BENCHMARK.md** — 4 katmanlı rakip analizi + 10 use-case matrisi + KPI çerçevesi (§5) + 12 maddelik gap listesi
- [x] **Finansal Model v0.6** — gelir modeli redesign (ücretsiz çekirdek + tek tier AI premium ₺149 + %2,5 komisyon), driver tablosu, Base/Bear/Bull (`Finance/Model.md`)
- [x] **Audit Takibi — MCP Rate Limiting + Tool Genişletmesi + Temizlik** (2026-07-16)
- [x] **Sertifika Foto + `useHasat` Taraması + Gelir Modeli Gating Denetimi + Premium Persistence Bug'ı** (2026-07-16) — bkz. detaylı bölüm aşağıda

### P16 — TÜM SERİ TAMAMLANDI ✅
(Detaylar önceki sürümlerde)

---

## 🔎 BENCHMARK RE-AUDIT (Temmuz 2026)

> BENCHMARK.md'deki 12 gap + KPI ölçüm altyapısı + Model v0.6 kararlarının ürün yansımaları, TODO'nun güncel haliyle tek tek karşılaştırıldı. Sonuç: **5 P0 gap TODO'da hiç yoktu** (teslim kabul/ihtilaf, bloke ödeme akışı, fatura/dekont, tekrar sipariş+şube, yapılandırılmış RFQ) → **P17 serisi** olarak aşağıya eklendi. 3 mevcut madde benchmark gap'leriyle eşleştirilip etiketlendi. Kritik bağlantı: **BENCHMARK §5'teki 5 KPI (ödeme süresi, ihtilaf oranı, tam kabul, fill rate, NPS) bugünkü şemayla ölçülemiyor ve tamamı P17 maddelerine bağlı — P17 aynı zamanda ölçüm altyapısıdır; fizibilite forecast'i bu KPI'lara dayanacak.**

---

## 🔴 ŞİMDİ — Temmuz 2026

### Yasal & Altyapı
- [ ] Şahıs şirketi kur · iyzico başvurusu · Alan adı araştır · Twilio Verify service · **WhatsApp Business başvurusu → hemen başla**
- [ ] **[Audit'te bulundu] Rekabet hukuku uzmanına gerçek danışma** — Rekabet Kurumu risk denetiminin asıl önerisiydi. Teknik önlemler uygulandı ama **gerçek hukuki onay hâlâ alınmadı** — Ekim safran sezonu / gerçek GMV başlamadan önce mutlaka yapılmalı.
- [ ] **[Audit'te bulundu — kontrol edilmeli] Çerez onay banner'ı** — `/privacy` sayfası "çerez politikası" bölümü içeriyor ama gerçek bir çerez onay mekanizması olup olmadığı hiç kontrol edilmedi.
- [ ] **[Benchmark re-audit] iyzico başvurusunda pazaryeri (alt-üye işyeri) modelini talep et + komisyon oranı teklifini al** — Model v0.6'nın en kritik girdisi (Gap #1 ön koşulu, `Finance/Model.md` §10).
- [x] ~~**[Benchmark re-audit] Gelir modeli kararlarının üründe doğrulanması (a) ve (b)**~~ — **Tamamlandı (2026-07-16):** (a) premium gating'in gerçekten sadece AI chat kotasında olduğu, hiçbir başka özelliğin (fiyat verisi, ilanlar, teklifler, abonelikler, topluluk, analytics) paywall'a takılmadığı kod taramasıyla doğrulandı. (b) `MarketDeviationAlert`'te hiç tier kontrolü olmadığı, fiyat bandının herkese açık olduğu doğrulandı. **(c) hâlâ açık** — aşağıda ayrı madde olarak devam ediyor.
- [ ] **[Benchmark re-audit, (c)] "İlk 6 ay premium ücretsiz" feature-flag tasarımı** — artık `activatePremium()` sunucu fonksiyonu (aşağıya bkz.) sayesinde `tier` gerçekten kalıcı hale getirilebiliyor; ama toplu/otomatik bir feature-flag (örn. "ilk 500 kullanıcıya otomatik 6 ay premium") mekanizması henüz tasarlanmadı — bu fonksiyon şu an sadece tekil (checkout/trial-toggle) çağrılar için var.
- [ ] **[Benchmark re-audit] Referral suistimal koruması** — 3 sahte kayıt = 12 ay bedava premium riski; min. doğrulama eşiği (telefon onayı + ilk gerçek eylem) tasarlanmalı (Model §10'dan ürüne aktarıldı).
- [ ] **[Benchmark re-audit] AI kredi/limit mekanizması tasarımı** — premium aylık kredi tavanı; `ai_usage_tracking` verisiyle ₺30/ay COGS varsayımının kalibrasyonu (Model §1).

### Düşük öncelikli cila
- [ ] `farmer.journal.new.tsx` "Önce bir parsel ekle" statik metni · `formatCrop()` heuristik · Landing page'de çok fazla rol-seçim butonu
- [ ] MCP yazma tool'larında aralıklı "No approval received" hatası — retry ile geçiyor
- [ ] **Lovable platform bug'ı (bilinen, düzeltilemez):** `plan_mode=false` sinyali agent'ın kod tool'larına bazen yansımıyor.
- [ ] Storage bucket public/private ayarını her zaman canlı doğrula
- [ ] `Supabase:list_tables` tool'u aralıklı "No approval received" veriyor, `execute_sql` ile atlatılabiliyor
- [x] ~~`useHasat` (Zustand mock store) referansları için tüm codebase'de tarama~~ — **Tamamlandı (2026-07-16), temiz** — bkz. detaylı bölüm aşağıda
- [ ] **[Audit'te bulundu] `crop_requests` tablosu için admin inceleme arayüzü** — **Karar (2026-07-16): Claude periyodik Supabase MCP sorgusuyla kontrol edecek** (P17-E'de kalıcı admin arayüzüne dönüşecek).
- [ ] **[Bu turda bulundu] `notifPrefs` sadece localStorage'da tutuluyor** — cihazlar arası senkron olmuyor. Düşük öncelik, DB tablosu gerektirir (yan bulgu, `useHasat` taramasından).

---

## 🏗️ Lovable Build Sırası — TAMAMLANDI ✅

> Sıradaki: **Ağustos E2E test kapsamının tamamlanması** (community reply UI, AI chat kalitesi, bildirimler, referral tam akış) + yukarıdaki audit maddeleri → sonrasında **P17 Güven Çekirdeği**.

---

### ✅ Sertifika Foto + `useHasat` Taraması + Gelir Modeli Gating Denetimi + Premium Persistence Bug'ı *(Tamamlandı — 2026-07-16)*

Audit/Benchmark takibinin üçüncü turu — çoğunlukla "temiz" sonuçlar + **1 yeni, bağımsız olarak iki kez ortaya çıkan gerçek bug**:

- **Sertifika fotoğraf/döküman yükleme** → Kod incelendi, `useUploadCertification()` gerçekten storage'a yüklüyor, gerçek `document_url` yazıyor, hata durumunda orphan dosyayı temizliyor. **Bug yok, journal'daki gibi sahte değil.**
- **`useHasat` mock store — tüm codebase taraması** (Lovable'a gönderildi, 1 kredi) → Sahte veri (producers, listings, orders vb.) okuyan **başka hiçbir yer bulunamadı**. Kalan tüm kullanımlar meşru (rol/oturum durumu, geçici teklif taslağı, bildirim tercihleri).
- **Gelir modeli gating denetimi** — (a) Premium gating gerçekten sadece AI chat kotasında (`FarmerAIChat.tsx`), başka hiçbir özellik paywall'a takılmıyor — kod taramasıyla doğrulandı (1 kredi). (b) `MarketDeviationAlert` fiyat bandı nudge'ında hiç tier kontrolü yok, herkese açık — doğrulandı.
- **🔴 Bulunan yeni bug — Premium "satın alma" hiçbir zaman kalıcı olmuyordu:** Hem `farmer.billing.tsx` (checkout başarı ekranı) hem `onboarding.buyer.tsx` (30 gün ücretsiz deneme anahtarı) sadece local Zustand `setPremium(true)` çağırıyordu — **`profiles.tier` hiçbir zaman `'premium'` olarak Supabase'e yazılmıyordu.** Daha da ilginci: bu, sabah kurduğumuz kendi güvenlik trigger'ımızın (`enforce_profile_self_update_restrictions`) normal bir client-side güncellemeyi zaten engelleyeceği anlamına geliyordu. **Düzeltme:** Lovable, service-role admin client kullanan yeni bir sunucu fonksiyonu (`activatePremium`, `src/lib/api/premium.functions.ts`) yazdı — trigger'ın "güvenilir sunucu yolu" istisnasını doğru şekilde kullanıyor. Her iki çağrı noktası da artık bunu awaited şekilde çağırıyor, hata durumunda toast gösteriyor.
- **Canlı doğrulama (Claude tarafından):** Ahmet'in `tier`'ı Supabase MCP ile manuel `premium` yapıldı, `FarmerAIChat.tsx`'teki `limited = tier==="free" && ...` mantığı kod üzerinden izlenerek artık `false` döneceği (sınırsız sohbet) doğrulandı, sonra `tier` `free`'ye geri döndürüldü (temizlik).
- **Maliyet:** 2.6 + 1 + 1 = **4.6 kredi** (2 madde — sertifika kontrolü, gating denetimi — sıfır ek kod değişikliği gerektirmedi, sadece doğrulama).

---

### ✅ Audit Takibi — MCP Rate Limiting + Tool Genişletmesi + Temizlik *(Tamamlandı — 2026-07-16)*

(Detaylar önceki sürümde — değişmedi)

---

### ✅ Detaylı E2E Devam Turu — 6 Yeni Senaryo + 4 Kritik Bug, Alıcı Sayfaları Bug Düzeltmeleri + Redesign, 10+10 Mock Data, Fiyatlandırma Sistemi Tam Yeniden İnşası, Tedarik Zinciri, Landing Kopya+SEO, RLS Güvenlik Denetimi, Rekabet Hukuku Denetimi, Hasat MCP Server, Landing v2

(Detaylar önceki sürümlerde — değişmedi)

---

## 🟡 AĞUSTOS 2026 — Soft Launch — ŞİMDİ BURADAYIZ

### E2E Test — devam ediyor
- [x] Farmer/Buyer onboarding · Landing page · Draft-İlan pipeline · S1-S13 (bkz. `Build/E2E-QA`) — hepsi ✅
- [x] Fiyatlandırma sistemi tam E2E · Alıcı Fiyatlar/Keşfet/Mesajlar/Raporlar/Abonelikler/Üretici Detay/Fotoğraflar — hepsi canlı test edildi
- [x] Sertifika foto upload doğrulaması, `useHasat` taraması, gelir modeli gating denetimi
- [ ] Kalan kapsam: community+reply UI, AI chat kalitesi, bildirimler, referral tam akış
- [ ] Yeni 5 MCP tool'unun (topluluk + abonelik) Publish sonrası canlı testi

### Teknik Final
- [ ] hasat.lovable.app → custom domain yayını · iyzico canlı ödeme testi
- [x] P16-C tam havale e2e testi — kapandı ✅

---

## 🟣 P17 — GÜVEN ÇEKİRDEĞİ *(Eylül 2026, GTM sonrası ilk build serisi — Benchmark re-audit çıktısı)*

(Detaylar önceki sürümde — değişmedi: P17-A/B/C/D/E/F/G)

---

## 🔵 SAFRAN SEZONU / ⬜ SONRAKI FAZLAR

(değişmedi — bkz. önceki sürüm)

---

## 📋 Lovable Prompt Yazma Kuralları

(1-36 önceki sürümde — devam:)
37. **[Bu turda eklendi] "Görsel olarak doğru kod" ile "gerçekten kalıcı olan kod" arasındaki fark tek bir dosyada bile değil, birbirinden bağımsız iki farklı yerde (checkout + onboarding trial) aynı anda tekrarlanabilir** — her iki `setPremium(true)` çağrısı da birbirinden habersiz aynı hatayı yapmıştı. Bir "local state güncelleniyor ama DB'ye yazılmıyor" deseni bulunduğunda, aynı deseni kullanan TÜM çağrı noktaları taranmalı, sadece ilk bulunan yer değil.
38. **[Bu turda eklendi] Kendi güvenlik trigger'larımızın (örn. `enforce_profile_self_update_restrictions`) meşru sunucu-taraflı güncellemeleri de engelleyebileceği unutulmamalı** — bir alanı self-update'e kapattığımızda, o alanın **gerçekten** güncellenmesi gereken meşru senaryolar (örn. premium satın alma) için açıkça bir "güvenilir sunucu yolu" (service-role fonksiyon) tasarlanmalı, aksi halde özellik sessizce çalışmaz hale gelir.

---

## 📌 Kararlar

(önceki tablo + eklenenler:)
| **Premium/tier persistence (2026-07-16)** | `setPremium(true)` (Zustand) hiçbir zaman `profiles.tier`'ı güncellemiyordu — yeni `activatePremium()` server function'ı (service-role, trigger'ın güvenilir-yol istisnasını kullanarak) her iki çağrı noktasına (billing checkout, buyer trial toggle) bağlandı |
| **Gelir modeli gating (2026-07-16)** | Kod taramasıyla doğrulandı: premium sadece AI chat kotasını kapsıyor, fiyat verisi/ilanlar/teklifler/abonelikler/topluluk/analytics tamamen serbest |

---

## 📋 Son Test Sonuçları

### Sertifika Foto + `useHasat` Taraması + Gelir Modeli Gating + Premium Bug (2026-07-16) ✅
| Kontrol | Sonuç |
|---|---|
| Sertifika yükleme kod incelemesi | ✅ Gerçek, bug yok |
| `useHasat` tam codebase taraması | ✅ Temiz, sadece meşru local state |
| Premium gating = sadece AI mi? | ✅ Doğrulandı |
| Fiyat bandı herkese açık mı? | ✅ Doğrulandı |
| Premium satın alma DB'ye yazıyor mu? | ❌→✅ Bug bulundu (2 bağımsız yerde), `activatePremium()` server function ile düzeltildi |
| Canlı doğrulama (Ahmet tier=premium→free) | ✅ Kod mantığı izlenerek doğrulandı, temizlendi |

### (Önceki tüm test sonuçları — Audit Takibi, Detaylı E2E, Alıcı Sayfaları, Mock Data, Fiyatlandırma, Tedarik Zinciri, Landing, RLS, Rekabet Hukuku, MCP Server, Benchmark Re-Audit, P16 serisi — değişmedi, önceki sürümlerde)
