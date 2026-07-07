---
title: Hasat — Master Roadmap & Build Log
updated: 2026-07-07
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

### P16 — Tamamlanan Promptlar
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

### Bug Fixes — Tamamlanan
- [x] B1: Journal tag rendering (`[key:value]` chips)
- [x] B4: Hasat Dönemi chip — dinamik ay (P16-I ile crop_config'den kalıcı çözüldü)
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

### Araştırma & Planlama
- [x] Saffron fiyat/yield araştırması · Financial model v0.5 (4-kat indoor) · GTM planı · Vault yeniden yapılandırma
- [x] P16-H şema kararı: iki katmanlı model çözüldü (Tier 1: listing_harvest_entries + Tier 2: parcel_id) — detay `Build/DB-Schema.md`
- [x] Landing page tasarım auditı: bevel.health (UI) + hilberts.ai (içerik zenginliği) referans alındı, Çiftçi/Alıcı ayrı persona anlatısı + AI özellik kanıtlama deseni bu iki siteden sentezlendi

---

## 🔴 ŞİMDİ — Temmuz 2026

### Yasal & Altyapı
- [ ] Şahıs şirketi kur — noterden git (~₺2-3K, 1 gün)
- [ ] iyzico.com başvurusu yap (şirket belgesi gerekli, onay ~2 hafta)
- [ ] Alan adı araştır: hasat.com.tr / hasat.app / hasat.co — [C] karşılaştır
- [ ] Twilio: Verify service oluştur
- [ ] Twilio: webhook URL → Supabase Edge Function URL ile güncelle
- [ ] **WhatsApp Business başvurusu → hemen başla (2-4 hafta Meta review)**
  > 2026-07-06 notu: WhatsApp OTP kanalı test edildi, teslimat başarısız oldu (Supabase→Twilio isteği 200 döndü ama SMS/WA hiç gelmedi). SMS kanalı çalışıyor. Kök neden Twilio/Meta tarafında — bu başvuru tamamlanana kadar login ekranında SMS'i varsayılan yapmayı düşün.

### Bug Fix Batch 3 (Lovable prompt)
- [ ] [C] B2: JournalEntryCard quality default → 'A'
- [ ] [C] B5: `safran_soğanı` slug → "Safran Soğanı"

> Not: B4 P16-I ile crop_config üzerinden kalıcı olarak çözüldü.

### Düşük öncelikli cila (acil değil)
- [ ] `farmer.journal.new.tsx` içindeki "Önce bir parsel ekle" metni hâlâ tıklanamaz statik yazı. Kritik değil çünkü Günlük ana sayfasındaki "+ Parsel" linki ve Settings'teki "+ Parsel Ekle" butonu zaten çalışıyor.

---

## 🏗️ Lovable Build Sırası

> **Doğru sıra:** ~~F~~ → ~~H-Ext~~ → ~~G~~ → ~~I~~ → ~~H~~ → ~~Bug Fix Batch 4~~ → ~~J~~ → **D**
> Sıradaki: **P16-D** (ToS + Gizlilik, en düşük öncelik) — aksi halde P16 build sırası tamamlandı, sırada Ağustos E2E testleri var (bkz. aşağıda)

---

### ✅ P16-J (genişletilmiş) — Ana Tanıtım Landing Page + Indoor Farming Başvurusu *(Tamamlandı — 2026-07-07)*

Orijinal P16-J kapsamı (indoor farming ilgi formu, ayrı `/indoor-basvuru` sayfası) genişletildi: hem "hasat.com.tr ana tanıtım sayfası" TODO maddesiyle hem de bu formla birleştirilip **`/` üzerindeki tam bir pazarlama landing page'i** olarak tek seferde inşa edildi. Karar süreci: bevel.health (UI) ve hilberts.ai (persona-segmented içerik) audit edilip, Çiftçi/Alıcı için ayrı, somut, ekran-görüntülü anlatım + AI özelliklerinin gerçek soru-cevap örnekleriyle kanıtlanması deseni bu ikisinden sentezlendi.

**Uygulanan (`/` rotası, `src/routes/index.tsx` komple yeniden yazıldı):**
1. Hero — "Tarladan sofraya, aracısız." + iki büyük CTA (Çiftçiyim/Alıcıyım), mevcut marka tokenleri (`--dark`, `--saffron`, `--gold`, `--hwhite`) korunarak
2. Sorun bölümü — Çiftçi/Alıcı için ayrı 3'er madde
3. Çiftçiyim bölümü — 6 özellik kartı (vitrin, günlük, izlenebilirlik, pazarlık, stok, referral)
4. Alıcıyım bölümü — 5 özellik kartı (keşfet, ürün geçmişi/sahtecilik karşıtı vurgu, izlenebilir rozet, teklif, sipariş takibi)
5. Nasıl Çalışır — Ahmet (Safranbolu çiftçi) ve Zeynep (İstanbul alıcı) için somut 4 adımlık yolculuk
6. Hasat AI — 3 kart, her biri gerçek WhatsApp-tarzı soru-cevap örneğiyle (fiyat uyarı, izlenebilirlik skoru, WhatsApp asistanı)
7. Güven bölümü — komisyon şeffaflığı, veri güvenliği, çiftçi doğrulaması
8. Indoor Farming bölümü — `#indoor-basvuru` anchor'lı, ayrı route değil; form + `indoor_interest_leads` tablosu + Twilio SMS bildirimi (Berkin: +905421241011)
9. Footer
10. **`/` yönlendirme düzeltmesi:** Eskiden sadece client-side Zustand cache'ine bakıyordu (kırılgan). Artık `login.tsx` ile aynı desende gerçek `supabase.auth.getSession()` + profil kontrolü yapıyor — anonim ziyaretçi, yeni kayıt ve mevcut girişli kullanıcı senaryolarının hepsi ayrı ayrı doğrulandı.

**Karşılaşılan ve düzeltilen hata:** İlk denemede indoor form submit'te `Missing Supabase environment variable(s): SUPABASE_SERVICE_ROLE_KEY` hatası alındı — server fonksiyonu gereksiz yere admin/service-role client kullanıyordu. Anon client + mevcut "Anyone can submit indoor interest" RLS policy'si zaten yeterliydi, düzeltilip anon client'a geçirildi.

**Canlı doğrulama (2026-07-07):** Test başvurusu ("Berkin Savcıözen / +905421241011 / Istanbul / danışmanlık") DB'ye yazıldı, Twilio SMS bildirimi doğru formatta ulaştı.

---

### ✅ Bug Fix Batch 4 — Onboarding Kritik (GTM BLOCKER) *(Tamamlandı — 2026-07-07)*

2026-07-06 E2E testinde (05421241011 ile) bulunan 3 kritik bug, tek promptta düzeltildi ve **canlı olarak yeniden test edilip doğrulandı.**

**Uygulanan:**
1. `onboarding.farmer.tsx` → `finish()` artık "Şimdilik atla" durumunda bile step 2'de girilen `name`/`city`'yi koruyor; sadece gerçekten boşsa "Çiftçi" fallback'i devreye giriyor.
2. Onboarding'de toplanan crops + land size artık gerçekten kaydediliyor — `finish()` otomatik olarak `farms` + `parcels` satırı oluşturuyor (`is_primary=true`). `parcels.is_primary` kolonu eklendi.
3. `farmer.settings.tsx` → Parsellerim bölümüne çalışan bir **"+ Parsel Ekle"** butonu ve formu (isim, şehir, dönüm, ürünler, fotoğraf) eklendi, `useCreateParcel`'a bağlı.
4. tsgo temiz geçti.

**Canlı doğrulama (2026-07-07):**
- Settings → "+ Parsel Ekle" ile "Settings Parsel" oluşturuldu, DB'de doğrulandı.
- Günlük ana sayfası → "+ Parsel" linkiyle "Parsel from Günlük" oluşturuldu, DB'de doğrulandı.
- Her iki yol da `farms`/`parcels` tablosuna doğru şekilde yazıyor.

**Kalan küçük not:** `farmer.journal.new.tsx`'teki "Önce bir parsel ekle" metni hâlâ tıklanamaz statik yazı (bu promptun kapsamında değildi). Kritik değil — yeni bir çiftçi bu ekrana gelmeden önce Günlük ana sayfasındaki çalışan "+ Parsel" linkini görüyor. **GTM blocker olarak kapatıldı.**

---

### ✅ P16-F — Topluluk Mesajlaşma *(Tamamlandı — 2026-07-03)*

Implement edildi ve kod incelemesi + canlı DB doğrulamasıyla test edildi.

**Küçük bulgular:**
- `community_posts.comments_count` kolonu DB'de mevcut ama hiç güncellenmez (ölü kolon). UI bu kolonu kullanmıyor, `replyCount`'u ayrı count sorgusuyla hesaplıyor — kullanıcıya görünen bir bug yok. Acil değil.
- Optimistic UI yok — mutation sonrası `invalidateQueries` ile refetch yapılıyor. Küçük gecikme var, kritik değil.
- Test hesabı notu: DB'de Ahmet `905001234567` olarak kayıtlı (login akışı `90` prefix'ini otomatik ekliyor).

---

### ✅ P16-H Extension — Stok Takibi + Batch Sayfası *(Tamamlandı — 2026-07-03)*

**Uygulanan:**
- `listing_harvest_entries` tablosu + RLS, `listings.parcel_id` kolonu
- `useListingStock` hook — bağlı hasat kaydı yoksa `listing.quantity`'e fallback
- Vitrin + Keşfet'te stok/"Tükendi" badge'i, teklif sayfasında disable
- Server-side stock guard trigger (`trg_enforce_offer_stock`) — **düzeltme uygulandı:** orijinal promptta `offers.status`'ün `'pending_payment'` değeri olduğu varsayılmıştı ama DB'de böyle bir değer yok; gerçek "ödeme bekleniyor" durumu `status='accepted' AND payment_status IN ('unpaid','pending_transfer')`. Trigger, `OLD.status IS DISTINCT FROM 'accepted' AND NEW.status='accepted'` geçişinde ateşlenecek şekilde düzeltilerek implement edildi.
- Batch sayfası `/batch/$listingId` (planlanan `/farmer/batch/...` yerine root-level — farmer ve buyer aynı route'u `isOwner` kontrolüyle paylaşıyor)
- Günlük'te "Bu hasatı bir ürüne bağla" bölümü
- Buyer view'da notlar gizli, costs hiç render edilmiyor

---

### ✅ P16-G — Çiftçi Referral Programı UI *(Tamamlandı — 2026-07-03)*

**Uygulanan:**
- `profiles.referral_code` (unique) + `referred_by`, mevcut kayıtlara backfill
- `handle_new_user` trigger'ı her yeni kayıtta otomatik kod üretecek şekilde güncellendi
- `/farmer/referral` sayfası: kod gösterimi, Kopyala / Linki Paylaş / WhatsApp CTA, davet edilen çiftçiler listesi
- Sidebar'a "Arkadaşını Davet Et" (masaüstü + mobil)
- `/join?ref={code}` public route — kodu DB'de doğruluyor
- OTP kayıt sonrası `applyStoredReferral` hook'u ile `referred_by` yazılıyor

---

### ✅ P16-I — Crop Config & Production Method *(Tamamlandı — 2026-07-06)*

**Uygulanan:**
- `crop_config` tablosu — DB'deki gerçek 7 crop için satır (safran, safran_soğanı, lavanta, fındık, zeytin, zeytinyağı, tıbbi bitkiler), sadece plandaki 2 değil
- `parcels.production_method`, `price_feed.source_type` kolonları
- B4 fix: `SeasonBanner` artık `crop_config`'den dinamik ay okuyor
- B19 fix: Keşfet kategori sayacı `category_group`'a göre gruplanıyor
- AI kutuları crop-agnostic: in-app chat, `ai-box-insights`, `whatsapp-ai-webhook` dinamik `crop_config` context'i kullanıyor
- Bonus: önceden var olan `/buyer/orders*` ve `/login` search-param tsgo hataları da bu turda temizlendi

---

### ✅ P16-H — Ürün Yaşam Döngüsü / İzlenebilirlik *(Tamamlandı — 2026-07-06)*

**Uygulanan:**
- `computeCoverage` — sezon penceresi son replant/dikim'den itibaren, gerekli adım/toplam gerekli adım oranı
- Tier'ler: Belgeleniyor / Temel / İyi Belgelenmiş / Tam İzlenebilir
- `CoverageBadge` — keşfet, vitrin, public `/s/$slug`
- `ProvenanceTimeline` — buyer "Ürün Geçmişi", teklif sayfası + batch sayfası; cost/not redaksiyonu (sadece tarih/miktar/kalite/foto)
- "İlk Sezon" rozeti (replant kaydı yoksa)
- Farmer "Alıcı bunu görecek" önizlemesi (`BuyerPreview`) — eksik adımlar öneri, blocker değil
- Anti-fraud: `harvest_entries.step_key` + `trg_enforce_harvest_date_lock` trigger (aktif/satılmış ilana bağlı hasadın tarihi kilitleniyor)

**Not:** Bu prompt `plan_mode=true` ile gönderilmesine rağmen Lovable direkt build etti (plan sunmadı) — entegrasyon davranışı beklenenden farklıydı, ama sonuç kod incelemesi + DB doğrulamasıyla doğru çıktı.
**Küçük gözlem:** Journal'da yeni hasat kayıtları artık `step_key` ile otomatik etiketleniyor (`WORK_TO_STEP_KEY`), ama P16-H öncesi eski kayıtlarda `step_key = null` — bu eski kayıtlar coverage hesabına girmeyebilir. Kritik değil, ileride backfill düşünülebilir.

---

### ⏭ P16-D — ToS + Gizlilik *(En düşük öncelik — deploy sonrası)*

```
Add Terms of Service and Privacy Policy pages:
- Routes: /terms and /privacy (public, no auth)
- Simple text pages in Turkish, footer links on login + landing
- ToS: %5 GMV komisyon, farmer/buyer sorumlulukları, ihtilaf çözümü
- Privacy: telefon numarası kullanımı, Supabase depolama, Twilio SMS/WhatsApp
```

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

## 🟡 AĞUSTOS 2026 — Soft Launch

### E2E Test (P16 tümü bittikten sonra) — devam ediyor
- [x] [C Web] Farmer onboarding E2E — 05421241011 ile tamamlandı, 3 kritik bug bulundu ve düzeltildi (Bug Fix Batch 4)
- [x] [C Web] Landing page E2E — anonim ziyaretçi/yeni kayıt/mevcut giriş senaryoları + indoor form uçtan uca test edildi
- [ ] Buyer onboarding E2E — 05398545295 ile (henüz yapılmadı)
- [ ] Kalan kapsam: parsel/sertifika, listing, teklif/müzakere, stok takibi, IBAN ödeme, community+reply, AI chat, bildirimler, vitrin URL, referral, izlenebilirlik

### Teknik Final
- [ ] hasat.lovable.app → custom domain yayını
- [x] ~~hasat.com.tr landing page (tek sayfa, basit) → Lovable~~ — P16-J ile birleştirilip tamamlandı, custom domain yayını hâlâ bekliyor
- [ ] iyzico canlı ödeme testi + tam entegrasyon

### İlk Kullanıcılar
- [ ] İlk 3 farmer beyaz eldiven onboarding (telefonda birlikte)
- [ ] İlk 3 buyer outreach (İstanbul organik restoran/mağaza)
- [ ] İlk 5 farmer listesi: gerçek ürün, fiyat, fotoğraflı
- [ ] 🚀 **İlk gerçek işlem** — duyur (Instagram Reel)

---

## 🔵 SAFRAN SEZONU — Ekim–Kasım 2026

> Hasat sezonu: Ekim–Kasım. Yılın en kritik penceresi.

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
  Not: Safran/lavanta için otomatik kaynak yok — manuel küratörlük kalıcı çözüm olabilir

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
6. AI prompt'ları crop-agnostic yaz — `crop` + `crop_config` parametre olarak geç; statik metin yok
7. Offer/durum alanlarında gerçek enum değerlerini prompt yazmadan önce DB'den doğrula — plandaki varsayılan durum adı DB'deki gerçek değerle uyuşmayabilir (bkz. P16-H Extension `pending_payment` düzeltmesi)
8. `plan_mode=true` her zaman güvenli bir "sadece planla" garantisi vermeyebilir — sonucu (commit sha değişti mi) kontrol et
9. Yeni bir akış (onboarding, form vb.) yazdırırken "veri buraya kaydediliyor mu" ve "bu veriyi sonra düzenleyebileceğim bir UI var mı" sorularını ayrıca sor — sessiz boşluklar Lovable'ın kendi taahhüt ettiği kapsamda bile atlanabiliyor
10. Bir bug fix promptu birden fazla dosyayı/ekranı etkiliyorsa, fix sonrası **her ekranı ayrı ayrı** canlı test et
11. Server-side fonksiyon yazdırırken admin/service-role client'a gerçekten ihtiyaç olup olmadığını sorgula — RLS zaten public erişime izin veriyorsa anon client yeterli olur, aksi halde `SUPABASE_SERVICE_ROLE_KEY` gibi tanımlı olmayan secret'lara bağımlılık yaratıp çalışma zamanında patlar (bkz. indoor form düzeltmesi)

---

## 📌 Kararlar

| Konu | Karar |
|---|---|
| Şirket türü | Şahıs (hızlı, 1 gün) |
| Ödeme altyapısı | iyzico (onay ~2 hafta; bu sürede IBAN köprüsü) |
| İlk 3 ay monetizasyon | Ücretsiz |
| Ekim 2026 monetizasyon | %5 take rate açılışı |
| Sosyal medya | Berkin yapıyor, Claude içerik desteği |
| Alan adı | Belirsiz — araştırılacak |
| Hasat Deneyimi / Korm Marketplace | Phase 2 |
| İzlenebilirlik birimi | İki katmanlı: Tier 1 (listing_harvest_entries) + Tier 2 (parcel_id + parsel-sezon penceresi) — detay `Build/DB-Schema.md` |
| Listing = tek parsel | Blend yok — 2 parselden satış = 2 ayrı listing |
| Sezon başlangıcı anchor | Son dikim-tipi entry — takvim yılı değil |
| Yetersiz kayıt satışı bloke eder mi | Hayır — sadece coverage rozeti etkilenir |
| Stok kilidi | İlk `status='accepted'` geçişi kilitler; diğer aktif müzakereler bildirim alır |
| Journal audit trail | details jsonb Phase 2; sertifikasyon-grade generator Phase 2 |
| Otomatik fiyat çekimi | Phase 2+ — MVP'de manuel küratörlük yeterli |
| AI kutuları | crop-agnostic — P16-I ile düzeltildi |
| Claude Web araçları | Lovable MCP (send_message, read_file, get_project) + Supabase MCP (execute_sql, project `efuqpiaavrzimvstpdpm`) + GitHub MCP (get_file_contents — READ ONLY, yazma 403 veriyor) |
| Vault sync | `hasat-vault` reposu **bilinçli olarak PUBLIC** tutuluyor (2026-07-06 kararı) — GitHub MCP connector'ı private repo scope'una erişemediği için. Web Claude güncel TODO içeriğini hazırlar, kullanıcı manuel yapıştırır. |
| Onboarding test numaraları | Çiftçi: 05421241011 (auth id `69ca6368-bfb7-48fd-829e-79ddc9edbf75`, 2 parseli var) · Alıcı: 05398545295 (henüz test edilmedi) — WhatsApp OTP çalışmıyor, SMS kullan |
| Landing page tasarım referansları | bevel.health (UI/visual storytelling) + hilberts.ai (persona-segmented içerik derinliği) — `Build/` altında audit notu tutulabilir |
| İletişim numarası | Berkin: +905421241011 — hem Twilio SMS bildirim hedefi (`BERKIN_NOTIFY_PHONE` secret) hem `HASAT_WHATSAPP_NUMBER` sabiti olarak kullanılıyor |

---

## 📋 Son Test Sonuçları

### P16-J (genişletilmiş) — Landing Page + Indoor Form (2026-07-07) ✅
| Test | Sonuç | Not |
|---|---|---|
| `/` sayfası tüm bölümlerle render oluyor | ✅ | Hero, sorun, çiftçi/alıcı feature grid, nasıl çalışır, AI, güven, indoor, footer |
| Auth-aware redirect — anonim ziyaretçi | ✅ | Landing page görünüyor, redirect yok |
| Auth-aware redirect — mevcut girişli kullanıcı | ✅ | Gerçek Supabase session kontrolüyle doğru dashboard'a yönleniyor |
| Auth-aware redirect — yeni kayıt | ✅ | Onboarding zaten `/`'a hiç uğramıyor, regresyon yok |
| Indoor form submit (ilk deneme) | ❌ → ✅ | `SUPABASE_SERVICE_ROLE_KEY` eksik hatası — anon client'a geçirilerek düzeltildi |
| Indoor form → DB kaydı | ✅ | Canlı doğrulandı (Berkin Savcıözen / Istanbul / danışmanlık) |
| Indoor form → Twilio SMS bildirimi | ✅ | Doğru formatta ulaştı |
| tsgo typecheck | ✅ | Temiz geçti (iki ayrı turda) |

### Bug Fix Batch 4 — Onboarding Kritik Düzeltmeler (2026-07-07) ✅
| Test | Sonuç | Not |
|---|---|---|
| "Şimdilik atla" artık name/city'yi silmiyor | ✅ | Kod incelemesiyle doğrulandı |
| Onboarding'de crops/land → otomatik ilk parsel oluşturuyor | ✅ | `parcels.is_primary` kolonu eklendi |
| Settings → "+ Parsel Ekle" | ✅ | Canlı test: "Settings Parsel" oluşturuldu, DB'de doğrulandı |
| Günlük ana sayfası → "+ Parsel" | ✅ | Canlı test: "Parsel from Günlük" oluşturuldu, DB'de doğrulandı |
| tsgo typecheck | ✅ | Temiz geçti |

**Sonuç: GTM blocker kapatıldı.** Yeni bir çiftçi artık onboarding'i tamamlayıp, Settings veya Günlük'ten parsel ekleyip Günlük'e kayıt atabiliyor.

### Farmer Onboarding E2E — 05421241011 (2026-07-06) ⚠️ 3 kritik bug bulundu → 2026-07-07'de düzeltildi
| Test | Sonuç | Not |
|---|---|---|
| WhatsApp OTP gönderimi | ❌ | Supabase→Twilio 200 döndü, mesaj gelmedi. Kök neden Twilio/WhatsApp tarafında, araştırılmadı |
| SMS OTP gönderimi + doğrulama | ✅ | Çalıştı |
| Yeni profil oluşturma (`handle_new_user` trigger) | ✅ | `role`, `referral_code` doğru atanıyor |
| Onboarding step 2/3 + crops/land + parsel akışı | ✅ | Bug Fix Batch 4 ile düzeltildi |

### P16-H — Ürün Yaşam Döngüsü / İzlenebilirlik (2026-07-06) ✅
| Test | Sonuç | Not |
|---|---|---|
| Coverage score hesaplama | ✅ | Sezon penceresi + gerekli adım oranı doğru |
| Tier'ler (Belgeleniyor/Temel/İyi/Tam) | ✅ | |
| CoverageBadge — keşfet/vitrin/public vitrin | ✅ | Üç yerde de |
| Buyer "Ürün Geçmişi" timeline | ✅ | Teklif sayfası + batch sayfası |
| Cost/not redaksiyonu | ✅ | Sadece tarih/miktar/kalite/foto görünüyor |
| Anti-fraud tarih kilidi | ✅ | DB trigger + UI'da kayıt/olay tarihi farkı gösterimi |

### P16-I — Crop Config & Production Method (2026-07-06) ✅
| Test | Sonuç | Not |
|---|---|---|
| `crop_config` tablo + RLS | ✅ | 7 crop için satır |
| `parcels.production_method`, `price_feed.source_type` | ✅ | |
| AI kutuları crop-agnostic | ✅ | |

### P16-G — Çiftçi Referral Programı UI (2026-07-03) ✅
| Test | Sonuç |
|---|---|
| `referral_code` + `referred_by` kolonları | ✅ |
| `/farmer/referral` sayfası | ✅ |
| `/join?ref={code}` | ✅ |

### P16-H Extension — Stok Takibi + Batch Sayfası (2026-07-03) ✅
| Test | Sonuç |
|---|---|
| `listing_harvest_entries` tablo + RLS | ✅ |
| Server-side stock guard trigger | ✅ |
| Batch sayfası | ✅ |

### P16-F — Topluluk Reply Threads + Farmer Profil Linkleri (2026-07-03) ✅
| Test | Sonuç | Not |
|---|---|---|
| "{n} yorum" badge | ✅ | |
| Yanıtla composer → DB kaydı | ⚠️ Kısmi | Optimistic UI yok, kritik değil |

### P16-E — Gerçek Zamanlı Fiyat Verisi (2026-07-02) ✅
### P16-C — IBAN Ödeme Köprüsü (2026-07-02) ✅ kısmi
| Test | Sonuç |
|---|---|
| Tam havale e2e (pending_transfer → farmer confirm) | ⏳ Yeni offer gerekiyor (eski offer orphan) |
