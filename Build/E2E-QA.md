---
title: Hasat — E2E QA Test Dokümanı
updated: 2026-07-31
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

### S25 — P23-M5-a Mobil İskelet + `hasat-core` İkinci Hedefi + Tesisat

> **M5-a-ek güncellemesi (2026-07-31):** M5-a `main`'e merge edilip
> doğrulandı (Berkin, 2026-07-30). Bu turda S25'in B bölümü **yeniden
> yazıldı** — eski hali gerçek cihaz + Expo Go varsayıyordu, ama Apple
> Developer hesabı henüz onaylanmadı ve elde Android cihaz yok. Yeni B
> bölümü Berkin'in kararına göre **iOS Simulator build + tarayıcı tabanlı
> Appetize.io** yoluyla koşuluyor. A ve C bölümleri M5-a turunun tarihi kaydı
> olarak korunuyor (zaten merge edilip doğrulandı); bu turun kendi PR'ları
> (types tazeliği düzeltmesi, `.env` bekçisi, `eas.json`) için ayrı bir merge
> listesi aşağıda "M5-a-ek merge listesi" başlığında.

**Arka plan:** `Build/P23-Mobile.md` → M5-a, `TODO.md` → "P23-M5-a" +
"P23-M5-a-ek" build logları, `Build/Shared-Architecture.md` → ikinci hedef +
drift kör noktasının kapanışı.

#### M5-a-ek merge listesi
| Repo | İçerik |
|---|---|
| `hasat-core` | `core/db/types.ts` yeniden üretildi (`recipes.rest_minutes` + iki eksik KPI view eklendi), `core/.manifest` güncellendi, yeni `types-freshness.yml` (DB↔core tazelik CI'ı) + `check-types-freshness.mjs`, `drift-check.yml`'e `.env` bekçisi adımı + `check-env-guard.mjs` |
| `hasat-mobile` | `eas.json` (yeni `simulator` build profili) |
| `hasat-vault` | Bu doküman + `TODO.md` (kural #111 + build log) + `P23-Mobile.md` + `Store-Compliance.md` + `_Context.md` |

`hasat-core`'daki `core/db/types.ts` değişikliği **dual-target sync
Action'ını tetikleyecek** — `hasat-core` PR'ı merge edilince
`hasat-mobile` ve `hasat-d2c-marketplace`'e otomatik birer "hasat-core sync"
PR'ı açılacak (bkz. A bölümündeki 6 adımlık merge sırası, aynı prosedür
geçerli). **Bu sync PR'ları da merge edilmeden tip düzeltmesi hedeflere
inmez.**

Claude Code'un bu turda **doğrudan doğrulayabildiği** (statik): canlı
Supabase şemasından (`efuqpiaavrzimvstpdpm`, Supabase MCP ile doğrudan
erişildi) `supabase gen types` yeniden üretildi, committed dosyayla diff
**yalnızca** `rest_minutes` kolonu + iki KPI view'ının FK referanslarını
ekliyor (beklenen, temiz bir diff); `check-types-freshness.mjs` hem güncel
tipe karşı yeşil hem eski (bayat) sürüme karşı kasıtlı olarak kırmızı
verdiği doğrulandı; `.env` bekçisi üç senaryoda test edildi (temiz dosya →
geçti, `EXPO_PUBLIC_` öneki olmayan satır eklendi → reddetti, `service_role`
kalıbı taşıyan bir isim eklendi → reddetti), sonra `hasat-mobile/.env` orijinal
haline geri alındı (`git status` ile doğrulandı, iz kalmadı).

⚠️ **Bulunan bir sınır (dürüstçe raporlanıyor):** İstenen 5 kalıp
(`service_role`, `SECRET`, `PRIVATE`, `TOKEN`, `PASSWORD`) literal alt dize
eşleşmesiyle uygulandı. Görev metninde örnek olarak verilen
`EXPO_PUBLIC_SERVICE_KEY` ismi bu 5 kalıbın **hiçbirini** literal olarak
içermiyor (`SERVICE_KEY` ≠ `service_role`) — yani bu spesifik örnek isim
bekçiyi geçebilir. `KEY` kelimesini kalıba eklemek de çözüm değil: mevcut
`.env`'deki meşru `EXPO_PUBLIC_SUPABASE_PUBLISHABLE_KEY` satırı da `KEY`
içeriyor, bu yüzden `KEY`'i yasaklamak gerçek/meşru satırı da reddederdi.
Bekçi tam olarak istenen 5 kalıple kuruldu ve literal `service_role` gibi
isimleri doğru yakalıyor (test edildi) — ama isim-kalıbı denetimi bir
**savunma katmanı**dır, tam garanti değil; asıl garanti gerçek sırların hiç
`.env`'ye yazılmamasıdır (EAS Environment Variables kullanılır, bkz.
`Shared-Architecture.md`). Kural #107 gereği bu sınır burada açıkça
işaretleniyor, sessizce "KEY" eklenip mevcut satır kırılmadı.

#### A. `hasat-core` + `hasat-mobile` PR'larını merge et (M5-a turu — tarihi kayıt, zaten yapıldı)
1. Önce `hasat-core`'daki PR'ı merge et.
2. `hasat-core`'un `sync-to-web.yml` Action'ı otomatik tetiklenip
   `hasat-mobile`'a (ve `hasat-d2c-marketplace`'e) bir "hasat-core sync" PR'ı
   açmalı — **bu PR'ları da merge et** (aksi halde M5 açık maddesi olarak
   kapanan sürüm-gerisi kör noktası tekrar canlı hale gelir, bkz.
   `Shared-Architecture.md`).
3. `hasat-mobile`'daki asıl scaffold PR'ını merge et.

| # | Adım | Beklenen |
|---|---|---|
| 1 | `hasat-core` PR'ını GitHub'da aç, diff'i gözden geçir | `core/supabase/client.ts` yeni, `sync-to-web.yml`/`drift-check.yml` matrix'li |
| 2 | Merge et | `main`'e iner |
| 3 | Actions sekmesinde `sync core to targets` workflow'unun koştuğunu izle | İki job (web + mobil) yeşil, her ikisine de "hasat-core sync" PR'ı açılmış |
| 4 | O iki sync PR'ını da merge et | `hasat-mobile`/`hasat-d2c-marketplace`'te `src/lib/core/` güncel |
| 5 | `drift-check.yml`'i elle tetikle (workflow_dispatch) | İki hedefte de "Sapma yok" + "Sürüm gerisi yok" — yeşil |
| 6 | `hasat-mobile` scaffold PR'ını merge et | Expo app + login ekranı `main`'e iner |

#### B. Simülatörde marka + OTP + oturum kalıcılığı — **yeniden yazıldı (M5-a-ek, Appetize.io yoluyla)**

**Neden değişti:** Eski B bölümü Expo Go/gerçek cihaz varsayıyordu — Apple
Developer bireysel hesabı henüz onaylanmadı (başvuru yapıldı, 2026-07-30/31
kararı) ve elde Android cihaz yok. Onay gelene kadar mobil doğrulama **iOS
Simulator build + tarayıcı tabanlı Appetize.io** ile yapılacak — Berkin'in
kendi tarayıcısından, hiçbir hesap/kurulum gerekmeden.

**Ön koşul (bir kere, terminalden — `hasat-mobile` klasöründe):**
1. `npx eas-cli build --profile simulator --platform ios` — `eas.json`'daki
   yeni `simulator` profiliyle (internal distribution, `ios.simulator: true`)
   bulutta bir iOS Simulator `.app`'i derler; Apple Developer hesabı
   **gerekmez**. İlk çalıştırmada proje EAS'a bağlı değilse `eas init`
   akışını kendisi başlatır (bkz. `TODO.md` → "EAS kurulumu" adım adım
   talimat).
2. Build bitince expo.dev'deki build sayfası bir indirme linki verir —
   tarayıcıdan `.tar.gz`'i indir, aç, içindeki `.app`'i çıkar.
3. https://appetize.io/upload adresine tarayıcıdan git, `.app`'i sürükle-bırak
   yükle (hesap açmaya gerek yok, ücretsiz plan aylık ~100 dk sunuyor).
4. Appetize bir simülatör penceresi açar — bu, telefon ekranının tarayıcıda
   çalışan bir kopyası.

**Test adımları (kural #104 — kullanıcı-akışı dilinde):**

| # | Adım | Beklenen |
|---|---|---|
| 1 | Appetize penceresinde "Tap to play"a bas, uygulamanın açılmasını bekle | Splash sonrası `/login` ekranı açılıyor |
| 2 | **Login ekranının renklerine bak** (arka plan, buton, başlık rengi) | Expo'nun varsayılan mavi/beyaz teması **DEĞİL** — `hasat-core/core/design/tokens.ts`'teki Hasat marka renkleri görünüyor. Bu, design token bağlantısının somut görsel kanıtı — "kodda doğru" ile "ekranda doğru" arasındaki fark burada kapanıyor. |
| 3 | Telefon alanına test hesabını gir: `5001234567` (çiftçi) ya da `5009876543` (alıcı) | "Kod Gönder" aktifleşiyor |
| 4 | Kod Gönder'e bas, OTP ekranına geç, `123456` yaz, Giriş Yap'a bas | `/home`'a yönleniyor, telefon numarası + rol doğru görünüyor |
| 5 | **Asıl test:** Appetize'ın üst menüsünden uygulamayı sonlandır (Restart App / uygulamayı kapat, simülatör session'ını kapatmadan), tekrar başlat | **Tekrar `/login` istemiyor** — direkt `/home`'a düşüyor. Oturum `expo-secure-store` (AES anahtarı) + AES-şifreli `AsyncStorage`'da kalıcı olduğunu kanıtlıyor. |
| 6 | `/home`'daki "Çıkış yap"a bas | `/login`'e dönüyor |
| 7 | **Asıl test:** Uygulamayı adım 5'teki gibi tekrar kapat/aç | Bu sefer `/login` gösteriyor — çıkış gerçekten oturumu temizlemiş (SecureStore + AsyncStorage'daki anahtar/şifreli veri silinmiş) |

> **Appetize'da "kapat/aç" netliği:** Appetize'da gerçek bir telefonun task
> switcher'ından silme hareketi yok; en yakın karşılığı üst menüdeki "Restart
> App" (uygulamayı sonlandırıp aynı session içinde yeniden başlatır).
> Session'ın kendisi tamamen kapatılıp yeniden açılırsa (sayfa yenilenip
> yeni bir "Tap to play"e basılırsa) bazı durumlarda sıfır cihaz gibi
> davranabilir — adım 5/7'de **hangisinin kullanıldığı QA notuna
> yazılmalı**, çünkü ikisi farklı şeyi test eder (uygulama kapat/aç vs.
> cihaz sıfırlama).

⚠️ Appetize'ın ücretsiz planı dakika sınırlı — testi art arda, gereksiz
beklemeden yap.

#### Simülatörde DOĞRULANAMAYACAKLAR — açıkça işaretli

Bu dördü **gerçek cihaz** ve/veya ücretli Apple Developer hesabı gerektirir;
Appetize/Simulator yoluyla test edilmiş sayılmaz:

| Ne | Neden simülatörde olmuyor |
|---|---|
| **Push bildirimleri** | Appetize/iOS Simulator gerçek APNs/FCM teslimatını simüle etmiyor |
| **Gerçek uçak modu** | Simülatörün "ağ yok" hali bir cihazın radyosunu kapatmasıyla aynı değil — offline-önbellek testinin **asıl hali** (Apple 4.2'nin gerçek testi, `Build/Store-Compliance.md` → madde 6) gerçek cihazda yapılmalı |
| **Keychain/SecureStore'un cihazdaki gerçek davranışı** | iOS Simulator'ın Keychain'i cihazın Secure Enclave'ine dayanmıyor; AES anahtarının gerçek cihazda kalıcılığı/güvenliği ayrı doğrulanmalı |
| **Performans** | Appetize bulutta çalışan bir simülatörün ekran akışını gösteriyor — gerçek cihazın CPU/GPU/pil davranışını yansıtmıyor |

Bu dördü `TODO.md` → **"Apple hesabı gelince koşulacak testler"** başlığı
altında birikiyor, Apple Developer hesabı onaylanıp gerçek cihaza kurulum
mümkün olunca tek turda koşulacak.

#### C. Web'de auth regresyonu yok (canlı auth'a dokunan tek nokta)
| # | Adım | Beklenen |
|---|---|---|
| 1 | `hasat.lovable.app/login`'de normal telefon+OTP girişi yap (kendi hesabınla ya da test hesabıyla) | Öncekiyle birebir aynı akış — hiçbir görsel/davranışsal fark yok |
| 2 | Giriş yaptıktan sonra sayfayı yenile (F5) | Oturum düşmüyor, tekrar login istenmiyor |
| 3 | Tarayıcıyı tamamen kapat, tekrar aç, `hasat.lovable.app`'e git | Hâlâ giriş yapılmış durumda (localStorage korunmuş) |
| 4 | DevTools → Application → Local Storage'da Supabase'in oturum anahtarını kontrol et | Değer var, önceki formatla aynı görünüyor |

**Beklenen sonuç: A (6/6, tarihi — zaten yapıldı) + B (7/7, yeni Appetize
yolu) + C (4/4) geçiyor.** B ve C Claude Code tarafından **doğrulanamadı**
(bu oturumun ağ kısıtı + gerçek tarayıcı/simülatör erişimi yok) — bu QA'nın
asıl amacı tam da bu iki bölüm.

#### D. Ayrıca kontrol edilecek (kapsam dışı bulgular)
- ✅ **Çözüldü (bu turda):** `hasat-core/db/types.ts`'te `recipes.rest_minutes`
  eksikti (M4-c'den beri tip üretimi yenilenmemişti) — canlı şemadan yeniden
  üretildi, `hasat-core`'a commit edildi, dual-target sync PR'ları açılacak
  (merge listesine bkz. yukarıda). Kalıcı çözüm olarak `types-freshness.yml`
  CI kontrolü eklendi (kural #111).
- Süre filtresi düzeltmesi (`hasat.lovable.app/tarifler`, "Süre" filtresi):
  Ekşi Mayalı Ekmek/Köme artık "1 saatten uzun" filtresine düşmüyor (aktif
  süreleri düşük), ama kartlarında/detay sayfasında turuncu "Önceden başlamak
  gerekir" rozeti görünmeli. (Bu turun kapsamı dışında, hâlâ açık.)

---

### S26 — P23-M5-b Tarif Ekranları + Offline Önbellek

**Arka plan:** `Build/P23-Mobile.md` → M5-b, `TODO.md` → "P23-M5-b" build
log, `Build/P23-Mobile-Visual-Spec.md` → "2. Offline Durumu". Bu turda
gerçek bir simülatör build'i **gerekiyor** — önceki turların (S25) build'i
`app/home.tsx`'in eski "Giriş yapıldı ✓" yer tutucusunu taşıyor, tarif
ekranlarını içermiyor.

**Merge listesi**
| Repo | İçerik |
|---|---|
| `hasat-core` | `scripts/check-env-guard.mjs` (kara liste → beyaz liste), `drift-check.yml` yorum güncellemesi |
| `hasat-mobile` | `app/home.tsx` (tarif listesi), yeni `app/recipe/[slug].tsx`, `src/lib/hasat/*`, `src/lib/offline/*`, `src/lib/net/*`, `src/components/hasat/*`, `package.json` (`expo-sqlite`, `expo-network`), yeni `.github/workflows/env-guard.yml` |
| `hasat-vault` | Bu doküman + `TODO.md` + `P23-Mobile.md` + `_Context.md` |

> ⚠️ **Önce yeni bir simulator build al:** Actions → **"EAS Simulator Build
> (iOS)"** → **Run workflow** → bitince özet sayfasındaki linkten `.tar.gz`'i
> indir → `.app`'i çıkar → https://appetize.io/upload → sürükle-bırak yükle.
> S25'teki build'i tekrar kullanma — bu turun kodu farklı.

**Test adımları (kural #104 — kullanıcı-akışı dilinde):**

| # | Adım | Beklenen |
|---|---|---|
| 1 | Yeni build'i Appetize'a yükle, "Tap to play" | Splash sonrası `/login` |
| 2 | Test hesabıyla gir (`5001234567` ya da `5009876543`, OTP `123456` — mobilde çalışmıyorsa gerçek SMS gerekir, bkz. `TODO.md` madde 1) | `/home`'a düşüyor, ama artık **tarif listesi** görünüyor — "Giriş yapıldı ✓" yer tutucusu YOK |
| 3 | Listede en az bir kartın kapak/temsili görselini incele | Fotoğraf varsa görünüyor; yoksa crop görseli + "Temsili görsel" etiketi; o da yoksa nötr placeholder emoji — boş kutu YOK |
| 4 | "Cevizli Üzümlü Köme" ya da "Ekşi Mayalı Tam Buğday Ekmeği" kartına bak | "⏰ Önceden başlamak gerekir" rozeti görünüyor (rest_minutes 4320/1020 dk, eşik 120 dk) |
| 5 | Bir tarife dokun, detay ekranına geç | Malzemeler + Hazırlanışı bölümleri, süre satırı "X dk hazırlık · Y dk pişirme · Z dk dinlenme" formatında (tek sayıya toplanmamış) |
| 6 | Malzeme listesine bak | En az bir "eşleşti" (fiyat/min. sipariş görünür), çoğunluk nötr "Hasat'ta henüz yok" rozeti (buton YOK — "Talep Et" bu turda kapsam dışı), tuz/maydanoz gibi platform-dışı malzemelerde hiçbir rozet yok |
| 7 | Porsiyon +/− butonlarına dokun | Malzeme miktarları değişiyor (porsiyon ölçekleme çalışıyor) |
| 8 | Detay ekranından çık, "Çıkış ✕" ile çıkış yap, tekrar gir | Rol/oturum davranışı M5-a'dakiyle aynı — regresyon yok |
| 9 | **Offline (kısmi doğrulama):** cihazın/simülatörün Wi-Fi'ını kapat (tam uçak modu değil — bkz. not), listeye dön | Üstte "📶✕ Çevrimdışısınız · görünen tarifler önbellekten" şeridi + liste **normal render** (ayrı bir offline ekranı yok) |
| 10 | Wi-Fi kapalıyken daha önce açılmamış bir tarife git (varsa) | Önbellekte olmadığı için "Bu tarif önbellekte yok — internete bağlanıp bir kez açtıktan sonra çevrimdışı da görünür" mesajı |
| 11 | **Uçak modu (P23-M5-b-ek — bulk detay prefetch):** Wi-Fi'ı tekrar aç, listeye dönüp birkaç saniye bekle (arka planda 18 tarifin tamamı önbelleklensin), sonra **uçak moduna al → uygulamayı yeniden başlat** ("Restart App", session'ı kapatma) → liste görünüyor → **daha önce hiç açılmamış** bir tarife tıkla | Liste önbellekten normal render oluyor; tıklanan tarifte "Bu tarif önbellekte yok" mesajı YERİNE **adımlar ve malzemeler** görünüyor — çünkü artık yalnızca daha önce açılmış tarifler değil, listenin tamamı arka planda önbelleklendi |

> **Appetize'da oturum testi netliği (S25'teki notla aynı, burada da
> geçerli):** Adım 8-10 arası **sayfayı yenileme** (yeni bir "Tap to
> play"/yeni session) — session yenilemesi simüle edilen cihazın
> depolamasını (AsyncStorage + sqlite dosyası dahil) sıfırlayabilir, bu da
> "önbellek boşmuş gibi" yanlış bir sonuç üretir. Appetize'ın üst
> menüsündeki **"Restart App"** kullanılmalı (uygulamayı sonlandırıp aynı
> session içinde yeniden başlatır), session'ın kendisi kapatılmamalı.

⚠️ **Adım 9-11 gerçek uçak modu YERİNE geçmez** — Appetize/iOS Simulator'da
"uçak modu" seçeneği cihazın radyosunu kapatmaz, olsa olsa Wi-Fi'ı kapatan
adım 9-10 gibi bir yaklaşık durumdur (adım 11'deki "uçak moduna al" da bu
sınır içinde okunmalı — simülatörün kendi ayarları üzerinden en yakın
yaklaşımdır, gerçek radyo kapatma değildir). Apple 4.2'nin asıl testi
(cihazın radyosu tamamen kapalıyken, App Review reviewer'ının yapacağı
tam senaryo) yalnızca gerçek cihazda yapılabilir; bu madde `TODO.md` →
"Apple hesabı gelince koşulacak testler" listesinde zaten duruyordu,
P23-M5-b-ek'in bulk detay prefetch'i eklendiği için önemi arttı — aynı
listeye "gerçek uçak modunda bir tarife dokunup adım+malzeme görünüyor mu"
adımı da eklendi, liste maddesi değişmedi ama kapsamı genişledi.

**Beklenen sonuç: 11/11.** Bu S26'nın tamamı Claude Code tarafından
**doğrulanamadı** (bu oturumda simülatör/cihaz erişimi yok, kural #103) —
RPC/veri doğruluğu Supabase MCP ile SQL üzerinden ayrıca kanıtlandı (bkz.
`TODO.md` → "P23-M5-b" madde 6, "P23-M5-b-ek" madde 1 ve 6), ama ekranda
gerçekten doğru render olduğu, gerçek `recipe_views` ağ yazımı, bulk
detay prefetch'in gerçek çalışma zamanı davranışı ve sqlite'ın gerçek
çalışma zamanı davranışı yalnızca Berkin'in bu QA'sıyla kanıtlanabilir.

---

### S27 — P23-M6 Native Yetenekler (Pişirme Modu · AI Import · Push)

**Arka plan:** `Build/P23-Mobile.md` → M6, `TODO.md` → "P23-M6" build log,
`Build/P23-Mobile-Visual-Spec.md` → "1. Pişirme Modu" ve "3. AI Import
Akışı", `Build/Store-Compliance.md` → "Durum tablosu (2026-08-03)".

> ⚠️ **ADIM 0 — ÖNCE YENİ BİR SİMULATOR BUILD AL VE APPETIZE'A YÜKLE.**
> Actions → **"EAS Simulator Build (iOS)"** → **Run workflow** → bitince
> özet sayfasındaki linkten `.tar.gz`'i indir → `.app`'i çıkar →
> https://appetize.io/upload → sürükle-bırak yükle. **S26'nın build'ini
> tekrar kullanma:** o build'de pişirme modu, AI import ve push kodu YOK,
> ayrıca bu turda dört yeni native modül eklendi (`expo-keep-awake`,
> `expo-notifications`, `expo-image-picker`, `expo-device`) — yeni bir
> derleme olmadan hiçbiri cihazda mevcut olmaz. Kota: ayda 15 iOS build,
> 2 kullanıldı.

**Merge listesi**
| Repo | İçerik |
|---|---|
| `hasat-core` | `core/db/types.ts` (yeniden üretildi: `device_tokens.updated_at` + `rpc_register_device_token`), `core/.manifest` |
| `hasat-mobile` | Yeni `app/cook/[slug].tsx`, `app/import.tsx`, `src/lib/native/*`, `src/lib/hasat/{import,myRecipes}.ts`, `src/components/hasat/PushPermissionCard.tsx`; değişen `app/home.tsx`, `app/recipe/[slug].tsx`, `app/_layout.tsx`, `app.json`, `src/lib/hasat/recipes.ts`, `src/lib/offline/*` |
| `hasat-vault` | Bu doküman + `TODO.md` + `P23-Mobile.md` + `Store-Compliance.md` + `_Context.md` |
| Supabase | `p23_m6_device_token_takeover` migration'ı **zaten uygulandı** (canlı) — merge gerektirmiyor, bilgi amaçlı |

> **Merge sırası önemli:** önce `hasat-core` → sync PR'ları `hasat-mobile`
> ve web'e insin → sonra `hasat-mobile`. Aksi halde drift "sürüm-gerisi"
> kontrolü hedefleri bayat görür (bkz. `Shared-Architecture.md`).

**Test adımları (kural #104 — kullanıcı-akışı dilinde):**

| # | Adım | Beklenen |
|---|---|---|
| 1 | Yeni build'i Appetize'a yükle, "Tap to play", test hesabıyla gir (gerçek SMS — Berkin kararı, `TODO.md` → M5-b-ek madde 4) | Tarif listesi açılıyor; üstte **iki sekme**: "Hasat Tarifleri" / "Defterim"; sağ altta **"+ Tarif Ekle"** butonu |
| 2 | İlk açılışta bildirim izni kartına bak | Sistem dialogu DEĞİL, önce Hasat'ın kendi kartı: "Haberin olsun mu?" + neden istendiğinin açıklaması + "Şimdi değil" / "Bildirimlere izin ver" |
| 3 | "Bildirimlere izin ver"e dokun | Şimdi sistem dialogu çıkıyor. İzin verilince kart kayboluyor. ⚠️ Simülatörde token alınamaz (gerçek cihaz gerekiyor) — hata görünmemeli, uygulama normal çalışmaya devam etmeli |
| 4 | "Şimdi değil"i seçtiysen: uygulamayı yeniden başlat | Kart tekrar çıkabilir (izin hâlâ "sorulmamış" durumda) — çökme/boş ekran olmamalı |
| 5 | Bir tarife gir, "Hazırlanışı" bölümüne in | Adımların üstünde **"👨‍🍳 Pişirmeye Başla"** butonu; süreli adım varsa altında "Süreli adımlarda zamanlayıcı çalışır, ekran kararmaz" notu |
| 6 | "Pişirmeye Başla"ya dokun | Tam ekran mod: üstte ✕ ve "1 / N", ilerleme çubuğu, büyük punto adım metni, altta tam genişlikli "← Önceki / Sonraki →" |
| 7 | Süresi olan bir adıma ilerle (örn. `Zeytinyağlı Nohut Yemeği`) | Adım metninin **altında** ayrı timer kartı: büyük `mm:ss`, "▶ Başlat" ve "↺ Sıfırla" |
| 8 | "Başlat"a dokun | Önce "Süre dolunca haber verelim mi?" açıklama kartı (izin daha önce verilmediyse), ardından geri sayım işlemeye başlıyor. **Geri sayım kartı beklemiyor** — dialog açıkken de sayıyor |
| 9 | "Durdur" → "Devam et" → "Sıfırla" | Durdurunca sayım duruyor, devam edince kaldığı yerden sürüyor, sıfırlayınca başlangıç değerine dönüyor |
| 10 | **Timer'ın arka plan doğruluğu:** timer çalışırken uygulamayı arka plana at (Appetize'da ana ekrana dön), ~1 dakika bekle, geri gel | Kalan süre **geçen gerçek süre kadar azalmış** olmalı (donmuş/geride kalmış değil) — zaman-damgası yaklaşımının asıl testi. ⚠️ Appetize'ın arka plan davranışı gerçek cihazdan farklı olabilir; kesin test gerçek cihazda |
| 11 | `Cevizli Üzümlü Köme` adım 6'ya (3 günlük kurutma) git | Geri sayım **YOK**; "Tahmini süre: 3 gün" + "geri sayım tutulmuyor" açıklaması |
| 12 | `Kekikli Zeytinyağı Ezmesi` gibi süresiz bir adıma git | Timer kartı **hiç görünmüyor** (boş/gri kart yok) |
| 13 | ✕ ile pişirme modundan çık, tarife dön | Normal detay ekranı; ekran kararma davranışı normale dönmüş olmalı (gözle doğrulanamaz — bkz. aşağıdaki not) |
| 14 | "+ Tarif Ekle" → "✍️ Metin Yapıştır" → gerçek bir tarif metni yapıştır → "Tarifi Çıkar" | "⏳ Tarif okunuyor…" (belirsiz spinner, ilerleme çubuğu YOK), ardından **"Kontrol Et"** ekranı |
| 15 | "Kontrol Et" ekranında başlığı değiştir, bir malzemenin miktarını düzelt, bir malzeme sil, bir adım ekle | Her alan düzenlenebiliyor; malzemelerde crop rozeti YOK (tasarım gereği — eşleştirme editoryal) |
| 16 | "Kaydet" | "✅ Defterine kaydedildi · Bu tarif yalnızca sana görünür" ekranı |
| 17 | "Defterime dön" → "Defterim" sekmesi | Yeni tarif listede; "🔒 yalnızca sana görünür · metinden" etiketi |
| 18 | **"Hasat Tarifleri" sekmesine geç** | Eklediğin tarif **BURADA GÖRÜNMEMELİ** — public korpus 18 tarif olarak kalmalı (kullanıcı importunun korpusa karışmadığının gözle kanıtı) |
| 19 | Defterim'deki kendi tarifine gir, "Pişirmeye Başla" | Kendi tarifinde de pişirme modu çalışıyor (girdiğin süreler geri sayıma dönmüş olmalı) |
| 20 | "+ Tarif Ekle" → çok kısa bir metin (<20 karakter) yapıştırmayı dene | "Tarifi Çıkar" butonu pasif — boşuna AI çağrısı yapılmıyor |
| 21 | "+ Tarif Ekle" → tarif olmayan bir metin (örn. bir haber paragrafı) yapıştır | Anlaşılır Türkçe hata: "Gönderdiğinde bir tarif bulamadık…" — çıplak hata kodu YOK |
| 22 | Çıkarım ekranından ✕ ile çık (kaydetmeden) | Defterim'de yarım bir taslak **birikmemeli** |
| 23 | Wi-Fi kapalıyken "+ Tarif Ekle" | "Çevrimdışısın — tarif çıkarma için internet bağlantısı gerekiyor" + üç seçenek de pasif |
| 24 | Wi-Fi kapalıyken "Defterim" sekmesi | Boş liste değil, açıklayıcı metin: "Defterin çevrimdışı görüntülenemiyor…" |
| 25 | Wi-Fi kapalıyken bir public tarifte pişirme modu | **Tamamen çalışıyor** — adımlar önbellekten, timer yerel (Apple 4.2 argümanının çekirdeği) |
| 26 | "Çıkış ✕" → tekrar giriş | Regresyon yok; çıkışta push token'ı da siliniyor (gözle görünmez, DB'den kontrol edilebilir) |

**Beklenen sonuç: 26/26** (adım 0 dahil değil).

#### ⚠️ Appetize/simülatörde TEST EDİLEMEYECEKLER — ayrı tutuluyor

Bu üçü yukarıdaki listeye **bilerek alınmadı**; "geçti" işaretlenemez,
gerçek cihaz bekliyorlar (`TODO.md` → "Apple hesabı gelince koşulacak
testler"):

| Ne | Neden simülatörde olmuyor |
|---|---|
| **Kamera** (adım 14'ün fotoğraf yolu: "📷 Fotoğraf Çek") | iOS Simulator'da kamera donanımı yok — buton izin dialoguna kadar gidebilir ama gerçek bir kare üretilemez. "🖼 Galeriden Seç" simülatörün örnek fotoğraflarıyla kısmen denenebilir, ama o da yazılı tarif fotoğrafı değildir (OCR yolu gerçek bir tarif sayfası ister) |
| **Push** (gerçek bildirim teslimatı) | Simülatör gerçek APNs/FCM token'ı üretmez (`Device.isDevice` false); ayrıca Android FCM V1 anahtarı ve iOS APNs anahtarı henüz EAS'a yüklenmedi. Yani hem cihaz hem kredansiyel eksik |
| **Gerçek uçak modu** (adım 23-25'in tam hali) | Appetize/iOS Simulator'ın "uçak modu"su cihazın radyosunu kapatmaz; Wi-Fi kapatmak en yakın yaklaşımdır. Apple 4.2'nin asıl testi (reviewer'ın yapacağı) yalnızca gerçek cihazda |

Ayrıca **ekranı uyanık tutma** (adım 13) gözle doğrulanamaz: bulut
simülatörünün ekran kararma zamanlayıcısı gerçek cihazınkiyle aynı değil.
Pil etkisi de yalnızca gerçek cihazda ölçülebilir.

**Bu S27'nin tamamı Claude Code tarafından doğrulanamadı** (kural #103 —
oturumda simülatör/cihaz yok). Sunucu tarafı ayrıca ve gerçekten
doğrulandı: `device_tokens` devri gerçek insert/update ile,
`extract-recipe` gerçek bir kullanıcı JWT'siyle gerçek çağrıyla
(`visibility='private'`, `author_type='kullanici'`, kota 429 dahil) —
detay ve tam tablo: `TODO.md` → "P23-M6" madde 5.

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
- **2026-07-31:** **S25'in B bölümü yeniden yazıldı (P23-M5-a-ek — test altyapısı, bayat tipler, `.env` bekçisi).** Apple Developer bireysel hesabı onay bekliyor, elde Android cihaz yok, gerçek iPhone'a kurulum ücretli hesap olmadan mümkün değil — bu yüzden eski B bölümünün Expo Go/gerçek cihaz varsayımı iOS Simulator build (`eas.json`'a yeni `simulator` profili) + tarayıcı tabanlı Appetize.io yoluyla değiştirildi. Yeni B: marka renklerinin görsel kanıtı (Expo varsayılanı değil), OTP girişi, uygulama kapat/aç sonrası oturum kalıcılığı, çıkış sonrası oturumun gerçekten temizlendiği. Push/gerçek uçak modu/Keychain-SecureStore cihaz davranışı/performans simülatörde doğrulanamayacağı açıkça işaretlendi (`TODO.md` → "Apple hesabı gelince koşulacak testler"). Ayrıca bu turda `hasat-core/db/types.ts`'teki bayat `recipes.rest_minutes` eksikliği (M4-c'den beri tip üretimi yenilenmemişti — drift check yeşil kaldı çünkü core↔hedef tutarlıydı, DB↔core değildi) canlı şemadan yeniden üretilerek düzeltildi, kalıcı çözüm olarak `types-freshness.yml` CI kontrolü eklendi (kural #111); `hasat-mobile/.env`'e içerik bekçisi eklendi (her satır `EXPO_PUBLIC_` ile başlamalı + `service_role`/`SECRET`/`PRIVATE`/`TOKEN`/`PASSWORD` kalıpları reddedilir), kasten bozulup exit 1 verdiği doğrulanıp geri alındı — bir sınır bulundu ve raporlandı: görev metnindeki `EXPO_PUBLIC_SERVICE_KEY` örneği bu 5 literal kalıbın hiçbirini içermiyor, `KEY` kalıbı da eklenemez çünkü meşru `EXPO_PUBLIC_SUPABASE_PUBLISHABLE_KEY` satırını kırar.
- **2026-08-03:** **S26 eklendi (P23-M5-b tarif ekranları + `expo-sqlite` offline önbellek).** Yeni bir simülatör build'i gerektiriyor (S25'in build'i eski yer tutucu ekranı taşıyor). RPC/veri doğruluğu Supabase MCP ile SQL üzerinden doğrudan doğrulandı (`TODO.md` → "P23-M5-b" madde 6); ekranda render, gerçek ağ `recipe_views` yazımı ve sqlite'ın çalışma zamanı davranışı bu oturumda test edilemedi (simülatör/cihaz yok) — Berkin'in QA'sına kaldı. Kural #107 gereği iki madde (mobil test giriş yolu, çiftçi girişi) yalnızca araştırılıp sunuldu, karar verilmedi.
- **2026-08-03:** **S26'ya adım 11 eklendi (P23-M5-b-ek — bulk detay prefetch + uçak modu).** Liste ağdan çekilince artık 18 tarifin tamamının detayı arka planda önbelleklendiği için, yeni adım özellikle **daha önce hiç açılmamış bir tarife** uçak modundayken dokunmayı test ediyor (eski adım 9-10 yalnızca Wi-Fi kapatmayı test ediyordu, bu da hâlâ gerçek uçak modu değil — bkz. adım 11'in üstündeki not). Berkin kararı gereği (madde 4/5, `TODO.md` → "P23-M5-b-ek") test girişi gerçek SMS ile, çiftçi rolüyle de tüm ekranlar erişilebilir kaldığı için bu S26 senaryosu değişmedi.
- **2026-08-03:** **S27 eklendi (P23-M6 native yetenekler — pişirme modu, AI import, push).** İlk adım yeni bir simulator build alıp Appetize'a yüklemek (S26'nın build'inde bu turun kodu ve dört yeni native modül yok). Sunucu tarafı bu turda gerçekten doğrulandı: `device_tokens` UNIQUE(token) devri gerçek insert/update ile (önce arıza birebir üretildi: client'ın düz upsert'ü RLS USING'e takılıp `42501` veriyor), `extract-recipe` gerçek bir kullanıcı JWT'siyle `pg_net` üzerinden çağrıldı ve kaydın `visibility='private'`/`author_type='kullanici'` olduğu, client'ın gönderdiği `public` değerinin yok sayıldığı, kota aşımında 429 döndüğü kanıtlandı; test verisi temizlendi. Kamera, push teslimatı ve gerçek uçak modu ayrı bir tabloda "simülatörde test edilemez" olarak tutuluyor — S27'nin 26 adımına dahil değil.
