---
title: Hasat — DB Schema Referansı
updated: 2026-06-30
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
`id` uuid · `user_id` uuid (FK→auth.users) · `company_name` · `company_type` enum · `monthly_volume` numeric  
⚠️ user_id'de UNIQUE constraint yok — ON CONFLICT çalışmaz, DELETE+INSERT yap

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
