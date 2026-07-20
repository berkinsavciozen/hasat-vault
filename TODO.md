---
title: Hasat — Master Roadmap & Build Log
updated: 2026-07-20
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
- [x] Hasat MCP Server (16→21 tool, tümü canlı test edildi) · Rekabet Hukuku Risk Denetimi · Kapsamlı RLS Güvenlik Denetimi
- [x] E2E-QA Dokümanı (`Build/E2E-QA`) · Tedarik Zinciri Görsel Yeniden Tasarımı
- [x] Fiyatlandırma Sistemi Tam Yeniden İnşası · 10+10 Mock Data (2 yıllık) · Alıcı Sayfaları Bug Düzeltmeleri
- [x] Detaylı E2E Devam Turu — 6 Yeni Senaryo (S8-S13) + 4 Kritik "Sahte Veri/Kırık Akış" Bug'ı
- [x] Tam Konuşma Denetimi + BENCHMARK.md + Finansal Model v0.6
- [x] Audit Takibi — MCP Rate Limiting + Tool Genişletmesi + Temizlik
- [x] Sertifika Foto + `useHasat` Taraması + Gelir Modeli Gating Denetimi + Premium Persistence Bug'ı
- [x] Premium Sayfası Yeniden İnşası + Referral Ödül Mekanizması + AI Kredi/Limit Sistemi + Çerez Kontrolü + 5 MCP Tool Canlı Testi
- [x] Bildirim Ayarları DB Bağlantısı + SMS Mimarisi Client'tan DB'ye Taşındı + AI Chat Kalite Denetimi (2026-07-19)
- [x] P18-0 Arayüz Denetimi (2026-07-20)
- [x] Faz A — Community Alıcı Sayfası + Gerçek Beğeni Butonu + "Hava Sor" İpucu Kaldırıldı (2026-07-20)
- [x] **P18-0.2 — Ana Sayfa Retention + Fiyat Verisi Doğruluk Denetimi** (2026-07-20) — bkz. P18 bölümü aşağıda

### P16 — TÜM SERİ TAMAMLANDI ✅
(Detaylar önceki sürümlerde)

---

## 🔎 BENCHMARK RE-AUDIT (Temmuz 2026)

(Değişmedi — bkz. önceki sürüm)

---

## 🔴 ŞİMDİ — Temmuz 2026

### Yasal & Altyapı
- [ ] Şahıs şirketi kur · iyzico başvurusu · Alan adı araştır · Twilio Verify service · **WhatsApp Business başvurusu → hemen başla**
- [ ] **[Audit'te bulundu] Rekabet hukuku uzmanına gerçek danışma** — hâlâ açık, insan aksiyonu.
- [x] ~~Çerez onay banner'ı~~ — Tamamlandı, gerek yok.
- [ ] **[Benchmark re-audit] iyzico başvurusunda pazaryeri modeli + komisyon teklifi talebi** — hâlâ açık, insan aksiyonu.
- [x] ~~Gelir modeli kararlarının üründe doğrulanması~~ — Tamamlandı.
- [x] ~~Toplu "ilk N kullanıcıya otomatik 6 ay premium" feature-flag mekanizması~~ — Tamamlandı (2026-07-20).
- [x] ~~Referral suistimal koruması~~ / ~~AI kredi/limit mekanizması~~ — Tamamlandı.

### Düşük öncelikli cila
- [ ] `farmer.journal.new.tsx` "Önce bir parsel ekle" statik metni · `formatCrop()` heuristik · Landing page'de çok fazla rol-seçim butonu
- [ ] MCP yazma tool'larında aralıklı "No approval received" hatası — retry ile geçiyor
- [ ] **Lovable platform bug'ı (bilinen, düzeltilemez):** `plan_mode=false` sinyali agent'ın kod tool'larına bazen yansımıyor.
- [ ] Storage bucket public/private ayarını her zaman canlı doğrula
- [ ] **[Audit'te bulundu] `crop_requests` tablosu için admin inceleme arayüzü** — Claude periyodik kontrol edecek (P17-E'ye kadar)
- [ ] **[Bu turda bulundu, düşük öncelik, temizlik] `price_points` tablosu ve `usePricePoints()` hook'u ölü kod adayı** — tabloda her ürün için tek satır var, hepsi 2026-06-15 tarihli (tek seferlik seed, hiç güncellenmemiş), `price_history`'den farklı büyük/küçük harf kullanıyor (örn. "Safran" vs "safran"). Hiçbir aktif ekranda render edilmediği doğrulandı (`PricesPageBody`/`MarketDeviationAlert` ikisi de `get_price_history_summary` RPC'sini kullanıyor, bu tabloyu değil). Aktif bug değil — ileride yanlışlıkla kullanılmasını önlemek için kaldırılması veya "deprecated" olarak işaretlenmesi önerilir.
- [x] ~~`create_subscription` rol-temizliği eksik~~ — Tamamlandı (2026-07-20).
- [x] ~~`FarmerAIChat.tsx` header'ı "Premium • sınırsız sohbet"~~ — Tamamlandı.
- [x] ~~`useHasat.setNotifPref` dead code~~ — Tamamlandı.
- [x] ~~AI chat'te "hava sor" ipucu tamamen kurgusal~~ — Tamamlandı (2026-07-20).

---

## 🏗️ Lovable Build Sırası — TAMAMLANDI ✅

> Sıradaki: **P18** (Arayüz Yenileme, hemen başlanabilir) → **Faz C / P17** (Eylül, ayrıca değerlendirilecek). **Faz D** (Piyasa Zaman Serisi backend'i) P18'e paralel, GTM'i bloklamayan, istenildiğinde yapılacak küçük bir ek — bkz. aşağı.

---

### ✅ Faz A — Community Alıcı Sayfası + Gerçek Beğeni + "Hava Sor" Kaldırıldı *(Tamamlandı — 2026-07-20)*
(Değişmedi — bkz. önceki sürüm)

---

### ✅ (Önceki tüm tamamlanan işler — Bildirim/SMS, Premium/Referral/AI Limit, Sertifika/useHasat/Gating, Audit Takibi, Detaylı E2E, Alıcı Sayfaları, Mock Data, Fiyatlandırma, Tedarik Zinciri, Landing, RLS, Rekabet Hukuku, MCP Server, Benchmark Re-Audit, P16 serisi)

(Detaylar önceki sürümlerde — değişmedi)

---

## 🧭 SIRADAKİ FAZ PLANI (2026-07-20 itibarıyla)

**Faz A ✅, Faz B ✅ (5. madde dahil) — hepsi tamamlandı.** Ağustos E2E kapsamı resmi olarak kapandı. Sıradaki: **P18 (Arayüz Yenileme)** — UI-only, backend'e dokunmuyor, hemen başlanabilir. **Faz D** (bkz. aşağı, P18-F'nin altında) P18'e paralel/sonrasında, küçük ve isteğe bağlı bir backend eki.

**Faz C (P17, Eylül) zamanlama gerekçesi:** P17 GTM-sonrası bir seri, asıl bağımlılık 25 Ağustos lansmanı + P18'in tamamlanması.

---

## 🎨 P18 — ARAYÜZ YENİLEME (UI-Only, Backend/DB Değişikliği YOK)

> **Ortak Guardrail (her prompt'a eklenir):** *"Bu görev SADECE frontend/UI katmanını değiştirir. `supabase/migrations/`, `supabase/functions/`, veya herhangi bir DB şema/RLS/trigger dosyasına dokunma. Sadece `src/` altında çalış."* Her build sonrası Claude, diff'in yalnızca `src/`'i etkilediğini + migration sayısının değişmediğini Supabase MCP ile doğrular.

### ✅ P18-0 — Denetim *(Tamamlandı — 2026-07-20, Lovable kredisi kullanmadan)*
(Değişmedi — bkz. önceki sürüm: AI chat deeplink event, `Sparkline`/`price_alerts` kullanılmıyor, mobil bildirim tablosu bug'ı, bildirim tercihleri route'u kodda çalışıyor.)

### ✅ P18-0.2 — Ana Sayfa Retention + Fiyat Verisi Doğruluk Denetimi *(Tamamlandı — 2026-07-20, Lovable kredisi kullanmadan)*
Berkin'in talebi üzerine iki ek denetim yapıldı:

**1. Retention — çiftçi + alıcı ana sayfa:** `farmer.home.tsx`'te `AIBox` (gerçek, edge function destekli içgörü kutusu, kendi coach-tooltip'i ile) zaten var — iyi bir temel. Ama: bekleyen teklif/sipariş hiç yüzeyde değil (hepsi `Teklifler` sekmesine gömülü), geçen seneye göre kıyas yok, "SEZON DIŞI" durumuna özel içerik yok. **Alıcı tarafında ayrı bir "home" hiç yok** — `buyer.discover.tsx` hem keşif hem varsayılan iniş sayfası, kişiselleştirme (bekleyen teklif, abonelik yenileme, tetiklenen fiyat alarmı) hiçbir yerde yok. → **P18-H**.

**2. Onboarding — `onboarding.farmer.tsx` bulundu ama bu bir kayıt sihirbazı** (ad/şehir/ürün/arazi/sertifika toplayıp DB'ye yazıyor), "nasıl kullanılır" eğitimi değil. `AIBox`'ın `localStorage` tabanlı coach-tooltip deseni bu iş için genişletilebilir. → **P18-I**.

**3. 🔴 Fiyat verisi doğruluk denetimi (Berkin'in son isteği):** Supabase'de canlı sorgu ile kontrol edildi:
   - `Fiyatlar` sayfası (çiftçi + alıcı, aynı `PricesPageBody`) ve `MarketDeviationAlert` (home'daki ürün kartlarında) **%100 gerçek veri kullanıyor** — ikisi de `get_price_history_summary` RPC'sinden geliyor, 5-üretici eşiği ve resmi/topluluk ayrımı doğru uygulanıyor. **Sahte veri yok, aktif bug yok.**
   - `price_history` tablosunda **gerçek çok-noktalı geçmiş var** (domates 80 satır, safran 20 satır, elma/lavanta/patates 6'şar satır, 2024'ten bugüne) ama bu veri hiçbir RPC/hook üzerinden **seri** (zaman serisi) olarak dışarı açılmıyor — sadece tek bir ortalama/stddev'e sıkıştırılıyor. Gerçek bir sparkline/trend çizgisi için bu, **yeni bir salt-okunur RPC gerektiriyor** → P18 kapsamı dışında, bkz. **Faz D** aşağıda.
   - `price_points` tablosu **ölü kod adayı** (tek seferlik seed, hiç güncellenmemiş, hiçbir ekranda kullanılmıyor) — yukarı "Düşük öncelikli cila" bölümüne not edildi.
   - **Karar:** P18-F, mevcut gerçek veriyi (arama/filtre/watchlist/timestamp) daha iyi sunar; sahte/sentetik bir trend çizgisi **eklemez**. `Sparkline` component'i opsiyonel bir `series` prop'u kabul edecek şekilde inşa edilir — veri yoksa gösterilmez, icat edilmez. Gerçek seri Faz D ile geldiğinde tek satırlık bağlamayla devreye girer.

### Faz Sıralaması (güncellendi): A → B → H → I → G → F → E → C → D

---

### P18-A — Tema Katmanı + Ortak Component Seti
**Amaç:** Lavender AI rengini saffron/gold ailesine taşı, WhatsApp yeşilini (`#25D366`) sadece gerçek `wa.me` linklerine ayır, 48px dokunma hedefleri, ve farmer+buyer arasında paylaşılan bir component seti.
**Dokunacağı dosyalar:** `src/styles.css`, `src/components/hasat/ai-chat/FarmerAIChat.tsx`, yeni `src/components/hasat/common/` klasörü.

---

### P18-B — Çiftçi Ana Sayfa: Sohbet-Önde Giriş Akışı
**Amaç:** Quick action'ları mevcut AI chat deeplink event'ine (`hasat:ai-chat:open`) yönlendir; ana sayfaya kalıcı "Hasadını yaz…" giriş çubuğu ekle.
**Dokunacağı dosyalar:** `src/routes/farmer.home.tsx`, `FarmerAIChat.tsx`.

---

### P18-H — Ana Sayfa Retention Katmanı (Çiftçi + Alıcı) *(yeni — 2026-07-20)*
**Amaç:** Her iki personanın da uygulamayı açmak için günlük bir sebebi olsun.
**Kapsam:**
- Çiftçi home: "Bekleyen" kartı (yanıt bekleyen teklif/sipariş sayısı — mevcut `useFarmerOffers`/`useFarmerOrders`'tan client-side hesaplanır, yeni sorgu yok), geçen sezona göre kıyas (`entries`'ten client-side), sezon-farkında AIBox promptu.
- Alıcı: `buyer.discover.tsx`'in üstüne kişisel bir "Senin İçin" şeridi — bekleyen teklifler (`useBuyerOffers`), yaklaşan abonelik teslimatı (`useMySubscriptions`), tetiklenen fiyat alarmları (`usePriceAlerts`, zaten var). Hepsi mevcut hook'lardan, yeni sorgu yok.
**Dokunacağı dosyalar:** `farmer.home.tsx`, `buyer.discover.tsx`.

**Lovable Prompt (plan_mode=true ile gönderilecek):**
> Çiftçi ve alıcı ana sayfalarına kişisel/retention odaklı bir katman ekle. **SADECE `src/` içinde çalış, yeni sorgu/migration yok — hepsi mevcut hook'ların client-side yeniden düzenlenmesi.**
> 1. `farmer.home.tsx`'e, `AIBox`'ın üzerine, mevcut `useFarmerOffers`/`useFarmerOrders` verisinden hesaplanan bir "Bekleyen" kartı ekle: yanıt bekleyen teklif sayısı + hazırlanan sipariş sayısı, tıklanınca ilgili sekmeye gider. "Bu Sezon" kartına, `entries` verisinden geçen aynı döneme göre basit bir yüzde kıyas ekle (veri yoksa gösterme).
> 2. `buyer.discover.tsx`'in en üstüne, mevcut kart gridinin üzerine, yatay kaydırmalı küçük bir "Senin İçin" şeridi ekle: bekleyen teklif sayısı (`useBuyerOffers`), en yakın abonelik teslimatı (`useMySubscriptions`), aktif fiyat alarmı sayısı (`usePriceAlerts`). Her biri ilgili sayfaya link.

---

### P18-I — Basit Onboarding Tutorial (Çiftçi) *(yeni — 2026-07-20)*
**Amaç:** Kayıt sihirbazından (`onboarding.farmer.tsx`, dokunulmuyor) tamamen ayrı, "nasıl kullanılır" ürün turu.
**Kapsam:** `/farmer/home`'a ilk gelişte (veya "Nasıl Çalışır?" linkinden istendiğinde) açılan, `AIBox`'ın `localStorage` tabanlı coach-tooltip desenini genişleten 4-5 adımlık spotlight turu: AIBox, "Hasadını yaz" çubuğu (P18-B), WhatsApp linki, Vitrin, Fiyatlar. Görüldü bilgisi `localStorage`'da (DB'ye dokunmuyor).
**Dokunacağı dosyalar:** yeni `src/components/hasat/OnboardingTour.tsx`, `farmer.home.tsx`, `farmer.tsx` (shell'e "Nasıl Çalışır?" linki).

**Lovable Prompt:**
> `AIBox.tsx`'teki coach-tooltip desenini (`localStorage` flag + tek seferlik gösterim) örnek alarak `src/components/hasat/OnboardingTour.tsx` adında yeni bir component oluştur. **SADECE `src/` içinde çalış.** 4-5 adımlık bir spotlight/tooltip dizisi: (1) AIBox'ı işaret et, (2) "Hasadını yaz" çubuğunu işaret et, (3) WhatsApp linkini işaret et, (4) Vitrin sekmesini işaret et, (5) Fiyatlar sekmesini işaret et. Her adımda kısa açıklama + "İleri"/"Atla" butonu. Tamamlandığında veya atlandığında `localStorage.setItem("hasat_onboarding_tour_done", "1")`. `farmer.home.tsx`'te, bu flag yoksa ve kullanıcı ilk kez home'a geliyorsa (mevcut `isEmpty` durumuyla ilişkilendirilebilir) turu otomatik başlat. `farmer.tsx` shell'deki "Daha" menüsüne, turu istendiğinde yeniden başlatan bir "Nasıl Çalışır?" linki ekle.

---

### P18-C — Sipariş Durumu & WhatsApp Deeplink'leri
**Amaç:** Adım timeline'ı öne çıkar, her siparişte "WhatsApp'tan takip et" deeplink'i.
**Dokunacağı dosyalar:** `farmer.orders.index.tsx`, `buyer.orders.$orderId.tsx`, `OrderTimeline.tsx`.

---

### P18-D — Alıcı Raporları/Siparişleri: Tablo Hissini Kart Hissine Çevir
**Amaç:** "Tedarikçi Güveni" ve "Son Siparişler" bölümlerini kart estetiğine çevir.
**Dokunacağı dosyalar:** `buyer.reports.tsx`.

---

### P18-E — Vitrin Kartı: Paylaşılabilir Önizleme
**Amaç:** `/s/$slug` genel vitrini paylaşılabilir hale getir, çiftçinin kendi Vitrin sayfasına canlı önizleme ekle.
**Dokunacağı dosyalar:** `s.$slug.tsx`, `farmer.storefront.tsx`.

---

### P18-F — Piyasa & Performans Görünümü (Fiyatlar + Analitik) *(kapsam düzeltildi — 2026-07-20)*
**Amaç:** Fiyatlar ve Analitik sayfalarını borsa/finans hissiyle yeniden tasarla — **sadece gerçek veriyle.**
**Kapsam:**
- `PricesPageBody.tsx`: arama/filtre çubuğu (`useCropsWithPriceData` üzerinde client-side), her karta gerçek `distinctFarmerCount`/`stddevPrice`'ı görsel bir "veri gücü"/aralık göstergesine çevir, `usePriceAlerts`/`useCreatePriceAlert`/`useTogglePriceAlert` hook'larına bağlı bir yıldız/watchlist ikonu (gerçek, zaten var). **Sparkline/trend çizgisi EKLENMEZ** — gerçek zaman serisi backend'i (Faz D) gelene kadar; component opsiyonel bir `series` prop'u kabul edecek şekilde yazılır ki Faz D geldiğinde tek satırla bağlanabilsin.
- `farmer.analytics.tsx`: 3 statik stat kartı + tek "top ürün" satırı → son 6 ay ciro trendi + ürün bazlı kırılım. Bu **tamamen gerçek veriyle** yapılabilir (mevcut `useFarmerOrders`/`useFarmerListings`'ten client-side hesap, `buyer.reports.tsx`'teki aylık bar chart mantığına benzer) — yeni sorgu veya backend gerekmiyor.
**Dokunacağı dosyalar:** `PricesPageBody.tsx`, `farmer.analytics.tsx`.

**Lovable Prompt:**
> `Fiyatlar` ve `Analitik` sayfalarını piyasa/borsa uygulaması hissiyle yeniden tasarla. **SADECE `src/` içinde çalış, yeni migration yok, sahte/sentetik veri EKLEME.**
> 1. `PricesPageBody.tsx`'e üstte bir arama/filtre çubuğu ekle (`useCropsWithPriceData` listesi üzerinde client-side filtreleme). Her `PriceSummaryCard`'a mevcut `usePriceAlerts`/`useCreatePriceAlert`/`useTogglePriceAlert` hook'larını kullanarak bir yıldız/watchlist ikonu ekle. `distinctFarmerCount` ve stddev aralığını daha görsel bir şekilde sun (örn. küçük bir aralık çubuğu). **Sparkline veya herhangi bir trend çizgisi ekleme** — gerçek zaman serisi verisi şu an mevcut değil; component'i, ileride gerçek veri geldiğinde kullanılacak opsiyonel bir `series?: { date: string; avg: number }[]` prop'u kabul edecek şekilde yaz (prop verilmezse hiçbir çizgi render edilmez, placeholder da render edilmez).
> 2. `farmer.analytics.tsx`'teki 3 statik stat kartını ve tek "top ürün" satırını, `useFarmerOrders`/`useFarmerListings`'ten client-side hesaplanan bir zaman eksenli görünüme çevir: son 6 ay ciro trendi + ürün bazlı kırılım (`buyer.reports.tsx`'teki aylık bar chart mantığına benzer şekilde). Tüm veri gerçek, mevcut sorgulardan.

---

### P18-G — Ayarlar Yeniden Grupla + Mobil Bildirim Tablosu Düzeltmesi
**Amaç:** Profil+Parsellerim+Sertifikalar → tek accordion "Çiftlik Bilgileri"; mobil bildirim tablosu düzeltmesi.
**Dokunacağı dosyalar:** `farmer.settings.tsx`, `farmer.settings.notifs.tsx`.

---

## 🟠 FAZ D — Piyasa Zaman Serisi Backend'i *(küçük, isteğe bağlı, P18'e paralel — 2026-07-20)*

> P18'in aksine bu **backend değişikliği içerir** (yeni salt-okunur RPC). GTM'i bloklamaz, P18-F'nin frontend'i zaten buna hazır bekleyecek şekilde inşa edildi. İstenildiği zaman, tek başına yapılabilir.

**Amaç:** `price_history` tablosundaki gerçek geçmiş veriyi (domates 80, safran 20, elma/lavanta/patates 6'şar satır) haftalık gruplanmış bir zaman serisi olarak dışarı açmak, böylece Fiyatlar sayfasında gerçek bir sparkline/trend çizgisi gösterilebilsin.

**Plan:**
1. Yeni `get_price_history_series(p_crop text, p_weeks int default 12)` SECURITY DEFINER RPC'si — `get_price_history_summary`'nin aynı mantığını (5 farklı üretici eşiği, topluluk/resmi ayrımı) haftalık bucket'lara uygular. Her bucket için yeterli veri yoksa o nokta `null`/atlanır (sahte doldurma yapılmaz).
2. `queries.ts`'e `usePriceHistorySeries(crop)` hook'u eklenir, `PriceHistorySummary` tipine paralel bir `PriceHistorySeries` tipi tanımlanır.
3. `PricesPageBody.tsx`'teki `PriceSummaryCard`, P18-F'de eklenen opsiyonel `series` prop'unu bu yeni hook'tan besler — **tek satırlık bağlama**, UI zaten hazır.
4. Veri yoğunluğu düşük ürünler (elma/lavanta/patates — 6 satır/yıl) için UI, "sınırlı geçmiş veri" etiketiyle dürüstçe seyrek bir çizgi gösterir, yoğun görünmesi için icat edilmiş nokta eklenmez.

---

## 🟡 AĞUSTOS 2026 — Soft Launch — ŞİMDİ BURADAYIZ

### E2E Test
- [x] S1-S17 (tüm senaryolar) — **AĞUSTOS E2E KAPSAMI RESMİ OLARAK TAMAMLANDI ✅**

### Teknik Final
- [ ] hasat.lovable.app → custom domain yayını · iyzico canlı ödeme testi
- [x] P16-C tam havale e2e testi — kapandı ✅

---

## 🟣 P17 — GÜVEN ÇEKİRDEĞİ *(Eylül 2026 — Faz C)*

(Değişmedi — bkz. önceki sürüm: P17-A/B/C/D/E/F/G)

---

## 🔵 SAFRAN SEZONU / ⬜ SONRAKI FAZLAR

(Değişmedi — bkz. önceki sürüm)

---

## 📋 Lovable Prompt Yazma Kuralları

(1-46 önceki sürümde — devam:)
47. **[Bu turda eklendi] Bir UI fazında "trend/grafik" gibi görsel bir istek geldiğinde, önce altındaki gerçek veri var mı diye DB'yi doğrudan sorgula** — component'i "gerçekçi görünen" ama fabrikasyon bir seriyle inşa etme isteği çok kolay gelir; gerçek veri yoksa, göstermemek icat etmekten her zaman daha iyidir.
48. **[Bu turda eklendi] Gerçek veri var ama seri/zaman-boyutlu erişilebilir değilse, bunu UI-only bir fazın dışına çıkarıp küçük, izole bir backend fazı olarak ayrı tut** — frontend'i o veriyi kabul edecek şekilde (opsiyonel prop/hook) inşa et ki backend geldiğinde tek satırlık bir bağlama yeterli olsun.

---

## 📌 Kararlar

(önceki tablo + eklenenler:)
| **Faz A tamamlandı (2026-07-20)** | Ağustos E2E kapsamı resmi olarak kapandı. |
| **P18 ↔ Faz B ayrımı (2026-07-20)** | Toplu premium feature-flag, backend gerektirdiği için P18'e dahil edilmedi. |
| **P17/Faz C zamanlama gerekçesi (2026-07-20)** | Asıl bağımlılık 25 Ağustos lansmanı + P18'in tamamlanması. |
| **P18-F kapsam düzeltmesi (2026-07-20)** | Fiyatlar/Analitik sayfalarında sahte/sentetik trend verisi kullanılmayacak. `price_history`'de gerçek çok-noktalı veri var ama seri olarak erişilebilir değil — bu ayrı, küçük bir backend fazına (**Faz D**) bırakıldı, GTM'i bloklamıyor. Frontend, bu veri geldiğinde tek satırla bağlanacak şekilde (opsiyonel `series` prop'u) inşa edilecek. |
| **P18-H/P18-I eklendi (2026-07-20)** | Retention katmanı (her iki persona için ana sayfa) ve çiftçiye özel basit onboarding tutorial'ı P18'e eklendi, sıralama A→B→H→I→G→F→E→C→D olarak güncellendi. |

---

## 📋 Son Test Sonuçları

### P18-0.2 — Ana Sayfa Retention + Fiyat Verisi Doğruluk Denetimi (2026-07-20) ✅
| Kontrol | Sonuç |
|---|---|
| Çiftçi home'da bekleyen aksiyon/kıyas yüzeyde mi | ❌ Yok — P18-H'de eklenecek |
| Alıcı için ayrı, kişiselleştirilmiş bir home var mı | ❌ Yok, Discover ikisini de yapıyor — P18-H'de eklenecek |
| Gerçek "nasıl kullanılır" onboarding turu var mı | ❌ Yok, sadece kayıt sihirbazı var — P18-I'de eklenecek |
| `PricesPageBody`/`MarketDeviationAlert` gerçek veri mi kullanıyor | ✅ Evet, `get_price_history_summary` RPC, sahte veri yok |
| `price_history` tablosunda gerçek çok-noktalı geçmiş var mı | ✅ Var (domates 80, safran 20, diğerleri 6 satır) ama seri olarak erişilebilir değil — Faz D |
| `price_points` tablosu aktif kullanımda mı | ❌ Hayır, tek seferlik seed, ölü kod adayı — temizlik notu eklendi |

### (Önceki tüm test sonuçları — Faz A/B, Bildirim/SMS, Premium/Referral/AI Limit, Sertifika/useHasat/Gating, Audit Takibi, Detaylı E2E, Alıcı Sayfaları, Mock Data, Fiyatlandırma, Tedarik Zinciri, Landing, RLS, Rekabet Hukuku, MCP Server, Benchmark Re-Audit, P16 serisi, P18-0 — değişmedi, önceki sürümlerde)
