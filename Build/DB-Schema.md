---
title: Hasat — DB Schema Referansı
updated: 2026-07-29
tags: [hasat, supabase, schema]
---

# Hasat Supabase DB Schema
> Lovable prompt yazarken veya SQL çalıştırırken buraya bak. Supabase: `efuqpiaavrzimvstpdpm`

## Enum Types
| Enum | Değerler |
|---|---|
| `unit_type` | `g`, `kg`, `L` |
| `quality_grade` | `A`, `B`, `C` |
| `company_type` | `restoran`, `otel`, `organik_market`, `ihracatci`, `diger` |
| `certification_type` | `organik`, `iso`, `cografi`, `hasat`, `premium`, `yeni` |
| `notif_channel` | `sms` — cast zorunlu: `ARRAY['sms']::notif_channel[]` |

## Tablolar (17 adet)

### `profiles`
`id` uuid (FK→auth.users) · `role` text · `name` text · `phone` text · `city` text · `premium` bool · `tier` text  
⚠️ Phone format: `905XXXXXXXXX` (+ prefix'siz) · UNIQUE constraint on phone (P15 sonrası eklendi)

### `buyer_profiles`
`id` uuid · `user_id` uuid (FK→auth.users) · `company_name` (nullable, P23-M1-a) · `company_type` enum · `monthly_volume` numeric  
⚠️ user_id'de UNIQUE constraint yok — ON CONFLICT çalışmaz, DELETE+INSERT yap
⚠️ `company_name` **P23-M1-a'dan itibaren nullable** — bireysel (ev kullanıcısı) segment şirket adı girmeden onboarding'i tamamlayabilir. Mevcut satırlara dokunulmadı.

### `buyer_addresses` (P23-M1-a'da dokümante edildi)
`id` uuid · `buyer_id` uuid (FK→auth.users) · `label` text · `address` text · `city` text · `is_default` bool DEFAULT false · `created_at` / `updated_at`  
RLS: `buyer_addresses_owner_all` — tek `ALL` policy, `auth.uid() = buyer_id` (SELECT/INSERT/UPDATE/DELETE hepsi kapsıyor, orders'taki UPDATE eksikliği burada yok)  
⚠️ **`trg_enforce_single_default_address`** (BEFORE INSERT OR UPDATE, P23-M1-a): bir satır `is_default=true` yapıldığında aynı `buyer_id`'nin diğer adresleri otomatik `false`'a çekilir — hem web hem mobil aynı DB mantığını kullanır (Kural #106).

### `farms` / `parcels`
`parcels.farm_id` NOT NULL → önce `farms` insert et  
`parcels.location_label` (location değil) · crops: `text[]`

### `harvest_entries` (= journal)
`parcel_id` · `farmer_id` · `harvest_date` date · `crop` text · `quantity` numeric · `unit` unit_type · `quality` quality_grade · `photo_urls` text[] · `notes` text · `costs` jsonb  
⚠️ Tablo adı `journal_entries` değil

### `certifications`
`type` text · `verified_at` timestamptz · `expires_at` date · `document_url` text  
⚠️ `issuer`, `status`, `valid_until` yok

### `listings`
`farmer_id` · `harvest_entry_id` · `crop` text · `quantity` numeric · `unit` text · `price_per_unit` numeric · `min_order` numeric · `quality` text · `description` text · `photo_urls` text[] · `status` text  
⚠️ `crop` (crop_type değil) · `title`, `harvest_date`, `parcel_id` yok
⚠️ **`trg_enforce_min_order_le_quantity`** (BEFORE INSERT, P23-M1-a): `min_order > quantity` olan yeni ilan insert'i reddedilir. **CHECK constraint DEĞİL, bilinçli karar** — CHECK her UPDATE'te de enforce olur ve `enforce_offer_stock()`'un stok tükettikçe `quantity`'yi `min_order` altına düşürmesini (legal durum) kırardı. Trigger sadece INSERT'te çalışır, mevcut ilanların güncellenmesini engellemez. Hâlâ tabloda başka hiç CHECK constraint yok.

### `offers` (P15 sonrası güncel)
Ana alanlar + P15 eklentileri:
- `ball_side` text CHECK ('farmer','buyer') DEFAULT 'farmer'
- `current_price` numeric
- `current_quantity` numeric  
- `payment_status` text CHECK ('unpaid','pending','paid') DEFAULT 'unpaid'
- offer_status enum: `pending_farmer`, `negotiating`, `pending_payment`, `active`, `delivered` + legacy (`pending`→`pending_farmer`, `counter`→`negotiating`, `accepted`→`pending_payment`)

### `offer_messages` (P15 — yeni tablo)
`offer_id` uuid→offers · `sender_role` text CHECK('farmer','buyer') · `sender_id` uuid→auth.users · `price` numeric · `quantity` numeric · `note` text · `created_at`  
RLS: SELECT = offer'ın buyer/farmer'ı · INSERT = sender_id = auth.uid()

### `orders` / `order_timeline`
Order kabul sonrası oluşur. order_timeline adımları takip eder.

### `community_posts`
`author_id` (farmer_id değil) · `content` · `category` · `likes_count` int · `comments_count` int

### `price_alerts`
`farmer_id` (user_id değil) · `crop` (crop_type değil) · `condition` (direction değil) · `active` bool (is_active değil) · `channels` notif_channel[]

### `harvest_subscriptions`
`buyer_id` · `farmer_id` · `next_harvest_date` date · `estimated_qty` numeric · `volume_commitment` numeric · `price_lock` bool · `locked_price` numeric

### `notifications`
In-app bildirimler. Phase 7'de eklendi.

## Test Kullanıcıları
| Rol | Telefon | UUID | Profil |
|---|---|---|---|
| Farmer | 905001234567 | 0868e4fe-86d2-4c5d-8ba5-f15fd4fac146 | Ahmet Yılmaz, Safranbolu |
| Buyer | 905009876543 | 032eb467-661d-4df4-adf5-3d277d9b6549 | Zeynep Kaya, Istanbul |
OTP test: `123456`

## Seed Insert Sırası
profiles → farms → parcels → certifications → listings → harvest_entries → community_posts → price_alerts → harvest_subscriptions → buyer_profiles
UUID'leri hardcode et — phone lookup ambiguous column hatası verir.


## Önerilen Şema Değişiklikleri — Temmuz 2026 (Ürün İzlenebilirliği, P16-H)

> Build başlamadan önce `TODO.md` P16-H'deki açık kararlar netleşmeli.

- **`listings.harvest_entry_id` sorunu:** şu an 1:1. Bir listing genelde tek hasat kaydından değil, sezon boyunca birden fazla `harvest_entries` kaydından (bakım + hasat) besleniyor. Seçenekler:
  1. Join table `listing_harvest_entries` (listing_id, harvest_entry_id) — M:N, en temiz
  2. `listings`'e `parcel_id` + `season_start`/`season_end` ekle, ilgili `harvest_entries` runtime'da parcel_id + tarih aralığına göre çekilir — şema değişikliği daha az ama query mantığı daha karmaşık
- **Entry type ayrımı yok:** `harvest_entries` şu an hasat olaylarını (quantity, harvest_date) modelliyor; bakım aktiviteleri (`[work:ilaçlama]` gibi) `notes` içinde serbest tag olarak tutuluyor (bkz. B1 bug). MVP için: mevcut tag sistemini parse ederek kullan, yeni enum/kolon açma — sezon öncesi şema riski almaya değmez.
- **Coverage hesaplama:** crop bazlı "beklenen adımlar" listesi kod/config seviyesinde tutulmalı (DB'de ayrı tablo gerekmez), her adım için ilgili tag/entry var mı diye kontrol edilir.
- **Buyer'a asla gösterilmeyecek alanlar:** `costs` (jsonb), ham `notes` metni. Sadece kürasyonlu özet (adım adı + tarih + varsa fotoğraf) gösterilir.
- **Tamper kontrolü:** `harvest_entries`'e `created_at` (Supabase default) zaten var — buyer view'da "olay tarihi" (`harvest_date`) ile "kayıt tarihi" (`created_at`) arasındaki farkın büyük olduğu (ör. 8 ay sonra toplu girilmiş) kayıtlar ayrı işaretlenmeli veya listing'e bağlandıktan sonra `harvest_entries` edit'i kilitlenmeli.


### Ek Not — Journal'ı Organik Sertifikasyon/ÜKD Muadili Olarak Tasarlamak

> Berkin talebi (Temmuz 2026): journal, ileride TKDK/organik sertifikasyon dokümantasyonunu Hasat verisinden üretebilecek kadar kapsamlı olsun. Şimdi generator'ı build etmiyoruz — sadece veri modelini bu ihtiyacı destekleyecek şekilde tasarlıyoruz ki sonradan pahalı bir migration/backfill gerekmesin.

**Türkiye'nin resmi Üretici Kayıt Defteri (ÜKD) ve genel organik denetim pratiği şu alanları zorunlu kılıyor** (bkz. Tarım Bakanlığı ÜKD SSS, USDA/Oregon Tilth recordkeeping guides):
- Girdi uygulamaları: ürün ticari adı, miktar, birim, uygulama tarihi, uygulama şekli (püskürtme/toprak/yaprak), amaç (hangi zararlı/hastalık veya gübreleme nedeni), hasat öncesi bekleme süresi
- Sulama: tarih + miktar
- Saha faaliyetleri: toprak hazırlığı, ekim/dikim, hasat tarihi — parsel bazında
- Satış/denetim izi: hasat → satış zincirinin geriye izlenebilir olması (bu zaten P16-H buyer timeline ile aynı veri)

**Sorun:** Mevcut `[work:ilaçlama][health:3]` tag sistemi bu alanları (ürün adı, miktar, amaç, bekleme süresi) yakalamıyor — sadece kategori + skor. Bu haliyle buyer timeline için yeterli ama sertifikasyon-grade denetim izi için yetersiz.

**Öneri (MVP'yi bloklamadan):** Yeni kolonlar çoğaltmak yerine, `costs` jsonb'ye paralel bir `details` jsonb kolonu ekle (`entry_type`, `product_name`, `quantity`, `unit`, `method`, `purpose`, `pre_harvest_interval_days` gibi opsiyonel key'ler). Tek migration, Lovable-friendly, ve UI şimdilik sadece kullandığı alanları gösterir — ama veri gelecekte sertifikasyon raporu / ÜKD dijital muadili üretmek için orada bekler.

**Retention notu:** Organik denetim kayıtları genelde 5 yıl saklanmalı (USDA benchmarkı) — Hasat'ta otomatik silme/arşivleme özelliği eklenirse journal/harvest_entries bu kuralın dışında tutulmalı.

**Konumlandırma fikri:** "Hasat günlüğü = dijital Üretici Kayıt Defteri" çerçevesi, hem çiftçi edinimi (kağıt defter yerine) hem de ileride sertifikasyon kuruluşlarıyla ortaklık için güçlü bir açı olabilir (Phase 2+, şimdi build etme).


## Şema Ekleri — Temmuz 2026 (P16-I, P16-J)

### Yeni Enum
| Enum | Değerler |
|---|---|
| `production_method` | `indoor`, `outdoor` |
| `interest_type` | `danışmanlık`, `ortaklık`, `diğer` |

### Yeni Tablo: `crop_config`
`crop` text PK · `display_name` text · `default_unit` unit_type · `harvest_window_start_month` int · `harvest_window_end_month` int · `lifecycle_steps` jsonb · `price_benchmark_source` text · `category_group` text
Seed: `safran`, `lavanta`. Amaç: B4 (Hasat Dönemi chip), B19 (kategori sayacı), P16-E (fiyat tablosu) ve P16-H (lifecycle şablonları) tek buradan okusun — yeni ürün eklemek kod değişikliği değil, tek satır INSERT olsun.
⚠️ **`safran_soğanı.default_unit`** P23-M1-a'da `'adet'`'ten `'kg'`'ye düzeltildi — `unit_type` enum'u yalnızca `g`/`kg`/`L` içeriyor, `adet` yok; eski değer ilan oluşturmada gizli insert hatası riskiydi. Enum'a `adet` **eklenmedi** (P21 birim-uyuşmazlığı trigger'ını ve stok toplamalarını kirletir).

### `parcels` — yeni kolon
`production_method` production_method enum, mevcut satırlara default `outdoor`.

### Yeni Tablo: `indoor_interest_leads`
`id` uuid · `name` text · `phone` text · `city` text · `interest_type` interest_type · `note` text · `created_at` timestamptz
RLS: INSERT public (auth gerektirmez) · SELECT sadece Berkin (admin).


## 🔧 P16-H Şema Kararı — Çözüldü (İki Katmanlı Model, Temmuz 2026)

> Bu bölüm, yukarıdaki "Önerilen Şema Değişiklikleri — Temmuz 2026 (Ürün İzlenebilirliği, P16-H)" bölümündeki **`listings.harvest_entry_id` sorunu** maddesini ve orada sunulan "Seçenek 1 (join table) / Seçenek 2 (parcel_id+season_start/end)" ikilemini **geçersiz kılar** — ikisi birden, farklı katmanlarda uygulanıyor. (Obsidian patch API bu oturumda çalışmadığı için eski madde fiziksel olarak yukarıda duruyor; geçerli karar bu bölümdür.)

### Tier 1 — Fulfillment (ne satılıyor)
- Yeni tablo: `listing_harvest_entries` (`listing_id` uuid→listings, `harvest_entry_id` uuid→harvest_entries)
- `listings.harvest_entry_id` kolonu bu join table'a migrate edilip düşürülür (mevcut her listing için tek satır insert edilerek migrate edilir)
- Amaç: bir listing'in sattığı miktarın hangi hasat günü kayıtlarının toplamından geldiğini izlemek — sipariş bütünlüğü için

### Tier 2 — Provenance (buyer'ın gördüğü hikaye)
- `listings`'e yeni kolon: `parcel_id` uuid→parcels (oluşturulurken set edilir, Tier 1'den bağımsız)
- Buyer timeline query'si: `SELECT * FROM harvest_entries WHERE parcel_id = listing.parcel_id AND crop = listing.crop AND harvest_date >= (son dikim-tipi kaydın tarihi) ORDER BY harvest_date`
- Bu sorgu `listing_harvest_entries` bağlantısına bakmaz — sulama/ilaçlama gibi miktarsız kayıtlar da dahil olur, çünkü buyer'ın güven ihtiyacı "bu tam gram'ın kağıt izi" değil "bu parsel dürüst yönetildi mi"

### Yeni iş kuralları
- Bir listing = tek parsel. Parsel karıştırma (blend) yok; çiftçi birden fazla parselden satmak isterse birden fazla listing açar.
- "Sezon başlangıcı" sabit takvim değil — o parsel+crop için en son dikim-tipi (`entry_type`/tag = dikim/ekim) kaydı. Kayıt yoksa coverage hesaplamasında bu açıkça bir eksik olarak işaretlenir (bkz. P16-H buyer badge mantığı), sessizce "tam geçmiş yok" diye atlanmaz.
- Coverage skoru Tier 2 (parsel-sezon) üzerinden hesaplanır, Tier 1 bağlantı sayısı üzerinden değil.

### Not
`entry_type` ayrımı (dikim/bakım/hasat/işleme) hâlâ bu bölümün üstünde tanımlı "Entry type ayrımı yok" maddesindeki MVP kararına bağlı: mevcut tag sistemi (`[work:...]`) parse edilerek kullanılacak, yeni kolon açılmayacak — "son dikim kaydı" da bu tag sisteminden (örn. `[work:dikim]`) okunacak.


## P16-H Genişletme — Stok + Batch Sayfası (Temmuz 2026 onayı)

### Stok hesaplama (yeni kolon değil, canlı sorgu)
```
available_quantity(listing) =
  SUM(harvest_entries.quantity WHERE id IN (SELECT harvest_entry_id FROM listing_harvest_entries WHERE listing_id = listing.id))
  − SUM(offer.current_quantity WHERE listing_id = listing.id AND status IN ('pending_payment','active','delivered'))
```
- MVP ölçeğinde (≈30 farmer, ≈60 listing) canlı sorgu yeterli — cache kolonu/trigger gerekmez
- Server-side guard: offer `pending_payment`'a geçerken bu değeri kontrol eden trigger — P15 A2'deki `get_my_role_for_offer` guard pattern'inin aynısı, farklı bir kural seti (miktar kontrolü) ile tekrar kullanılacak
- Kilit kuralı: ilk `pending_payment`'a ulaşan offer kilitler; aynı batch için diğer aktif `negotiating` offer'lar stok yetersiz kalırsa otomatik `expired`/iptal + buyer'a bildirim

### Batch sayfası veri kaynağı
- Batch = bir `listings` satırı + ona bağlı `listing_harvest_entries` kayıtları (P16-H Tier 1)
- Sayfa sorgusu: `listing_harvest_entries` → `harvest_entries` (kronolojik liste) + `available_quantity` hesaplaması + `offers`/`orders` (o `listing_id`'ye ait satış geçmişi)
- Farmer view: tüm alanlar (costs dahil) · Buyer view: P16-H'nin redaction kuralı uygulanır (costs/notes gizli, sadece küratörlü özet)
- Auto-suggest (listing oluşturma anında): `parcel_id` + `crop` + `quality` eşleşen ve tarihçe yakın (öneri: ±7 gün, kesin eşik değil) henüz hiçbir listing'e bağlanmamış `harvest_entries`'i öner — sadece UI önerisi, DB'ye yazılan gerçek batch tanımı farmer'ın onayladığı `listing_harvest_entries` seçimidir

## Şema Ekleri — Temmuz 2026 (Fiyat Otomasyonu Şeması, P16-I'ye eklendi)
- `price_feed` — yeni kolon: `source_type` text CHECK (`manual`, `api`) DEFAULT `manual`
- `crop_config` — yeni kolon: `auto_price_source` jsonb (nullable) — endpoint + ürün-kodu eşlemesi tutar; safran/lavanta seed'inde `null`
- Not: canlı çekim job'u (pg_cron/Edge Function) **build edilmiyor** — sadece şema hazır bekliyor. Detay/kaynak taraması: `Research/Market.md` → "Otomatik Fiyat Toplama Araştırması"

## P23-M1-a — Şema Borçları Kapatıldı (2026-07-28)

`Build/P23-Mobile.md` M1'de listelenen "küçük şema borçları"nın veritabanı tarafı. `hasat-core`/subtree işi kapsam dışı (P23-M1-b).

1. **`crop_config.safran_soğanı.default_unit`**: `'adet'` → `'kg'`. `unit_type` enumuna `adet` eklenmedi (bkz. yukarı, `crop_config` bölümü).
2. **`listings` — `min_order > quantity`**: CHECK constraint yerine `trg_enforce_min_order_le_quantity` (BEFORE INSERT). Gerekçe yukarıda `listings` bölümünde. Mevcut ihlal eden tek satır (ilan `63b0cf1b-8554-4d6e-9e12-0fdd6ebb1a85`, Ahmet Yılmaz, kekik, `min_order=10kg`/`quantity=5kg`) `min_order=5`'e düşürülerek düzeltildi, `quantity`'ye dokunulmadı.
3. **`buyer_profiles.company_name`**: NOT NULL → nullable (bkz. yukarı).
4. **`buyer_addresses`**: `trg_enforce_single_default_address` (BEFORE INSERT OR UPDATE) eklendi (bkz. yukarı). UPDATE RLS'in zaten var olduğu (`buyer_addresses_owner_all`, cmd=ALL) doğrulandı — orders'taki UPDATE-policy eksikliği burada tekrarlanmadı.

**Doğrulama:** Her madde gerçek SQL sorgusuyla ve gerçek insert/update denemesiyle test edildi (Kural #96) — ayrıntılar PR açıklamasında. Advisor taraması (`get_advisors`) yeni trigger fonksiyonlarında `search_path` uyarısı verdi, ikisi de mevcut trigger konvansiyonuyla (`SET search_path = public`) düzeltildi.

## P22-G — Rutin Bakım Tarih/Filtre Düzeltmesi + Trigger Temizliği (2026-07-28)

**A — `buyer_addresses` çift trigger:** P23-M1-a'da eklenen `trg_enforce_single_default_address` (→ `tg_enforce_single_default_address()`) ile daha önce var olan `trg_buyer_addresses_clear_default` (→ `buyer_addresses_clear_default()`) aynı işi (diğer varsayılan adresi temizleme) yapıyordu — ikincisi `updated_at`'i de güncelliyordu, birincisi güncellemiyordu. `trg_enforce_single_default_address` + fonksiyonu düşürüldü, `buyer_addresses`'te artık tek "varsayılan adres temizleme" trigger'ı var (+ ayrı `trg_buyer_addresses_updated_at`, farklı bir kaygı — satırın kendi `updated_at`'ini her UPDATE'te damgalıyor). Gerçek insert/update ile doğrulandı.

**C — Yeni view: `v_routine_maintenance_status`** (kural #106 — hesap DB'de, client sadece filtreler). Çiftçi/parsel/crop/bakım-işi (`farmer_journal_prefs` × `parcels.crops` × `harvest_entries.journal_entry_type_id`) başına: `last_performed_date`, `frequency_days`, `next_due_date`, `never_performed`, `is_event_based`, `is_overdue`. `security_invoker=true` — SECURITY DEFINER değil, alttaki tabloların RLS'ine güveniyor (yeni advisory eklemedi). Mobil (P23) aynı view'ı okuyacak.

**E — `notify_new_crop_type_request()` SMS'i genişletildi:** Önceden sadece ürün adı + çiftçi adı gönderiyordu, form 7 alan topluyordu (birim, kategori, hasat ayı başlangıç/bitiş, "nasıl yetiştiriliyor" notu, ek not — hiçbiri SMS'e yansımıyordu). Berkin kararıyla ("kritik alanları ekle + notu kısalt"): birim, kategori, hasat ayı aralığı ve kısaltılmış (≤80 karakter, "ek not" > "nasıl yetiştiriliyor" önceliğiyle) bir not eklendi. Buyer'ın katalog-boşluğu SMS'ine de kısaltılmış not eklendi (JS tarafında, `useCreateCropRequest`). `notify-admin` edge function'ının 300 karakter limiti var, önceki mesajlar ~40-60 karakterdi — teknik sınır sorunu değildi. Gerçek Twilio testiyle doğrulandı.

**Kök neden notu (B):** "P22-F verileri taşıdı ama hesap eski kaynağa bakıyor" hipotezi çürütüldü — `harvest_entries.journal_entry_type_id` okuma zaten doğruydu. Asıl bug: `useCreateEntry`'nin `onSuccess`'i bu sorgunun React Query cache key'ini invalidate etmiyordu, "Yaptım" sonrası satır SPA içinde güncellenmiyordu. Ayrıntılar: [PR #5](https://github.com/berkinsavciozen/hasat-d2c-marketplace/pull/5).

## P23-M2 — Tarif Backend'i (2026-07-29)

`Build/P23-Mobile.md` M2. **Tamamen ekleyici** — `offers`/`orders`/`listings`/`harvest_entries` akışlarına hiç dokunulmadı, `unit_type` enum'una dokunulmadı.

### Yeni tablolar (7)

#### `recipes`
`id` uuid PK · `slug` text **UNIQUE** (SEO) · `title` · `description` · `cover_photo_url` · `servings` int · `prep_minutes` int · `cook_minutes` int · `difficulty` text CHECK(`kolay`/`orta`/`zor`) · `cuisine` · `diet_tags` text[] · `status` text CHECK(`draft`/`published`) · `visibility` text CHECK(`public`/`private`) · `source_type` text CHECK(`manual`/`text`/`photo`/`url`) · `source_url` · `owner_id` uuid→profiles **nullable** · `author_type` text CHECK(`hasat`/`ciftci`/`sef`) · `extraction_confidence` numeric CHECK(0..1) · `created_at`/`updated_at`
- `owner_id IS NULL` = **editoryal public korpus**. `owner_id` dolu = kullanıcı defteri.
- `trg_recipes_updated_at` → mevcut `set_updated_at()`
- Index: `recipes_slug_key` (unique), `recipes_public_published_idx` (partial: public+published), `recipes_owner_idx` (partial)

#### `recipe_steps`
`recipe_id` uuid→recipes **ON DELETE CASCADE** · `step_no` int CHECK(>0) · `instruction` · `photo_url` · `timer_seconds` int CHECK(>0) — M6 pişirme modunun temeli
UNIQUE(`recipe_id`, `step_no`)

#### `recipe_ingredients`
`recipe_id` uuid→recipes CASCADE · `sort_order` int · `crop` text→`crop_config(crop)` **nullable**, ON UPDATE CASCADE / ON DELETE SET NULL · `free_text_name` (tuz/un gibi platform-dışı) · `quantity` numeric · `unit` text (**culinary birim**) · `note` · `is_key_ingredient` bool
- CHECK `recipe_ingredients_name_present`: `crop` veya `free_text_name`'den en az biri dolu olmalı
- ⚠️ **`crop` editoryal olarak bir kez doldurulur.** Malzeme→crop bağlantısı runtime'da fuzzy text matching ile **YAPILMAZ** — `extract-recipe` bile `crop`'u daima `null` bırakır.

#### `crop_culinary_meta`
`crop` text PK→`crop_config(crop)` CASCADE · `is_edible` bool · `culinary_aliases` text[] · `conversion_hints` jsonb · `created_at`/`updated_at`
- `is_edible=false` → tarif akışına **hiç girmez**.
- **`conversion_hints` birim sözleşmesi:** değerler **temel metrik birimdedir** — kütle crop'larında gram, hacim crop'larında (`default_unit='L'`) mililitre. Örn. `{"adet": 120}` = 1 adet 120 g. `fn_culinary_to_canonical` son adımda `crop_config.default_unit`'e çevirir. Böylece bir crop'un `default_unit`'i kg↔g değişse bile katsayılar geçerli kalır.

#### `recipe_saves`
`user_id` uuid→profiles CASCADE · `recipe_id` uuid→recipes CASCADE · UNIQUE(`user_id`,`recipe_id`)
KVKK: kişisel veri — gizlilik metni M7'de güncellenmeli.

#### `recipe_rfq_links`
`recipe_id` uuid→recipes CASCADE · `crop_request_id` uuid→`crop_requests` CASCADE · UNIQUE(`recipe_id`,`crop_request_id`)
Huni atfının **tek sert bağı** — "bu talep şu tariften doğdu".

#### `device_tokens`
`user_id` uuid→profiles CASCADE · `token` text **UNIQUE** · `platform` text CHECK(`ios`/`android`) · `created_at`
`notif_channel` enum'unda `push` zaten var — yeni bildirim sistemi kurulmadı.

### `crop_config` — yeni kolon
`default_photo_url` text (ADD COLUMN IF NOT EXISTS). Görseller M3'te yüklenecek; bu turda sadece altyapı.

### Storage
**`crop-photos`** bucket, `public = true`.
- ⚠️ `storage_update_bucket` MCP aracı kullanılmadı — bucket doğrudan SQL ile açıldı ve `SELECT public FROM storage.buckets WHERE id='crop-photos'` ile **iki kez** (oluşturmada ve tur sonunda) `true` doğrulandı.
- `storage.objects` üzerinde bu bucket için **SELECT politikası açılmadı** — public bucket'ta obje URL'i zaten RLS'siz servis edilir; geniş SELECT politikası yalnızca "tüm dosyaları listeleme"yi açar (`listing-photos`/`parcel-photos`'ta duran advisor uyarısı bu). Yükleme service_role ile yapılır (yazma = admin).

### RLS — tablo başına

| Tablo | anon SELECT | authenticated SELECT | INSERT | UPDATE | DELETE |
|---|---|---|---|---|---|
| `recipes` | `visibility='public' AND status='published'` | public+published **veya** `owner_id=auth.uid()` | `owner_id=auth.uid() AND visibility='private'` | ✅ var — USING `owner_id=auth.uid()`, CHECK `owner_id=auth.uid() AND visibility='private'` | `owner_id=auth.uid()` |
| `recipe_steps` | üst tarif public+published | üst tarif görünür | üst tarif sahibi | ✅ var | üst tarif sahibi |
| `recipe_ingredients` | üst tarif public+published | üst tarif görünür | üst tarif sahibi | ✅ var | üst tarif sahibi |
| `crop_culinary_meta` | `true` (tarif sayfaları için şart) | `true` | — (admin) | — (admin) | — (admin) |
| `recipe_saves` | — | `user_id=auth.uid()` | `user_id=auth.uid()` | ✅ var | `user_id=auth.uid()` |
| `device_tokens` | — | `user_id=auth.uid()` | `user_id=auth.uid()` | ✅ var | `user_id=auth.uid()` |
| `recipe_rfq_links` | — | talebin sahibi | talebin sahibi | ⛔ **bilinçli yok** (append-only) | talebin sahibi |

**"admin" bu projede ne demek:** `profiles.role` yalnızca `farmer`/`buyer` içeriyor, `is_admin()` fonksiyonu **yok**. Mevcut admin erişim yöntemi = service-role anahtarlı edge function (`admin-kpi` + `x-admin-key`). Bu yüzden "yazma sadece admin" = **hiç politika yazılmaz**; service_role RLS'i baypas eder. Yeni bir admin rolü/deseni icat edilmedi.

**UPDATE politikası kuralı:** Mutasyon akışı olan **beş** tablonun (`recipes`, `recipe_steps`, `recipe_ingredients`, `recipe_saves`, `device_tokens`) hepsinde SELECT/INSERT'ten ayrı bir UPDATE politikası var ve **gerçek UPDATE ile 1 satır etkilediği** doğrulandı — `orders`'ta eksik olan ve tüm P17-B mutasyonlarını sessizce sıfır satıra düşüren hata burada tekrarlanmadı. `recipe_rfq_links` bilinçli olarak append-only.

### Mantık katmanı (kural #106) — 1 fonksiyon + 2 RPC + 2 view

#### `fn_culinary_to_canonical(p_crop text, p_quantity numeric, p_unit text) → numeric`
`STABLE`, `SET search_path = public`, SECURITY INVOKER.
1. Metrik birimler (`g`/`gr`/`gram`/`kg`/`ml`/`l`/`lt`/`litre`) ipucu gerektirmez.
2. Culinary birim **yalnızca** `conversion_hints`'ten çözülür; ipucu yoksa **NULL döner — uydurmaz**.
3. Temel metrik birimden `crop_config.default_unit`'e çevirir.
Birim metni `lower(btrim(...))` ile normalize edilir.

#### `rpc_recipe_availability(p_recipe_id uuid)`
Malzeme başına: `is_platform_crop`, `is_matched`, `active_listing_count`, `canonical_unit`, `best_price_per_canonical`, `crop_photo_url`.
- `is_edible=false` crop'lar **sonuçtan tamamen düşer**.
- Fiyat normalizasyonu: `price_per_unit × temel(kanonik) ÷ temel(ilan)` — ör. ₺25,50/g domates, kanonik kg → ₺25.500/kg.
- **SECURITY INVOKER** — private bir tarif için anon/başka kullanıcı çağırdığında 0 satır döner.

#### `rpc_recipe_shopping_list(p_recipe_id uuid, p_servings int)`
Porsiyona ölçekler (`p_servings / recipes.servings`), kanonik birime çevirir, **min_order'a yuvarlar**, `recipes_covered` ("bu miktar kaç tarif yapar") ve `estimated_cost` hesaplar.
- `purchase_canonical = GREATEST(needed, min_order)`; `rounded_up_to_min_order` bayrağı ayrıca döner.
- En iyi fiyatlı aktif ilanın `min_order`'ı kullanılır (fiilen alışverişin yapılacağı ilan).
- Dönüşüm ipucu yoksa `needed_canonical` NULL + `conversion_available=false` — UI "miktar hesaplanamadı" diyebilir.
- Platform-dışı malzemeler (tuz/un) listede **nötr satır** olarak kalır.
- **SECURITY INVOKER.**

#### `v_recipe_coverage` — `security_invoker=true`
Tarif başına `ingredient_count`, `crop_linked_count`, `off_platform_count`, `available_count`, `key_ingredient_count`, `key_available_count`, `coverage_pct`. Liste sayfalarında filtreleme/sıralama için. Yenilemez crop'lar sayıma girmez.

#### `v_kpi_recipe_funnel` — `security_invoker=true`
Aylık: `recipe_views`, `recipe_saves`, `recipe_attributed_requests`, `recipe_attributed_offers`, `recipe_attributed_orders`, `request_to_order_pct`.
- Mevcut 20 KPI view'ının deseni: `anon`/`authenticated`'a **GRANT yok**; erişim service-role anahtarlı admin edge function ile (`admin-kpi` deseni).
- ⚠️ **`recipe_views` şu an NULL.** Görüntüleme olayını üretecek yüzey (web `/tarifler`) M4'te doğuyor; olay tablosu onunla birlikte eklenecek. Onaylı 7 tablo kapsamı **değiştirilmedi** (8. tablo eklenmedi).
- ⚠️ **Atıf zinciri:** `recipe_rfq_links` **tek sert bağdır** (tarif→talep). Talepten sonrası (teklif/sipariş) **sezgisel atıftır**: aynı alıcı + tarifin malzeme crop'u + talep tarihinden sonra. Gerekçe: `crop_requests` ile `offers`/`orders` arasında **hiç FK yok** (2026-07-29 canlı DB'de doğrulandı).

⚠️ **İki view da `security_invoker=true`** — `SELECT reloptions FROM pg_class` ile doğrulandı. Atlanırsa view RLS'i baypas eder (`v_routine_maintenance_status`'ta doğru yapılmıştı, aynı disiplin).
⚠️ Üç yeni fonksiyonun hepsinde `SET search_path = public` baştan var — advisor taramasında **hiçbiri uyarı üretmedi**.

### Seed
- `crop_culinary_meta.is_edible`: **70 crop'un tamamı** `category_group`'tan mekanik türetildi. Yenilemez = `endustri_bitkisi` (pamuk, şeker_pancarı, tütün) + `safran_soğanı` → **4 crop**, 66 yenilebilir.
- `culinary_aliases` + `conversion_hints`: yalnızca **3 test crop'u** — `domates` (mainstream), `kekik` (niş), `pamuk` (yenilemez, ikisi de bilinçli boş). Kalan 67 → M3.

⚠️ **`gül` yenilebilir kaldı.** Görev metninin 4. maddesinde örnek yenilemez listesinde "gül yağlık" geçiyordu, ama 5. maddedeki operatif seed kuralı (`category_group`'tan mekanik türet) `gül`'ü `tibbi_bitki` grubunda bırakıyor. Mekanik kural uygulandı (gül reçeli/gül suyu gerçek culinary kullanım, `crop_config`'teki `gül` jenerik — yağlık/çayır ayrımı yok). **Bu karar otonom alındı, Berkin onayı yok.** Değiştirmek tek satır: `UPDATE crop_culinary_meta SET is_edible=false WHERE crop='gül';`

### Edge function: `extract-recipe`
`verify_jwt = true` (kullanıcı tetiklemeli — `sync-izmir-hal-prices`'taki cron istisnası burada geçerli değil).
- Modaliteler: `mode='text'` (yapıştırma) ve `mode='photo'` (yazılı tarif fotoğrafı, vision+OCR). YouTube/link ve yemek fotoğrafından tahmin **yok** → M9.
- **Sunucuda zorlananlar:** `visibility='private'`, `status='draft'`, `owner_id` = JWT `sub` claim'i. Client'ın gönderdiği `visibility`/`status`/`owner_id` **yok sayılır**.
- `extraction_confidence` modelden gelir, 0..1'e clamp'lenir.
- Kota: mevcut `can_send_ai_message` / `increment_ai_usage` (`ai_usage_tracking`). **Yeni kota altyapısı kurulmadı.**
- `recipe_ingredients.crop` daima `null` — runtime fuzzy eşleştirme yasağı.
- Sağlayıcı: Lovable AI gateway, `google/gemini-3-flash-preview` (mevcut `ai-box-insights` deseni), `response_format: json_object`.

⚠️ **`supabase/config.toml`'a `[functions.extract-recipe]` girdisi eklenmedi** — web reposu Lovable'ın yönettiği alan ve bu tur "frontend işi yok" kapsamındaydı. Deploy edilmiş halde `verify_jwt=true` (API yanıtıyla doğrulandı). Lovable ileride config.toml'dan toplu deploy yaparsa bu girdinin eklenmesi gerekir — **M4'te kapatılacak açık madde.**

⚠️ **`author_type` kullanıcı importlarında `hasat` (default) kalıyor.** Onaylı değer kümesi (`hasat`/`ciftci`/`sef`) kullanıcı importu için bir karşılık içermiyor. Kullanıcı importu zaten `owner_id` + `visibility='private'` + `source_type≠'manual'` üçlüsüyle kesin ayırt ediliyor; `author_type` yalnızca public korpus satırlarında anlamlı. Değer kümesini genişletmek şema kararı olduğu için **yapılmadı** — Berkin'in kararına bırakıldı.

## P23-M2-ek — Huni Ölçümünün Tamamlanması (2026-07-29)

Berkin onaylı kapsam değişikliği: **7 tablo → 8 tablo + 1 nullable kolon.** Amaç, `v_kpi_recipe_funnel`'ı sezgisel atıftan kurtarıp beş basamağın tamamını sert FK'lerle doldurmak.

### Yeni tablo: `recipe_views`

`id` uuid PK · `recipe_id` uuid→recipes **ON DELETE CASCADE** · `user_id` uuid→profiles **nullable**, ON DELETE SET NULL · `session_id` text · `created_at` timestamptz

- **KVKK:** IP ve user-agent **bilinçli olarak loglanmıyor.** Gizlilik metni M7'de ele alınacak.
- `user_id` giriş yapmamış ziyaretçide NULL — tekilleştirme `session_id` ile yapılır.
- Index: `recipe_views_recipe_idx`, `recipe_views_created_idx`.

**RLS:**

| Komut | Politika |
|---|---|
| INSERT | `recipe_views public insert` → `to anon, authenticated` · `with check (user_id is null or user_id = auth.uid())` |
| SELECT | **politika yok** + `anon`/`authenticated`'tan GRANT çekildi → yalnızca service_role |
| UPDATE | **bilinçli yok** — append-only olay tablosu |
| DELETE | **bilinçli yok** |

```sql
revoke all on public.recipe_views from anon, authenticated;
grant insert on public.recipe_views to anon, authenticated;
```

Bu, mevcut 20 KPI view'ının kilidinin bir tabloya uygulanmış hali: uygulama rolleri yazabilir, okuyamaz. `WITH CHECK`'teki tek kısıt, başkasının adına görüntüleme yazılamaması — diğer tüm tablolardaki (`requested_by = auth.uid()`, `user_id = auth.uid()`) bütünlük guard'ının aynısı, yeni desen değil. Giriş yapmamış ziyaretçide `auth.uid()` NULL olduğu için anon INSERT'i kısıtlamıyor.

### Yeni kolon: `offers.source_recipe_id`

`uuid` nullable → `recipes(id)` **ON DELETE SET NULL**. Trigger yok, constraint yok, default yok.

**Yeni desen değil:** `offers.subscription_id → harvest_subscriptions` zaten aynı işi yapan mevcut bir "bu teklif nereden doğdu" kolonu; aynı konvansiyon izlendi. Partial index: `offers_source_recipe_idx ... where source_recipe_id is not null`.

Mevcut `offers` politikaları yeni kolonu olduğu gibi kapsıyor — `Buyers insert offers` (INSERT, `auth.uid() = buyer_id`) ve `Both parties update offer` (UPDATE) üzerinden yazılıp okunabildiği gerçek mutasyonla doğrulandı. **Yeni politika gerekmedi.**

### `recipes.author_type` += `kullanici`

```sql
CHECK (author_type = ANY (ARRAY['hasat','ciftci','sef','kullanici']))
```

`hasat`/`ciftci`/`sef` = editoryal public korpus. `kullanici` = AI ile içe aktarılmış kullanıcı defteri. `extract-recipe` artık bu değeri yazıyor (öncesinde default `hasat` kalıyordu ve import editoryal içerikmiş gibi görünüyordu — eksik veri). **Mevcut satırlara dokunulmadı**, yalnızca izin verilen değer kümesi genişledi.

### `v_kpi_recipe_funnel` — yeniden yazıldı (v2)

⚠️ **Önceki sürüm reddedildi.** Eski view `crop_requests` üzerinden "aynı alıcı + aynı crop + talepten sonra" tipi bir çıkarım yapıyordu. Bu sessizce **fazla atıf** üretir: düzenli domates alan bir alıcının her domates siparişi tarif katmanına yazılırdı, tarif katmanı işe yaramadığı halde yarıyormuş gibi görünürdü. **North Star metriğinde yanlış sayı, hiç sayı olmamasından kötüdür.**

Yeni sürümde **uçtan uca yalnızca sert join** var — hiçbir yerde çıkarım, zaman penceresi veya crop eşleştirmesi yok:

```
recipe_views                                    (1. görüntüleme)
  -> recipe_saves                               (2. kayıt)
  -> recipe_rfq_links -> crop_requests          (3. talep — MALZEME YOK yolu)
  -> offers.source_recipe_id                    (4. teklif — MALZEME VAR yolu)
  -> orders.offer_id -> offers.source_recipe_id (5. sipariş)
```

**3. ve 4. basamak paralel çıkış yollarıdır, ardışık değil:** malzeme eşleşmediyse kullanıcı talep açar, eşleştiyse doğrudan teklif verir. Bu yüzden "talep → teklif" oranı hesaplanmıyor; yalnızca gerçekten ardışık olan iki oran veriliyor.

| Kolon | Anlam |
|---|---|
| `month` | Olay ayı |
| `recipe_views` / `unique_viewers` | Ham görüntüleme / tekil izleyici (`coalesce(user_id::text, session_id)`) |
| `recipe_saves` | O ay yapılan kaydetmeler |
| `recipe_requests` | `recipe_rfq_links` ile tarife bağlı `crop_requests` |
| `recipe_offers` | `source_recipe_id` dolu teklifler |
| `recipe_orders` | O tekliflerden doğan siparişler |
| `recipe_offers_converted` | **O ay açılan** tekliflerin siparişe dönen sayısı (kohort) |
| `view_to_save_pct` | Ardışık oran |
| `offer_to_order_pct` | **Kohort** oranı — aylık sipariş/aylık teklif değil; ay sınırını geçen teklifleri yanlış saymaz |

`security_invoker=true` (`pg_class.reloptions` ile doğrulandı) · `anon`/`authenticated`'a **GRANT yok** (20 KPI view deseni).

### ⚠️ `v_kpi_recipe_funnel` — üç bilinen sınır (M4 açık maddesi)

P23-M3 denetiminde tespit edildi, düzeltilmedi — kapsam dışıydı (M2-ek'in
"sert join" düzeltmesi ayrı bir sorunu çözdü, bu üçü farklı bir sınıf).

1. **Yüzdeler kohortsuz.** `view_to_save_pct` gibi oranlar aynı takvim
   ayının görüntülemesini aynı ayın kaydına bölüyor — bir tarif ayın son
   günü görüntülenip kaydı ertesi aya sarkarsa o ay düşük, ertesi ay
   yapay yüksek görünür. Yavaş dönen bir huninin (Hasat'ın kendi
   konumlandırması — "hızlı teslimat değiliz") bu view'da yanıltıcı sonuç
   üretmesi olası.
2. **Tarif kırılımı yok.** View aylık toplam veriyor, hangi tarifin
   çalışıp hangisinin çalışmadığı görülemiyor. Çözümü ucuz: tüm
   basamaklarda (`recipe_views`, `recipe_saves`, `recipe_rfq_links`,
   `offers.source_recipe_id`) zaten `recipe_id` mevcut — per-recipe
   companion view (`v_kpi_recipe_funnel_by_recipe` gibi) doğrudan
   yazılabilir, şema değişikliği gerekmez.
3. **Gerçek bir "kayıt" basamağı yok.** `recipe_saves` zaten giriş yapmış
   kullanıcıyı ölçüyor (`user_id` NOT NULL) — anon bir ziyaretçinin
   "beğendim ama giriş yapmadım" sinyali kayboluyor. Çözüm: kayıt anında
   (giriş yapılmamışsa da) `session_id` yakalamak — `recipe_views`'ta
   zaten var olan `session_id` desenini `recipe_saves`'e taşımak.

Üçü de M4'te (web tarif yüzeyi, `/tarifler`) ele alınacak — o zaman gerçek
trafik olacağı için düzeltmenin gerçek veriyle doğrulanması mümkün olacak.

---

## P23-M3 — Tarif İçeriği + Culinary Seed + Görsel Altyapısı (2026-07-30)

`Build/P23-Mobile.md` M3. **Tamamen ekleyici** — hiçbir şema değişikliği
yok, sadece veri (18 tarif + adım + malzeme satırı, 14 crop'un
`crop_culinary_meta` seedi). Gerekçe ve crop kararı: `Build/P23-Mobile.md`
→ M3 bölümü.

### İçerik
18 tarif (10 Katman 1, 7 Katman 2, 1 safran) — `recipes` + `recipe_steps`
(98 satır) + `recipe_ingredients` (117 satır). Tüm tarifler
`author_type='hasat'`, `visibility='public'`, `status='published'`,
`owner_id=NULL`, `cover_photo_url=NULL` (kapak fotoğrafı Berkin'in işi,
aşağıda).

**Malzeme modelleme kuralı (bu turda netleşti):** `crop_config`'te gerçek
bir karşılığı olan malzeme (soğan, sarımsak, havuç, patates, limon,
pul_biber, nane, susam, kimyon, nar dahil — sadece 14 odak crop değil)
**her zaman `crop` FK'sine bağlandı**, ama odak-crop olmayanlar için
**yalnızca metrik birim (g/kg/ml) kullanıldı** — çünkü onların
`conversion_hints`'i bu turda boş kaldı (M4/M9'a bırakıldı) ve culinary
birimle (örn. "1 adet soğan") bağlanırlarsa `rpc_recipe_shopping_list`
sessizce NULL dönerdi. İşlenmiş/türetilmiş ürünler (domates salçası, nar
ekşisi, un, pirinç) editoryal olarak `free_text_name`'de bırakıldı — ham
crop'un kendisi değil, ayrı bir market ürünü oldukları için (aynı mantık
`buğday`→`un` ayrımında zaten P23-M2'den beri var).

### `crop_culinary_meta` seed — 14 crop
`zeytinyağı`, `nohut`, `mercimek`, `kekik`, `fındık`, `ceviz`, `buğday`,
`domates`, `biber`, `patlıcan`, `üzüm`, `incir`, `elma`, `safran`.
`culinary_aliases` + `conversion_hints` (temel metrik birimde — kütle
crop'larında gram, `zeytinyağı`da ml) tam dolduruldu; domates/kekik M2'den
genişletildi (yeni birim eklenmedi, mevcut korunup teyit edildi). Kalan 56
crop boş kaldı — M4/M9'da tamamlanacak.

### Doğrulama (kural #96, 2026-07-30, Claude Code + Supabase MCP)
| Kontrol | Sonuç |
|---|---|
| Her `recipe_ingredients.crop` gerçek bir `crop_config.crop` slug'ına çözülüyor mu | ✅ 0 çözümlenemeyen satır |
| `rpc_recipe_shopping_list` — 18 tarifin tamamı, varsayılan porsiyon | ✅ 68/68 crop-bağlı satırda `needed_canonical` dolu, 0 NULL |
| `rpc_recipe_shopping_list` — 2× porsiyon ölçekleme | ✅ 0 NULL (ölçekleme dönüşümü kırmadı) |
| `rpc_recipe_availability` — yenilemez crop'lar (`pamuk`/`tütün`/`şeker_pancarı`/`safran_soğanı`) | ✅ 18 tarifin hiçbirinde 0 satır |
| Crop dağılım raporu | ✅ `safran` tam 1 tarifte; `zeytinyağı` 12, `soğan` 6 (sık kullanılan platform-dışı-olmayan malzeme) |
| Anon rolüyle yayınlanmış 18 tarif okunabiliyor mu | ✅ 18/18 `recipes` + adım/malzeme satırları (SEO şartı) |
| `get_advisors: security` | ✅ Bu turdan kaynaklı yeni uyarı yok (mevcut, ilgisiz uyarılar değişmedi) |

### Görsel altyapı

#### `crop-photos` isimlendirme konvansiyonu
Bucket zaten var (`crop-photos`, `public=true`, P23-M2'de açıldı). Bu turda
karar verilen konvansiyon:

- **Düz ad alanı** (subfolder yok) — bucket'ın tek amacı crop varsayılan
  fotoğrafları, karışma riski yok.
- **Dosya adı = crop slug'ının ASCII'ye çevrilmiş hali** + `.jpg`
  (Türkçe karakter → ASCII, tarif slug kuralıyla aynı ilke): ğ→g, ı→i,
  ş→s, ç→c, ö→o, ü→u. Örn. `zeytinyağı` → `zeytinyagi.jpg`.
- **Format/boyut önerisi:** JPG ya da WEBP, yatay (4:3 ya da 16:9), kart
  UI'da kırpılacağı için kenarlarda önemli detay olmamalı, ~1200×900,
  <300 KB (mobil veri kullanımı için).

#### Berkin'e teslim edilecek 14 dosya
| crop (slug) | Dosya adı |
|---|---|
| zeytinyağı | `zeytinyagi.jpg` |
| nohut | `nohut.jpg` |
| mercimek | `mercimek.jpg` |
| kekik | `kekik.jpg` |
| fındık | `findik.jpg` |
| ceviz | `ceviz.jpg` |
| buğday | `bugday.jpg` |
| domates | `domates.jpg` |
| biber | `biber.jpg` |
| patlıcan | `patlican.jpg` |
| üzüm | `uzum.jpg` |
| incir | `incir.jpg` |
| elma | `elma.jpg` |
| safran | `safran.jpg` |

⚠️ 13 vs 14 tutarsızlığı (bkz. `P23-Mobile.md` → M3) burada da geçerli —
liste 14 dosya içeriyor, otonom karar.

#### `default_photo_url` güncelleme SQL'i (hazır, **uygulanmadı** — dosyalar yok)
```sql
-- Berkin 14 dosyayı crop-photos bucket'ına yükledikten SONRA çalıştırılacak.
UPDATE public.crop_config SET default_photo_url =
  'https://efuqpiaavrzimvstpdpm.supabase.co/storage/v1/object/public/crop-photos/zeytinyagi.jpg'
  WHERE crop = 'zeytinyağı';
UPDATE public.crop_config SET default_photo_url =
  'https://efuqpiaavrzimvstpdpm.supabase.co/storage/v1/object/public/crop-photos/nohut.jpg'
  WHERE crop = 'nohut';
UPDATE public.crop_config SET default_photo_url =
  'https://efuqpiaavrzimvstpdpm.supabase.co/storage/v1/object/public/crop-photos/mercimek.jpg'
  WHERE crop = 'mercimek';
UPDATE public.crop_config SET default_photo_url =
  'https://efuqpiaavrzimvstpdpm.supabase.co/storage/v1/object/public/crop-photos/kekik.jpg'
  WHERE crop = 'kekik';
UPDATE public.crop_config SET default_photo_url =
  'https://efuqpiaavrzimvstpdpm.supabase.co/storage/v1/object/public/crop-photos/findik.jpg'
  WHERE crop = 'fındık';
UPDATE public.crop_config SET default_photo_url =
  'https://efuqpiaavrzimvstpdpm.supabase.co/storage/v1/object/public/crop-photos/ceviz.jpg'
  WHERE crop = 'ceviz';
UPDATE public.crop_config SET default_photo_url =
  'https://efuqpiaavrzimvstpdpm.supabase.co/storage/v1/object/public/crop-photos/bugday.jpg'
  WHERE crop = 'buğday';
UPDATE public.crop_config SET default_photo_url =
  'https://efuqpiaavrzimvstpdpm.supabase.co/storage/v1/object/public/crop-photos/domates.jpg'
  WHERE crop = 'domates';
UPDATE public.crop_config SET default_photo_url =
  'https://efuqpiaavrzimvstpdpm.supabase.co/storage/v1/object/public/crop-photos/biber.jpg'
  WHERE crop = 'biber';
UPDATE public.crop_config SET default_photo_url =
  'https://efuqpiaavrzimvstpdpm.supabase.co/storage/v1/object/public/crop-photos/patlican.jpg'
  WHERE crop = 'patlıcan';
UPDATE public.crop_config SET default_photo_url =
  'https://efuqpiaavrzimvstpdpm.supabase.co/storage/v1/object/public/crop-photos/uzum.jpg'
  WHERE crop = 'üzüm';
UPDATE public.crop_config SET default_photo_url =
  'https://efuqpiaavrzimvstpdpm.supabase.co/storage/v1/object/public/crop-photos/incir.jpg'
  WHERE crop = 'incir';
UPDATE public.crop_config SET default_photo_url =
  'https://efuqpiaavrzimvstpdpm.supabase.co/storage/v1/object/public/crop-photos/elma.jpg'
  WHERE crop = 'elma';
UPDATE public.crop_config SET default_photo_url =
  'https://efuqpiaavrzimvstpdpm.supabase.co/storage/v1/object/public/crop-photos/safran.jpg'
  WHERE crop = 'safran';
```

#### Tarif kapak fotoğrafları — eksik listesi
Tüm 18 tarif `cover_photo_url=NULL` ile oluşturuldu (Berkin'in işi, task D
kararı). UI mantığı P23-M2'deki crop fotoğraf fallback'iyle aynı ilkeyi
izlemeli: `recipe.cover_photo_url ?? crop_config.default_photo_url ??
placeholder` (tarifin ilk `is_key_ingredient=true` malzemesinin crop'u
üzerinden). 18 tarifin tamamı bu fallback'e muhtaç — ayrı bir "eksik"
listesi yok, hepsi eksik.

#### "Temsili görsel" etiketi kararı
UI'da bir crop görseli (ya da tarif kapağı fallback'i) **temsili** olarak
kullanıldığında ekranda **"temsili görsel"** etiketi zorunlu — bu M4'in işi
(tarif/ürün yüzeyleri o fazda kuruluyor), ama karar burada kayıt altına
alınıyor: stok fotoğrafını çiftçinin ürünüymüş ya da tarifin kendi çekimi
gibi göstermek, Hasat'ın menşe/güven tezini içeriden çürütür (bkz.
`Build/P23-Mobile.md` → "Fotoğraf stratejisi"). Kural hem crop
`default_photo_url` fallback'i hem de tarif kapak fotoğrafı fallback'i için
aynı şekilde geçerli.
