---
title: Hasat — Master Roadmap & Build Log
updated: 2026-07-02
tags:
  - hasat
  - todo
  - roadmap
  - build
---

# Hasat — Master Roadmap & Build Log
> GTM Hedefi: **25 Ağustos 2026** · Günlük 1-2 saat · `[C]` = Claude ile · `[C Chrome]` = Chrome test

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

### Bug Fixes — Tamamlanan
- [x] B1: Journal tag rendering (`[key:value]` chips)
- [x] B4: Hasat Dönemi chip — dinamik ay
- [x] B6: Sertifika expiry badge (Süresi Geçti / Yakında Sona Eriyor)
- [x] B11: Safran birim → ₺18.400/kg+
- [x] B12: Farmer panelinde alıcı adı → Zeynep Kaya
- [x] B13: Analytics empty state → gerçek veri
- [x] B15: Teslim tarihi → dd.MM.yyyy (shadcn Calendar)
- [x] B16: Sipariş kartında telefon → maskeli
- [x] B18: Abonelikler placeholder → "Keşfet'e Git" CTA
- [x] B19: Keşfet kategoriler sayacı → düzeltildi
- [x] ListingSheet description prefill + save
- [x] Stepper min_order clamping
- [x] Buyer Tamamlanan tab filter + Accept → orders satırı otomatik oluşturma

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

### Bug Fix Batch 3 (Lovable prompt)
- [ ] [C] B2: JournalEntryCard quality default → 'A'
- [ ] [C] B5: `safran_soğanı` slug → "Safran Soğanı"

> Not: B4 P16-I ile crop_config üzerinden kalıcı olarak çözülecek.

---

## 🏗️ Lovable Build Sırası

> **Doğru sıra:** F → H-Ext → G → H → I → J → D

---

### 🔄 P16-F — Topluluk Mesajlaşma *(ŞU AN IMPLEMENT EDİLİYOR)*

```
Implement P16-F: Community reply threads + farmer profile links.

1. DB Migration:
   ALTER TABLE public.community_posts
     ADD COLUMN IF NOT EXISTS parent_id uuid
       REFERENCES public.community_posts(id) ON DELETE CASCADE;
   CREATE INDEX IF NOT EXISTS idx_community_posts_parent
     ON public.community_posts(parent_id) WHERE parent_id IS NOT NULL;
   Existing RLS covers replies — no new policy needed.

2. Data layer (src/lib/hasat/queries.ts):
   - Extend Post type: parentId: string | null, replyCount: number
   - useCommunityPosts: add .is("parent_id", null) filter; fetch reply counts
     in second query (SELECT parent_id WHERE parent_id IN rootIds), aggregate client-side
   - Add useReplies(postId) — fetch WHERE parent_id = postId, ORDER BY created_at ASC
   - Add useCreateReply() — INSERT { content, author_id, parent_id }
     then best-effort INSERT into notifications:
     { user_id: root_author_id, type: 'reply', related_id: new_reply.id,
       message: '{name} gönderine yanıt verdi' } — swallow errors silently
     Optimistic update appends to ["communityReplies", postId] cache

3. UI (src/routes/farmer.community.tsx):
   - Track expandedId: string | null; clicking a post card toggles
   - Badge: "{n} yorum" below post content (uses replyCount)
   - When expanded: ReplyThread block — compact rows (avatar initial + name + content + relative time)
   - Empty state: "Henüz yorum yok"
   - Composer: Input + "Yanıtla" button, always visible when expanded, clears on success
   - stopPropagation on like button to prevent toggle

4. Farmer profile links — buyer-side navigation:
   Everywhere a farmer name appears in the buyer panel, make it a link to /s/{farmer_slug}:
   - Buyer discover page (listing cards) · Community feed (post author) · Offer thread header · Buyer orders list
   slug = profiles.name lowercased, spaces→hyphens. Fallback: /s/{profile.id}
   Route already exists from P16-B.

5. tsgo typecheck.

Notes: Max depth 1 — no nested replies. Notification insert is best-effort.
```

---

### ⏭ P16-H Extension — Stok Takibi + Batch Sayfası *(P16-F biter bitmez — P16-G'den önce)*

> **Acil:** listing.quantity düşülmüyor — overselling riski. Safran sezonu öncesi çözülmeli.

```
Implement P16-H Extension: Inventory tracking + batch page.

1. DB:
   ALTER TABLE public.listings
     ADD COLUMN IF NOT EXISTS parcel_id uuid REFERENCES public.parcels(id);

   CREATE TABLE IF NOT EXISTS public.listing_harvest_entries (
     listing_id uuid REFERENCES public.listings(id) ON DELETE CASCADE,
     harvest_entry_id uuid REFERENCES public.harvest_entries(id) ON DELETE CASCADE,
     PRIMARY KEY (listing_id, harvest_entry_id)
   );
   GRANT SELECT ON public.listing_harvest_entries TO anon, authenticated;
   GRANT INSERT, DELETE ON public.listing_harvest_entries TO authenticated;
   ALTER TABLE public.listing_harvest_entries ENABLE ROW LEVEL SECURITY;
   CREATE POLICY "Farmers manage their batch links" ON public.listing_harvest_entries
     FOR ALL TO authenticated
     USING (EXISTS (SELECT 1 FROM listings l WHERE l.id = listing_id AND l.farmer_id = auth.uid()))
     WITH CHECK (EXISTS (SELECT 1 FROM listings l WHERE l.id = listing_id AND l.farmer_id = auth.uid()));
   CREATE POLICY "Public read batch links" ON public.listing_harvest_entries
     FOR SELECT TO anon, authenticated USING (true);

2. Available quantity hook useListingStock(listingId) — client-side computed:
   available_quantity = SUM(linked harvest_entries.quantity)
     MINUS SUM(offer.quantity WHERE offer.status IN ('pending_payment','active','delivered'))

3. UI — Vitrin + Teklif Ver:
   - Show available_quantity on farmer vitrin and buyer discover listing cards
   - If <= 0: "Tükendi" badge + disable "Teklif Ver" for buyers (farmer can still edit)

4. Server-side stock guard (BEFORE trigger on offers):
   On status → 'pending_payment': recompute available_quantity.
   If insufficient → RAISE EXCEPTION 'Stok yetersiz'.
   (Same pattern as P15 A2 accept guard.)
   Other active negotiations NOT auto-cancelled — they get notification on next action.

5. Batch page (new route /farmer/batch/$listingId):
   - Linked harvest_entries: chronological (date, quantity, quality, notes)
   - Calculated stock + related orders/sales for this listing
   - Entry from Günlük harvest_entry detail: "Bu hasatı bir ürüne bağla" → listing selector

6. Buyer-facing batch view (same route, filtered): costs + notes redacted.

7. tsgo typecheck.

Rule: stok reservation only on first pending_payment — negotiation does not reserve stock.
```

---

### ⏭ P16-G — Çiftçi Referral Programı UI

```
Implement P16-G: Farmer referral program UI.

1. DB:
   ALTER TABLE public.profiles
     ADD COLUMN IF NOT EXISTS referral_code text UNIQUE,
     ADD COLUMN IF NOT EXISTS referred_by uuid REFERENCES public.profiles(id);
   UPDATE public.profiles SET referral_code = UPPER(LEFT(id::text, 6)) WHERE referral_code IS NULL;
   Update handle_new_user trigger: set referral_code = UPPER(LEFT(NEW.id::text, 6)) on INSERT.

2. New route: /farmer/referral
   - Referral code prominent display
   - "Kopyala" button → clipboard toast
   - "Linki Paylaş" → copies https://hasat.lovable.app/join?ref={code}
   - WhatsApp share → wa.me pre-filled: "Hasat'ta ürünlerini D2C satmaya başla! Davet kodum: {code} — https://hasat.lovable.app/join?ref={code}"
   - Placeholder: "Davet ettiğin çiftçiler burada görünecek"
   - Add "Arkadaşını Davet Et" to farmer sidebar nav

3. /join?ref={code} route:
   - On load: store ref in localStorage key "hasat_ref"
   - After OTP registration: UPDATE profiles SET referred_by = (SELECT id FROM profiles WHERE referral_code = stored_code LIMIT 1)
   - Clear localStorage after write

4. tsgo typecheck.
```

---

### ⏭ P16-H — Ürün Yaşam Döngüsü / İzlenebilirlik

> Önkoşul: P16-H Extension (parcel_id + listing_harvest_entries) ve P16-I (crop_config) implement edilmiş olmalı.

```
Implement P16-H: Product lifecycle traceability — buyer-facing provenance.

Prerequisites: P16-H Extension and P16-I must already be implemented.

1. Coverage score (client-side):
   Season window = parcel + crop, from most recent replant/dikim entry onward (if none: earliest entry).
   lifecycle_steps from crop_config (required steps only for score denominator).
   coverage = steps with ≥1 matching entry / total required steps
   Tiers: Temel (<40%), İyi Belgelenmiş (40–79%), Tam İzlenebilir (≥80%)
   No crop_config match → "Belgeleniyor" placeholder.

2. Coverage badge on listing cards (buyer discover + public vitrin): informational, not a purchase gate.

3. Buyer UI — listing detail "Ürün Geçmişi" timeline:
   - harvest_entries for listing's parcel from season start (chronological)
   - Each: date, type label (from crop_config lifecycle labels), quantity if applicable, photo thumbnail
   - Redaction: costs (jsonb) + free-text notes MUST NOT appear in buyer view
   - 0 entries: "Bu ürün henüz belgelenmemiş" — neutral tone
   - No replant entry found: "İlk Sezon" badge — neutral framing, not negative

4. Farmer UI — pre-publish listing preview:
   "Alıcı bunu görecek" preview + missing lifecycle steps as suggestions (not blockers).

5. Anti-fraud — date locking:
   harvest_entry linked to active/delivered listing → lock entry_date field
   OR show both "kayıt tarihi" (created_at) and "olay tarihi" (entry_date) so buyer can detect backdating.

6. tsgo typecheck.
```

---

### ⏭ P16-I — Crop Config & Production Method

```
Implement P16-I: Crop configuration table + production method + crop-agnostic AI.

1. DB — crop_config:
   CREATE TABLE IF NOT EXISTS public.crop_config (
     crop text PRIMARY KEY,
     display_name text NOT NULL,
     default_unit text NOT NULL DEFAULT 'kg',
     harvest_window_start_month int,
     harvest_window_end_month int,
     lifecycle_steps jsonb,
     price_benchmark_source text,
     category_group text
   );
   GRANT SELECT ON public.crop_config TO anon, authenticated;
   ALTER TABLE public.crop_config ENABLE ROW LEVEL SECURITY;
   CREATE POLICY "Public read crop_config" ON public.crop_config FOR SELECT TO anon, authenticated USING (true);

   INSERT INTO public.crop_config VALUES
     ('safran','Safran','g',10,11,
      '[{"key":"replant","label":"Korm/Dikim","required":true},{"key":"care","label":"Bakım","required":false},{"key":"harvest","label":"Hasat","required":true},{"key":"drying","label":"Kurutma","required":true}]',
      'Manuel','baharat'),
     ('lavanta','Lavanta','kg',6,8,
      '[{"key":"pruning","label":"Budama","required":false},{"key":"care","label":"Bakım","required":false},{"key":"harvest","label":"Hasat","required":true},{"key":"distillation","label":"Distilasyon","required":false}]',
      'Manuel','tıbbi_bitki')
   ON CONFLICT DO NOTHING;

2. DB — parcels production_method:
   ALTER TABLE public.parcels
     ADD COLUMN IF NOT EXISTS production_method text
     CHECK (production_method IN ('indoor','outdoor')) DEFAULT 'outdoor';

3. DB — price_feed source_type:
   ALTER TABLE public.price_feed
     ADD COLUMN IF NOT EXISTS source_type text
     CHECK (source_type IN ('manual','api')) DEFAULT 'manual';

4. Fix B4 (Hasat Dönemi chip): read harvest_window_start/end_month from crop_config — no hardcoded months.
5. Fix B19 (Keşfet kategori sayacı): map listing crop to crop_config.category_group.

6. AI boxes audit — crop-agnostic:
   Find any AI prompt hardcoding 'safran', 'Ekim', 'Kasım', or season windows.
   Replace with dynamic values from crop_config (harvest_window, lifecycle_steps, display_name).
   LLM receives crop + crop_config as context.

7. tsgo typecheck.
```

---

### ⏭ P16-J — Indoor Farming İlgi/Ortaklık Landing Page

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

### E2E Test (P16 tümü bittikten sonra)
- [ ] [C Chrome] Tüm MVP özelliklerini uçtan uca test et
  Kapsam: OTP login, onboarding, parsel/sertifika, listing, teklif/müzakere, stok takibi, IBAN ödeme, community+reply, AI chat, bildirimler, vitrin URL, referral, izlenebilirlik

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
| Stok kilidi | İlk pending_payment kilitler; diğerleri bildirim alır |
| Journal audit trail | details jsonb Phase 2; sertifikasyon-grade generator Phase 2 |
| Otomatik fiyat çekimi | Phase 2+ — MVP'de manuel küratörlük yeterli |
| AI kutuları | crop-agnostic yapılacak (P16-I) |

---

## 📋 Son Test Sonuçları

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
