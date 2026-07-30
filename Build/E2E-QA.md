---
title: Hasat — E2E QA Test Dokümanı
updated: 2026-07-30
tags:
  - hasat
  - qa
  - e2e
  - test
---

# Hasat — E2E QA Test Dokümanı

**Bu bir "yaşayan" dokümandır.** Her yeni feature/prompt sonrası, ilgili test senaryosu bu dosyaya eklenir ve gerçekten çalıştırılır. Berkin ve Claude bu dosyayı birlikte kullanır — Claude senaryoyu tasarlar+çalıştırır, Berkin sonucu görür ve gerekince müdahale eder.

**Tasarım ilkesi: Berkin'in müdahalesi minimum olmalı.** Bu yüzden otomasyonun merkezi, **sabit OTP'li seed hesaplar** (Ahmet/Zeynep) — bunlarla Claude, Berkin'in hiçbir SMS/tıklama işlemi olmadan rol değiştirip uçtan uca senaryo koşabilir.

---

## Otomasyon Mimarisi

| Araç | Ne için | Kim gibi çalışır |
|---|---|---|
| **Hasat MCP** (`hasat.lovable.app/mcp`) | Gerçek kullanıcı aksiyonları | O an OAuth ile bağlı **tek bir gerçek kullanıcı** (RLS ile doğal sınırlı) |
| **Supabase MCP** (admin) | DB doğrulama, temizlik, RLS denetimi | Admin/service-role — **asla kullanıcı simülasyonu için kullanılmaz** |

**Not (2026-07-16):** Artık 10 çiftçi + 10 alıcı mock data mevcut (bkz. TODO.md) — Ahmet/Zeynep dışında da gerçekçi 2 yıllık geçmişi olan hesaplar var (Mehmet/Hüseyin/Ayşe/Mustafa/Fatma domates kümesi, İbrahim lavanta, Osman patates, Emine elma, Zehra safran). Hasat MCP hâlâ sadece Ahmet/Zeynep'in sabit-OTP'siyle bağlanabiliyor (diğerleri dinamik OTP'siz, MCP login'i yok) — bu yüzden diğer mock çiftçiler/alıcılar için testler Supabase MCP üzerinden veri-katmanı simülasyonuyla yapılıyor.

---

## Test Hesapları

### 🟢 Birincil otomasyon hesapları (SABİT OTP)
| Rol | Telefon | OTP |
|---|---|---|
| Çiftçi | `05001234567` | `123456` (Ahmet Yılmaz) |
| Alıcı | `05009876543` | `123456` (Zeynep Kaya) |

### 🟡 İkincil hesaplar (DİNAMİK OTP — Berkin'in SMS'i gerekir)
| Rol | Telefon |
|---|---|
| Çiftçi | `05421241011` |
| Alıcı | `05398545294` |

---

## Rol Değiştirme Prosedürü (Hasat MCP)

1. Berkin: Connectors → Hasat → Disconnect → Add custom connector (aynı URL) → Connect
2. İlgili sabit-OTP hesabın telefonunu gir → `123456` → İzin Ver
3. Claude: `tool_search` ile tool listesini tazele

**Bilinen tuhaflık:** MCP yazma tool'larında aralıklı **"No approval received"** hatası — 1-2 retry sonrası kendiliğinden geçiyor. `Supabase:list_tables` tool'unda da aynı davranış gözlendi (2026-07-16) — `execute_sql` ile `information_schema.tables` sorgusuna geçilerek atlatıldı.

---

## Test Senaryo Kataloğu

### S1-S7
(Detaylar değişmedi — bkz. önceki sürüm. Özet: Parsel→Draft→Yayınla ✅, Teklif/Pazarlık/Ödeme ✅, Fiyat Özeti ✅, Onboarding ✅, Topluluk Moderasyonu ✅ bug bulundu+düzeltildi, Landing ✅, Sistematik RLS Denetimi ✅ 2 kritik+2 orta bulgu düzeltildi.)

### S8 — Global Fiyatlar Sayfası (Tüm Ürünler)
- **Adımlar:** 5 mock ürün (domates/patates/elma/safran/lavanta) için `get_price_history_summary()` tek seferde çağrıldı.
- **Son çalıştırma:** 2026-07-16 ✅ **TAM GEÇTİ** — Domates 5 çiftçiyle gerçek agregasyon (`insufficient_data:false`, ort. ₺25,40), diğer 4 ürün doğru şekilde "yeterli veri yok" (1-2 çiftçi). `official_data` hepsinde `null` (HKS otomasyonu Faz 2, henüz veri yok — hata değil, beklenen).

### S9 — Keşfet'te Gerçek Üretici İsmi Gösterimi
- **Adımlar:** Tüm aktif ilanlar `public_farmer_profiles` ile join simüle edildi (gerçek `useActiveListings` mantığı).
- **Son çalıştırma:** 2026-07-16 ✅ **TAM GEÇTİ** — 17/17 aktif ilan doğru isim/şehirle eşleşti, hiç "Üretici" fallback'i yok.
- **Yan bulgu ve düzeltme:** Ahmet Yılmaz'ın `profiles.city`'si önceki bir testte yanlışlıkla "Antalya" olmuştu (gerçeği Safranbolu, Karabük) — düzeltildi.
- **Not:** "Berkin Savcıözen" (eski dinamik-OTP test hesabı) üzerinde 12 Temmuz tarihli, mock trend'le tesadüfen aynı fiyatlı yalnız bir "Domates" ilanı bulundu — zararsız test artığı, hiçbir sipariş/ödemeye bağlı değil, agregasyonu etkilemiyor, temizlenmedi (önemsiz).

### S10 — Parsel/İlan Fotoğrafı Gösterimi
- **Adımlar:** Bucket public-fix sonrası, gerçek yüklenmiş bir fotoğrafın (Ahmet'in "Kuzey Tarla" parseli) tarayıcıda gerçekten yüklenip yüklenmediği kontrol edildi.
- **Son çalıştırma:** 2026-07-16 ✅ **TAM GEÇTİ** — Berkin'in canlı ekran görüntüsüyle doğrulandı, fotoğraf "Parseli Düzenle" modalinde doğru görünüyor. (Claude'un kendi `web_fetch` tool'u bu URL'i doğrulayamadı — Supabase storage URL'leri önceden görülmemiş domain kısıtı yüzünden erişilemiyor, bu yüzden görsel doğrulama Berkin'in ekran görüntüsüyle tamamlandı.)

### S11 — Alıcı "Görüşmeler" Gelen Kutusu
- **Adımlar:** Zeynep'in tüm tekliflerinin `public_farmer_profiles` + `listings` + `offer_messages` ile birleştirilmiş hali (gerçek `useBuyerConversations()` mantığı) simüle edildi.
- **Son çalıştırma:** 2026-07-16 ✅ **TAM GEÇTİ** — 10 teklif, hepsi doğru üretici adı/ürün/durumla eşleşti. `last_message: null` olan kayıtlar beklenen (doğrudan kabul edilmiş, mesajlaşma adımı olmayan teklifler) — mesajlı senaryo zaten S2'de (Kekik pazarlığı) ayrıca doğrulanmıştı.

### S12 — Alıcı Raporlar Veri Doğruluğu
- **Adımlar:** `isPaidOrder()`/`orderRowTotal()` mantığı gerçek mock siparişlere karşı test edildi.
- **Son çalıştırma:** 2026-07-16 ✅ **GEÇTİ** — Tüm mock siparişler doğru şekilde `delivered`+`paid`. `current_price` düzeltmesi yapısal olarak doğru; mock veride hiçbir teklif pazarlıkla değişmediği için (hepsi direkt kabul) `price_per_unit`==`current_price`, gerçek fark daha önce S2'de (₺700→₺800 pazarlığı) kanıtlanmıştı.

### S13 — Alıcı Abonelikler
- **Adımlar:** Zeynep→Ahmet arası gerçek bir `harvest_subscriptions` satırı oluşturuldu (MCP tool'u yok, Supabase MCP ile).
- **Son çalıştırma:** 2026-07-16 ✅ **TAM GEÇTİ** — Abonelik oluşturuldu (kalıcı bırakıldı, gerçek/temsili veri). Kritik ek doğrulama: `profiles` RLS policy'sinin "Buyers read related farmer profiles" kuralı `harvest_subscriptions` ilişkisini açıkça kapsıyor — yani Keşfet'teki "Üretici" regresyonu bu sayfada YAŞANMAZ (ilişki zaten var olduğu için join çalışır).

### S14 — Günlük Fotoğraf Yükleme (Bug Bulundu + Düzeltildi)
- **Adımlar:** `farmer.journal.new.tsx` kod incelemesi.
- **Son çalıştırma (fix öncesi):** 2026-07-16 ❌ **BUG BULUNDU** — Dosya seçici UI tam çalışıyormuş gibi görünüyordu ama `save()` her zaman `photos: []` gönderiyordu, `useCreateEntry()`'de hiç upload mantığı yoktu. Kullanıcı hiç fark etmeden fotoğrafı sessizce kaybediyordu.
- **Son çalıştırma (fix sonrası):** 2026-07-16 ✅ `uploadHarvestPhotos()` eklendi (parsel/ilan yükleyicileriyle aynı desen, `harvest-photos` bucket'ına), tsgo temiz. Canlı uçtan uca test (gerçek dosya seçip kaydetme) bir sonraki oturumda yapılabilir.

### S15 — Parti (Batch) Sayfası Görsel Zenginlik
- **Son çalıştırma:** 2026-07-16 ✅ İlanın kapak fotoğrafı carousel'i eklendi. `ProvenanceTimeline`'ın hasat fotoğrafı render mantığı zaten doğruydu (S14 düzelince otomatik veri alacak).

### S16 — Üretici Detay Sayfası (Kritik Bug Bulundu + Düzeltildi)
- **Adımlar:** `buyer.producer.$id.tsx` kod incelemesi.
- **Son çalıştırma (fix öncesi):** 2026-07-16 ❌ **KRİTİK BUG** — Sayfa `useHasat` (eski prototip Zustand store) üzerinden tamamen sahte veri gösteriyordu (rating, yorumlar, verim geçmişi, sonraki hasat, sipariş sayısı — hepsi hardcoded/kurgusal). Gerçek bir çiftçi ID'siyle 404 veriyordu ya da yanlış bilgi gösteriyordu. "Alıcı Yorumları" özelliğinin DB'de (`reviews`/`ratings` tablosu) hiç karşılığı olmadığı doğrulandı.
- **Son çalıştırma (fix sonrası):** 2026-07-16 ✅ Sayfa `public_farmer_profiles`, gerçek aktif ilanlar/parseller, `harvest_entries`'ten türetilen verim/kalite istatistikleri, gerçek sipariş sayısı, gerçek abonelik (varsa) veya `crop_config` hasat penceresi tahminiyle (yoksa) yeniden inşa edildi. Sahte "Alıcı Yorumları", GPS rozeti, "0 Anlaşmazlık" pill'i tamamen kaldırıldı (kullanıcı kararıyla — sahte veri yerine dürüstçe kaldırma). tsgo temiz.

### S17 — Abonelik Oluştur Sayfası (Kritik Bug Bulundu + Düzeltildi)
- **Adımlar:** `buyer.subscription.$producerId.tsx` kod incelemesi.
- **Son çalıştırma (fix öncesi):** 2026-07-16 ❌ **KRİTİK BUG** — Aynı sahte `useHasat` store'unu kullanıyordu VE "Abonelik Oluştur" butonu gerçek `useCreateSubscription()` mutasyonunu hiç çağırmıyordu, sadece client-only bir Zustand action'ı (`addSubscription`) çalıştırıyordu. **Bu sayfadan geçen hiçbir alıcı gerçekte hiçbir zaman abone olmuyordu.**
- **Son çalıştırma (fix sonrası):** 2026-07-16 ✅ Gerçek çiftçi/ilan verisine bağlandı (primary crop + referans fiyat gerçek aktif ilandan), gerçek `useCreateSubscription()` mutasyonu çağrılıyor, hata durumunda toast, sadece başarılı mutation sonrası dialog açılıyor. tsgo temiz. Canlı create-then-cleanup testi yapılmadı (mantık S13'te zaten doğrulanan `useCreateSubscription` ile birebir aynı) — düşük risk.

### S18 — P23-M1-a Şema Borçları (Çiftçi + Alıcı Akışları)

**Arka plan:** Bu 4 madde salt veritabanı düzeltmesi — arka planda (Supabase MCP ile) gerçek insert/update denemeleriyle doğrulandı. Aşağıdaki adımlar Berkin'in **uygulama üzerinden** aynı davranışı bir kullanıcı gibi görmesi için.

1. **Safran Soğanı birim düzeltmesi**
   - Çiftçi (Ahmet) → Depo/Vitrin → "+ Yeni Ürün" → Ürün olarak "Safran Soğanı" seç.
   - **Beklenen:** Birim alanı artık "kg" ile geliyor (önceden "adet" gelip kayıtta sessiz hataya yol açabiliyordu). Ürünü kaydet, hatasız kaydedildiğini doğrula.

2. **Min. sipariş, stoktan büyük olamaz**
   - Çiftçi → "+ Yeni Ürün" → herhangi bir ürün seç → Miktar: 1 kg, Min. Sipariş: 5 kg gir → Kaydet.
   - **Beklenen:** Kayıt reddedilir, bir hata görülür (min. sipariş miktardan büyük olamaz anlamında) — ürün oluşturulmaz.
   - Ardından: mevcut bir ilanı aç (ör. Ahmet'in "Kekik" ilanı) ve normal satış/pazarlık akışıyla stoğun azaldığı bir senaryoyu (P21 teklif kabul akışı) çalıştır.
   - **Beklenen:** Stok azaltma **engellenmiyor** — bu ilan zaten min. siparişin üstünde açılmıştı, satış devam edebiliyor (yeni kural sadece *yeni ilan oluşturmayı* koruyor, mevcut ilanların güncellenmesini engellemiyor).

3. **Bireysel alıcı onboarding — şirket adı olmadan**
   - Yeni bir alıcı olarak kayıt ol → Segment: Bireysel (ev kullanıcısı) seç → Şirket adı alanını boş bırak → Devam Et.
   - **Beklenen:** Profil hatasız oluşturuluyor, "şirket adı zorunlu" hatası artık çıkmıyor.

4. **Varsayılan adres — diğerleri otomatik kalkıyor**
   - Alıcı (Zeynep) → Adreslerim → "Ev" adresini ekle, "Varsayılan yap"a tıkla.
   - Aynı ekrandan "İş" adresini ekle, "Varsayılan yap"a tıkla.
   - Adres listesine dön.
   - **Beklenen:** Sadece "İş" adresinin yanında varsayılan rozeti görünüyor; "Ev" artık varsayılan değil (önceden ikisi de varsayılan kalabiliyordu).

**Backend doğrulama (2026-07-28, Supabase MCP ile tamamlandı):** Her 4 madde gerçek SQL/insert/update denemesiyle test edildi — geçersiz `min_order` insert'i reddedildi, geçerli insert kabul edildi, mevcut ilanın stok-azaltma UPDATE'i engellenmedi; `buyer_profiles` NULL `company_name` ile insert kabul edildi; `buyer_addresses`'te ardışık INSERT/UPDATE ile default bayrağının doğru şekilde tek satırda kaldığı doğrulandı; tüm test verisi temizlendi. Ayrıntılar: PR açıklaması + `Build/DB-Schema.md` → "P23-M1-a — Şema Borçları Kapatıldı".

### S19 — P22-G Rutin Bakım Tarih/Filtre Düzeltmesi + Trigger Temizliği

**Arka plan:** Berkin'in canlı testinde bulduğu 4 semptom (tarih bilgisi yok, filtre süzmüyor, "Yaptım" listeden düşmüyor, tarih girilemiyor) + `buyer_addresses` çift trigger + P22-E SMS eksikliği. Kök neden: hesap zaten doğru kaynağı okuyordu ama `useCreateEntry` ilgili sorguyu invalidate etmiyordu; hesap ayrıca client'ta yaşıyordu (kural #106 ihlali) — `v_routine_maintenance_status` view'ına taşındı. Ayrıntılar: [PR #5](https://github.com/berkinsavciozen/hasat-d2c-marketplace/pull/5).

1. **DB doğrulaması (2026-07-28, Supabase MCP ile tamamlandı):** `v_routine_maintenance_status` gerçek veriyle 4 senaryoda test edildi — aynı parselde 2 farklı crop (Anason Armut Parsel: armut+anason, QA Test Parseli: safran+lavanta, ikisi de mevcut gerçek veri), sıklığı olan iş (Sulama Yap, freq=1 — gerçek `last_performed_date`/`next_due_date` doğru hesaplandı), sıklığı olmayan olay-bazlı iş (İlaçlama Yap — geçici pref+kayıt eklenip `is_event_based=true` doğrulanıp temizlendi), gecikmiş iş (Kekik parseli, 10 gün önce yapılmış sulama — `is_overdue=true` doğrulanıp temizlendi). `buyer_addresses` tek-trigger davranışı Zeynep'in adresleriyle yeniden doğrulandı (iki adres, sırayla varsayılan yapıldı, tek satır varsayılan kaldı, `updated_at` doğru damgalandı, test verisi temizlendi).
2. **SMS doğrulaması (gerçek Twilio testi):** Çiftçi "Yeni Ürün Türü Talebi" formuyla test kaydı oluşturuldu (`net._http_response` id=54) — SMS artık ürün adı, birim, kategori, hasat ayı aralığı ve notu içeriyor (öncesinde sadece ürün adıydı, bkz. id=53'teki eski format). Buyer'ın katalog-dışı SMS'i de doğrudan `notify-admin` çağrısıyla test edildi (id=55) — not artık mesajda. Test verisi temizlendi.
3. **Frontend:** `tsc --noEmit` temiz. Tarayıcı/dokunma testi bu oturumun ağ kısıtlaması yüzünden yapılamadı (bkz. S14 notu) — aşağıdaki adımlarla Berkin'in doğrulaması gerekiyor:
   - `/farmer/journal` → Rutin Bakım sekmesinde her satırda "Son: … · Sıradaki: …" görünüyor mu?
   - "✓ Yaptım" formunda tarih seçici var mı, geçmiş bir tarih girilebiliyor mu?
   - Bir işi "Yaptım" dedikten sonra (sıradaki tarihi bu hafta içine düşmüyorsa) Bugün/Bu Hafta'da kayboluyor, Tümü'nde görünmeye devam ediyor mu?
   - Sıklığı olmayan bir iş (İlaçlama Yap gibi) her filtrede her zaman görünüyor mu?
   - Çiftçinin yeni ürün türü talebi + buyer'ın katalog-dışı talebi SMS'te artık daha fazla alan içeriyor mu?

---

### S20 — P23-M2 Tarif Backend'i (ekleyici)

> ⚠️ **Bu tur bilinçli olarak "az tarayıcı adımı" içeriyor.** P23-M2 tamamen backend: 7 tablo, RLS, 1 fonksiyon + 2 RPC + 2 view, 1 storage bucket ve bir edge function. Tarif arayüzü **M4'te** doğacak — yani bu turda ekranda görünecek yeni bir şey yok. Bu yüzden aşağıdaki test case'in ağırlığı **regresyon kontrolü** (mevcut akışlar bozulmadı mı) üzerinde; yeni özellik doğrulaması **veri katmanında** yapıldı ve sonuçları burada listelendi.

**Arka plan:** `Build/P23-Mobile.md` M2. Kapsam kuralı: canlı akışlara (teklif/sipariş/ilan/günlük) dokunulmadı, `unit_type` enum'una dokunulmadı, hiçbir mevcut davranış değiştirilmedi. Ayrıntılı şema: `Build/DB-Schema.md` → "P23-M2 — Tarif Backend'i".

#### A. Veri katmanı doğrulaması (2026-07-29, Claude Code + Supabase MCP ile tamamlandı — kural #96)

Tüm testler canlı DB'de gerçek SQL/insert ile koşuldu, test verisi sonunda **tamamen silindi** (kalan: 0 tarif, 0 malzeme, 0 adım, 0 kayıt, 0 bağ, 0 token; yalnızca 70 satırlık `crop_culinary_meta` seed'i kaldı — kalıcı referans verisi).

1. **Birim dönüşümü (`fn_culinary_to_canonical`) — 12 vaka**
   `domates 3 adet → 0,360 kg` · `domates 2 yemek kaşığı → 0,030 kg` · `domates 500 g → 0,500 kg` (metrik, ipucu gerekmedi) · `kekik 1 çay kaşığı → 0,001 kg` · `kekik 1 demet → 0,025 kg` · `zeytinyağı 250 ml → 0,250 L` · `safran 2 g → 2 g` · `" Adet "` (boşluklu, büyük harf) → 0,120 kg (normalize ediliyor).
   **Uydurmama testi:** `domates 1 demet` (ipucu yok) → **NULL** · `pamuk 1 adet` → **NULL** · `safran 1 tutam` (M3'e bırakılan ipucu) → **NULL** · bilinmeyen crop → **NULL**. ✅ 12/12.

2. **3 crop testi — crop-agnostic ilkesi (mainstream + niş + yenilemez)**
   Test tarifine `domates` (mainstream), `kekik` (niş), `pamuk` (yenilemez), `tuz` (platform-dışı), `safran` ve `lavanta` malzemeleri kondu.
   - `rpc_recipe_availability` sonucunda **`pamuk` satırı hiç görünmedi** — `is_edible=false` filtresi kanıtlandı. ✅
   - `domates`: 5 aktif ilan, en iyi fiyat **₺25.500/kg** (ilan ₺25,50/g → kanonik kg'ye doğru çevrildi). ✅
   - `kekik`: 1 aktif ilan, ₺100/kg. ✅
   - `tuz`: `is_platform_crop=false`, nötr satır olarak kaldı. ✅

3. **Alışveriş listesi (`rpc_recipe_shopping_list`)**
   - **Porsiyon ölçekleme:** tarif 4 kişilik, 8 kişilik istendi → `scale_factor=2`, domates 3 adet → 6 adet → 0,720 kg. ✅
   - **min_order yuvarlaması (görevdeki birebir örnek):** `lavanta` — tarif **2 g** istiyor, ilanın min_order'ı **10 g** → alınacak **10 g**, `rounded_up_to_min_order=true`, **`recipes_covered = 5,00`**. ✅
   - Dönüşüm ipucu olmayan `tuz` satırında `needed_canonical=NULL`, `conversion_available=false` — UI "miktar hesaplanamadı" diyebilir, uydurulmuyor. ✅

4. **Kapsama view'ı (`v_recipe_coverage`)** — 6 malzemeli test tarifi için `ingredient_count=5` (pamuk sayıma girmedi), `off_platform_count=1` (tuz), `available_count=4`, `coverage_pct=100`. ✅

5. **Huni view'ı (`v_kpi_recipe_funnel`)** — gerçek veriyle: 1 kayıt, 1 tarif-atıflı talep, **1 teklif** (Zeynep'in 2 kalemli tek safran teklifi `DISTINCT` ile bir kez sayıldı, çift saymadı), 0 sipariş (o teklif siparişe dönmemişti). ✅ `recipe_views` NULL — M4'e bırakıldı (bkz. DB-Schema notu).

6. **RLS — anon ve authenticated rolleri gerçekten simüle edilerek**

   | Test | Sonuç |
   |---|---|
   | **anon** public+published tarifi görüyor | ✅ 1 satır |
   | **anon private tarifi GÖREMİYOR** | ✅ **0 satır** |
   | anon, private tarifin malzemelerini doğrudan sorguluyor | ✅ 0 satır |
   | anon, private tarif için `rpc_recipe_availability` çağırıyor | ✅ **0 satır** (SECURITY INVOKER — RPC sızdırmıyor) |
   | anon, private tarif için `rpc_recipe_shopping_list` çağırıyor | ✅ 0 satır |
   | anon, `v_recipe_coverage`'da private tarifi arıyor | ✅ 0 satır (`security_invoker=true` çalışıyor) |
   | anon `crop_culinary_meta` okuyor | ✅ 70 satır (tarif sayfaları için şart) |
   | **sahibi (Zeynep)** kendi private tarifini görüyor | ✅ 1 satır |
   | **başkası (Ahmet)** Zeynep'in private tarifini görüyor mu | ✅ **0 satır** |

7. **Mutasyon testleri — gerçek INSERT/UPDATE ile (kural #97)**

   | Test | Sonuç |
   |---|---|
   | Kullanıcı `visibility='public'` tarif yazmaya çalışıyor | ✅ **Reddedildi** (`42501 row-level security`) |
   | Kullanıcı başkasının adına tarif yazmaya çalışıyor | ✅ Reddedildi |
   | Kullanıcı kendi private tarifini yazıyor | ✅ Kabul |
   | **Kendi tarifinde UPDATE gerçekten 1 satır etkiliyor** | ✅ **1 satır** (orders'taki sessiz-sıfır hatası yok) |
   | Kullanıcı kendi private tarifini public'e çevirmeye çalışıyor | ✅ **Reddedildi** — public korpus korumalı |
   | Editoryal (sahipsiz) tarifi UPDATE etmeye çalışıyor | ✅ 0 satır |
   | `recipe_saves` / `device_tokens` kendi satırında INSERT + UPDATE | ✅ Çalışıyor, UPDATE 1 satır |
   | Başkasının `device_tokens` satırını görme | ✅ 0 satır |
   | `recipe_rfq_links` kendi talebine bağ kurma | ✅ Kabul |

8. **Edge function `extract-recipe` — hem metin hem görsel girdiyle gerçekten çağrıldı**
   (Bu oturumda dış HTTPS egress politikayla kapalı olduğu için çağrılar veritabanı üzerinden `pg_net` ile, **gerçek kullanıcı JWT'siyle** yapıldı — sabit-OTP ile alınan Zeynep oturumu.)

   | Çağrı | Sonuç |
   |---|---|
   | **Metin girdisi** ("Kekikli Fırın Domates" yapıştırma) | ✅ HTTP 200 — 5 malzeme, 4 adım, `servings=4`, `prep=15`, `cook=40` |
   | **Görsel girdisi** (yazılı tarif fotoğrafı, Türkçe, "Mercimek Çorbası") | ✅ HTTP 200 — OCR+vision doğru okudu: 5 malzeme, 4 adım, `servings=4`, `prep=10`, `cook=30`, `source_type='photo'` |
   | Client `visibility:'public'`, `status:'published'`, `owner_id:<başka kullanıcı>` gönderiyor | ✅ **Sunucu hepsini yok saydı** — kayıt `private` / `draft` / `owner_id=JWT'nin sahibi` oldu |
   | `extraction_confidence` dolduruldu mu | ✅ Üç çağrıda da dolu |
   | Malzemeler bir crop'a bağlandı mı | ✅ **Hayır** — `crop_bagli=0`, runtime fuzzy eşleştirme yasağına uyuluyor |
   | Kota mevcut altyapıyla mı sayıldı | ✅ `ai_usage_tracking.message_count = 3` (tam 3 başarılı çağrı) — yeni kota sistemi kurulmadı |

9. **Storage** — `crop-photos` bucket'ı doğrudan SQL ile açıldı; `SELECT public FROM storage.buckets WHERE id='crop-photos'` **iki kez** (oluşturmada ve tur sonunda) `true` döndü. ✅

10. **Advisor taraması** (`get_advisors: security`) — yeni 7 tablo, 2 view ve 3 fonksiyonun **hiçbiri** uyarı üretmedi. `security_invoker=true` iki view'da da `pg_class.reloptions` ile doğrulandı. Mevcut (P23-M2 öncesi) uyarılar değişmedi.

#### B. Berkin'in tarayıcı adımları — **regresyon kontrolü** (yeni ekran yok)

> Amaç: "hiçbir mevcut davranış değişmedi" iddiasını uygulamada görmek. Lansmana 4 hafta var; bu turun tek riski, ekleyici olması gereken bir değişikliğin yanlışlıkla mevcut bir akışa dokunmuş olması.
> ⚠️ Kural #109: teste başlamadan önce Lovable'da **Publish**'e bas.

| # | Adım | Beklenen |
|---|---|---|
| 1 | `hasat.lovable.app` → alıcı olarak gir (`905009876543`, OTP `123456`) | Giriş normal çalışıyor |
| 2 | **Keşfet**'i aç | Ürün kartları eskisi gibi listeleniyor, sayı ve fiyatlar değişmemiş |
| 3 | Bir ürün detayına gir, partileri gör | Partiler ve toplam miktar eskisiyle **aynı** |
| 4 | Bir partiye teklif ver ekranını aç (göndermene gerek yok) | Ekran normal açılıyor, miktar/birim alanları doğru |
| 5 | **Taleplerim / Talep Oluştur** akışını aç | Talep formu normal çalışıyor (bu tur `crop_requests`'e yeni bir bağ tablosu eklendi, form davranışı değişmemeli) |
| 6 | Çıkış yap, çiftçi olarak gir (`905001234567`, OTP `123456`) | Giriş çalışıyor |
| 7 | Depo/Vitrin → **"+ Yeni Ürün"** ile bir ilan oluştur | Kayıt hatasız — birim listesi hâlâ yalnızca **kg / g / L** (culinary birimler enum'a **eklenmedi**, buraya sızmamalı) |
| 8 | **Günlük** ve **Rutin Bakım** sekmelerini aç | P22-G davranışı aynen duruyor, listeler doluyor |
| 9 | Alıcı olarak bir ürünü sepete/teklife kadar götür (P21 çoklu-batch akışı) | Stok ve birim-uyuşmazlığı kontrolleri eskisi gibi çalışıyor |
| 10 | Lovable editöründe küçük bir değişiklik iste ve diff'e bak | `src/lib/core/` altında **hiçbir dosya** değişmemiş (kural #105) |

**Beklenen sonuç: 10/10 "değişmedi".** Herhangi bir adımda davranış farkı görülürse bu tur ekleyici olmayı başaramamış demektir — TODO.md'ye bug olarak girilmeli.

---

#### C. P23-M2-ek — Huni ölçümünün tamamlanması (2026-07-29)

> Bu ek turda da **yeni ekran yok** — S20-B'deki regresyon adımları geçerliliğini koruyor. Aşağıdakiler veri katmanı doğrulamasıdır.

**Kapsam:** `recipe_views` tablosu · `offers.source_recipe_id` kolonu · `v_kpi_recipe_funnel` yeniden yazıldı (sezgisel atıf kaldırıldı) · `author_type` += `kullanici`.

1. **Beş basamak da gerçek veriyle doldu — tek transaction, sonunda ROLLBACK**

   > ⚠️ Test bilinçli olarak **transaction içinde** koşturuldu ve geri alındı: `offers`'a INSERT `trg_offer_received` trigger'ını, o da `dispatch_sms`/`pg_net`'i tetikliyor. ROLLBACK sayesinde **hiçbir gerçek SMS gönderilmedi** — `net.http_request_queue` tur sonunda boş doğrulandı.

   Kurulan senaryo: 1 public tarif · 3 görüntüleme (2 anon farklı `session_id` + 1 girişli) · 1 kaydetme · **talep yolu** (`crop_request` + `recipe_rfq_links`) · **doğrudan teklif yolu** (`offers.source_recipe_id` dolu, gerçek aktif domates ilanı üzerinde) · o teklife bağlı 1 sipariş.

   | Kolon | Beklenen | Gerçekleşen |
   |---|---|---|
   | `recipe_views` | 3 | ✅ 3 |
   | `unique_viewers` | 3 | ✅ 3 |
   | `recipe_saves` | 1 | ✅ 1 |
   | `recipe_requests` (malzeme yok yolu) | 1 | ✅ 1 |
   | `recipe_offers` (malzeme var yolu) | 1 | ✅ 1 |
   | `recipe_orders` | 1 | ✅ 1 |
   | `recipe_offers_converted` | 1 | ✅ 1 |
   | `view_to_save_pct` | 33,33 | ✅ 33,33 |
   | `offer_to_order_pct` | 100,00 | ✅ 100,00 |

2. **Sezgisel atfın gerçekten kalktığının kanıtı** — canlı DB'de **121 teklif** ve **120 sipariş** var, hiçbirinde `source_recipe_id` dolu değil. Yeni view bu durumda **0 satır** döndürüyor. Eski view aynı veride Zeynep'in safran teklifini "tarife atfedilmiş teklif" olarak sayıyordu (bir önceki turun doğrulama tablosunda `recipe_attributed_offers = 1` görünüyor). Fazla atıf ortadan kalktı. ✅

3. **`recipe_views` RLS**

   | Test | Sonuç |
   |---|---|
   | **anon INSERT** (giriş yapmamış ziyaretçi, `user_id` NULL) | ✅ Kabul — satır düştü, `session_id` doğru, `user_id` NULL |
   | **anon SELECT** | ✅ **Reddedildi** — `42501: permission denied for table recipe_views` |
   | **anon `v_kpi_recipe_funnel` SELECT** | ✅ **Reddedildi** — `42501: permission denied for view v_kpi_recipe_funnel` |
   | Girişli kullanıcı kendi adına INSERT | ✅ Kabul |
   | Girişli kullanıcı **başkasının** `user_id`'siyle INSERT | ✅ **Reddedildi** — `42501: row-level security` |

4. **`offers.source_recipe_id` — gerçek mutasyonla UPDATE doğrulaması**
   Mevcut `Buyers insert offers` politikasıyla teklif `source_recipe_id = NULL` olarak yazıldı, sonra mevcut `Both parties update offer` politikasıyla dolduruldu. INSERT sonrası NULL → UPDATE sonrası dolu, **ayırt edilebilir değerle** kanıtlandı. ✅ **Yeni UPDATE politikası gerekmedi** — mevcut politika yeni kolonu kapsıyor.

5. **`author_type = 'kullanici'`** — `extract-recipe` gerçek çağrıyla (metin girdisi, gerçek kullanıcı JWT'si) test edildi: HTTP 200, dönen kayıtta `"author_type":"kullanici"`, `visibility` private, `status` draft. ✅ Öncesinde default `hasat` kalıyordu.

6. **Advisor taraması** — `recipe_views`, `offers.source_recipe_id` ve yeniden yazılan `v_kpi_recipe_funnel` **hiç uyarı üretmedi.** `security_invoker=true` `pg_class.reloptions` ile yeniden doğrulandı.

7. **Test verisi temizliği** — 0 tarif, 0 malzeme, 0 adım, 0 `recipe_views`, 0 kota satırı; `offers` 121 ve `orders` 120'de kaldı (**dokunulmadı**); SMS kuyruğu boş. Kalıcı olan tek şey 70 satırlık `crop_culinary_meta` seed'i. ✅

---

### S21 — P23-M3 Tarif İçeriği + Culinary Seed (ekleyici, veri katmanı)

> ⚠️ Bu tur da S20 gibi **ekrana yansımıyor** — tarif arayüzü hâlâ M4'te
> doğacak (`/tarifler` misafire açık sayfası). 18 tarif + culinary seed
> canlı DB'de var ama hiçbir web/uygulama ekranında henüz görünmüyor. Bu
> yüzden aşağıdaki test case'in ağırlığı yine **regresyon kontrolü**
> (mevcut akışlar bozulmadı mı) üzerinde; yeni içerik doğrulaması veri
> katmanında yapıldı, sonuçları `Build/DB-Schema.md` → "P23-M3" bölümünde.

**Arka plan:** `Build/P23-Mobile.md` M3. Kapsam kuralı: canlı akışlara
(teklif/sipariş/ilan/günlük) dokunulmadı, `unit_type` enum'una dokunulmadı,
`crop_config`/`crop_culinary_meta`'nın 14 odak crop dışındaki satırlarına
dokunulmadı, hiçbir mevcut davranış değiştirilmedi.

#### A. Veri katmanı doğrulaması — Claude Code + Supabase MCP (kural #96)
Ayrıntılı sonuç tablosu: `Build/DB-Schema.md` → "P23-M3" → "Doğrulama"
bölümü. Özet: 18/18 tarif anon rolüyle okunabiliyor, 68/68 crop-bağlı
malzeme satırında alışveriş listesi miktarı NULL dönmüyor, yenilemez
crop'lar (pamuk/tütün/şeker_pancarı/safran_soğanı) hiçbir sonuçta yok,
safran tam 1 tarifte, yeni advisor uyarısı yok.

#### B. Berkin'in tarayıcı adımları — regresyon kontrolü (yeni ekran yok)

| # | Adım | Beklenen |
|---|---|---|
| 1 | `hasat.lovable.app` → alıcı olarak gir (`905009876543`, OTP `123456`) | Giriş normal çalışıyor |
| 2 | **Keşfet**'i aç, birkaç ürüne bak | Ürün kartları eskisi gibi listeleniyor, bu tur hiçbir ürün/ilan verisine dokunmadı |
| 3 | Çiftçi (Ahmet, `905001234567`) → Depo/Vitrin → "+ Yeni Ürün" ile bir ilan oluştur, herhangi bir ürün seç | Kayıt hatasız — birim listesi hâlâ yalnızca **kg / g / L** (bu tur culinary birimleri `crop_culinary_meta`'ya yazdı, `unit_type` enum'una asla sızmadı) |
| 4 | Aynı çiftçi → **Günlük**/**Rutin Bakım** sekmelerini aç | P22-G davranışı aynen duruyor |
| 5 | Lovable editöründe projeyi aç, önizlemeyi yükle | Kırmızı build hatası yok — bu tur hiç frontend kodu değiştirmedi |

**Beklenen sonuç: 5/5 "değişmedi".** Bu turda görülecek yeni bir ekran
**yok** — 18 tarifin kendisini görmek isteyen biri için tek yol Supabase
Dashboard'dan `recipes` tablosuna bakmak (M4'e kadar).

---

### S22 — P23-M4-a Public Tarif Yüzeyi + DB Eki + Ölçümleme

> **Kural #109 — QA'nın İLK adımı Publish'e basmak.** Bu tur frontend kodu
> Claude Code tarafından `hasat-d2c-marketplace` reposuna PR olarak açıldı
> (Lovable'ın kendisi değil). PR merge edildikten sonra bile `main`'deki
> kod **Lovable'da Publish'e basılmadan** `hasat.lovable.app`'e inmez — bu
> S20/S21'in aksine, bu tur **gerçekten yeni bir ekran** açıyor
> (`/tarifler`), o yüzden bu kez atlanamaz.

**Arka plan:** `Build/P23-Mobile.md` M4-a. Kapsam: `/tarifler` +
`/tarifler/$slug` (misafire açık, SSR/SEO), malzeme kartı 3 durumu,
`recipe_views` ölçümleme, `v_kpi_recipe_funnel_by_recipe`. Talep Et akışı,
admin heatmap, Gap #9 → M4-b (bu turda yok).

#### A. Veri katmanı doğrulaması — Claude Code + Supabase MCP (kural #96)
Ayrıntılı sonuç tablosu: `Build/DB-Schema.md` → "P23-M4-a". Özet: `anon` ve
`authenticated` rolüyle gerçek `recipe_views` insert'i test edildi
(`session_id` her ikisinde de dolu), `v_kpi_recipe_funnel_by_recipe` iki
test görüntülemesini doğru saydı, kekik/fındık (eşleşen) ve nohut/zeytinyağı
(eşleşmeyen) ile `min_order` yuvarlaması gerçek veriyle doğrulandı, yeni
view'ın `security_invoker=true` olduğu ve `anon`/`authenticated`'a GRANT
**olmadığı** (bu projede yeni view'lara varsayılan grant düştüğü fark
edildi, elle revoke edildi) doğrulandı. Test verisi (3 `recipe_views`
satırı) silindi.

#### B. Sandbox kısıtı — canlı tarayıcı testi Claude Code tarafında yapılamadı
Bu oturumun ağ politikası `efuqpiaavrzimvstpdpm.supabase.co`'ya SSR sırasında
doğrudan bağlanmayı engelledi (P24'te Berkin'in de karşılaştığı aynı kısıt —
bkz. `TODO.md` P24 notu, "tarayıcının Supabase host'una doğrudan bağlanmasını
403 ile engelledi"). Bunun yerine: (a) üretim build'i (`vite build`, SSR
dahil) hatasız tamamlandı, `/tarifler` ve `/tarifler/$slug` için ayrı SSR
chunk'ları üretildi; (b) `tsc --noEmit` ve `eslint` temiz; (c) `/tarifler`'in
statik `head()` meta'sının (başlık/description/canonical) gerçekten SSR
HTML'ine yazıldığı ham `curl` çıktısıyla doğrulandı — sayfanın veri kısmı
ağ kısıtı yüzünden hata verse bile meta etiketleri doğru render oldu; (d)
`/tarifler/$slug`'ın **dinamik** JSON-LD'si (loader verisine bağlı) bu
sandbox'ta canlı doğrulanamadı — kod incelemesi + tip kontrolü + build
başarısıyla güveniliyor, **gerçek view-source kanıtı aşağıdaki B adımlarında
Berkin'den bekleniyor.**

#### C. Berkin'in tarayıcı adımları

| # | Adım | Beklenen |
|---|---|---|
| 1 | Lovable editörünü aç, **Publish**'e bas | Yeni build yayınlandı |
| 2 | Gizli sekme (giriş yapmadan) → `hasat.lovable.app/tarifler` | Tarif listesi açılıyor, kapak fotoğrafı yoksa crop görseli + "Temsili görsel" etiketi ya da nötr placeholder görünüyor (boş kutu yok) |
| 3 | Zorluk/süre/diyet filtrelerini dene | Liste filtreleniyor |
| 4 | "Şu an Hasat'ta tam alınabilir tarifler" kutusunu işaretle | Neredeyse boş/tek sonuç dönüyor — **bu beklenen, hata değil** (gerçek arz 18 tarifin sadece birinde %100 kapsıyor) |
| 5 | Bir tarife tıkla, örn. **Kekikli Zeytinyağı Ezmesi** | Detay sayfası açılıyor; **kekik** malzemesi "Ürün sayfasına git" linkiyle + fiyatla görünüyor, **zeytinyağı/susam** nötr "Hasat'ta henüz yok" (link/CTA yok), **tuz** hiç durum etiketi olmadan sade görünüyor |
| 6 | Porsiyon sayacını + / − ile değiştir | Malzeme miktarları yeniden hesaplanıyor (RPC yeniden çağrılıyor) |
| 7 | Adımlarda bekleme/pişirme süresi olanlara bak | Süre (⏱ dk/sa/gün) görünüyor |
| 8 | Sayfa kaynağını görüntüle (view-source / Ctrl+U) | `<title>`, meta description, `rel="canonical"`, `application/ld+json` (`"@type":"Recipe"`) HTML kaynağında gerçekten var |
| 9 | Alıcı olarak giriş yap (`905009876543`, OTP `123456`), aynı tarife git, "Ürün sayfasına git" tıkla | `/buyer/discover`'a yönlendiriyor |
| 10 | Alıcı → **Keşfet** sekmesini aç | Yeni "🍽️ Tarif fikri mi arıyorsun?" kutusu görünüyor (bottom nav'da yeni sekme **yok** — 5 slot dolu kuralına uyuldu), tıklayınca `/tarifler`'e gidiyor |
| 11 | Supabase Dashboard → `recipe_views` tablosu | Adım 5/9'daki görüntülemelerin gerçek satırları var, `session_id` dolu |
| 12 | Çiftçi/alıcı diğer ekranlar (Keşfet ürün listesi, Günlük, Siparişler) | Bu tur hiçbirine dokunmadı — davranış aynı |

**Beklenen sonuç: 12/12 geçiyor.** Adım 4'ün "boş/az sonuç" dönmesi bug
değildir — arz gerçeğinin (`Build/P23-Mobile.md` → "Arz gerçeği") doğrudan
yansımasıdır.

---

### S23 — P23-M4-b Talep Et Akışı + Admin Talep Isı Haritası + Gap #9

> **Kural #109 — QA'nın İLK adımı Publish'e basmak.** Bu tur da (S22 gibi)
> Claude Code tarafından `hasat-d2c-marketplace` reposuna PR olarak açıldı.
> Merge sonrası bile Lovable'da **Publish**'e basılmadan `hasat.lovable.app`
> eski build'i göstermeye devam eder.

**Arka plan:** `Build/P23-Mobile.md` M4-b. Kapsam: eşleşmeyen malzemede
"Talep Et" CTA'sı + guest niyet takibi, "haber ver" (mevcut
`crop_request_match` deseni), admin talep ısı haritası, Gap #9 (mevcut
batch/traceability sayfasına link), ve M4-a'nın 3 bulgusunun düzeltilmesi
(malzeme büyük harf, `totalTime`, `image`). Tam ayrıntı: `Build/DB-Schema.md`
→ "P23-M4-b".

#### A. Veri katmanı + uçtan uca doğrulama — Claude Code + Supabase MCP (kural #96)
Ayrıntılı sonuç tablosu: `Build/DB-Schema.md` → "P23-M4-b" → "Doğrulama".
Özet: gerçek RLS simülasyonuyla (Zeynep, authenticated) `crop_requests` +
`recipe_rfq_links` insert edildi, `v_kpi_recipe_funnel_by_recipe`'de o
tarifin talep basamağı 0→1 arttığı doğrulandı; ardından Ahmet için yeni bir
aktif `zeytinyağı` ilanı eklenip **"haber ver" trigger'ının** (a) bölge
uyuşmadığında doğru şekilde **atladığı**, (b) bölge boş/uyuştuğunda
`notifications` satırı yazıp talebi `status='added'`'e çektiği ayrı ayrı
kanıtlandı; gerçek SMS gönderilmediği (`net._http_response`) doğrulandı;
tüm test verisi (crop_requests/recipe_rfq_links/listings/notifications)
silindi, funnel view'ı 0'a döndü. `totalTime` düzeltmeleri (13/18 tarif)
SQL ile önce/sonra listelendi. Yeni `v_kpi_crop_demand_heatmap` view'ının
`anon`/`authenticated` grant'i olmadığı (kural #110) ve `security_invoker=true`
olduğu doğrulandı.

#### B. Sandbox kısıtı — bu turda `bun install` de engellendi (yeni bulgu)
Önceki turlarda ağ kısıtı yalnızca canlı Supabase SSR/tarayıcı erişimini
kapatıyordu (bkz. S22 → B). Bu turda ayrıca `bun install`'ın kullandığı
paket mirror'ı (`bun.lock`'a kilitli `*-npm.pkg.dev/lovable-core-prod/...`
tarball URL'leri) org egress politikasıyla 403 aldı — tam bağımlılık kurulumu
yapılamadı. Kısmen kurulu `node_modules` ile `tsc --noEmit`, `eslint` ve
`prettier` çalıştırıldı: değişen/yeni dosyalarda **sıfır yeni hata**
(değişiklik öncesi/sonrası tsc çıktısı satır-satır özdeş, kalan hatalar
tamamen pre-existing implicit-any deseni + eksik paketler — `recharts`,
`zod`, `@lovable.dev/*`). Gerçek `vite build` bu oturumda **yapılamadı** —
Lovable/Berkin'in ortamında doğrulanmalı.

#### C. Berkin'in tarayıcı adımları

| # | Adım | Beklenen |
|---|---|---|
| 1 | Lovable editörünü aç, **Publish**'e bas | Yeni build yayınlandı |
| 2 | `hasat.lovable.app/tarifler/zeytinyagli-nohut-yemegi` gibi zeytinyağı/susam içeren bir tarife git | **Zeytinyağı** malzemesinin altında artık gri bir pil değil, turuncu **"Talep Et →"** butonu var |
| 3 | Aynı sayfada eşleşen bir malzemeye bak (ör. **domates**, varsa) | "Ürün sayfasına git" linkinin altında **"🔍 Parselden tabağa: kaynağını gör"** linki var (Gap #9) — tıklayınca parti/izlenebilirlik sayfası açılıyor |
| 4 | Giriş yapmadan (gizli sekme) "Talep Et"e bas | `/login`'e yönleniyor |
| 5 | Telefonla giriş yap (yeni bir test numarası, onboarding'den geçecek şekilde) | Onboarding tamamlanınca `/buyer/discover`'a düşüyor, üstte **"Yarım kalan talebiniz var"** bandı görünüyor |
| 6 | Bandaki linke tıkla | Tarif sayfasına dönüyor, **"Talep Et" formu otomatik açılıyor**, malzeme adı önceden dolu/kilitli |
| 7 | Zaten giriş yapmış bir alıcıyla (Zeynep, `905009876543`/`123456`) doğrudan "Talep Et"e bas | Form direkt açılıyor (login'e gitmiyor), miktar/birim alanı tarifin ihtiyacına göre **önceden dolu** geliyor |
| 8 | Formu gönder | "Talebiniz alındı… bu ürün geldiğinde size de haber vereceğiz" mesajı görünüyor |
| 9 | Bir tarifte, önceki bekleme/soğutma/mayalanma içeren adımı olan (ör. Cevizli Üzümlü Köme, Ekşi Mayalı Ekmek) sürelere bak | Toplam süre artık "N dk" değil, "N gün M sa" gibi okunabilir bir biçimde ve gerçekçi (köme için ~3 gün) |
| 10 | Sayfa kaynağını görüntüle (view-source) — kapak fotoğrafı olmayan bir tarifte | JSON-LD'de `"image"` alanı **hiç yok** (temsili görsel varsa bile) — Berkin bir tarife gerçek kapak fotoğrafı yükleyince bu alan otomatik belirmeli |
| 11 | Admin → `/admin/kpi` → anahtarı gir → **"Talep Isı Haritası"** sekmesi | Tablo açılıyor; **zeytinyağı** satırı "9 tarifte temel malzeme" + "Aktif ilan: Yok" ile turuncu vurgulu görünüyor; adım 8'de gönderilen test talebi (temizlenmemişse) o crop'un satırında görünüyor |
| 12 | Bildirim zilini aç (adım 8'in ardından, eşleşen bir çiftçi ilan açtığında) | Yeni bir "🎉 Talep ettiğiniz ürün geldi" bildirimi görünüyor, tıklayınca `/tarifler`'e gidiyor |

**Beklenen sonuç: 12/12 geçiyor.** Adım 10'un "image alanı yok" göstermesi
bug değildir — 18/18 tarifin `cover_photo_url`'i hâlâ NULL olduğu sürece
beklenen davranıştır.

---

### S24 — P23-M4-c `cook_minutes` Semantik Düzeltmesi + SEO Keşfedilebilirliği (M4'ün kapanışı)

> **Kural #109 — QA'nın İLK adımı Publish'e basmak.** Bu tur da Claude Code
> tarafından PR olarak açıldı; merge sonrası Lovable'da **Publish**'e
> basılmadan `hasat.lovable.app` eski build'i göstermeye devam eder.

**Arka plan:** `Build/P23-Mobile.md` M4-c. M4-b'de bir kural #107 ihlali
bulunup düzeltildi: bekleme süresini tutacak kolon olmadığı için
`cook_minutes`'a sessizce eklenmişti (muhammara "45 dk pişirme" — gerçeği
15 dk). Yeni `recipes.rest_minutes` kolonu + 18 tarifin tamamının yeniden
sınıflandırılması + `sitemap.xml`'in dinamikleştirilmesi + iç link ağı bu
turun konusu. Tam ayrıntı: `Build/DB-Schema.md` → "P23-M4-c".

#### A. Veri katmanı + kod doğrulaması — Claude Code (kural #96)
Ayrıntılı tablo: `Build/DB-Schema.md` → "P23-M4-c" → tam prep/cook/rest
tablosu + doğrulama. Özet: 18/18 tarif gerçek SQL ile yazılıp okunarak
doğrulandı; `cook_minutes`'ın hiçbiri 120 dk'yı aşmıyor (en yüksek 60 dk,
İncir Reçeli); JSON-LD'nin üç süreyi doğru ürettiği gerçek DB değerleriyle
simüle edilip kanıtlandı (`PT20M`/`PT15M`/`PT1H5M` gibi); `sitemap.xml`
gerçek veriyle üretilip `xmllint --noout` ile geçerli XML olduğu ve 18
tarif URL'i içerdiği doğrulandı; liste + iç link'lerin gerçek `<a href>`
olduğu TanStack Router `Link` bileşeninin kaynak kodundan doğrulandı.

#### B. Sandbox kısıtı — `bun install` bu turda da engellendi
M4-b'de bulunan kısıt (Lovable'ın özel paket mirror'ı org egress
politikasıyla kapalı) bu turda da geçerliydi. Kısmi `node_modules` ile
`tsc --noEmit`/`eslint`/`prettier` çalıştırıldı — değişen dosyalarda sıfır
yeni hata. Gerçek `vite build` **yine yapılamadı** — M4'ün üç turunda da
(M4-a hariç) hiç koşmadı, Lovable/Berkin'in ortamında doğrulanmalı.

#### C. Berkin'in tarayıcı adımları

| # | Adım | Beklenen |
|---|---|---|
| 1 | Lovable editörünü aç, **Publish**'e bas | Yeni build yayınlandı |
| 2 | `hasat.lovable.app/tarifler/cevizli-biber-ezmesi-muhammara`'ya git | Üst bilgide artık tek bir "N dk" değil, **"20 dk hazırlık · 15 dk pişirme · 30 dk dinlenme"** gibi ayrı üç süre görünüyor |
| 3 | `hasat.lovable.app/tarifler/cevizli-uzumlu-kome`'ye git | Süre artık "3 gün" mertebesinde makul görünüyor, "72 saat pişirme" gibi saçma bir "pişirme süresi" **yok** |
| 4 | Sayfa kaynağını görüntüle (view-source) — muhammara sayfasında | JSON-LD'de `cookTime` **"PT15M"** (15 dk), `totalTime` **"PT1H5M"** (65 dk) — `cookTime` artık `totalTime`'la aynı değil |
| 5 | Herhangi bir tarif sayfasının altına in | Aynı ana malzemeyi paylaşan **2-3 diğer tarife** link var (örn. cevizli tariflerde "ceviz ile diğer tarifler") |
| 6 | O linke tıkla | İlgili tarif sayfası açılıyor |
| 7 | `hasat.lovable.app/sitemap.xml`'i tarayıcıda aç | Geçerli bir XML dönüyor, içinde 18 `/tarifler/...` URL'i var |
| 8 | `hasat.lovable.app/robots.txt`'i aç | `Sitemap:` satırı var, `/tarifler` hiçbir yerde engellenmemiş |
| 9 | `/tarifler` liste sayfasında bir tarif kartına sağ tık → "Bağlantı adresini kopyala" (ya da view-source'ta ara) | Kartın gerçek bir `<a href="/tarifler/...">` olduğu görünüyor, salt `onClick`'e bağlı değil |
| 10 | Google Search Console'a (varsa) `sitemap.xml`'i gönder | (Bu adım Berkin'in kendi hesabında, bu QA'nın kapsamı dışında ama önerilir) |

**Beklenen sonuç: 9/9 geçiyor** (10. adım opsiyonel/bilgi amaçlı).

---

## Feature Sonrası Süreç

1. Yeni prompt tamamlanınca yeni S-numarası eklenir
2. Mümkünse hemen çalıştırılır, sonuç not edilir
3. Bug bulunursa TODO.md'ye eklenir, düzeltme sonrası tekrar koşulup ✅ işaretlenir
4. Vault'a aynı workflow'la commit edilir

---

## Test Verisi Temizlik Kuralları
- Sabit-OTP hesaplar (Ahmet/Zeynep) + 9+9 mock hesap: temizlenmez (birikim sorun değil, gerçek gösterim verisi)
- Dinamik-OTP hesaplar: her onboarding testi sonrası sıfırlanır
- Geçici test verisi (S5 gibi): senaryo sonunda hemen silinir

---

## Bilinen Kısıtlar
- Bir MCP bağlantısı = bir kullanıcı; sadece Ahmet/Zeynep'in MCP login'i var, diğer 9+9 mock hesap için Supabase MCP üzerinden veri-katmanı simülasyonu kullanılıyor
- Topluluk post/reply, sertifika/foto upload, WhatsApp AI, referral ziyareti, veri indirme, **abonelik oluşturma** — MCP tool'u yok, hibrit/manuel ya da Supabase MCP simülasyonu
- Landing page/mobil/görsel doğrulama MCP ile test edilemez, Berkin'in ekran görüntüsü gerekiyor
- `web_fetch` tool'u Supabase storage URL'lerine (önceden görülmemiş domain) erişemiyor — fotoğraf yükleme doğrulaması bu yüzden her zaman Berkin'in görsel kontrolüne ihtiyaç duyacak
- Yeni MCP tool'u → Publish + connector reconnect gerekiyor
- MCP yazma tool'larında (bazı Supabase tool'larında da) aralıklı "No approval received"

---

## Değişiklik Geçmişi
- **2026-07-09/10:** Doküman oluşturuldu, S1-S7 koşuldu.
- **2026-07-16:** **S8-S17 eklendi.** Fiyatlandırma/Keşfet/Görüşmeler/Raporlar/Abonelikler canlı test edildi (S8-S13, hepsi ✅). Detaylı fotoğraf/sayfa incelemesinde **4 yeni kritik bug bulunup düzeltildi** (S14-S17): günlük foto yükleme tamamen sahteydi, Parti sayfası kapak fotoğrafı yoktu, Üretici Detay ve Abonelik Oluştur sayfaları **tamamen mock/sahte veriye bağlıydı ve gerçekte hiçbir zaman DB'ye yazmıyordu**. Bu son ikisi bugüne kadarki en kritik "sessizce çalışmıyor" bulgularından — kullanıcıya hiçbir hata göstermeden özelliğin var olmadığı senaryolar.
- **2026-07-28:** **S18 eklendi (P23-M1-a şema borçları).** Safran Soğanı birim bug'ı, `listings.min_order>quantity` BEFORE INSERT trigger'ı (+1 ihlal eden satır düzeltmesi), `buyer_profiles.company_name` nullable, `buyer_addresses` tek-varsayılan-adres trigger'ı — hepsi Supabase MCP ile gerçek SQL/insert/update testleriyle doğrulandı, Berkin'in uygulama üzerinden yapacağı QA adımları eklendi.
- **2026-07-28:** **S19 eklendi (P22-G rutin bakım tarih/filtre + trigger temizliği).** `buyer_addresses` çift trigger'ı (P23-M1-a'nın kendi eklediği trigger'la çakışıyordu) düşürüldü; rutin bakım hesabı `v_routine_maintenance_status` view'ına taşındı (kural #106); asıl bug (eksik React Query invalidation) bulunup düzeltildi; "Yaptım" formuna tarih seçici eklendi; P22-E SMS'lerine eksik alanlar eklendi. Hepsi gerçek veri/Twilio testiyle doğrulandı; tarayıcı testi Berkin'e kaldı.
- **2026-07-30:** **S22 eklendi (P23-M4-a public tarif yüzeyi).** Bu, S18/S19'dan farklı olarak **gerçekten yeni bir ekran** açıyor (`/tarifler`, `/tarifler/$slug`) — kural #109 gereği QA'nın ilk adımı Lovable'da Publish'e basmak. Malzeme kartının 3 durumu (eşleşti/nötr/platform-dışı) ve `min_order` yuvarlaması gerçek veriyle doğrulandı; `recipe_views` + yeni `v_kpi_recipe_funnel_by_recipe` gerçek `anon`/`authenticated` RLS simülasyonuyla test edildi. Bu oturumun ağ politikası Supabase'e canlı SSR sırasında erişimi engellediği için (P24'teki aynı kısıt), tam tarayıcı testi + detay sayfasının dinamik JSON-LD'sinin view-source kanıtı Berkin'e kaldı.
- **2026-07-30:** **S23 eklendi (P23-M4-b Talep Et + admin talep ısı haritası + Gap #9).** Eşleşmeyen malzemede baskın-durum "Talep Et" CTA'sı + guest niyet takibi (`localStorage` + `/login`'in `next` param'ı), mevcut `crop_request_match`/`dispatch_sms` deseni yeniden kullanılarak "haber ver" (yeni bir trigger, yeni tablo yok), `/admin/kpi`'ye talep ısı haritası sekmesi, Gap #9 mevcut `/batch/$listingId` sayfasına link olarak kapandı. Uçtan uca gerçek RLS simülasyonuyla doğrulandı: talep+huni atfı, "haber ver"in bölge eşleşmesine göre doğru tetiklenip/atlandığı, gerçek SMS gönderilmediği. Ayrıca M4-a'nın 3 bulgusu (malzeme büyük harf, 13/18 tarifte eksik `totalTime`, `image` alanının temsili görselle karışması) düzeltildi. **Yeni bulgu:** bu oturumda `bun install` de org egress politikasıyla engellendi (Lovable'ın özel paket mirror'ı) — `tsc`/`eslint`/`prettier` kısmi kurulumla temiz sonuç verdi ama gerçek `vite build` yapılamadı, Lovable/Berkin'in ortamında doğrulanmalı.
- **2026-07-30:** **S24 eklendi (P23-M4-c `cook_minutes` düzeltmesi + SEO — M4'ün kapanışı).** S23'te sessizce yapılan bir kural #107 ihlali (bekleme süresi tutacak kolon olmadığı için `cook_minutes`'a eklenmişti, muhammara "45 dk pişirme" gösteriyordu, gerçeği 15 dk) bulunup düzeltildi: yeni `recipes.rest_minutes` kolonu, 18 tarifin tamamı adım metninden yeniden sınıflandırıldı (`cook_minutes` en yükseği artık 60 dk). `totalTime` = prep+cook+rest türetilmiş değeri; üç süre detay sayfasında ayrı gösteriliyor. SEO: `sitemap.xml` dinamikleştirildi (18 tarif + public vitrinler), `robots.txt` zaten doğruydu, aynı ana malzemeyi paylaşan tariflere SSR'da (client-side değil) iç link eklendi — hepsi gerçek `<a href>`. `bun install` bu turda da engellendi, gerçek `vite build` M4'ün üç turunda da (a hariç) hiç koşmadı.
