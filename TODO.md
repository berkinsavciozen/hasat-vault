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
- [x] Buyer akışı: onboarding, discovery, offer, negotiation, payment (simulated)
- [x] AI özellikleri: tüm sayfalar AI analiz kutusu, WhatsApp AI chat (P9-P14)
- [x] Bildirim sistemi: in-app + Twilio SMS/WhatsApp (P7)
- [x] P15: Teklif/Müzakere state machine (ball_side, ping-pong, pending_payment)
- [x] P15-Completion A1-A4: backfill, accept guard, withdrawCounter, buyer Aktif tab
- [x] Auth/profiles fix: orphan cleanup, UNIQUE constraint, phone normalize

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
- [x] [C Web] P16-J (genişletilmiş): Ana tanıtım landing page + indoor farming başvuru formu
- [x] [C Web] Bug Fix Batch 3: quality default, crop slug, çok-ürünlü parsel seçici
- [x] [C Web] Crop/Şehir Konsolidasyonu: tek doğruluk kaynağı (`crop_config` + `TR_PROVINCES`) tüm uygulamada
- [x] [C Web] Draft-İlan Otomasyon Pipeline'ı: parsel → otomatik draft ilan → hasat kaydı → otomatik bağlama → Yayınla
- [x] [C Web] P16-D: ToS + Gizlilik sayfaları (`/terms`, `/privacy`)

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
- [x] tsgo: `/buyer/orders*` ve `/login` search-param tip hataları (P16-H ile birlikte temizlendi)
- [x] Onboarding "Şimdilik atla" veri kaybı (Bug Fix Batch 4)
- [x] Onboarding crops/land size hiç kaydedilmiyordu (Bug Fix Batch 4)
- [x] Parsel ekleme UI'ı hiçbir yerde yoktu — GTM blocker (Bug Fix Batch 4)
- [x] `/` sayfası gerçek Supabase session yerine sadece client cache'ine güveniyordu (landing page ile birlikte düzeltildi)
- [x] Indoor form ilk denemede `SUPABASE_SERVICE_ROLE_KEY` eksik hatası verdi — gereksiz admin client kullanımıydı, anon client + mevcut RLS policy yeterliydi
- [x] Çok ürünlü parselde Günlük kaydı her zaman `crops[0]`'a sabitleniyordu, ürün seçilemiyordu
- [x] 5 ayrı hardcoded crop listesi + 4 kopya emoji haritası vardı, tutarsız görünüyordu
- [x] Şehir alanları bazı yerlerde serbest metin, bazı yerlerde dropdown'dı — tutarsızdı
- [x] Parsel oluşturunca vitrinde otomatik ilan/kayıt oluşmuyordu — üç ayrı manuel adım gerekiyordu
- [x] Parsel/hasat kaydı sonrası Vitrin sayfası tazelenmiyordu (`farmerListings` invalidation eksikti)

### Araştırma & Planlama
- [x] Saffron fiyat/yield araştırması · Financial model v0.5 (4-kat indoor) · GTM planı · Vault yeniden yapılandırma
- [x] P16-H şema kararı: iki katmanlı model çözüldü (Tier 1: listing_harvest_entries + Tier 2: parcel_id)
- [x] Landing page tasarım auditı: bevel.health (UI) + hilberts.ai (içerik zenginliği) referans alındı
- [x] Lovable'a plan-mode audit yaptırıldı: crop/şehir tutarsızlıkları ve parsel→ilan→hasat bağlama boşluğu için

---

## 🔴 ŞİMDİ — Temmuz 2026

### Yasal & Altyapı
- [ ] Şahıs şirketi kur — noterden git (~₺2-3K, 1 gün)
- [ ] iyzico.com başvurusu yap (şirket belgesi gerekli, onay ~2 hafta)
- [ ] Alan adı araştır — [C] karşılaştır (isim "Hasat" ile sınırlı değil, tarafsız marka adı da düşünülüyor)
- [ ] Twilio: Verify service oluştur
- [ ] Twilio: webhook URL → Supabase Edge Function URL ile güncelle
- [ ] **WhatsApp Business başvurusu → hemen başla (2-4 hafta Meta review)**
  > 2026-07-06 notu: WhatsApp OTP kanalı test edildi, teslimat başarısız oldu. SMS kanalı çalışıyor. Bu başvuru tamamlanana kadar login ekranında SMS'i varsayılan yapmayı düşün.

### Düşük öncelikli cila (acil değil)
- [ ] `farmer.journal.new.tsx` içindeki "Önce bir parsel ekle" metni hâlâ tıklanamaz statik yazı. Kritik değil, iki başka çalışan yol var.
- [ ] `formatCrop()` gerçekte `crop_config.display_name`'i okumuyor, slug'ı title-case'e çeviren bir heuristik — şu anki tüm veri için doğru ama teorik uyuşmazlık riski var.
- [ ] `useDeleteParcel` sonrası draft ilan kartındaki "📍 parsel adı" etiketi sessizce kayboluyor — kozmetik.

---

## 🏗️ Lovable Build Sırası — TAMAMLANDI ✅

> ~~F~~ → ~~H-Ext~~ → ~~G~~ → ~~I~~ → ~~H~~ → ~~Bug Fix Batch 4~~ → ~~J~~ → ~~Bug Fix Batch 3~~ → ~~Crop/Şehir Konsolidasyonu~~ → ~~Draft-İlan Pipeline'ı~~ → ~~D~~
>
> **P16 serisinin tamamı bitti (2026-07-08).** Artık build sırasında bekleyen madde yok. Sıradaki gerçek öncelik: **Ağustos E2E test kapsamı** — GTM'den önce platformun uçtan uca çalıştığının doğrulanması.

---

### ✅ P16-D — ToS + Gizlilik *(Tamamlandı — 2026-07-08)*

**Uygulanan:**
- `/terms` — 10 bölüm: platform tanımı, %5 GMV komisyonu, çiftçi/alıcı sorumlulukları, ödeme/iade (48 saat itiraz penceresi), ihtilaf çözümü (mesajlaşma → destek arabuluculuğu → İstanbul mahkemeleri), hesap askıya alma, sorumluluk sınırı
- `/privacy` — 9 bölüm: toplanan veriler, telefon numarası kullanımı (pazarlama amaçlı paylaşılmadığı açıkça belirtilmiş), Supabase/Twilio üçüncü taraf açıklaması, veri saklama süreleri (aktif hesap boyunca, silme sonrası 30 gün, mali kayıtlar 10 yıl), KVKK/GDPR hakları, çerez politikası
- Landing page footer'ına ve login ekranına link eklendi
- Tasarım dili korundu (`--dark`, `--saffron`, `--gold`, serif başlıklar)
- tsgo temiz geçti

---

### ✅ Draft-İlan Otomasyon Pipeline'ı *(Tamamlandı — 2026-07-08)*

Kullanıcı gözlemi: parsel oluşturma, vitrine ürün ekleme ve hasat kaydını ilana bağlama üç ayrı, birbirinden kopuk manuel adımdı. Lovable'a plan-mode'da tam bir audit yaptırıldı (edge case'ler: çok ürünlü parsel, mevcut parsellerin backfill'i, parsel düzenlemede yeni ürün eklenmesi, parsel silme, RLS güvenliği) — sonra onaylı implementasyon gönderildi.

**Uygulanan:**
1. `listing_status` enum'una `draft` eklendi (var olanlar: active/sold/expired)
2. **Trigger — parsel INSERT:** Parselin `crops[]` dizisindeki **her ürün için ayrı** bir draft ilan otomatik oluşturuluyor (`parcel_id` bağlı, miktar=0, fiyat=`price_feed`'den son ortalama veya 0)
3. **Trigger — parsel UPDATE:** Sadece yeni eklenen ürünler için draft oluşturuyor, çıkarılan ürünlerin ilanlarına dokunmuyor
4. **Trigger — hasat kaydı INSERT:** Aynı `parcel_id` + `crop` eşleşen ilan(lar)a (draft veya active, hepsine) otomatik `listing_harvest_entries` bağlantısı kuruyor
5. **Backfill:** Mevcut tüm parseller ve hasat kayıtları için de geriye dönük draft/bağlantı oluşturuldu
6. `listings.parcel_id` FK → `ON DELETE SET NULL`
7. Vitrin: 3 sekme — **Ürünlerim / Taslak / Arşiv**. Taslak kartlarında "✓ Yayınla" butonu (miktar/fiyat 0 ise engelliyor)
8. Manuel "+ Yeni Ürün" akışı değişmedi
9. RLS zaten güvenliydi (doğrulandı, değişiklik gerekmedi)

**Karşılaşılan ve düzeltilen hata:** "Çok ürün eklerken sadece biri kaydediliyor" ve "otomatik ilan görünmüyor" şikayetleri geldi. DB incelemesi gösterdi ki veri katmanı doğru çalışıyordu — asıl sorun `useCreateParcel`/`useUpdateParcel`/`useCreateEntry`'nin `farmerListings`/`listingStock`/`listingBatchEntries` cache'ini tazelememesiydi. Düzeltildi, canlı doğrulandı.

---

### ✅ Crop/Şehir Konsolidasyonu *(Tamamlandı — 2026-07-08)*

**Bulunan:** 5 ayrı hardcoded crop listesi + 4 kopya emoji haritası, tutarsız şehir alanları.

**Uygulanan:**
- `src/lib/hasat/cities.ts` — `TR_PROVINCES` (81 il), tek kaynak
- `crop-config.ts` — `useCropOptions()` (canlı `crop_config`'ten okuyor), `cropEmoji()`, 10 kategori emoji haritası
- `CropChips.tsx` — paylaşılan, yükleme skeleton'lı chip seçici
- 5 hardcoded liste + 4 kopya emoji haritası kaldırılıp merkezi hale getirildi
- Settings profil şehir alanı → `TR_PROVINCES` dropdown
- Parsel "Şehir/İlçe" → İl dropdown + İlçe serbest metin

---

### ✅ Bug Fix Batch 3 *(Tamamlandı — 2026-07-08)*

1. B2 — `JournalEntryCard` quality default → `initial.quality ?? "A"`
2. B5 — crop slug gösterimi, merkezi `formatCrop()` 8 dosyaya uygulandı
3. Çok ürünlü parselde Günlük "Yeni Kayıt" ekranına ürün seçici eklendi

---

### ✅ P16-J (genişletilmiş) — Ana Tanıtım Landing Page + Indoor Farming Başvurusu *(Tamamlandı — 2026-07-07)*

`/` rotası komple yeniden yazıldı: Hero, Sorun, Çiftçiyim (6 kart), Alıcıyım (5 kart), Nasıl Çalışır (Ahmet/Zeynep hikayeleri), Hasat AI (3 kart + chat örneği), Güven, Indoor Farming formu (`#indoor-basvuru` anchor), Footer. Auth-aware redirect düzeltmesi (gerçek Supabase session kontrolü). Indoor form'daki `SUPABASE_SERVICE_ROLE_KEY` hatası anon client'a geçirilerek düzeltildi. Canlı doğrulandı: form DB'ye yazıyor, Twilio SMS bildirimi geliyor.

---

### ✅ Bug Fix Batch 4 — Onboarding Kritik (GTM BLOCKER) *(Tamamlandı — 2026-07-07)*

3 kritik bug düzeltildi: "Şimdilik atla" veri kaybı, crops/land hiç kaydedilmiyordu, parsel ekleme UI'ı yoktu. İki ayrı temiz hesapla (2026-07-07, 2026-07-08) doğrulandı.

---

### ✅ P16-F, P16-H Extension, P16-G, P16-I, P16-H, P16-E, P16-C

Detaylar önceki TODO sürümlerinde — hepsi tamamlandı ve test edildi. Özet: topluluk mesajlaşma, stok takibi + batch sayfası, referral programı, crop config (70 kayıt), izlenebilirlik/coverage sistemi, gerçek zamanlı fiyat verisi, IBAN ödeme köprüsü (tam e2e testi hâlâ bekliyor, aşağıda).

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
- [x] [C Web] Landing page E2E — anonim ziyaretçi/yeni kayıt/mevcut giriş senaryoları + indoor form
- [x] [C Web] Parsel → Draft İlan → Hasat Bağlama → Yayınla pipeline'ı E2E test edildi
- [ ] **Buyer onboarding E2E — 05398545295 ile — SIRADAKİ ADIM**
- [ ] Uçtan uca senaryo: Ahmet'in yayınladığı bir ürüne Zeynep teklif versin → pazarlık → IBAN ödeme → sipariş tamamlansın (bu P16-C'nin tam havale testini de kapatır)
- [ ] Kalan kapsam: sertifika, community+reply (buyer tarafı), AI chat, bildirimler, vitrin URL (buyer görünümü), referral, izlenebilirlik (buyer görünümü)

### Teknik Final
- [ ] hasat.lovable.app → custom domain yayını
- [x] ~~hasat.com.tr landing page~~ — P16-J ile birleştirilip tamamlandı, custom domain yayını hâlâ bekliyor
- [ ] iyzico canlı ödeme testi + tam entegrasyon
- [ ] P16-C tam havale e2e testi (pending_transfer → farmer confirm) — yukarıdaki senaryoyla kapanacak

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
| Onboarding test numaraları | Çiftçi: 05421241011 · Alıcı: 05398545295 (henüz test edilmedi) |
| Landing page tasarım referansları | bevel.health (UI) + hilberts.ai (persona-segmented içerik) |
| İletişim numarası | Berkin: +905421241011 — `BERKIN_NOTIFY_PHONE` + `HASAT_WHATSAPP_NUMBER` |
| İlan durumu (listing_status) | 4 değerli enum: `draft`, `active`, `sold`, `expired`. Draft'lar RLS ile buyer'lardan gizli. |
| Draft ilan otomasyonu | Parsel oluşturulunca/düzenlenince her ürün için otomatik draft; hasat kaydı → otomatik bağlanıyor; manuel "Yayınla" ile aktif ediliyor |
| Hukuki sayfalar | `/terms`, `/privacy` — %5 komisyon, ihtilaf çözümü İstanbul mahkemeleri, KVKK/GDPR hakları |

---

## 📋 Son Test Sonuçları

### P16-D — ToS + Gizlilik (2026-07-08) ✅
| Test | Sonuç |
|---|---|
| `/terms` ve `/privacy` route'ları | ✅ |
| Landing + login footer linkleri | ✅ |
| tsgo typecheck | ✅ |

### Draft-İlan Otomasyon Pipeline'ı (2026-07-08) ✅
| Test | Sonuç | Not |
|---|---|---|
| `listing_status` enum'una `draft` eklendi | ✅ | |
| Parsel oluşturunca her ürün için draft ilan | ✅ | Çok ürünlü parselde her ürün ayrı ilan alıyor |
| Hasat kaydı → otomatik bağlantı | ✅ | parcel_id + crop eşleşmesiyle |
| Backfill mevcut veriler için | ✅ | |
| RLS güvenliği | ✅ | Zaten güvenliydi |
| Vitrin 3 sekme + Yayınla akışı | ✅ | |
| Cache invalidation eksikti | ❌ → ✅ | Düzeltildi, tekrar test edildi |

### Crop/Şehir Konsolidasyonu (2026-07-08) ✅
### Bug Fix Batch 3 (2026-07-08) ✅
### P16-J (genişletilmiş) — Landing Page + Indoor Form (2026-07-07) ✅
### Bug Fix Batch 4 — Onboarding Kritik Düzeltmeler (2026-07-07, 2026-07-08 tekrar teyit) ✅
### P16-H, P16-I, P16-G, P16-H Extension, P16-F, P16-E (Temmuz 2026) ✅

### P16-C — IBAN Ödeme Köprüsü ✅ kısmi
| Test | Sonuç |
|---|---|
| Tam havale e2e (pending_transfer → farmer confirm) | ⏳ Ağustos E2E turunda kapanacak |
