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
- [x] **Bildirim Ayarları DB Bağlantısı + SMS Mimarisi Client'tan DB'ye Taşındı + AI Chat Kalite Denetimi** (2026-07-19) — bkz. detaylı bölüm aşağıda
- [x] **P18-0 Arayüz Denetimi** (2026-07-20) — bkz. P18 bölümü aşağıda

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
- [ ] **[Benchmark re-audit] Toplu "ilk N kullanıcıya otomatik 6 ay premium" feature-flag mekanizması** — hâlâ açık.
- [x] ~~Referral suistimal koruması~~ / ~~AI kredi/limit mekanizması~~ — Tamamlandı.

### Düşük öncelikli cila
- [ ] `farmer.journal.new.tsx` "Önce bir parsel ekle" statik metni · `formatCrop()` heuristik · Landing page'de çok fazla rol-seçim butonu
- [ ] MCP yazma tool'larında aralıklı "No approval received" hatası — retry ile geçiyor
- [ ] **Lovable platform bug'ı (bilinen, düzeltilemez):** `plan_mode=false` sinyali agent'ın kod tool'larına bazen yansımıyor.
- [ ] Storage bucket public/private ayarını her zaman canlı doğrula
- [ ] **[Audit'te bulundu] `crop_requests` tablosu için admin inceleme arayüzü** — Claude periyodik kontrol edecek (P17-E'ye kadar)
- [ ] **[Bu turda bulundu, düşük öncelik] `create_subscription` rol-temizliği eksik** — `harvest_subscriptions.buyer_id`'nin `role='buyer'` bir profile ait olduğunu zorlayan bir CHECK eklenebilir.
- [ ] **[Bu turda bulundu, kozmetik] `FarmerAIChat.tsx` header'ı "Premium • sınırsız sohbet"** — artık 500/ay tavanı var, "ayda 500 mesaj" gibi düzeltilebilir.
- [ ] **[Bu turda bulundu] `useHasat.setNotifPref` + store'daki `notifPrefs` slice'ı artık dead code** — bildirim ayarları gerçek DB'ye taşındığı için bu local state hiç kullanılmıyor, temizlenebilir (Lovable'ın kendi notu).
- [ ] **[Bu turda bulundu, karar bekliyor] AI chat'te "hava sor" ipucu tamamen kurgusal** — hiç hava durumu kaynağı yok. Karar bekleniyor: (a) ipucunu kaldır, (b) Open-Meteo gibi ücretsiz bir kaynakla gerçek hava bağlamı ekle.

---

## 🏗️ Lovable Build Sırası — TAMAMLANDI ✅

> Sıradaki: aşağıdaki **"Sıradaki Faz Planı"** bölümüne bkz.

---

### ✅ Bildirim Ayarları + SMS Mimarisi + AI Chat Kalite Denetimi *(Tamamlandı — 2026-07-19)*

**1. Bildirim tercihleri ayarları — 7. "sahte" bulgu, düzeltildi:** `farmer.settings.notifs.tsx` yine `useHasat` local store'a yazıyordu, DB'ye hiç dokunmuyordu. `notif_prefs` tablosunda **sıfır satır** vardı (tüm SMS'ler sessizce opt-out). **Düzeltme:** Sayfa gerçek `notif_prefs` tablosuna bağlandı (`useNotifPrefs`/`useUpdateNotifPrefs`), 22 mevcut kullanıcı için varsayılan satır oluşturuldu (`new_offer_sms=true`), `handle_new_user` yeni kayıtlar için de aynısını yapacak şekilde güncellendi.

**2. 🔴 Bugünün en önemli mimari bulgusu — SMS gönderimi client-side'da yaşıyordu, DB'de hiç değildi:** Canlı E2E testinde (MCP ile gerçek bir teklif oluşturulup loglar kontrol edilerek) bulundu — `send-sms` fonksiyonu **sadece** tarayıcıdan `useCreateOffer`'ın `onSuccess`'inde çağrılıyordu. MCP, direkt SQL, veya gelecekteki her türlü entegrasyon bu yolu **atlıyor**, SMS asla gönderilmiyordu. Ayrıca kabul/karşı teklif/ödeme onayı olaylarında **hiç** SMS bağlantısı yoktu. **Düzeltme:** `pg_net` uzantısı açıldı, yeni `dispatch_sms()` fonksiyonu (notif_prefs kontrolü + Twilio çağrısı) `notify_offer_received()` trigger'ına bağlandı, client-side çağrı kaldırıldı. **Gerçek bir Twilio isteğiyle (201 yanıtı) canlı doğrulandı** — test verisi temizlendi. Kapsam bilinçli olarak sadece `new_offer` ile sınırlı tutuldu (diğer olaylar için `notif_prefs`'te henüz SMS kolonu yok) — genişletme aşağıya, sıradaki faz planına taşındı.

**3. AI Chat kalite denetimi — 2 bulgu:**
   - Alıcı için AI chat hiç yok (aktif bug değil, ama component ileride alıcıya taşınırsa `fetchContextBlock`'un hep boş döneceği ve sistem promptunun çiftçi diliyle konuşacağı doğrulandı — not edildi).
   - **🔴 8. "vaat edilen ama arkası boş" bulgusu:** "Hava sor" ipucu (`FarmerAIChat.tsx`) tamamen kurgusal, hiçbir hava durumu kaynağı yok — bir çiftçi sorarsa model tamamen halüsinasyon üretir. Karar bekliyor (yukarı bkz.).

**Maliyet:** Bildirim ayarları 3.3 + SMS mimarisi 2.8 + AI chat denetimi 2 = **8.1 kredi**.

---

### ✅ (Önceki tüm tamamlanan işler — Premium/Referral/AI Limit, Sertifika/useHasat/Gating, Audit Takibi, Detaylı E2E, Alıcı Sayfaları, Mock Data, Fiyatlandırma, Tedarik Zinciri, Landing, RLS, Rekabet Hukuku, MCP Server, Benchmark Re-Audit, P16 serisi)

(Detaylar önceki sürümlerde — değişmedi)

---

## 🧭 SIRADAKİ FAZ PLANI (2026-07-19 itibarıyla)

> Bugüne kadarki her tur, "bir önceki turun bulduğu son 1-2 maddeyi kapat" şeklinde ilerledi ve her seferinde yeni, daha derin bir "sahte veri/kırık akış" bulgusu ortaya çıkardı (bugüne kadar toplam **8 bağımsız örnek**: journal foto, Üretici Detay, Abonelik Oluştur, Premium sayfası, referral ödülü, bildirim ayarları, SMS mimarisi, hava sor). Bu noktada, tek tek "bir sonraki küçük şey" yerine, **kalan kapsamı 3 net fazda** gruplamak daha sağlıklı:

### Faz A — Ağustos E2E Kapsamını Resmi Olarak Kapatmak (GTM'den önce, kısa)
Kalan sadece 2 gerçek açık madde var:
1. **Community reply UI (alıcı tarafı)** — henüz hiç bakılmadı, muhtemelen benzer bir "gerçek mi sahte mi" kontrolü gerekiyor.
2. **"Hava sor" kararı** — (a) kaldır ya da (b) gerçek Open-Meteo entegrasyonu. Karar + varsa implementasyon.

Bunlar bitince **Ağustos E2E kapsamı resmi olarak tamamlanmış olur** — TODO'daki "Sıradaki: P17" notu gerçek anlamda doğru hale gelir.

### Faz B — Bugünün Bulduğu Takip İşleri (küçük, dağınık, ama gerçek)
Bugünkü 8 bulgunun bıraktığı **küçük ek işler** (hiçbiri acil değil, ama biriktirmemek için toplu halledilebilir):
- SMS kapsamını genişletme: `notif_prefs`'e `offer_accepted_sms`, `payment_confirmed_sms` gibi kolonlar + `notify_order_status()`/`notify_offer_accepted()` trigger'larına `dispatch_sms()` bağlama (bugünün deseniyle birebir aynı, tekrarı kolay)
- `create_subscription` rol-temizliği CHECK'i
- `useHasat` dead code temizliği (bildirim slice'ı)
- Kozmetik: "Premium • sınırsız" → "ayda 500 mesaj"
- Toplu premium feature-flag mekanizması (Benchmark'tan kalan)

### Faz C — P17 Güven Çekirdeği (Eylül, büyük)
TODO'daki P17-A/B/C/D/E/F/G serisi — bloke ödeme, teslim/ihtilaf akışı, gerçek review sistemi, fatura, RFQ, tekrar sipariş, KPI görünümü. Bu, **Faz A bitmeden başlamamalı** (roadmap'in kendi sıralaması da bunu söylüyor).

**Not (2026-07-20):** Aşağıdaki **P18 — Arayüz Yenileme** serisi bu üç fazla **paralel** yürüyebilir — tamamen frontend/UI katmanında, backend/DB şemasına dokunmuyor, Faz A/B/C'nin sırasını bloklamıyor.

---

## 🎨 P18 — ARAYÜZ YENİLEME (UI-Only, Backend/DB Değişikliği YOK)

> **Tetikleyici:** Berkin'in 3 kriterli arayüz modernizasyonu talebi (persona ilgi alanları, persona'nın en sık kullandığı uygulamalar + eylem alma UX sistemleri, AI+WhatsApp-native girdi/çıktı) + ekran görüntüsü üzerinden bulunan somut UX sorunları.
> **Ortak Guardrail (her prompt'a eklenir):** *"Bu görev SADECE frontend/UI katmanını değiştirir. `supabase/migrations/`, `supabase/functions/`, veya herhangi bir DB şema/RLS/trigger dosyasına dokunma. Sadece `src/` altında çalış."* Her build sonrası Claude, diff'in yalnızca `src/`'i etkilediğini + migration sayısının değişmediğini Supabase MCP ile doğrular.

### ✅ P18-0 — Denetim *(Tamamlandı — 2026-07-20, Lovable kredisi kullanmadan)*
Mevcut dosya ağacı, tema katmanı (`styles.css`), farmer shell (`farmer.tsx`), AI chat (`FarmerAIChat.tsx`), Vitrin (`farmer.storefront.tsx`), public storefront (`s.$slug.tsx`), Discover (`buyer.discover.tsx`), Reports (`buyer.reports.tsx`), Fiyatlar (`PricesPageBody.tsx`), Analitik (`farmer.analytics.tsx`), Ayarlar (`farmer.settings.tsx`, `farmer.settings.notifs.tsx`) okundu. Bulgular:
- WhatsApp entry noktası zaten var (`wa.me` linki + `whatsapp-ai-webhook` stub) ama gömülü/küçük.
- AI chat deeplink event sistemi (`hasat:ai-chat:open`) zaten var — quick action'lar bunu kullanmıyor.
- `Sparkline.tsx` component'i **var ama hiçbir yerde kullanılmıyor**.
- `price_alerts` (watchlist) tablosu + hook'ları **var ama UI'da hiç yüzeyde değil**.
- **Mobil bug bulundu:** `farmer.settings.notifs.tsx`'teki 3 kanal kolonu (`w-20` × 3 + gap) 375px ekranda "Olay" etiketini sıkıştırıyor — masaüstü tablo mantığı mobilde bozuluyor.
- "Bildirim Tercihleri" butonu kodda **çalışıyor** (route + hook'lar doğru bağlı) — Berkin'in gördüğü sorun muhtemelen Lovable edit-mode overlay'i, canlı/preview URL'de doğrulanmalı.

### Faz Sıralaması (önerilen): A → B → G → F → E → C → D

---

### P18-A — Tema Katmanı + Ortak Component Seti
**Amaç:** Lavender AI rengini saffron/gold ailesine taşı, WhatsApp yeşilini (`#25D366`) sadece gerçek `wa.me` linklerine ayır, 48px dokunma hedefleri, ve farmer+buyer arasında paylaşılan bir component seti (`StatCard`, `SectionCard`, foto `ListingCard`, standart `Sparkline` kullanımı, mobil-güvenli toggle-tablo deseni).
**Dokunacağı dosyalar:** `src/styles.css`, `src/components/hasat/ai-chat/FarmerAIChat.tsx` (renk), yeni `src/components/hasat/common/` klasörü.

**Lovable Prompt (plan_mode=true ile gönderilecek):**
> Hasat'ın tema katmanını ve tekrar eden UI parçalarını güncelle. **SADECE `src/` içinde çalış, `supabase/` klasörüne dokunma.**
> 1. `src/styles.css`'teki `--lav` rengini AI chat'te kullanmayı bırak; AI ile ilgili tüm vurgu renklerini (`FarmerAIChat.tsx` FAB, mesaj balonu border'ı, sparkles ikonu) mevcut `--saffron`/`--gold` paletine taşı. `#25D366` (WhatsApp yeşili) sadece gerçek `wa.me` linklerinde/ikonlarında kullanılsın, başka hiçbir yerde.
> 2. Tüm interaktif elemanlarda (buton, link, toggle, sekme) minimum 48×48px dokunma alanı olduğundan emin ol; küçük olanları büyüt.
> 3. `src/components/hasat/common/` altında yeniden kullanılabilir 3 component oluştur: `StatCard` (label+value+opsiyonel accent — analytics/reports'ta tekrar eden pattern), `SectionCard` (başlık+içerik wrapper — settings/reports'ta tekrar eden pattern), `PhotoListingCard` (foto+başlık+fiyat — storefront/discover'daki kart deseninin ortak hali). Mevcut kullanım yerlerini bunlarla değiştirme — sadece component'leri oluştur, P18'in sonraki fazları kullanacak.
> 4. `farmer.settings.notifs.tsx`'teki 3 kanallı tablo görünümünü mobilde (≤640px) her olay için dikey kart + 3 etiketli toggle satırına çevir, masaüstünde mevcut tablo kalsın.

---

### P18-B — Çiftçi Ana Sayfa: Sohbet-Önde Giriş Akışı
**Amaç:** Quick action'ları form sayfalarına değil, mevcut AI chat deeplink event'ine (`hasat:ai-chat:open`) yönlendir; ana sayfaya kalıcı "Hasadını yaz…" giriş çubuğu ekle; WhatsApp'ı ikinci sınıf link'ten home'da birinci sınıf kanala çıkar.
**Dokunacağı dosyalar:** `src/routes/farmer.home.tsx`, `src/components/hasat/ai-chat/FarmerAIChat.tsx` (varsa küçük ek).

**Lovable Prompt:**
> `src/routes/farmer.home.tsx`'i güncelle. **SADECE `src/` içinde çalış.**
> 1. Mevcut "Hasat Kaydet" quick action'ını `/farmer/journal/new`'e değil, `window.dispatchEvent(new CustomEvent("hasat:ai-chat:open", { detail: { prefill: "Hasat kaydı eklemek istiyorum: " } }))` tetiklemesine çevir (bu event zaten `FarmerAIChat.tsx`'te dinleniyor). "Vitrine Ekle" ve "Alıcı Bul" formlarını olduğu gibi bırak — sadece hasat kaydı akışı chat-first olsun.
> 2. Sayfanın en üstüne, AIBox'ın üzerine, tek satırlık bir "Hasadını yaz veya WhatsApp'tan gönder…" giriş çubuğu ekle: tıklanınca aynı `hasat:ai-chat:open` event'ini boş prefill ile tetikler; yanında küçük bir WhatsApp ikonu `wa.me` linkine gitsin.
> 3. Boş durum (isEmpty) kartındaki CTA'ları da aynı mantıkla güncelle.
> Backend/DB'ye dokunma, sadece mevcut event sistemini ve route'ları yeniden bağlıyorsun.

---

### P18-C — Sipariş Durumu & WhatsApp Deeplink'leri
**Amaç:** Getir-tarzı adım timeline'ı öne çıkar, her siparişte "WhatsApp'tan takip et" deeplink'i.
**Dokunacağı dosyalar:** `src/routes/farmer.orders.index.tsx`, `src/routes/buyer.orders.$orderId.tsx`, `src/components/hasat/OrderTimeline.tsx`.

**Lovable Prompt:**
> Sipariş detay ve liste sayfalarını güncelle. **SADECE `src/` içinde çalış.**
> 1. `OrderTimeline.tsx` component'ini mevcut adım verisiyle (sent→accepted→preparing→shipped→delivered) daha görsel bir dikey/yatay adım göstergesi haline getir (aktif adım vurgulu, tamamlanan adımlar check'li).
> 2. Her sipariş kartına/detayına, üreticinin telefon numarası mevcutsa, `https://wa.me/{numara}` linkiyle "WhatsApp'tan sor" butonu ekle (mevcut `tel:` linkinin yanına, onu değiştirmeden).
> Gerçek WhatsApp Business API entegrasyonu bu fazın kapsamı DIŞINDA — sadece `wa.me` deeplink'leri.

---

### P18-D — Alıcı Raporları/Siparişleri: Tablo Hissini Kart Hissine Çevir
**Amaç:** `buyer.reports.tsx`'teki "Tedarikçi Güveni" ve "Son Siparişler" yoğun metin satırlarını, Discover'daki kart estetiğine yakın bir hale getir.
**Dokunacağı dosyalar:** `src/routes/buyer.reports.tsx`.

**Lovable Prompt:**
> `src/routes/buyer.reports.tsx`'teki "Tedarikçi Güveni" ve "Son Siparişler" bölümlerini yeniden tasarla. **SADECE `src/` içinde çalış.**
> Mevcut yoğun metin satırlarını, her tedarikçi/sipariş için görsel ağırlığı olan kartlara çevir (üretici adı daha büyük, teslim oranı bir mini progress/badge olarak, sipariş kartlarında ürün fotoğrafı varsa göster). KPI kartları ve aylık bar chart olduğu gibi kalsın, veri kaynağı ve hesaplamalar (`useBuyerAnalytics`, `orderRowTotal`, `isPaidOrder`) değişmeyecek — sadece görsel sunum.

---

### P18-E — Vitrin Kartı: Paylaşılabilir Önizleme
**Amaç:** `/s/$slug` genel vitrini bir "sosyal medyada paylaşılabilir" kart haline getir (hero banner, native share), ve çiftçinin kendi Vitrin sayfasına bu görünümün canlı önizlemesini ekle.
**Dokunacağı dosyalar:** `src/routes/s.$slug.tsx`, `src/routes/farmer.storefront.tsx`.

**Lovable Prompt:**
> `src/routes/s.$slug.tsx` genel vitrin sayfasını yeniden tasarla. **SADECE `src/` içinde çalış.**
> 1. Sayfanın başına, üreticinin en yeni parsel fotoğrafını (varsa) arka plan olarak kullanan bir hero banner ekle (üzerinde üretici adı, şehir, sertifika rozetleri — mevcut veri, yeni sorgu gerekmiyor).
> 2. `navigator.share` destekleniyorsa native paylaşım butonu ekle ("Bu vitrini paylaş"), desteklenmiyorsa mevcut `copyVitrinLink` fonksiyonuna geri dön (zaten var, sadece fallback olarak kullan).
> 3. `farmer.storefront.tsx`'e, üst kısımda küçük bir "Alıcılar böyle görecek" canlı önizleme kartı ekle — aynı hero banner düzenini kullanan, tıklanınca `/s/$slug`'a giden bir link/kart. Mevcut "Alıcı bunu görecek" metin notunu bu görsel önizlemeyle değiştir ya da yanına ekle.
> Yeni veri sorgusu gerekmiyor — mevcut `useStorefront`/`vitrinUrl` fonksiyonlarını kullan.

---

### P18-F — Piyasa & Performans Görünümü (Fiyatlar + Analitik)
**Amaç:** `Fiyatlar` (`PricesPageBody.tsx`) ve `Analitik` (`farmer.analytics.tsx`) sayfalarını borsa/finans uygulaması hissiyle yeniden tasarla: arama/filtre, zaman aralığı, sparkline, watchlist.
**Dokunacağı dosyalar:** `src/components/hasat/PricesPageBody.tsx`, `src/routes/farmer.analytics.tsx`.

**Lovable Prompt:**
> `Fiyatlar` ve `Analitik` sayfalarını piyasa/borsa uygulaması hissiyle yeniden tasarla. **SADECE `src/` içinde çalış, yeni migration yok.**
> 1. `PricesPageBody.tsx`'e üstte bir arama/filtre çubuğu ekle (ürün adına göre filtrele — mevcut `crops` listesi üzerinde client-side filtreleme, yeni sorgu yok). Her `PriceSummaryCard`'a mevcut `Sparkline` component'ini kullanarak küçük bir trend göstergesi ekle (şimdilik `hasat.avgPrice` etrafında sentetik/placeholder bir seri kabul edilebilir — gerçek zaman serisi verisi `price_history` tablosunda mevcutsa onu kullan, DB şemasına dokunma). Hasat topluluk verisi ve resmi HKS verisi görsel olarak ayrı iki seri/blok olarak kalsın, asla birleştirilmesin (rekabet hukuku ayrımı korunmalı).
> 2. Her karta, mevcut `usePriceAlerts`/`useCreatePriceAlert`/`useTogglePriceAlert` hook'larını kullanarak bir yıldız/watchlist ikonu ekle (bu hook'lar ve `price_alerts` tablosu zaten var, sadece UI'da hiç yüzeyde değildi — sadece bağla, yeni backend mantığı yazma).
> 3. `farmer.analytics.tsx`'teki 3 statik stat kartını ve tek "en çok ciro getiren ürün" satırını, zaman ekseni olan bir görünüme çevir: son 6 ay ciro trendi (mevcut `useFarmerOrders` verisinden client-side hesapla, `buyer.reports.tsx`'teki aylık bar chart mantığına benzer), ürün bazlı kırılım (crop breakdown, aynı mantık), `Sparkline` kullanımı. Yeni sorgu gerekmiyor, mevcut `useFarmerListings`/`useFarmerOrders` üzerinden hesapla.

---

### P18-G — Ayarlar Yeniden Grupla + Mobil Bildirim Tablosu Düzeltmesi
**Amaç:** `farmer.settings.tsx`'teki düz kart yığınını gruplu hale getir; Profil+Parsellerim+Sertifikalar → tek bir açılır/kapanır "Çiftlik Bilgileri" bölümü; Bildirim Tercihleri/Premium/Verilerim/Hesap kendi başlarına üst seviyede kalır.
**Dokunacağı dosyalar:** `src/routes/farmer.settings.tsx`.

**Lovable Prompt:**
> `src/routes/farmer.settings.tsx`'i yeniden grupla. **SADECE `src/` içinde çalış.**
> Mevcut "Profil", "Parsellerim", "Sertifikalar" bölümlerini (Section component'leriyle render edilen) tek bir açılır/kapanır (accordion/collapsible, shadcn `Accordion` component'i mevcut — `src/components/ui/accordion.tsx`) "Çiftlik Bilgileri" bölümü içine topla, varsayılan olarak kapalı başlasın. "Banka Bilgileri", "AI Asistan", "Bildirim Tercihleri", "Verilerim", "Hesap" bölümleri kendi başlarına üst seviyede, sıralamaları değişmeden kalsın. Hiçbir state/mutation/hook mantığı değişmeyecek — sadece hangi JSX'in hangi wrapper içinde render edildiği değişiyor.
> Ayrıca P18-A'da eklenen mobil-güvenli toggle-tablo pattern'i `farmer.settings.notifs.tsx`'e henüz uygulanmadıysa burada uygula (P18-A component'i varsa onu kullan).

---

## 🟡 AĞUSTOS 2026 — Soft Launch — ŞİMDİ BURADAYIZ

### E2E Test — devam ediyor
- [x] S1-S13 · Fiyatlandırma/Keşfet/Mesajlar/Raporlar/Abonelikler/Üretici Detay/Fotoğraflar · Sertifika/useHasat/Gating · Premium/Referral/AI Limit · **Bildirim ayarları + SMS mimarisi + AI chat denetimi** — hepsi ✅
- [ ] Kalan kapsam (Faz A): **community+reply UI**, **AI chat "hava sor" kararı**

### Teknik Final
- [ ] hasat.lovable.app → custom domain yayını · iyzico canlı ödeme testi
- [x] P16-C tam havale e2e testi — kapandı ✅

---

## 🟣 P17 — GÜVEN ÇEKİRDEĞİ *(Eylül 2026 — Faz C, bkz. yukarı)*

(Değişmedi — bkz. önceki sürüm: P17-A/B/C/D/E/F/G)

---

## 🔵 SAFRAN SEZONU / ⬜ SONRAKI FAZLAR

(Değişmedi — bkz. önceki sürüm)

---

## 📋 Lovable Prompt Yazma Kuralları

(1-43 önceki sürümde — devam:)
44. **[Bu turda eklendi] UI-only bir seri (P18 gibi) yürütürken her prompt'a açık bir "SADECE `src/`" guardrail'i eklenmeli** ve her build sonrası diff + migration sayısı Supabase MCP ile doğrulanmalı — backend'e sızma riskini build zamanında değil, prompt yazım zamanında engellemek daha güvenli.
45. **[Bu turda eklendi] Bir component/hook "var ama kullanılmıyor" durumundaysa (örn. `Sparkline.tsx`, `price_alerts` hook'ları), yeni bir şey inşa etmek yerine önce mevcut olanı bağlamak denenmeli** — kredi maliyeti çok daha düşük, risk çok daha az.

---

## 📌 Kararlar

(önceki tablo + eklenenler:)
| **SMS mimarisi (2026-07-19)** | Dispatch client-side'dan DB'ye (`pg_net` + `dispatch_sms()`) taşındı — kaynak (web/MCP/gelecek) fark etmeksizin tutarlı çalışır. Kapsam şimdilik sadece `new_offer`, genişletme Faz B'ye bırakıldı. |
| **Faz planı (2026-07-19)** | Kalan iş Faz A (Ağustos E2E kapanışı — community reply + hava sor kararı) → Faz B (bugünün küçük takip işleri, toplu) → Faz C (P17, Eylül) olarak gruplandı. |
| **P18 kapsam kararları (2026-07-20)** | WhatsApp: sadece UI (deeplink+nudge), gerçek Business API entegrasyonu ayrı bir faz olarak ertelendi. Tema: full revamp (tokens + tüm sayfalar). Free-tier AI gating: şimdilik dokunulmuyor (6 ay ücretsiz flag'i zaten aktif). AI chat rengi lavender'dan saffron/gold ailesine taşınıyor; WhatsApp yeşili (#25D366) sadece gerçek `wa.me` linklerinde. |
| **P18 sıralama (2026-07-20)** | A → B → G → F → E → C → D — günlük kullanım yüzeylerini (home, ayarlar) önce, düşük frekanslı cila işlerini (vitrin paylaşım, sipariş timeline, raporlar) sona bırakan sıralama. |

---

## 📋 Son Test Sonuçları

### P18-0 Arayüz Denetimi (2026-07-20) ✅
| Kontrol | Sonuç |
|---|---|
| AI chat deeplink event sistemi (`hasat:ai-chat:open`) | ✅ Var, kullanılmıyor — P18-B'de bağlanacak |
| `Sparkline.tsx` component'i | ✅ Var, hiçbir yerde kullanılmıyor — P18-F'de bağlanacak |
| `price_alerts` watchlist hook'ları | ✅ Var, UI'da yüzeyde değil — P18-F'de bağlanacak |
| Bildirim tercihleri route/hook bağlantısı | ✅ Kodda doğru bağlı — Berkin'in gördüğü sorun muhtemelen Lovable edit-mode overlay'i |
| `farmer.settings.notifs.tsx` mobil genişlik | ❌ 375px'te 3 kanal kolonu label'ı sıkıştırıyor — P18-A/G'de düzeltilecek |
| Public vitrin (`/s/$slug`) paylaşılabilirlik | ❌ Düz liste, hero/paylaşım yok — P18-E'de düzeltilecek |
| Buyer reports tablo hissi | ❌ "Tedarikçi Güveni"/"Son Siparişler" yoğun metin — P18-D'de düzeltilecek |
| Fiyatlar/Analitik zaman serisi görünümü | ❌ Yok, düz statik kartlar — P18-F'de düzeltilecek |

### Bildirim Ayarları + SMS Mimarisi + AI Chat Denetimi (2026-07-19) ✅
| Kontrol | Sonuç |
|---|---|
| `notif_prefs` DB bağlantısı + 22 kullanıcı backfill | ✅ |
| SMS: MCP üzerinden oluşturulan teklif → önceden hiç SMS tetiklemiyordu | ❌→✅ Bulundu, `pg_net`/`dispatch_sms()` ile düzeltildi |
| SMS canlı doğrulama (gerçek Twilio 201 yanıtı) | ✅ Test verisi temizlendi |
| Alıcı AI chat taraması | ✅ Yok, aktif bug değil, not edildi |
| "Hava sor" kaynak taraması | ❌ Hiç kaynak yok, karar bekliyor |

### (Önceki tüm test sonuçları — Premium/Referral/AI Limit, Sertifika/useHasat/Gating, Audit Takibi, Detaylı E2E, Alıcı Sayfaları, Mock Data, Fiyatlandırma, Tedarik Zinciri, Landing, RLS, Rekabet Hukuku, MCP Server, Benchmark Re-Audit, P16 serisi — değişmedi, önceki sürümlerde)
