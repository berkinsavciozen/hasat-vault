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
- [x] **BENCHMARK.md** — 4 katmanlı rakip analizi + 10 use-case matrisi + KPI çerçevesi (§5) + 12 maddelik gap listesi
- [x] **Finansal Model v0.6** — gelir modeli redesign (ücretsiz çekirdek + tek tier AI premium ₺149 + %2,5 komisyon), driver tablosu, Base/Bear/Bull (`Finance/Model.md`)

### P16 — TÜM SERİ TAMAMLANDI ✅
(Detaylar önceki sürümlerde)

---

## 🔎 BENCHMARK RE-AUDIT (Temmuz 2026)

> BENCHMARK.md'deki 12 gap + KPI ölçüm altyapısı + Model v0.6 kararlarının ürün yansımaları, TODO'nun güncel haliyle tek tek karşılaştırıldı. Sonuç: **5 P0 gap TODO'da hiç yoktu** (teslim kabul/ihtilaf, bloke ödeme akışı, fatura/dekont, tekrar sipariş+şube, yapılandırılmış RFQ) → **P17 serisi** olarak aşağıya eklendi. 3 mevcut madde benchmark gap'leriyle eşleştirilip etiketlendi. Kritik bağlantı: **BENCHMARK §5'teki 5 KPI (ödeme süresi, ihtilaf oranı, tam kabul, fill rate, NPS) bugünkü şemayla ölçülemiyor ve tamamı P17 maddelerine bağlı — P17 aynı zamanda ölçüm altyapısıdır; fizibilite forecast'i bu KPI'lara dayanacak.**

---

## 🔴 ŞİMDİ — Temmuz 2026

### Yasal & Altyapı
- [ ] Şahıs şirketi kur · iyzico başvurusu · Alan adı araştır · Twilio Verify service · **WhatsApp Business başvurusu → hemen başla**
- [ ] **[Audit'te bulundu] Rekabet hukuku uzmanına gerçek danışma** — Rekabet Kurumu risk denetiminin asıl önerisiydi ("fiyat uyarı sistemi + AI fiyat tavsiyeleri özelliklerini bir avukata göster"). Teknik önlemler (band-based AI, RLS, moderasyon trigger'ı) uygulandı ama **gerçek hukuki onay hâlâ alınmadı** — Ekim safran sezonu / gerçek GMV başlamadan önce mutlaka yapılmalı.
- [ ] **[Audit'te bulundu — kontrol edilmeli] Çerez onay banner'ı** — `/privacy` sayfası "çerez politikası" bölümü içeriyor ama gerçek bir çerez onay mekanizması (banner) olup olmadığı hiç konuşulmadı/kontrol edilmedi. KVKK/GDPR açısından potansiyel bir boşluk olabilir — önce mevcut durumu kontrol et, gerekirse ekle.
- [ ] **[Benchmark re-audit] iyzico başvurusunda pazaryeri (alt-üye işyeri) modelini talep et + komisyon oranı teklifini al** — Model v0.6'nın en kritik girdisi: PSP maliyeti %1,25 varsayımı doğrulanmadan net take rate bilinmiyor (Gap #1 ön koşulu, `Finance/Model.md` §10).
- [ ] **[Benchmark re-audit] Gelir modeli kararlarının üründe doğrulanması** — (a) premium gating'in **sadece AI feature'larını** kapsadığını denetle (başka hiçbir şey paywall arkasında kalmamalı), (b) fiyat bandı nudge'ının tüm kullanıcılara açık olduğunu doğrula, (c) "ilk 6 ay premium ücretsiz" feature-flag ile değiştirilebilir mi kontrol et (Model §0 kararları).
- [ ] **[Benchmark re-audit] Referral suistimal koruması** — 3 sahte kayıt = 12 ay bedava premium riski; min. doğrulama eşiği (telefon onayı + ilk gerçek eylem) tasarlanmalı (Model §10'dan ürüne aktarıldı).
- [ ] **[Benchmark re-audit] AI kredi/limit mekanizması tasarımı** — premium aylık kredi tavanı; `ai_usage_tracking` verisiyle ₺30/ay COGS varsayımının kalibrasyonu (Model §1).

### Düşük öncelikli cila
- [ ] `farmer.journal.new.tsx` "Önce bir parsel ekle" statik metni · `formatCrop()` heuristik · Landing page'de çok fazla rol-seçim butonu
- [ ] MCP yazma tool'larında aralıklı "No approval received" hatası — retry ile geçiyor
- [ ] **Lovable platform bug'ı (bilinen, düzeltilemez):** `plan_mode=false` sinyali agent'ın kod tool'larına bazen yansımıyor.
- [ ] Storage bucket public/private ayarını her zaman canlı doğrula (Lovable'ın tool'u migration'a yansımayabiliyor)
- [ ] `Supabase:list_tables` tool'u aralıklı "No approval received" veriyor, `execute_sql` ile `information_schema.tables` sorgusuna geçilerek atlatılabiliyor
- [ ] `useHasat` (Zustand mock store) referansları için **tüm codebase'de bir kez daha tarama** yapılmalı — Üretici Detay ve Abonelik Oluştur sayfalarında bulundu, başka yerde de olabilir
- [ ] **[Audit'te bulundu] MCP tool setine rate limiting eklenmesi** — orijinal MCP güvenlik tasarımında not edilmişti ("düşük öncelik ama not edilmeli"), henüz uygulanmadı. Özellikle `create_offer`/`respond_to_offer` gibi yazma tool'larının kötüye kullanım potansiyeli var.
- [ ] **[Audit'te bulundu] `crop_requests` tablosu için admin inceleme arayüzü** — tablo + kullanıcı-tarafı form var ama bilinçli olarak "bu turda gerek yok" denip ertelenmişti, hiçbir zaman ileriye dönük bir görev olarak işaretlenmedi. Kullanıcılar ürün talebi bıraktıkça bunları görüp `crop_config`'e taşıyacak bir arayüz gerekiyor. *(Not: P17-E ile birleşebilir — aşağıda.)*
- [ ] **[Audit'te bulundu] MCP tool setini genişletme** — topluluk paylaşımı, sertifika yükleme, **abonelik oluşturma** (UI/mutation düzeltildi ama hâlâ MCP tool'u yok) için hiç tool yok. Tam E2E otomasyon kapsamı için gerekli.
- [ ] **[Audit'te bulundu] Berkin'in eski test hesabındaki (`d040fc98`, "Berkin Savcıözen") tek başına kalan "Domates" ilanı** — S9'da zararsız bulundu ama temizlik listesine hiç eklenmemişti, temizlenmeli.

---

## 🏗️ Lovable Build Sırası — TAMAMLANDI ✅

> Sıradaki: **Ağustos E2E test kapsamının tamamlanması** (sertifika foto, community reply UI, AI chat kalitesi, bildirimler, referral tam akış) + yukarıdaki audit maddeleri → sonrasında **P17 Güven Çekirdeği**.

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

## 🟣 P17 — GÜVEN ÇEKİRDEĞİ *(Eylül 2026, GTM sonrası ilk build serisi — Benchmark re-audit çıktısı)*

> Kaynak: BENCHMARK gap listesi #1-#6 + KPI §5. Ortak tema: güven tezinin **anlatıdan altyapıya** taşınması. Bu seri bitmeden 5 çekirdek KPI ölçülemez ve fizibilite forecast'i kalibre edilemez. Sıralama bağımlılığa göre:

- [ ] **P17-A · Bloke ödeme akışı (Gap #1, P0)** — iyzico pazaryeri modeliyle: `payments` tablosu + para platformda bloke → teslim onayı → çiftçiye aktarım → net iade politikası. *Use case: UC-5 (kapora dolandırıcılığına yapısal çözüm). KPI: "sipariş → para hesapta" süresi (hedef ≤3 gün → 24 saat, Ninjacart referansı).*
- [ ] **P17-B · Teslim kabul + ihtilaf akışı (Gap #2, P0)** — teslim fotoğrafı, kabul/kısmi kabul/red, 24 saatlik itiraz penceresi, `disputes` tablosu; P17-A'daki serbest bırakmayı tetikler. *Use case: UC-8 ("fotoğraftaki mal gelen mal değil" — HoReCa'nın 1 numaralı tedarikçi değiştirme sebebi). KPI: ihtilaf oranı ≤%2, tam kabul ≥%95.*
- [ ] **P17-C · Karşılıklı değerlendirme (Gap #3, P0)** — sipariş-sonrası iki yönlü puan+yorum, `reviews` tablosu, farmer profile'da agregat. E2E turunda kaldırılan sahte "Alıcı Yorumları"nın **gerçek** versiyonu; dürüstlük ilkesi gereği yalnızca tamamlanmış siparişe bağlı yorum. *Use case: UC-9 (ÇiftçidenEve "Yeşil Puan" karşılığı). KPI: NPS/puan ortalaması.*
- [ ] **P17-D · Sipariş dekontu → fatura yolu (Gap #4, P0 B2B)** — önce PDF sipariş dekontu, sonra e-belge entegratörü araştırması (e-fatura/e-müstahsil makbuzu). *Use case: UC-10 — kurumsal alıcı faturasız çalışamaz; Haljet'in "sipariş+fatura tek ekran" standardına cevap. HoReCa onboarding'in ön koşulu.*
- [ ] **P17-E · Yapılandırılmış RFQ (Gap #6, P1)** — `crop_requests`'i ürün/kalibre/miktar/il/tarih aralığı/hedef fiyat alanlarıyla genişlet + eşleşen çiftçilere bildirim + admin inceleme arayüzü (yukarıdaki audit maddesiyle birleşik). *Use case: UC-2 (DİTAP'ın talep bazlı modeli). KPI: fill rate ≥%40.*
- [ ] **P17-F · Tekrar sipariş + şube adresleri (Gap #5, P0 HoReCa)** — "son siparişi tekrarla" + kayıtlı listeler + `buyer_addresses` (çoklu şube). *Use case: UC-7 (TazeKasam pattern'i — HoReCa retention'ının bel kemiği). KPI: haftalık sipariş sıklığı, tekrar alıcı %30-60 bandı.*
- [ ] **P17-G · KPI ölçüm görünümü** — BENCHMARK §5.1-5.3 tablolarındaki metriklerin SQL view/dashboard olarak kurulması; bugün ölçülebilen 12 metrik hemen, P17-A/B/C/E bittikçe kalan 5'i. *Fizibilite forecast'inin veri kaynağı.*

---

## 🔵 SAFRAN SEZONU / ⬜ SONRAKI FAZLAR

(değişmedi + eklenen:)
- [ ] Faz 2 — Otomatik Resmi Fiyat Veri Servisi (TEPGE/TÜİK/HKS) — **[Benchmark Gap #7, P1]** hal min-max'ının `price_history` yanında "referans bant" olarak gösterimi (soğuk başlangıç çözümü, UC-1)
- [ ] **Gerçek bir "Alıcı Yorumları/Reviews" özelliği** — şimdilik tamamen kaldırıldı → **P17-C'ye taşındı** (Benchmark Gap #3 bunu P0 olarak işaretledi: güven tezli platformda review yokluğu tezle çelişiyor)
- [ ] **[Benchmark Gap #8, P1]** Lojistik adımı — sipariş akışına "taşımayı kim üstleniyor" + araç tipi (frigorifik) + teslim tutanağı; ileride 3PL anlaşması (UC-6)
- [ ] **[Benchmark Gap #9, P2]** "Parselden tabağa" QR menşe kartı — `public_parcel_cards` üstüne sipariş bağlantılı public görünüm; ihracatçı + premium HoReCa farklılaştırıcısı (UC-9)
- [ ] **[Benchmark Gap #10, P2]** SMS/WhatsApp bildirim genişletmesi — Twilio üstünden teklif/sipariş bildirimleri; internete uzak çiftçi segmenti (TAGEM Tarımsal Pazaryeri'nin SMS pattern'i)
- [ ] **[Benchmark Gap #11, P1→P2]** Onaylı alıcıya vade/cari — TazeKasam pattern'i; önce manuel onaylı pilot (UC-10)
- [ ] **[Benchmark Gap #12, P2]** Hasat öncesi finansman — partner banka/fintech ile; `harvest_subscriptions` verisi teminat sinyali (ProducePay pattern'i, UC-4)

---

## 📋 Lovable Prompt Yazma Kuralları

(1-32 önceki sürümde — devam:)
33. **[Audit'te eklendi] Bir özellik "bu turda gerek yok" diye bilinçli olarak ertelendiğinde, mutlaka bir TODO maddesi olarak işaretle** — `crop_requests` admin arayüzü tam olarak bu şekilde kayboldu: doğru bir karardı (kapsamı şişirmemek), ama hiçbir yerde "gelecekte yapılacak" olarak yazılmadığı için oturumlar arasında unutuldu.
34. **[Audit'te eklendi] Bir teknik/güvenlik denetiminin "önerilen aksiyon" kısmı, teknik düzeltme kadar önemlidir ve ayrıca takip edilmeli** — Rekabet Hukuku denetiminin en kritik önerisi ("bir avukata danış") teknik mitigasyonlar tamamlanınca gözden kayboldu. Bir denetim hem "biz ne yaptık" hem "kullanıcının hâlâ yapması gereken" olarak iki ayrı listede tutulmalı.
35. **[Benchmark re-audit'te eklendi] Her benchmark/finansal model revizyonundan sonra TODO'ya karşı re-audit yapılmalı** — strateji dokümanlarındaki gap'ler ve kararlar kendiliğinden backlog'a düşmüyor; bu turda 5 P0 gap'in TODO'da hiç olmadığı görüldü.

---

## 📌 Kararlar

(önceki tablo + eklenenler:)
| **Alıcı Yorumları/Reviews özelliği** | DB'de hiç karşılığı yoktu, tamamen kaldırıldı — gerçek bir review tablosu istenirse Faz 2'de ayrı bir özellik olarak inşa edilecek → **revize: P17-C olarak öne alındı (Benchmark Gap #3, P0)** |
| **Mock/sahte veri prensibi** | Bir UI elementi çalışıyormuş gibi görünse de, altındaki mutation/veri kaynağının gerçekten DB'ye bağlı olduğu ayrıca doğrulanmalı |
| **Tam konuşma denetimi (2026-07-16)** | Ayrı geçmiş konuşma bulunamadı (proje kapsamında tek uzun oturum) — denetim, bu oturumun sıkıştırılmış özeti dahil tam içeriğinin TODO ile karşılaştırılmasıyla yapıldı, 6 eksik madde bulundu ve eklendi |
| **Gelir modeli (Temmuz 2026)** | Çekirdek ürün tamamen ücretsiz · AI fiyat bandı herkese açık · tek tier AI premium ₺149/ay (test edilecek) · ilk 6 ay early adopter'lara ücretsiz (feature-flag) · referral 3 kullanıcı → 12 ay ücretsiz · %2,5 komisyon ana gelir — detay: `Finance/Model.md` §0 |
| **Benchmark re-audit (Temmuz 2026)** | BENCHMARK 12 gap + KPI çerçevesi TODO ile karşılaştırıldı; 5 eksik P0 gap → P17 serisi kuruldu, 3 mevcut madde gap etiketiyle eşlendi, reviews kararı revize edildi |

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

### Benchmark Re-Audit (Temmuz 2026) ✅
| Kontrol | Sonuç |
|---|---|
| BENCHMARK 12 gap ↔ TODO karşılaştırması | 5 P0 gap eksikti → P17-A/B/C/D/F olarak eklendi; Gap #6 → P17-E; Gap #7-#12 → Sonraki Fazlar'a etiketle eklendi/eşlendi |
| KPI §5 ölçüm altyapısı ↔ şema | 5 KPI ölçülemiyor (ödeme süresi, ihtilaf, tam kabul, fill rate, NPS) — tamamı P17'ye bağlandı, P17-G ölçüm görünümü eklendi |
| Model v0.6 ürün yansımaları | 4 yeni madde: PSP pazaryeri teklifi, AI-only gating denetimi, referral koruması, AI kredi/limit tasarımı |

### (Önceki tüm test sonuçları — Alıcı Sayfaları, Mock Data, Fiyatlandırma, Tedarik Zinciri, Landing, RLS, Rekabet Hukuku, MCP Server, P16 serisi — değişmedi, önceki sürümlerde)
