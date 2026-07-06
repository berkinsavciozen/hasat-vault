---
title: Hasat — Master Roadmap & Build Log
updated: 2026-07-06
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

### Araştırma & Planlama
- [x] Saffron fiyat/yield araştırması · Financial model v0.5 (4-kat indoor) · GTM planı · Vault yeniden yapılandırma
- [x] P16-H şema kararı: iki katmanlı model çözüldü (Tier 1: listing_harvest_entries + Tier 2: parcel_id) — detay `Build/DB-Schema.md`

---

## 🔴 ŞİMDİ — Temmuz 2026

### 🚨 Bug Fix Batch 4 — Onboarding Kritik (GTM BLOCKER) — 2026-07-06 E2E testinde bulundu

> Gerçek numaralarla (05421241011 çiftçi, 05398545295 alıcı henüz test edilmedi) uçtan uca onboarding testi yapıldı. Kod incelemesi + canlı DB ile doğrulanan 3 bug bulundu. **Bunlar düzeltilmeden hiçbir gerçek çiftçi onboarding'i tamamlayıp Günlük'e kayıt atamaz.** P16-J'den önce, kredi geldiğinde ilk sırada gönderilecek.

**Bulgular:**
1. `onboarding.farmer.tsx` → "Şimdilik atla" butonu sadece sertifika adımını (step 3) atlaması gerekirken, step 2'de girilen `name`/`city` bilgisini de siliyor, zorla `name="Çiftçi"`, `city=null` yazıyor (`finish(true)` fonksiyonu).
2. Onboarding'de toplanan "Ana Ürünlerin" (crops) ve "Arazi Büyüklüğü" (land dönüm) hiçbir DB kolonuna yazılmıyor — sadece client-side Zustand state'te tutuluyor, sayfa yenilenince/çıkışta kayboluyor. `profiles` tablosunda bu alanlar için kolon bile yok.
3. **En kritik:** Uygulamada hiçbir yerde "Parsel Ekle" UI'ı yok. Backend (`useCreateParcel` in `queries.ts`) tam çalışır durumda (farm oluşturma, foto upload dahil) ama onu çağıran hiçbir buton/form yok. Settings sadece mevcut parselleri düzenle/sil gösteriyor; Storefront ve Journal.new sayfaları "Ayarlar'dan ekleyin" diye yönlendiriyor ama orada da giriş noktası yok. **Yeni bir çiftçi ilk parselini asla oluşturamıyor, dolayısıyla Günlük'e hiç kayıt atamıyor.**

**Sıradaki Lovable promptu (kredi gelince ilk gönderilecek):**
```
Fix critical onboarding gaps found in E2E testing.

1. src/routes/onboarding.farmer.tsx — fix "Şimdilik atla" (skip) data loss:
   The finish(skip) function currently overwrites name/city with hardcoded
   "Çiftçi"/null even when skip=true, discarding whatever the user typed in
   step 2. "Şimdilik atla" should ONLY skip step 3 (certifications) — it must
   NOT touch name/city. Change so:
     const profileName = name.trim() || "Çiftçi"; // only fallback if truly empty
     city: city || null; // keep whatever was entered in step 2, regardless of skip
   Only certifications (step 3 specific fields) should be skipped when skip=true.

2. Persist crops + land size collected in onboarding step 2:
   ALTER TABLE public.parcels
     ADD COLUMN IF NOT EXISTS is_primary boolean DEFAULT false;
   -- (crops already exists as parcels.crops text[]; land size maps to parcels.area)
   In onboarding.farmer.tsx finish(): after upserting the profile, also create
   the farmer's first parcel via the same insert pattern as useCreateParcel
   (ensure a farms row exists, then insert into parcels with name derived from
   city or "Ana Parsel", area = land, crops = crops array, location_label = city).
   Skip this parcel creation if skip=true or crops/land were never touched.

3. Add a real "+ Parsel Ekle" entry point — this is the critical gap:
   In src/routes/farmer.settings.tsx, in the "Parsellerim" Section, add a
   "+ Parsel Ekle" button (same visual pattern as the existing "+ Sertifika Ekle"
   button right below it) that opens a Sheet with fields: Parsel Adı, Şehir/İlçe,
   Alan (dönüm), Ana Ürünler (multi-select from CROPS list used elsewhere),
   Fotoğraflar (PhotoUploader, max 5). On save, call useCreateParcel with these
   values. This must work for farmers with zero existing parcels (first-time use).

4. tsgo typecheck.
```

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

---

## 🏗️ Lovable Build Sırası

> **Doğru sıra:** ~~F~~ → ~~H-Ext~~ → ~~G~~ → ~~I~~ → ~~H~~ → **Bug Fix Batch 4 (onboarding)** → J → D
> Sıradaki: **Bug Fix Batch 4** (yukarıda, GTM blocker — kredi gelince ilk gönderilecek), sonra **P16-J**

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

### ⏭ P16-J — Indoor Farming İlgi/Ortaklık Landing Page *(Bug Fix Batch 4'ten sonra)*

```
Implement P16-J: Indoor farming interest capture page.

1. DB:
   CREATE TABLE IF NOT EXISTS public.indoor_interest_leads (
     id uuid DEFAULT gen_random_uuid() PRIMARY KEY,
     name text NOT NULL,
     phone text NOT NULL,
     city text,
     interest_type text CHECK (interest_type IN ('danışmanlık','ortaklık','diğer')),
     note text,
     created_at timestamptz DEFAULT now()
   );
   RLS: INSERT for anon + authenticated; SELECT for service_role only.

2. New public route: /indoor-basvuru (no auth, same pattern as /s/{slug})
   - Short pitch: indoor farming + kırsalda genç kalıcılığı + TKDK genç çiftçi bonusu değeri
   - Form: Ad, Telefon, Şehir, İlgi Tipi (danışmanlık/ortaklık/diğer), Not (optional)
   - Submit → INSERT into indoor_interest_leads → toast "Başvurunuz alındı"
   - WhatsApp CTA alongside form: wa.me pre-filled "Hasat indoor farming hakkında bilgi almak istiyorum"

3. On submit: Twilio notification to Berkin:
   "🌱 Yeni indoor başvuru: {name} / {phone} / {city} / {interest_type}"

4. tsgo typecheck.
```

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
- [x] [C Web] Farmer onboarding E2E — 05421241011 ile başlatıldı, 3 kritik bug bulundu (yukarıda Bug Fix Batch 4)
- [ ] Buyer onboarding E2E — 05398545295 ile (henüz yapılmadı, Lovable kredisi bekleniyor)
- [ ] Kalan kapsam: parsel/sertifika, listing, teklif/müzakere, stok takibi, IBAN ödeme, community+reply, AI chat, bildirimler, vitrin URL, referral, izlenebilirlik

### Teknik Final
- [ ] hasat.lovable.app → custom domain yayını
- [ ] [C] hasat.com.tr landing page (tek sayfa, basit) → Lovable
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
9. **Yeni bir akış (onboarding, form vb.) yazdırırken "veri buraya kaydediliyor mu" ve "bu veriyi sonra düzenleyebileceğim bir UI var mı" sorularını ayrıca sor** — P16-A/onboarding'de crops/land toplanıp hiç kaydedilmemiş, parsel oluşturmak için hiç UI olmaması gibi sessiz boşluklar Lovable'ın kendi taahhüt ettiği kapsamda bile atlanabiliyor

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
| Vault sync | `hasat-vault` reposu **bilinçli olarak PUBLIC** tutuluyor (2026-07-06 kararı) — GitHub MCP connector'ı private repo scope'una erişemediği için. İçerik kontrol edildi, hassas secret/kod yok. GitHub MCP bu repoyu okuyabiliyor ama YAZAMIYOR (403 Resource not accessible by integration) — Web Claude güncel TODO içeriğini hazırlar, kullanıcı manuel yapıştırır. Obsidian Git eklentisi bu vault'tan kaldırıldı (ayrı, kişisel bilgi içeren belgelerle karışma riski vardı). |
| Onboarding test numaraları | Çiftçi: 05421241011 (auth id `69ca6368-bfb7-48fd-829e-79ddc9edbf75`) · Alıcı: 05398545295 (henüz test edilmedi) — WhatsApp OTP çalışmıyor, SMS kullan |

---

## 📋 Son Test Sonuçları

### Farmer Onboarding E2E — 05421241011 (2026-07-06) ⚠️ 3 kritik bug bulundu
| Test | Sonuç | Not |
|---|---|---|
| WhatsApp OTP gönderimi | ❌ | Supabase→Twilio 200 döndü, mesaj gelmedi. Kök neden Twilio/WhatsApp tarafında, araştırılmadı |
| SMS OTP gönderimi + doğrulama | ✅ | Çalıştı (eski orphan auth kaydı temizlendikten sonra) |
| Yeni profil oluşturma (`handle_new_user` trigger) | ✅ | `role`, `referral_code` doğru atanıyor |
| Onboarding step 2 (isim/şehir/ürün/dönüm) | ⚠️ | Dolduruldu ama "Şimdilik atla" (step 3) basılınca hepsi siliniyor — **bug** |
| Onboarding step 3 "Şimdilik atla" | ❌ | `name`/`city` bilgisini de siliyor, sadece sertifikaları atlaması gerekirken — **bug, Batch 4'te** |
| Crops/land size DB'de kalıcı mı | ❌ | Hiçbir kolona yazılmıyor, sadece client state'te — **bug, Batch 4'te** |
| Günlük'e ilk kayıt atma (Hasat Ekle) | ❌ | Parsel yok, parsel ekleme UI'ı hiçbir yerde yok — **kritik bug, Batch 4'te** |

**DB doğrulaması:** `on_auth_user_created` trigger'ı sağlam ve aktif. `handle_new_user()` fonksiyonu referral_code çakışmasını doğru yönetiyor. `useCreateParcel` backend fonksiyonu tam çalışır durumda ama hiçbir UI'dan çağrılmıyor.

### P16-H — Ürün Yaşam Döngüsü / İzlenebilirlik (2026-07-06) ✅
| Test | Sonuç | Not |
|---|---|---|
| Coverage score hesaplama | ✅ | Sezon penceresi + gerekli adım oranı doğru |
| Tier'ler (Belgeleniyor/Temel/İyi/Tam) | ✅ | |
| CoverageBadge — keşfet/vitrin/public vitrin | ✅ | Üç yerde de |
| Buyer "Ürün Geçmişi" timeline | ✅ | Teklif sayfası + batch sayfası |
| Cost/not redaksiyonu | ✅ | Sadece tarih/miktar/kalite/foto görünüyor |
| "İlk Sezon" rozeti | ✅ | |
| Farmer "Alıcı bunu görecek" önizlemesi | ✅ | Eksik adımlar öneri, blocker değil |
| Anti-fraud tarih kilidi | ✅ | DB trigger + UI'da kayıt/olay tarihi farkı gösterimi |
| tsgo (+ önceki hatalar) | ✅ | `/buyer/orders*`, `/login` search-param hataları da temizlendi |

### P16-I — Crop Config & Production Method (2026-07-06) ✅
| Test | Sonuç | Not |
|---|---|---|
| `crop_config` tablo + RLS | ✅ | 7 crop için satır — DB'deki gerçek kullanımı kapsıyor |
| `parcels.production_method`, `price_feed.source_type` | ✅ | |
| B4 fix (dinamik hasat dönemi) | ✅ | |
| B19 fix (kategori sayacı) | ✅ | |
| AI kutuları crop-agnostic | ✅ | |

### P16-G — Çiftçi Referral Programı UI (2026-07-03) ✅
| Test | Sonuç | Not |
|---|---|---|
| `referral_code` + `referred_by` kolonları | ✅ | Canlı DB'de doğrulandı (Ahmet → `0868E4`) |
| Backfill mevcut kayıtlara | ✅ | |
| `handle_new_user` otomatik kod üretimi | ✅ | |
| `/farmer/referral` sayfası | ✅ | |
| Sidebar "Arkadaşını Davet Et" | ✅ | |
| `/join?ref={code}` | ✅ | |
| OTP kayıt sonrası `referred_by` yazımı | ✅ | |
| tsgo typecheck | ✅ | |

### P16-H Extension — Stok Takibi + Batch Sayfası (2026-07-03) ✅
| Test | Sonuç | Not |
|---|---|---|
| `listing_harvest_entries` tablo + RLS | ✅ | |
| `listings.parcel_id` kolonu | ✅ | |
| `useListingStock` hook | ✅ | |
| Vitrin + Keşfet stok/Tükendi badge | ✅ | |
| Teklif Ver disable | ✅ | |
| Server-side stock guard trigger | ✅ | `status='accepted'` geçişine düzeltilerek gönderildi |
| Batch sayfası | ✅ | `/batch/$listingId` — root-level, paylaşılan route |
| Günlük → "hasadı ürüne bağla" | ✅ | |
| Buyer view redaksiyonu | ✅ | |

### P16-F — Topluluk Reply Threads + Farmer Profil Linkleri (2026-07-03) ✅
| Test | Sonuç | Not |
|---|---|---|
| "{n} yorum" badge | ✅ | |
| Reply thread + empty state | ✅ | |
| Yanıtla composer → DB kaydı | ⚠️ Kısmi | Optimistic UI yok, refetch ile çalışıyor. Kritik değil |
| Buyer discover → /s/{slug} linki | ✅ | |
| Community feed → farmer linki | ✅ | |
| Offer thread header → farmer linki | ✅ | |
| Buyer orders → farmer linki | ✅ | |

### P16-E — Gerçek Zamanlı Fiyat Verisi (2026-07-02) ✅
| Test | Sonuç |
|---|---|
| Boş state "Fiyat verisi bekleniyor" | ✅ |
| "Fiyat Güncelle" farmer için görünür, buyer için gizli | ✅ |
| Dropdown: farmer listings + Diğer (yaz)... | ✅ |
| Submit → toast + kart / Sparkline ≥2 nokta | ✅ |
| AI nudge farmer/home + storefront: %25 üzerinde uyarısı | ✅ |

### P16-C — IBAN Ödeme Köprüsü (2026-07-02) ✅ kısmi
| Test | Sonuç |
|---|---|
| Farmer settings → Banka Bilgileri + kaydet | ✅ |
| Buyer pay page → Havale kartı + Manuel Onay badge | ✅ |
| Infinite spinner fix → error/retry state | ✅ |
| Tam havale e2e (pending_transfer → farmer confirm) | ⏳ Yeni offer gerekiyor (eski offer orphan) |
