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

### Araştırma & Planlama
- [x] Saffron fiyat/yield araştırması · Financial model v0.5 (4-kat indoor) · GTM planı · Vault yeniden yapılandırma
- [x] P16-H şema kararı: iki katmanlı model çözüldü (Tier 1: listing_harvest_entries + Tier 2: parcel_id) — detay `Build/DB-Schema.md`

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
- [ ] `farmer.journal.new.tsx` içindeki "Önce bir parsel ekle" metni hâlâ tıklanamaz statik yazı. Kritik değil çünkü Günlük ana sayfasındaki "+ Parsel" linki ve Settings'teki "+ Parsel Ekle" butonu zaten çalışıyor — yeni çiftçi bu ekrana varmadan önce parsel ekleyebiliyor. İstersen ileride bu metni de tıklanabilir/yönlendirici yapabiliriz.

---

## 🏗️ Lovable Build Sırası

> **Doğru sıra:** ~~F~~ → ~~H-Ext~~ → ~~G~~ → ~~I~~ → ~~H~~ → ~~Bug Fix Batch 4~~ → **J** → D
> Sıradaki: **P16-J** (Indoor Farming landing page)

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

**Kalan küçük not:** `farmer.journal.new.tsx`'teki "Önce bir parsel ekle" metni hâlâ tıklanamaz statik yazı (bu promptun kapsamında değildi). Ama artık kritik değil — çünkü yeni bir çiftçi bu ekrana gelmeden önce Günlük ana sayfasındaki çalışan "+ Parsel" linkini görüyor. **GTM blocker olarak kapatıldı**, ileride düşük öncelikli bir cila maddesi olarak kalabilir (yukarıda not edildi).

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

### ⏭ P16-J — Indoor Farming İlgi/Ortaklık Landing Page *(Sıradaki)*

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
- [x] [C Web] Farmer onboarding E2E — 05421241011 ile tamamlandı, 3 kritik bug bulundu ve düzeltildi (Bug Fix Batch 4)
- [ ] Buyer onboarding E2E — 05398545295 ile (henüz yapılmadı)
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
9. Yeni bir akış (onboarding, form vb.) yazdırırken "veri buraya kaydediliyor mu" ve "bu veriyi sonra düzenleyebileceğim bir UI var mı" sorularını ayrıca sor — sessiz boşluklar Lovable'ın kendi taahhüt ettiği kapsamda bile atlanabiliyor
10. Bir bug fix promptu birden fazla dosyayı/ekranı etkiliyorsa, fix sonrası **her ekranı ayrı ayrı** canlı test et — Bug Fix Batch 4'te Settings ve Günlük ana sayfası düzeldi ama Günlük'ün "Yeni Kayıt" alt ekranındaki ayrı bir dead-end kalmıştı (düşük öncelikli ama gözden kaçabilirdi)

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
| Vault sync | `hasat-vault` reposu **bilinçli olarak PUBLIC** tutuluyor (2026-07-06 kararı) — GitHub MCP connector'ı private repo scope'una erişemediği için. İçerik kontrol edildi, hassas secret/kod yok. GitHub MCP bu repoyu okuyabiliyor ama YAZAMIYOR (403 Resource not accessible by integration) — Web Claude güncel TODO içeriğini hazırlar, kullanıcı manuel yapıştırır. Obsidian vault tamamen kaldırıldı — `hasat-vault` reposu artık tek doğruluk kaynağı. |
| Onboarding test numaraları | Çiftçi: 05421241011 (auth id `69ca6368-bfb7-48fd-829e-79ddc9edbf75`, 2 parseli var) · Alıcı: 05398545295 (henüz test edilmedi) — WhatsApp OTP çalışmıyor, SMS kullan |

---

## 📋 Son Test Sonuçları

### Bug Fix Batch 4 — Onboarding Kritik Düzeltmeler (2026-07-07) ✅
| Test | Sonuç | Not |
|---|---|---|
| "Şimdilik atla" artık name/city'yi silmiyor | ✅ | Kod incelemesiyle doğrulandı |
| Onboarding'de crops/land → otomatik ilk parsel oluşturuyor | ✅ | `parcels.is_primary` kolonu eklendi |
| Settings → "+ Parsel Ekle" | ✅ | Canlı test: "Settings Parsel" oluşturuldu, DB'de doğrulandı |
| Günlük ana sayfası → "+ Parsel" | ✅ | Canlı test: "Parsel from Günlük" oluşturuldu, DB'de doğrulandı |
| Günlük "Yeni Kayıt" ekranındaki parsel alanı | ⚠️ Düşük öncelik | "Önce bir parsel ekle" hâlâ tıklanamaz statik metin — ama artık kritik değil, kullanıcı oraya varmadan önce zaten parsel ekleyebiliyor |
| tsgo typecheck | ✅ | Temiz geçti |

**Sonuç: GTM blocker kapatıldı.** Yeni bir çiftçi artık onboarding'i tamamlayıp (skip etse bile isim/şehir kaybolmadan), Settings veya Günlük'ten parsel ekleyip Günlük'e kayıt atabiliyor.

### Farmer Onboarding E2E — 05421241011 (2026-07-06) ⚠️ 3 kritik bug bulundu → 2026-07-07'de düzeltildi
| Test | Sonuç | Not |
|---|---|---|
| WhatsApp OTP gönderimi | ❌ | Supabase→Twilio 200 döndü, mesaj gelmedi. Kök neden Twilio/WhatsApp tarafında, araştırılmadı |
| SMS OTP gönderimi + doğrulama | ✅ | Çalıştı (eski orphan auth kaydı temizlendikten sonra) |
| Yeni profil oluşturma (`handle_new_user` trigger) | ✅ | `role`, `referral_code` doğru atanıyor |
| Onboarding step 2 (isim/şehir/ürün/dönüm) | ✅ | Bug Fix Batch 4 ile düzeltildi |
| Onboarding step 3 "Şimdilik atla" | ✅ | Bug Fix Batch 4 ile düzeltildi |
| Crops/land size DB'de kalıcı mı | ✅ | Bug Fix Batch 4 ile düzeltildi |
| Günlük'e ilk kayıt atma (Hasat Ekle) | ✅ | Bug Fix Batch 4 ile düzeltildi |

**DB doğrulaması:** `on_auth_user_created` trigger'ı sağlam ve aktif. `handle_new_user()` fonksiyonu referral_code çakışmasını doğru yönetiyor.

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
