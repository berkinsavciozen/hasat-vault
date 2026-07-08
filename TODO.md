---
title: Hasat — Master Roadmap & Build Log
updated: 2026-07-08
tags:
  - hasat
  - todo
  - roadmap
  - build
---

# Hasat — Master Roadmap & Build Log
> GTM Hedefi: **25 Ağustos 2026** · Günlük 1-2 saat · `[C]` = Claude ile · `[C Web]` = Web Claude (Lovable+Supabase+GitHub MCP)

---

## ✅ TAMAMLANDI

### Teknik Build
- [x] Supabase schema + seed data (17 tablo, RLS)
- [x] Farmer akışı: login, onboarding, journal, parcels, certifications, storefront, community, analytics, settings
- [x] Buyer akışı: onboarding (şirket + bireysel), discovery, offer, negotiation, payment (simulated)
- [x] AI özellikleri: tüm sayfalar AI analiz kutusu, WhatsApp AI chat (P9-P14)
- [x] Bildirim sistemi: in-app + Twilio SMS/WhatsApp (P7)
- [x] P15: Teklif/Müzakere state machine (ball_side, ping-pong, pending_payment)
- [x] P15-Completion A1-A4: backfill, accept guard, withdrawCounter, buyer Aktif tab
- [x] Auth/profiles fix: orphan cleanup, UNIQUE constraint, phone normalize, ON CONFLICT(id) düzeltmesi
- [x] Mobil responsiveness auditi ve düzeltmeleri (tüm uygulama)
- [x] Landing page premium yeniden tasarımı (v2) — "güven altyapısı" konumlandırması

### P16 — TÜM SERİ TAMAMLANDI ✅
- [x] [C] P16-A: Ürün (max 3) + Parsel (max 5) fotoğraf upload
- [x] [C] P16-B: Public vitrin URL `/s/{slug}` — girişsiz görüntülenebilir
- [x] [C] P16-C: IBAN ödeme köprüsü — havale → farmer onayı → sipariş aktif
- [x] [C] P16-E: Gerçek zamanlı fiyat verisi (price_feed, sparkline, AI nudge)
- [x] [C Web] P16-F: Topluluk reply threads + buyer→farmer profile linkleri
- [x] [C Web] P16-H Extension: Stok takibi + batch sayfası
- [x] [C Web] P16-G: Çiftçi referral programı UI
- [x] [C Web] P16-I: Crop config + production method + crop-agnostic AI
- [x] [C Web] P16-H: Ürün yaşam döngüsü / izlenebilirlik
- [x] [C Web] Bug Fix Batch 4: Onboarding kritik düzeltmeler (GTM blocker giderildi)
- [x] [C Web] P16-J (genişletilmiş): Ana tanıtım landing page + indoor farming başvuru formu (v1)
- [x] [C Web] Bug Fix Batch 3: quality default, crop slug, çok-ürünlü parsel seçici
- [x] [C Web] Crop/Şehir Konsolidasyonu: tek doğruluk kaynağı (`crop_config` + `TR_PROVINCES`) tüm uygulamada
- [x] [C Web] Draft-İlan Otomasyon Pipeline'ı: parsel → otomatik draft ilan → hasat kaydı → otomatik bağlama → Yayınla
- [x] [C Web] P16-D: ToS + Gizlilik sayfaları (`/terms`, `/privacy`)
- [x] [C Web] Mobil Responsiveness Audit + Düzeltmeleri
- [x] [C Web] Bireysel Alıcı Desteği: Şirket/Bireysel onboarding ayrımı + alıcı tipi gösterim düzeltmesi
- [x] [C Web] Landing Page v2 — Premium "Güven Altyapısı" Yeniden Tasarımı

### Bug Fixes — Tamamlanan
- [x] B1: Journal tag rendering (`[key:value]` chips)
- [x] B2: JournalEntryCard quality default → 'A'
- [x] B4: Hasat Dönemi chip — dinamik ay (P16-I ile crop_config'den kalıcı çözüldü)
- [x] B5: `safran_soğanı` slug → "Safran Soğanı" (merkezi `formatCrop()` ile 8 dosyada)
- [x] B6: Sertifika expiry badge (Süresi Geçti / Yakında Sona Eriyor)
- [x] B11: Safran birim → ₺18.400/kg+
- [x] B12: Farmer panelinde alıcı adı → Zeynep Kaya
- [x] B13: Analytics empty state → gerçek veri
- [x] B15: Teslim tarihi → dd.MM.yyyy (shadcn Calendar)
- [x] B16: Sipariş kartında telefon → maskeli
- [x] B18: Abonelikler placeholder → "Keşfet'e Git" CTA
- [x] B19: Keşfet kategoriler sayacı → düzeltildi (P16-I ile category_group'a göre)
- [x] ListingSheet description prefill + save
- [x] Stepper min_order clamping
- [x] Buyer Tamamlanan tab filter + Accept → orders satırı otomatik oluşturma
- [x] tsgo: `/buyer/orders*` ve `/login` search-param tip hataları
- [x] Onboarding "Şimdilik atla" veri kaybı, crops/land kaydedilmiyordu, parsel ekleme UI'ı yoktu (Bug Fix Batch 4)
- [x] `/` sayfası gerçek Supabase session yerine sadece client cache'ine güveniyordu
- [x] Indoor form `SUPABASE_SERVICE_ROLE_KEY` eksik hatası — anon client'a geçirildi
- [x] Çok ürünlü parselde Günlük kaydı her zaman `crops[0]`'a sabitleniyordu
- [x] 5 ayrı hardcoded crop listesi + 4 kopya emoji haritası — tutarsızlık
- [x] Şehir alanları bazı yerlerde serbest metin, bazı yerlerde dropdown'dı
- [x] Parsel oluşturunca vitrinde otomatik ilan oluşmuyordu — üç ayrı manuel adım gerekiyordu
- [x] Parsel/hasat kaydı sonrası Vitrin sayfası tazelenmiyordu (`farmerListings` invalidation eksikti)
- [x] `handle_new_user()` trigger'ında `ON CONFLICT (phone) DO NOTHING` — `ON CONFLICT (id)`'ye düzeltildi
- [x] Login OTP kutuları 360px'de taşıyordu
- [x] Alıcı alt navigasyonu 6 sekmeyi `grid-cols-5`'e sıkıştırıyordu
- [x] AI chat balonu ile Storefront/Fiyatlar sayfası FAB'ları çakışıyordu
- [x] Header'larda uzun başlıklar bildirim zilini ekran dışına itiyordu
- [x] Alıcı tipi her zaman "Restoran" gösteriliyordu — `profiles.buyer_type` denormalize edilerek düzeltildi

### Araştırma & Planlama
- [x] Saffron fiyat/yield araştırması · Financial model v0.5 (4-kat indoor) · GTM planı · Vault yeniden yapılandırma
- [x] P16-H şema kararı: iki katmanlı model çözüldü (Tier 1: listing_harvest_entries + Tier 2: parcel_id)
- [x] Landing page tasarım auditı v1: bevel.health (UI) + hilberts.ai (içerik zenginliği) referans alındı
- [x] Landing page tasarım auditı v2: kullanıcı brief'i ("Hasat sebze değil güven satıyor") — Stripe/Linear/Vercel/Apple referanslı premium yeniden konumlandırma
- [x] Lovable'a plan-mode audit'ler yaptırıldı: crop/şehir tutarsızlıkları, parsel→ilan→hasat bağlama boşluğu, mobil responsiveness, bireysel alıcı desteği, landing page v2 tasarımı için

---

## 🔴 ŞİMDİ — Temmuz 2026

### Yasal & Altyapı
- [ ] Şahıs şirketi kur — noterden git (~₺2-3K, 1 gün)
- [ ] iyzico.com başvurusu yap (şirket belgesi gerekli, onay ~2 hafta)
- [ ] Alan adı araştır — [C] karşılaştır (isim "Hasat" ile sınırlı değil, tarafsız marka adı da düşünülüyor)
- [ ] Twilio: Verify service oluştur
- [ ] Twilio: webhook URL → Supabase Edge Function URL ile güncelle
- [ ] **WhatsApp Business başvurusu → hemen başla (2-4 hafta Meta review)**
  > 2026-07-06 notu: WhatsApp OTP kanalı test edildi, teslimat başarısız oldu. SMS kanalı çalışıyor.

### Düşük öncelikli cila (acil değil)
- [ ] `farmer.journal.new.tsx` içindeki "Önce bir parsel ekle" metni hâlâ tıklanamaz statik yazı. Kritik değil, iki başka çalışan yol var.
- [ ] `formatCrop()` gerçekte `crop_config.display_name`'i okumuyor, slug'ı title-case'e çeviren bir heuristik.
- [ ] `useDeleteParcel` sonrası draft ilan kartındaki "📍 parsel adı" etiketi sessizce kayboluyor — kozmetik.
- [ ] Landing page'de birden fazla rol-seçim butonu var (Nav, Hero, bölüm CTA'ları, Final CTA) — kolayca yanlış tıklanabiliyor (bir testte çiftçi/alıcı karıştı). İleride azaltmayı düşün.
- [ ] Landing v2'nin kendi planında flag'lenen "ilk pas" sadeleştirmeleri: Türkiye haritası 6-8 pin'le sınırlı (zoom/pan yok), tedarik zinciri animasyonu tam node-tween değil scroll-crossfade, izlenebilirlik telefon mock'u scroll-scrub değil sıralı reveal, hero fotoğrafı tek bir Unsplash görseli. İstersen sonraki iterasyonda zenginleştirilebilir.
- [ ] Landing v2'nin canlı önizlemesi tarayıcıda görsel olarak henüz gözden geçirilmedi (Lovable'ın screenshot servisi gecikmeli önbellekliyor) — Berkin'in preview'i açıp onaylaması bekleniyor.

---

## 🏗️ Lovable Build Sırası — TAMAMLANDI ✅

> P16 serisi + mobil audit + bireysel alıcı desteği + landing page v2 bitti (2026-07-08). Sıradaki gerçek öncelik: **Ağustos E2E test kapsamı** — GTM'den önce platformun uçtan uca çalıştığının doğrulanması.

---

### ✅ Landing Page v2 — Premium "Güven Altyapısı" Yeniden Tasarımı *(Tamamlandı — 2026-07-08)*

Kullanıcıdan gelen kapsamlı bir tasarım brief'i işlendi: "Hasat sebze satmıyor, güven satıyor — marketplace bunun sadece aracı." Landing page'in "B2B tarım pazarı" gibi değil, Stripe/Linear/Vercel/Apple'a yakın premium bir SaaS gibi görünmesi hedeflendi. Prompt yazılmadan önce 5 kritik karar netleştirildi ve onaylandı, sonra Lovable'a plan-mode'da gönderilip planı onaylandıktan sonra build edildi.

**Netleştirilen kararlar (build öncesi):**
1. Yeni "premium" palet **sadece `/` landing page'e** uygulandı — uygulamanın geri kalanı (`--dark`/`--saffron`/`--gold`) değişmedi
2. Framer-motion veya harita kütüphanesi eklenmedi — Tailwind/CSS keyframes + IntersectionObserver + özel inline SVG Türkiye haritası
3. Gerçek çiftçi fotoğrafı yok — kimliksiz/watermark'sız stok görseller (toprak, safran, zeytin, lavanta, fındık) atmosfer için; illüstratif kartlar (Ahmet/Zeynep) açıkça "Örnek ilan"/"Örnek senaryo" etiketiyle işaretlendi
4. Güven Skoru isimleri tamamen Türkçe ve uygulamadaki gerçek sistemle birebir aynı: Belgeleniyor → Temel → İyi Belgelenmiş → Tam İzlenebilir
5. Sahte sosyal kanıt sayaçları (uydurma kullanıcı/çiftlik sayısı) **hiç eklenmedi** — dürüstlük gerekçesiyle tamamen atlandı

**Uygulanan (`src/routes/index.tsx` komple yeniden yazıldı, sadece bu dosya):**
1. Yeni local palet (`--lp-*` CSS değişkenleri, sadece bu route'a scope'lu): Deep Green `#1B3A2B`, Fresh Green accent `#4A9B5E`, Earth Brown `#6B4A32`, Cream `#F5F1E6`
2. Hero — desatüre/karartılmış tarla fotoğrafı arka plan, glassmorphism ürün kartı (Ahmet örneği, "Tam İzlenebilir" rozeti), sıralı çizilen 4 adımlı mini tarla günlüğü timeline'ı
3. Değer direkleri (Doğrudan Üreticiden / Tam İzlenebilir / Adil Fiyat)
4. Tedarik zinciri karşılaştırması — geleneksel 6 halkalı zincir (değer kaybı vurgusu) vs. Hasat'ın kısa direkt yolu
5. İzlenebilirlik bölümü — telefon mockup'ında sıralı açılan tarla günlüğü satırları
6. Pazar yeri önizlemesi — 4 "Örnek ilan" etiketli premium ürün kartı
7. İnteraktif Türkiye haritası — özel çizilmiş SVG anahat + 7 pulslayan pin (Safranbolu, Isparta, Ayvalık, Giresun, Malatya, İzmir, Antalya), tıkla-aç bilgi kartı
8. Çiftçi hikayesi — Ahmet'e atfedilen açıkça illüstratif alıntı + atmosferik toprak/el fotoğrafı (kimliksiz)
9. Alıcı personaları — 5 kart (Restoran/Kafe, Otel, Market/Manav, İhracatçı, Bireysel Tüketici)
10. Hasat AI — WhatsApp tarzı 2 örnek diyalog (soru-cevap + "bugün hasat yaptım" otomatik ilan güncellemesi)
11. Güven Skoru merdiveni — 4 gerçek tier, animasyonlu progress bar
12. Indoor Farming bölümü — form/DB/SMS bağlantısı korunarak yeni palette restyle edildi
13. Footer — `/terms`, `/privacy`, WhatsApp linkleri korunarak restyle edildi

**Korunan işlevsellik (doğrulandı):** Auth-aware session redirect, "Çiftçiyim"/"Alıcıyım" → `/login?role=` yönlendirmesi, `#indoor-basvuru` formu ve DB/SMS bağlantısı, `/terms` + `/privacy` footer linkleri — hiçbiri bozulmadı.

**tsgo:** Temiz geçti.

**Bekleyen:** Ekran görüntüsü servisi gecikmeli önbellekliyor, Berkin'in canlı preview'de görsel olarak gözden geçirip onaylaması gerekiyor.

---

### ✅ Bireysel Alıcı Desteği *(Tamamlandı — 2026-07-08)*

Canlı buyer onboarding testinde fark edildi: akış tamamen B2B varsayımıyla kurulmuştu. Lovable'a plan-mode audit yaptırıldı.

**Uygulanan:**
1. `company_type` enum'una `bireysel` eklendi
2. `onboarding.buyer.tsx` step 1'e **Şirket/Bireysel sekmesi**
3. `BuyerType` tipi genişletildi, "👤 Bireysel" rozeti eklendi
4. **Bonus fix:** alıcı tipi her zaman "Restoran" gösteriliyordu — `profiles.buyer_type` denormalize edilerek düzeltildi

---

### ✅ Mobil Responsiveness Audit + Düzeltmeleri *(Tamamlandı — 2026-07-08)*

360px viewport'ta tüm rotalar tarandı. Must-fix: login OTP kutuları, alıcı nav 5 slot + Daha sheet, AI/FAB çakışması, header truncate. Nice-to-have: 6 madde. Tümü uygulandı, tsgo temiz.

---

### ✅ P16-D — ToS + Gizlilik *(Tamamlandı — 2026-07-08)*

`/terms` (10 bölüm) + `/privacy` (9 bölüm), landing/login footer linkleri.

---

### ✅ Draft-İlan Otomasyon Pipeline'ı *(Tamamlandı — 2026-07-08)*

Parsel oluşturma/düzenleme → her ürün için otomatik draft ilan → hasat kaydı → otomatik bağlantı → manuel "Yayınla".

---

### ✅ Crop/Şehir Konsolidasyonu *(Tamamlandı — 2026-07-08)*

`useCropOptions()`, `TR_PROVINCES`, `CropChips.tsx`, `cropEmoji()` — tek kaynağa bağlandı.

---

### ✅ Bug Fix Batch 3 *(Tamamlandı — 2026-07-08)*

Quality default 'A', crop slug → display name, çok ürünlü parselde Günlük ürün seçici.

---

### ✅ P16-J (v1) — Landing Page + Indoor Form *(Tamamlandı — 2026-07-07, sonradan v2 ile üzerine yazıldı)*

İlk landing page yeniden yazımı — Landing Page v2 tarafından supersede edildi (2026-07-08).

---

### ✅ Bug Fix Batch 4 — Onboarding Kritik (GTM BLOCKER) *(Tamamlandı — 2026-07-07)*

"Şimdilik atla" veri kaybı, crops/land kaydedilmiyordu, parsel ekleme UI'ı yoktu.

---

### ✅ P16-F, P16-H Extension, P16-G, P16-I, P16-H, P16-E, P16-C

Detaylar önceki TODO sürümlerinde — hepsi tamamlandı ve test edildi.

---

### Sosyal Medya (Temmuz'da başla)
- [ ] Instagram hesabı aç: @hasatmarket veya @hasat.app
- [ ] İlk Reel çek: "Neden Hasat'ı kuruyorum" — 60 saniye, telefon selfie, Türkçe
- [ ] [C] 3 aylık içerik takvimi hazırla (Temmuz–Eylül, haftada 2-3 post)
- [ ] [C] Sosyal medya içerik şablonları oluştur

### Çiftçi Ağı Geliştirme (Hafta 2-4)
- [ ] [C] Safranbolu Safran Üreticileri Birliği iletişim bilgilerini bul + e-posta taslağı yaz
- [ ] Tarımsal Facebook + WhatsApp grupları araştır ve katıl (3 FB, 2-3 WA)
- [ ] [C] Tarım fuarları 2026 takvimi (Isparta Lavanta, Safranbolu Safran)
- [ ] LinkedIn'de "safran çiftçisi" araması — 5 profil, bağlantı isteği

---

## 🟡 AĞUSTOS 2026 — Soft Launch — ŞİMDİ BURADAYIZ

### E2E Test — devam ediyor
- [x] [C Web] Farmer onboarding E2E — 05421241011 ile tamamlandı, tüm bug'lar düzeltildi ve doğrulandı
- [x] [C Web] Landing page E2E (v1) — anonim ziyaretçi/yeni kayıt/mevcut giriş senaryoları + indoor form
- [x] [C Web] Parsel → Draft İlan → Hasat Bağlama → Yayınla pipeline'ı E2E test edildi
- [x] [C Web] Buyer onboarding E2E — 05398545294 ile tamamlandı — `ON CONFLICT` trigger bug'ı ve bireysel alıcı eksikliği bulundu, düzeltildi
- [ ] **Landing page v2 görsel doğrulama — Berkin'in canlı preview'de gözden geçirmesi bekleniyor**
- [ ] **Uçtan uca senaryo — SIRADAKİ ADIM:** Ahmet'in yayınladığı bir ürüne Zeynep (veya yeni buyer hesabı) teklif versin → pazarlık → IBAN ödeme → sipariş tamamlansın (bu P16-C'nin tam havale testini de kapatır)
- [ ] Kalan kapsam: sertifika, community+reply (buyer tarafı), AI chat, bildirimler, vitrin URL (buyer görünümü), referral, izlenebilirlik (buyer görünümü)

### Teknik Final
- [ ] hasat.lovable.app → custom domain yayını
- [x] ~~hasat.com.tr landing page~~ — P16-J ile birleştirilip tamamlandı, v2 ile geliştirildi
- [ ] iyzico canlı ödeme testi + tam entegrasyon
- [ ] P16-C tam havale e2e testi (pending_transfer → farmer confirm) — sıradaki senaryoyla kapanacak

### İlk Kullanıcılar
- [ ] İlk 3 farmer beyaz eldiven onboarding (telefonda birlikte)
- [ ] İlk 3 buyer outreach (İstanbul organik restoran/mağaza)
- [ ] İlk 5 farmer listesi: gerçek ürün, fiyat, fotoğraflı
- [ ] 🚀 **İlk gerçek işlem** — duyur (Instagram Reel)

---

## 🔵 SAFRAN SEZONU — Ekim–Kasım 2026

- [ ] 30 farmer, 15 buyer aktif · %5 take rate açılışı (Ekim başı) · ₺200K+ GMV hedefi
- [ ] [C] Safran sezonu kampanyası: içerik, push notification, buyer alert
- [ ] İlk 3 ay ücretsiz bitiyor → ₺99/mo farmer subscription açılışı (Kasım)

---

## ⬜ SONRAKI FAZLAR

### Platform Olgunlaşması
- [ ] ₺99/mo farmer subscription (5+ farmer) · ₺299/mo buyer premium (10+ buyer) · App Store/Play Store

### Phase 2 Özellikler
- [ ] **Hasat Deneyimi Rezervasyonu** — tarla ziyareti slotu; takvim + rezervasyon ödemesi
- [ ] **Korm Marketplace** — farmer→farmer safran soğanı alım-satımı
- [ ] **Otomatik Fiyat Toplama** — pg_cron + crop_config.auto_price_source; İzmir BB Hal API vb.
- [ ] **Landing v2 zenginleştirme** — Türkiye haritası bölge vurgulaması, tam node-tween tedarik zinciri animasyonu, scroll-scrub telefon mock'u, özel üretilmiş hero görseli

### Finansal Hedefler
- [ ] MRR ₺5K+ (Aralık 2026) · MRR ₺15K+ (Mart 2027 — M8) · MRR ₺50K+ (M18) · MRR ₺230K+ → **QUIT TETİKLEYİCİSİ** (M23)

### Çiftlik (Uzun Vadeli)
- [ ] TKDK başvurusu hazırlığı (M16 ~Eylül 2027) · TKDK başvurusu (M18 Ağustos 2027) 🔴
- [ ] Indoor çiftlik kurulumu (M24) · Outdoor safran ekimi (M28) 🔴 · Cofounder secured (M21)

---

## 📋 Lovable Prompt Yazma Kuralları

1. Tablo/kolon adlarını doğrula — Lovable yanlış tahmin eder
2. `ALTER TABLE ... ADD COLUMN IF NOT EXISTS` — her zaman IF NOT EXISTS kullan
3. Her yeni DB insert için RLS policy gerekip gerekmediğini kontrol et
4. UUID'leri hardcode et — phone lookup = ambiguous column hatası
5. Server-side kural: DB trigger'lar client bypass'ına karşı tek gerçek güvence
6. AI prompt'ları crop-agnostic yaz — `crop` + `crop_config` parametre olarak geç
7. Offer/durum alanlarında gerçek enum değerlerini prompt yazmadan önce DB'den doğrula
8. `plan_mode=true` her zaman güvenli bir "sadece planla" garantisi vermeyebilir — commit sha kontrolü şart
9. Yeni bir akış yazdırırken "veri kaydediliyor mu" ve "düzenleme UI'ı var mı" sorularını ayrıca sor
10. Bir bug fix birden fazla ekranı etkiliyorsa, her ekranı ayrı ayrı canlı test et
11. Server-side fonksiyonda admin/service-role client'a gerçekten ihtiyaç olup olmadığını sorgula
12. "İlk öğeyi sessizce varsay" deseninden kaçın — çok değerli alanlarda seçim UI'ı şart
13. Yeni bir DB trigger otomasyon zinciri kurulduğunda, ilgili client mutasyonlarının cache invalidation'ını da güncelle
14. Karmaşık, çok dosyalı değişiklik öncesi Lovable'a **plan-mode'da audit** yaptırmak çok değerli
15. `ON CONFLICT` yazarken çakışma hedefinin (`id` mi, doğal bir unique kolon mu) doğru olduğunu sorgula
16. RLS bir tabloyu sadece sahibine açıyorsa, o veriye başka bir rolün ihtiyacı varsa join yetmez — geniş okumaya açık bir tabloya denormalize et
17. **Büyük görsel/tasarım değişikliklerinde, palet/kapsam kararlarını (hangi route'lar etkilenecek, marka kimliği değişecek mi) prompt yazmadan önce netleştir** — Landing v2'de "sadece landing page" kararı erken netleştirilmeseydi tüm uygulamanın marka kimliği riske girebilirdi
18. Sahte veri/sosyal kanıt (kullanıcı sayısı, testimonial) gösterme baskısı olduğunda, gerçek veri yoksa **bölümü tamamen atla**, gizli placeholder bile bırakma — dürüstlük ilkesi

---

## 📌 Kararlar

| Konu | Karar |
|---|---|
| Şirket türü | Şahıs (hızlı, 1 gün) |
| Ödeme altyapısı | iyzico (onay ~2 hafta; bu sürede IBAN köprüsü) |
| İlk 3 ay monetizasyon | Ücretsiz |
| Ekim 2026 monetizasyon | %5 take rate açılışı |
| Sosyal medya | Berkin yapıyor, Claude içerik desteği |
| Alan adı | Belirsiz — araştırılacak, "Hasat" ismiyle sınırlı değil |
| Ürün kapsamı | Sadece özel/nadir ürünlerle sınırlı değil — 70 crop_config kaydı, her tür tarım ürünü |
| Alıcı kapsamı | Sadece B2B değil — **Bireysel Alıcı** segmenti resmi olarak destekleniyor |
| Marka konumlandırması | "Hasat sebze satmıyor, güven satıyor" — tarımsal ticaret için güven altyapısı, marketplace sadece mekanizma |
| Landing page palet kapsamı | Yeni "premium" palet (`--lp-*`) **sadece `/` route'una scope'lu** — uygulamanın geri kalanı `--saffron`/`--gold`/`--dark` kimliğinde kalıyor |
| Landing page görsel politikası | Gerçek olmayan kişi/testimonial gösterilmiyor; kimliksiz/watermark'sız stok görsel + açıkça "örnek" etiketli illüstratif kartlar |
| Sahte sosyal kanıt | Asla — gerçek kullanıcı sayısı olmadan sayaç/testimonial bölümü hiç eklenmiyor |
| Hasat Deneyimi / Korm Marketplace | Phase 2 |
| İzlenebilirlik birimi | İki katmanlı: Tier 1 (listing_harvest_entries) + Tier 2 (parcel_id + parsel-sezon penceresi) |
| Listing = tek parsel | Blend yok — 2 parselden satış = 2 ayrı listing |
| Sezon başlangıcı anchor | Son dikim-tipi entry — takvim yılı değil |
| Yetersiz kayıt satışı bloke eder mi | Hayır — sadece coverage rozeti etkilenir |
| Stok kilidi | İlk `status='accepted'` geçişi kilitler |
| Otomatik fiyat çekimi | Phase 2+ — MVP'de manuel küratörlük yeterli |
| AI kutuları | crop-agnostic — P16-I ile düzeltildi |
| Claude Web araçları | Lovable MCP + Supabase MCP + GitHub MCP (READ ONLY, yazma 403 veriyor) |
| Vault sync | `hasat-vault` reposu bilinçli olarak PUBLIC tutuluyor — GitHub MCP private repo'ya erişemediği için |
| Onboarding test numaraları | Çiftçi: 05421241011 · Alıcı: 05398545294 |
| Landing page tasarım referansları | v1: bevel.health + hilberts.ai · v2: Stripe/Linear/Vercel/Apple (kullanıcı brief'i) |
| İletişim numarası | Berkin: +905421241011 — `BERKIN_NOTIFY_PHONE` + `HASAT_WHATSAPP_NUMBER` |
| İlan durumu (listing_status) | 4 değerli enum: `draft`, `active`, `sold`, `expired`. Draft'lar RLS ile buyer'lardan gizli. |
| Draft ilan otomasyonu | Parsel oluşturulunca/düzenlenince her ürün için otomatik draft; hasat kaydı → otomatik bağlanıyor; manuel "Yayınla" ile aktif ediliyor |
| Hukuki sayfalar | `/terms`, `/privacy` — %5 komisyon, ihtilaf çözümü İstanbul mahkemeleri, KVKK/GDPR hakları |
| Alıcı tipi (company_type) | 6 değerli enum: `restoran`, `otel`, `organik_market`, `ihracatci`, `diger`, `bireysel`. `profiles.buyer_type` denormalize. |
| Güven Skoru tier isimleri | Tüm platformda (uygulama + landing) tutarlı: Belgeleniyor → Temel → İyi Belgelenmiş → Tam İzlenebilir |

---

## 📋 Son Test Sonuçları

### Landing Page v2 — Premium Yeniden Tasarım (2026-07-08) ✅ (görsel onay bekliyor)
| Test | Sonuç | Not |
|---|---|---|
| Plan-mode audit + onay akışı | ✅ | Kullanıcı 5 kritik kararı build öncesi netleştirdi |
| Palet sadece `/` route'una scope'lu | ✅ | `--lp-*` local CSS değişkenleri, global tokenlara dokunulmadı |
| 12 bölüm tam uygulandı | ✅ | Hero, değer direkleri, tedarik zinciri, izlenebilirlik, pazar yeri, harita, çiftçi hikayesi, alıcı personaları, AI, güven skoru, indoor, footer |
| Auth-redirect / CTA / indoor form / footer linkleri korundu | ✅ | Kod incelemesiyle doğrulandı |
| Sahte sosyal kanıt eklenmedi | ✅ | Talimata uygun, hiç bölüm yok |
| Güven Skoru Türkçe tier isimleri | ✅ | Mevcut sistemle birebir aynı |
| tsgo typecheck | ✅ | |
| Görsel doğrulama (canlı preview) | ⏳ | Ekran görüntüsü servisi gecikmeli, Berkin'in tarayıcıda gözden geçirmesi bekleniyor |

### Buyer Onboarding E2E — 05398545294 (2026-07-08) ⚠️ 2 bulgu → düzeltildi ✅
| Test | Sonuç | Not |
|---|---|---|
| SMS OTP gönderimi + doğrulama | ✅ | |
| İlk denemede `.single()` hatası | ❌ → ✅ | `ON CONFLICT (phone)` → `ON CONFLICT (id)` |
| Onboarding — sadece B2B akış | ⚠️ → ✅ | Bireysel alıcı desteği eklendi |
| Hesap oluşturma | ✅ | |

### Bireysel Alıcı Desteği (2026-07-08) ✅
### Mobil Responsiveness Audit + Düzeltmeleri (2026-07-08) ✅
### P16-D — ToS + Gizlilik (2026-07-08) ✅
### Draft-İlan Otomasyon Pipeline'ı (2026-07-08) ✅
### Crop/Şehir Konsolidasyonu (2026-07-08) ✅
### Bug Fix Batch 3 (2026-07-08) ✅
### P16-J (v1) — Landing Page + Indoor Form (2026-07-07) ✅ — v2 ile supersede edildi
### Bug Fix Batch 4 — Onboarding Kritik Düzeltmeleri (2026-07-07, 2026-07-08 tekrar teyit) ✅
### P16-H, P16-I, P16-G, P16-H Extension, P16-F, P16-E (Temmuz 2026) ✅

### P16-C — IBAN Ödeme Köprüsü ✅ kısmi
| Test | Sonuç |
|---|---|
| Tam havale e2e (pending_transfer → farmer confirm) | ⏳ Sıradaki uçtan uca senaryoda kapanacak |
