---
title: Hasat — AI Context
updated: 2026-07-29
tags: [hasat, ai-context]
---

# Hasat AI Context
> Yeni bir Claude / Claude Code oturumuna bu notu ver — anında tam bağlam sağlar.
> **Bu dosya tek bağlam kaynağıdır.** Güncel değilse oturum yanlış varsayımla çalışır — her faz sonunda güncellenmeli.

## Ne?
İki paralel iş kolu:
1. **Dijital:** Türkiye'nin organik/özel ürün çiftçileri için mobil tarım zekası + D2C pazar yeri. ₺99/mo çiftçi sub, %5 GMV komisyon, ₺299/mo alıcı premium.
2. **Çiftlik:** Gerçek hibrit indoor (50m², M24) + outdoor (5.5 dönüm safran + 1.0 dönüm lavanta, M28) — yaşayan lab + gelir kaynağı.

**Kurucu:** Dataroid'de Senior PM. M1–M22 yarı zamanlı, M23'ten itibaren tam zamanlı.

**North Star:** İhtilafsız tamamlanmış sipariş GMV'si (ham hacim değil).

### ⚠️ Crop-agnostic ilkesi
Platform hiçbir crop'a özel muamele göstermez — domates/elma/safran/lavanta hepsi `crop_config`'te eşit statüde (70 crop). Safran iş planında sık geçer çünkü **Çiftlik** iş kolunun ürünü; bu platform stratejisinin safran merkezli olduğu anlamına gelmez. Yeni özellik eklerken: varsayılanlar nötr olmalı, demo/örnek içerik çeşitli olmalı, her feature en az 2–3 crop tipiyle (mainstream + niş + yenilemez) doğrulanmalı. (P25 denetimi, 2026-07-25)

## Çiftlik (Safran + Lavanta) Doğrulanmış Sayıları (Nisan 2026)
> **Not:** Bu tablo Berkin'in KENDİ FİZİKSEL ÇİFTLİĞİNE aittir — Hasat DİJİTAL PLATFORMUNUN genel sayıları değildir. Platform 70 crop'u eşit destekler; safran bunlardan biridir, "amiral gemisi" değildir.

| Parametre | Değer | Güven |
|---|---|---|
| Korm toptan | ₺300/kg (100 kg/dönüm) | Yüksek |
| Safran D2C fiyatı | ₺400/g üretici direkt | Yüksek |
| Safran markalı Y3+ | ₺600–800/g Hasat platform | Orta |
| Safran hal toptan | ₺80–200/g (hedef kanal değil) | Yüksek |
| Y1 outdoor yield | 350g/dönüm (5.5 dönüm = 1,925g) | Yüksek |
| Y2 outdoor yield | 700g/dönüm (5.5 dönüm = 3,850g) | Orta |
| Indoor Y1 yield (50m², 4-kat) | ~400g | Orta |
| TKDK onay süresi | ~7 ay (Ağustos başvurudan) | Yüksek |
| TKDK penceresi | Ağustos–Eylül (yıllık) | Yüksek |

## Zaman Çizelgesi
| Ay | Olay |
|---|---|
| M1 (Mayıs 2026) | Build başlangıcı (yarı zamanlı) |
| M2 (Haziran 2026) | MVP teknik olarak hazır |
| M3 (Temmuz 2026) | GTM hazırlığı: ödeme, foto upload, public URL |
| M4 (Ağustos 2026) | 🚀 Soft launch — ilk gerçek işlem (25 Ağustos) |
| M6 (Ekim 2026) | **Tüm crop'larda** %5 komisyon açılışı (safran hasat sezonuyla aynı aya denk geliyor, ama karar crop-bağımsız) |
| M7 (Kasım 2026) | ₺99/mo sub açılışı |
| M16 | TKDK proje yazımı |
| M18 | TKDK başvurusu (Ağustos penceresi) |
| M23 | **QUIT JOB** — MRR ₺230K |
| M24 | Indoor çiftlik (50m²) |
| M28 | Outdoor safran ekimi (Ağustos) |
| M31 | Outdoor Y1 hasat: ~1,925g · ₺770K |

> P23 (mobil + tarif uygulaması) takvimi ayrı: `Build/Roadmap.md`

## Gelir Modeli
| Akış | Fiyat | Başlangıç |
|---|---|---|
| GMV komisyonu | %5 | Ekim 2026 |
| Farmer subscription | ₺99/mo | Kasım 2026 |
| Buyer premium | ₺299/mo | Ocak 2027 |
| Farm D2C | ₺400–600/g | M27 indoor hasat |

**İlk 3 ay (Ağustos–Ekim 2026): tamamen ücretsiz — GMV yarat, sonra para al.**

---

## Ürün Durumu (28 Temmuz 2026)

**Live:** https://hasat.lovable.app
**Builder:** Lovable AI · **Backend:** Supabase `efuqpiaavrzimvstpdpm`

### Tamamlanan fazlar
| Faz | Konu |
|---|---|
| P9–P14 | AI özellikleri |
| P15 | Teklif state machine (`ball_side`, ping-pong) — tamamlandı |
| P16 | Tüm seri (foto upload altyapısı, public vitrin `/s/$slug` dahil) |
| P17-B/C/E/F/G | Sipariş sonrası akış · karşılıklı değerlendirme · yapılandırılmış RFQ · tekrar sipariş + şube adresleri · 20 KPI view + `/admin/kpi` |
| P18 | Arayüz yenileme |
| P19 + P19-C | Borsa/fiyat deneyimi · İzmir Hal API pilotu + günlük cron |
| P20 | SMS/bildirim genişletmesi + lojistik bildirimleri |
| P21-A/B/C | Çoklu-batch mimarisi (`offer_items`, batch açma, çoklu-batch teklif) |
| P22-A…F | Care Journal → günlükle birleştirildi; crop glossary (70 crop × 204 satır) |
| P24 | Abonelik sistemi denetimi + regresyon düzeltmesi + discoverability |
| P25 | Crop-agnostic denetimi (frontend varsayılanları + vault dokümanları) |

### Açık işler
| İş | Durum |
|---|---|
| **P22-D+E+F tarayıcı QA (15 adım)** | ✅ Tamamlandı (2026-07-29, tüm adımlar geçti) |
| **Şirket tescili** | 🔴 Yapılmadı — hedef ~7 Ağustos. Üç şeyi blokluyor: P17-A, P17-D, ileride store organizasyon hesapları |
| **iyzico başvurusu** | 🔴 Tescil sonrası |
| P17-A — gerçek bloke ödeme (escrow) | 🔴 Şirkete bağlı |
| P17-D — fatura / e-müstahsil | 🔴 Şirkete bağlı |
| Rekabet hukuku danışmanlığı | 🔴 Yapılmadı |
| Glossary insan gözden geçirmesi | 🟡 P22-C içeriği AI üretimi, bölgesel doğrulama yapılmadı |
| `useSetDefaultAddress` diğer adresleri `false`'a çekmiyor | 🟡 Düşük öncelik (P23-M1'de kapanacak) |
| **P23 — Buyer Mobile & Recipe App** | 🟡 M0 + M1 kapandı; **M2 uygulandı (2026-07-29), tarayıcı QA (S20-B) bekliyor** → `Build/Roadmap.md` |

### BENCHMARK Gap durumu
Kapandı: #2 teslim/ihtilaf · #3 değerlendirme · #5 tekrar sipariş · #6 RFQ · #7 hal fiyat bandı · #8 lojistik · #10 bildirimler
Bloke: #1 escrow (P17-A) · #4 fatura (P17-D)
P23'e bağlandı: #9 parselden tabağa QR → M4
Yapılmadı: #11 vade/cari · #12 hasat öncesi finansman

---

## ⚠️ Arz gerçeği (canlı veri, 2026-07-28)

Bu sayılar ürün kararlarını doğrudan etkiliyor — özellikle tarif/tüketici tarafında:

| Ölçüm | Değer |
|---|---|
| `crop_config` toplam crop | 70 |
| Aktif ilanı olan crop | **9** |
| Toplam aktif ilan | 17 (+22 draft) |
| **Fotoğraflı ilan** | **0 / 39** — foto altyapısı var (P16-A), hiç kullanılmamış |
| `min_order` ölçeği | B2B — örn. safran 10 g × ₺900 = ₺9.000 minimum sepet |
| `bireysel` segment alıcı | 1 |

**Sonuçlar:** (a) tarif malzemeleri çoğunlukla eşleşmeyecek → "Talep Et" ana akış, kenar durum değil; (b) tüketici yüzeyinde ilan fotoğrafı yerine `crop_config.default_photo_url` (temsili görsel + etiket) kullanılıyor; (c) talep verisi crop bazında toplandığında **çiftçi kazanım öncelik listesi** oluyor.

**Test verisi uyarısı:** Mevcut ilanlarda tutarsız fiyatlar var (safran hem ₺900/g hem ₺350/kg) ve 1 ilanda `min_order > quantity`. `listings`'te hiç CHECK constraint yok. Tüketiciye açılan yüzeyler öncesi temizlenmeli.

---

## Tech Stack
| Katman | Araç |
|---|---|
| Frontend (web) | React 19 + TanStack Start (**SSR var** → tarif sayfaları SEO'lanabilir) |
| Styling | Tailwind 4 |
| Builder | Lovable AI |
| Backend | Supabase (Auth, DB, Storage, Realtime, Edge Functions, `pg_cron`) |
| Auth | Phone OTP — Twilio WhatsApp/SMS |
| State | TanStack Query |
| SMS | Twilio Edge Function `send-sms` |
| Deploy | hasat.lovable.app |

### Repolar (2026-07-28)
| Repo | İçerik | Kim yazıyor |
|---|---|---|
| `hasat-d2c-marketplace` | Web uygulaması | Lovable (`main`, sync bot `gpt-engineer-app[bot]`) + Claude Code (feature branch → PR) |
| `hasat-mobile` | Mobil (Expo + Expo Router + Nativewind) — **M5'te açılacak** | %100 Claude Code (Lovable RN üretemiyor) |
| `hasat-core` | Paylaşılan TS — **açıldı 2026-07-29 (M1-b).** Şu an içinde: DB tipleri, design token'ları, `convertQuantity`. Sorgu hook'ları + zod + storage adapter M5'e bırakıldı | Claude Code; subtree ile web'e iner (mobil hedefi M5'te) |
| `hasat-vault` | İş notları, roadmap, dokümanlar (**public** — kod/sır yok) | Claude Code PR + Berkin merge |

### Mobil stack (M5+)
- **Expo + EAS Build** — şirket Mac'i olduğu için local Xcode/imzalama yönetilemiyor; EAS bulutta derliyor ve submit ediyor
- **Expo Router** (dosya tabanlı) · **Nativewind** (Tailwind sözdizimi)
- **TanStack Query** web ile ortak (React Native'de çalışıyor)
- Oturum: `expo-secure-store` (web'de `localStorage`) — adapter sınırı
- Push: `device_tokens` + APNs (iOS) / FCM (Android); `notif_channel` enum'unda `push` **zaten mevcut**
- **Mobil v1'de checkout YOK** — akış "Talep Et"te biter, ödeme web'de

### Mimari ilke
İki client'ın da ihtiyaç duyduğu mantık **veritabanında** (RPC/view) yaşar; monorepo kurulmaz (Lovable sync'ini kırma riski). Detay: `Build/Shared-Architecture.md`.

---

## DB Kritik Notlar
- Journal tablosu = `harvest_entries` (`journal_entries` değil)
- **`harvest_entries` iki işi birden yapıyor:** gerçek hasat + rutin bakım kaydı (P22-F). Rutin bakım satırları `quantity=0`. Bu tabloyu okuyan her yer bu ayrımı filtrelemek zorunda — P22-F sonrası 4 gerçek regresyon bu yüzden çıktı.
- **Crop adı kanonik formu = `crop_config.crop` (lowercase slug)**, `display_name` değil. Karışık case P21-A'da backfill ile temizlendi; yeni eşleştirme mantığı slug'a normalize etmeli.
- **`unit_type` enum yalnızca `g`, `kg`, `L`** — `adet` YOK. Ama `crop_config.default_unit` (text) Safran Soğanı için `'adet'` tutuyor → gizli insert hatası riski (P23-M1'de kapanacak).
- Culinary birimler (`adet`, `demet`, `kaşık`) `unit_type`'a **eklenmeyecek** — P21'in birim-uyuşmazlığı trigger'ını kirletir. P23-M2'de bu kural şemaya gömüldü: culinary birim `recipe_ingredients.unit`'te düz text, dönüşüm yalnızca `crop_culinary_meta.conversion_hints` ile alışveriş listesi sınırında.
- **Tarif katmanı (P23-M2, 2026-07-29):** `recipes`, `recipe_steps`, `recipe_ingredients`, `crop_culinary_meta`, `recipe_saves`, `recipe_rfq_links`, `device_tokens` + `crop_config.default_photo_url` + `crop-photos` public bucket. Mantık DB'de: `fn_culinary_to_canonical`, `rpc_recipe_availability`, `rpc_recipe_shopping_list`, `v_recipe_coverage`, `v_kpi_recipe_funnel`.
- **Malzeme→crop eşleştirmesi runtime'da fuzzy text matching ile YAPILMAZ** — `recipe_ingredients.crop` editoryal olarak bir kez doldurulur; `extract-recipe` bu alanı daima `null` bırakır.
- **"Admin" diye bir DB rolü yok.** `profiles.role` yalnızca `farmer`/`buyer`; `is_admin()` fonksiyonu yok. Admin erişimi = service-role anahtarlı edge function (`admin-kpi` + `x-admin-key`). "Yazma sadece admin" pratikte "hiç politika yazma" demek — service_role RLS'i baypas eder.
- **`v_kpi_recipe_funnel` uçtan uca SERT JOIN (P23-M2-ek, 2026-07-29).** Beş basamak: `recipe_views` → `recipe_saves` → (`recipe_rfq_links`→`crop_requests` = malzeme yok yolu | `offers.source_recipe_id` = malzeme var yolu) → `orders.offer_id`. **Sezgisel atıf YOK** — önceki sürümdeki "aynı alıcı + aynı crop + talepten sonra" çıkarımı fazla atıf ürettiği için kaldırıldı. `crop_requests` ile `offers`/`orders` arasında hâlâ FK yok; bu yüzden teklif/sipariş atfı `offers.source_recipe_id` üzerinden yürüyor.
- **`recipe_views`** (P23-M2-ek): görüntüleme olayı. IP/user-agent loglanmaz (KVKK). INSERT anon dahil serbest, SELECT yalnızca service_role.
- **`offers.source_recipe_id`** (P23-M2-ek): nullable FK → `recipes`. `offers.subscription_id` ile aynı konvansiyon — "bu teklif nereden doğdu".
- **`recipes.author_type`** artık `kullanici` değerini de kabul ediyor — AI ile içe aktarılan tarifler editoryal korpustan böyle ayrılıyor.
- Parsel konum = `location_label`
- Community yazar = `author_id`
- Phone format = `905XXXXXXXXX` (+ prefix'siz)
- Offers: `ball_side`, `current_price`, `current_quantity`, `payment_status` (P15); `offer_messages` (P15); `offer_items` (P21-C, her offer'ın ≥1 item'ı var)
- `listing_harvest_entries` — batch ↔ hasat kaydı bağlantısı; `tg_enforce_link_unit_match` birim uyuşmazlığını engelliyor
- `buyer_profiles.company_name` **NOT NULL** — `bireysel` segment için onboarding sürtünmesi (P23-M1'de nullable olacak)
- `listings`'te **hiç CHECK constraint yok**
- `pg_cron`: jobid=1 İzmir Hal fiyat sync (06:00 UTC), jobid=2 abonelik hasat hatırlatması (07:00 UTC)
- → Detay: `Build/DB-Schema.md`

## Test Kullanıcıları
| Rol | Telefon | UUID |
|---|---|---|
| Farmer (Ahmet Yılmaz) | 905001234567 | 0868e4fe-86d2-4c5d-8ba5-f15fd4fac146 |
| Buyer (Zeynep Kaya) | 905009876543 | 032eb467-661d-4df4-adf5-3d277d9b6549 |

OTP test: `123456`

> ⚠️ Bu repo **public**. Test hesabı bilgileri burada duruyor — canlıya çıkmadan önce test OTP bypass'ının production'da geçerli OLMADIĞI doğrulanmalı. Berkin'in gerçek bildirim telefonu bu dosyaya **yazılmamalı**.

## Lovable Prompt Kuralları
1. Her prompt'a schema doğrulaması ekle (Lovable yanlış kolon adı tahmin eder)
2. `ALTER TABLE ... ADD COLUMN IF NOT EXISTS` — migration yok
3. RLS politikaları: her yeni insert için policy gerekebilir; **UPDATE için ayrı policy şart** (SELECT/INSERT kapsamıyor)
4. UUID'leri hardcode et — phone lookup ambiguous column hatası verir
5. **Lovable'ın metnine güvenilmez** — `get_diff` / gerçek SQL / canlı aksiyon ile doğrula. `plan_mode=true` güvenilir şekilde durmuyor.
6. **`src/lib/core/` altına dokunulmaz** — `hasat-core`'dan gelir (M1'den sonra geçerli)
> Tam liste (#1–#106): `TODO.md` → "Lovable/Supabase Prompt Yazma Kuralları"

---

## Kararlar

| Konu | Karar |
|---|---|
| **Şirket tipi** | 🔴 **AÇIK.** "Şahıs şirketi" varsayımı sorgulandı: Apple, organizasyon hesabı için **tüzel kişilik** şartı koyuyor — şahıs şirketi bireysel kaydolmak zorunda (App Store satıcı adı = kişisel ad, "Hasat" değil). Mali müşavir görüşü bekleniyor. Detay: `Build/Store-Compliance.md` |
| Ödeme | iyzico; onay süresince IBAN köprüsü |
| Apple Developer hesabı | **Bireysel, şimdi** ($99, D-U-N-S gerekmiyor, şirketten bağımsız) — Apple'ı kritik yoldan çıkarıyor |
| Play hesap tipi | M5'te karar (personal $25 → 12 tester×14 gün · organization → muaf ama D-U-N-S) |
| İlk 3 ay | Ücretsiz |
| Mobil platform | **Expo** (Capacitor değil — şirket Mac'i, local imzalama yok) |
| Mobil v1 kapsamı | **Checkout yok** — ödeme blokajını uygulamadan izole eder, Guideline 2.1 riskini kaldırır |
| Fiyat kilidi (`price_lock`) | Şimdilik sadece UI önerisi, enforcement yok (gerçek para akmıyor) |
| İzlenebilirlik rozeti | Rutin bakım kayıtları da rozeti yükseltiyor — bilinçli (daha kolay günlük = daha fazla belgeleme) |
| Sosyal medya | Berkin + Claude haftalık içerik desteği |

---

## Dosya Haritası
```
hasat-vault/
├── _Context.md              ← bu dosya (tek bağlam kaynağı)
├── TODO.md                  ← master roadmap + build log + kural #1-106
├── Build/                   ← (doğrulandı 2026-07-28)
│   ├── Roadmap.md              ← P23 görsel Gantt + kilometre taşları
│   ├── P23-Mobile.md           ← P23 kapsam, şema, M0-M9
│   ├── Shared-Architecture.md  ← web+mobil paylaşım mimarisi
│   ├── Store-Compliance.md     ← Apple 4.2, hesap tipleri, IAP, submit checklist
│   ├── DB-Schema.md            ← tablo + enum referansı
│   ├── App-Audit.md            ← bug + improvement takibi
│   ├── MCP-Referans.md
│   └── E2E-QA.md
├── Research/                ← Market.md doğrulandı; diğerleri kontrol edilmedi
└── Finance/
    └── Model.md                ← konsolide finansal model (v0.6)
```
