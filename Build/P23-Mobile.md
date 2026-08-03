---
title: Hasat — P23 Buyer Mobile & Recipe App
updated: 2026-07-31
tags:
  - hasat
  - p23
  - mobile
  - recipe
---

# P23 — Buyer Mobile & Recipe App

> Onaylandı: 2026-07-28 · Görsel takvim: `Build/Roadmap.md`
> **Kural: kapsam kesilmez, tarih ötelenir.**

---

## Stratejik çerçeve

**Tarif katmanı, marketplace katmanına kullanıcı çekmek için var.** Başarı ölçüsü tarif uygulamasının kendi geliri değil, **tarif → kayıt → talep → teklif → sipariş** dönüşümüdür. Bu yüzden `v_kpi_recipe_funnel` ve `recipe_rfq_links` yan özellik değil, çekirdektir — huni ölçülmezse katmanın işe yarayıp yaramadığı bilinemez.

### Konumlandırma (Berkin kararı, 2026-07-28)
Hasat bir hızlı teslimat uygulaması **değil**. Ürünler geç gelebilir, toplu alınmak zorunda olabilir — ama güvenilir ve kaliteli olur. Beklenti yönetimi (teslim süresi, minimum miktar, "bu miktar ~N tarif yapar") ürünün süsü değil, **güven tezinin uygulanmasıdır.**

### Arz gerçeği ve "Talep Et"
2026-07-28 denetimi: 70 crop'un **9'unda** aktif ilan var (17 aktif ilan). Bir tarifin malzemeleri çoğu zaman eşleşmeyecek. Berkin kararı: **eşleşmeyen malzeme için "Talep Et" butonu.**

Bu, eksikliği varlığa çeviriyor: `crop_requests` crop bazında toplandığında **"insanlar hangi ürünü istiyor ama Hasat'ta yok"** listesi doğuyor → doğrudan **çiftçi kazanım öncelik listesi**. Döngü: tarif → talep → hedefli çiftçi edinimi → arz → daha fazla eşleşen tarif.

**Şart:** Talep butonu ölü bir form olmamalı — talep eden kullanıcıya "o ürün geldiğinde haber ver" bağlantısı kurulmalı (mevcut `price_alerts` deseni).

---

## Platform kararı — neden Expo

**Kısıt:** Berkin'in bilgisayarı şirket Mac'i; local'de Xcode/imzalama yönetilemiyor. Capacitor bir Xcode projesi üretir ve elle müdahale ister → **iOS tarafı kapalı.** Expo + EAS Build tamamen bulutta derler ve submit eder.

**Kabul edilen bedel:** Lovable React Native üretemez. Web'de Lovable + Claude Code birlikte çalışır; **mobil %100 Claude Code.**

### Bedeli düşüren üç karar
- **Nativewind** — Tailwind sözdizimi React Native'de; web bileşenlerini port etmek sıfırdan yazmaya değil kopyalamaya yakın
- **Expo Router** — dosya tabanlı, TanStack Router'a benziyor; yönlendirme mantığı korunur
- **Monorepo yok** — bkz. `Build/Shared-Architecture.md` (Lovable sync'ini kırma riski)

---

## Mobil v1 kapsam kararı — checkout YOK

**Mobil v1'de ödeme ekranı bulunmayacak.** Akış "Talep Et" / teklif oluşturmada biter, ödeme web'e devredilir.

Bu tek karar üç sorunu birden çözer:
1. **Ödeme altyapısı (P17-A/iyzico) gecikirse uygulama bloke olmaz** — şirket bağımlılığı uygulamadan izole edilir
2. **App Store Guideline 2.1 riski kalkar** — çalışmayan bir "Ödemeyi Tamamla" ekranı "tamamlanmamış uygulama" reddine yol açabilirdi
3. **IAP tartışması biter** — fiziksel ürün ödemesi zaten IAP dışı, ama hiç ödeme ekranı olmaması tartışmayı tamamen kapatır

Ayrıca "yavaş/toplu/güvenilir" konumlandırmasıyla tutarlı: kullanıcı zaten anında satın almıyor.

---

## AI ile tarif çıkarma — modalite matrisi

| Girdi | Durum | Not |
|---|---|---|
| **Yazıyla yapıştırma** | ✅ M6 | Yapılandırılmış JSON çıkarma; en sağlam yol |
| **Yazılı tarif fotoğrafı** (kitap sayfası, el yazısı) | ✅ M6 | Vision + OCR |
| **Bitmiş yemek fotoğrafı** | ⏭️ M9 | Model gerçek tarifi bilemez, makul bir tarif *uydurur*. **"Tahmini tarif" etiketi zorunlu** |
| **YouTube / sosyal medya linki** | ⏭️ M9 | Transkript gerekiyor; ToS + telif riski. **Hukuki kontrol şartıyla** |

### Zorunlu tasarım kuralı
Kullanıcının içe aktardığı tarifler **varsayılan olarak private** olur ve **asla otomatik public korpusa girmez.**
- **Public korpus** = Hasat'ın editoryal, özgün içeriği
- **Kullanıcı importları** = kişisel defter

Şema bunu taşır: `recipes.visibility` (public/private), `source_type` (manual/text/photo/url), `source_url`, `owner_id`, `extraction_confidence`.

### Telif
Tarif *malzeme listesi* çoğu hukuk sisteminde telifli değil; *anlatım, açıklama metni ve fotoğraf* teliflidir. **Hiçbir siteden tarif kazınmaz.** İçerik özgün yazılır veya şeflerden lisanslı alınır — bu aynı zamanda `Market.md`'deki "kişisel şef / F&B ağı" alıcı edinme kanalıyla örtüşür (tek işten iki çıktı).

---

## Şema (M2)

| Tablo | Amaç | Kritik nokta |
|---|---|---|
| `recipes` | slug (SEO), başlık, porsiyon, süre, zorluk, mutfak, diyet etiketleri, kapak foto, status, `visibility`, `source_type`, `source_url`, `owner_id`, `author_type` | `visibility` private/public ayrımı zorunlu |
| `recipe_steps` | adım no, metin, foto, `timer_seconds` | Pişirme modunun temeli |
| `recipe_ingredients` | `crop` (nullable FK → `crop_config`), `free_text_name` (tuz/un), miktar, culinary unit (text), `is_key_ingredient` | **Eşleme runtime'da fuzzy text matching ile YAPILMAZ** — editoryal olarak bir kez `crop` FK'sine bağlanır |
| `crop_culinary_meta` | `is_edible`, `culinary_aliases[]`, `conversion_hints` jsonb | Dönüşüm sadece alışveriş listesi sınırında |
| `recipe_saves` | kaydetme/beğenme | KVKK: kişisel veri, gizlilik metni güncellenmeli |
| `recipe_rfq_links` | `recipe_id` ↔ `crop_request_id` | Huni atıfı — "bu talep şu tariften doğdu" |
| `device_tokens` | `user_id`, `token`, `platform` | Push altyapısı (ekleyici) |

**Ek kolon:** `crop_config.default_photo_url`

**View:** `v_kpi_recipe_funnel` — tarif görüntüleme → kayıt → talep → teklif → sipariş (North Star'a bağlı)

### Birim kuralı
Culinary birimler (`adet`, `demet`, `yemek kaşığı`, `tutam`, `bardak`) **`unit_type` enum'una GİRMEZ.** Enum sadece `g`/`kg`/`L` kalır — P21'de kurulan birim-uyuşmazlığı trigger'ını ve stok toplamalarını kirletmemek için. Dönüşüm `crop_culinary_meta.conversion_hints` ile alışveriş listesi sınırında yapılır.

### RLS
`recipes`/`recipe_steps`/`recipe_ingredients` → public olanlar için anon SELECT açık (SEO şart), yazma sadece admin/sahip. `recipe_saves` ve private tarifler → tamamen kullanıcıya özel.

---

## Fotoğraf stratejisi (Berkin kararı)

2026-07-28 denetimi: **39 ilanın 0'ında fotoğraf var** (17 aktif + 22 draft). Fotoğraf upload altyapısı var (P16-A) ama hiç kullanılmamış.

**Çözüm:** `crop_config.default_photo_url`. UI mantığı:
```
listing.photo_urls[0] ?? crop_config.default_photo_url ?? placeholder
```

**Şart:** Temsili görselin üzerine **"temsili görsel"** etiketi. Hasat'ın tezi menşe/güven — stok fotoğrafını o çiftçinin ürünüymüş gibi göstermek tezi içeriden çürütür. Görsellerin telifi temiz olmalı (kendi çekim / CC0 / satın alınmış).

Bu kararla fotoğraf **kritik yoldan çıktı** — 39 çiftçi fotoğrafı toplamak yerine ~20 crop görseli gerekiyor, tamamen Berkin'in kontrolünde.

---

## Kilometre taşları

### M0 — Açık işlerin kapanışı + hesaplar
- **P22-D+E+F birleşik tarayıcı QA (15 adım)** — `TODO.md`'de bekliyor, P23 başlamadan kapanmalı
- Şirket tipi kararı + tescil başlatma (bkz. `Store-Compliance.md`)
- **Apple Developer bireysel hesap açılışı** ($99, D-U-N-S gerekmiyor, şirketten bağımsız)
- Expo/EAS hesabı
- **Çıkış:** QA kapandı, hesap süreçleri başladı

### M1 — Paylaşılan çekirdek
- `hasat-core` repo + subtree + GitHub Action + drift guard + do-not-edit işaretleri
- Design token'ları, Supabase storage adapter
- **Küçük şema borçları:**
  - `Safran Soğanı` `default_unit='adet'` gizli bug'ı (`unit_type` enum'unda `adet` yok → insert hatası riski). Güvenli çözüm: `crop_config`'i `kg`'ye çekmek
  - `listings` CHECK `min_order <= quantity` (şu an 0 CHECK constraint var, 1 ilan bu kuralı ihlal ediyor)
  - `buyer_profiles.company_name` NOT NULL → nullable (ev kullanıcısı şirket adı giremez; `bireysel` segment için onboarding sürtünmesi)
  - `useSetDefaultAddress` diğer adresleri `false`'a çekmiyor (mevcut açık bug; mobil de aynı tabloyu kullanacak)
- **Çıkış:** Web'de **sıfır regresyon**, drift kontrolü yeşil

### M2 — Tarif backend'i (tamamen ekleyici)
- Şema + RLS + RPC'ler + `v_kpi_recipe_funnel` + `device_tokens` + `default_photo_url`
- AI çıkarma edge function (`extract-recipe`): metin + görsel; `ai_usage_tracking` ile limitli
- **Çıkış:** Gerçek SQL + RLS simülasyonu; **3 crop testi** (mainstream + niş + yenilemez filtresi)

### M3 — İçerik — ✅ **TAMAMLANDI (2026-07-30)**

#### Onaylanan crop odağı

Araştırmayla belirlendi, iki katman + safran:

| Katman | Crop'lar |
|---|---|
| **Katman 1 — dayanıklı omurga** | `zeytinyağı`, `nohut`, `mercimek`, `kekik`, `fındık`, `ceviz`, `buğday` (7) |
| **Katman 2 — taze, Ağustos–Ekim penceresi** | `domates`, `biber`, `patlıcan`, `üzüm`, `incir`, `elma` (6) |
| **Safran** | Tam 1 tarif, öne çıkarılmadan — diğerlerinin arasında bir crop (P25 crop-agnostic ilkesi) |

✅ **Sayı notu — ÇÖZÜLDÜ (P23-M4-a, 2026-07-30):** Görev metni bu listeyi
"13 odak crop" olarak adlandırmıştı, ama yukarıdaki liste 7+6+1=**14** crop
içeriyor. M3'te bu otonom olarak çözülmüştü: **14 crop'un tamamı** hem
`crop_culinary_meta` seedinde hem `default_photo_url` görsel listesinde
işlendi. **Berkin M4-a'da doğruladı: "13" kendi aritmetik hatasıydı, doğru
sayı 14** — artık açık madde değil. Gerekçe (M3'te otonom karar verilirken):
safran'ı culinary seedden hariç tutmak kendi tarifinin (Safranlı Zerde,
"1 tutam safran") `rpc_recipe_shopping_list`'te NULL dönmesine yol açardı —
bu da E doğrulamasının kendisini (aşağıda) ihlal ederdi ve P25
crop-agnostic ilkesiyle çelişirdi (safran'ı görsel altyapıdan dışlamak onu
ikinci sınıf crop yapardı, "öne çıkarma" değil "eksik bırakma" olurdu).

**Gerçekleşen dağılım:** Katman 1'den 10 tarif, Katman 2'den 7 tarif, safran
1 tarif = **18 tarif toplam** (hedef aralık 15–20 içinde).

#### Gerekçe (dört madde)

1. **İstanbul şeflerinin acısı yenilik değil, güvenilir tedarik.** Şefler
   zaten kullandıkları malzemenin (zeytinyağı, nohut, domates, kekik...) en
   iyisini kaynağından bulmak istiyor — niş/egzotik crop'lar bu acıyı
   çözmüyor. Bu yüzden mainstream crop odağı, egzotik değil.
2. **Dayanıklı ürün hasat penceresinden bağımsız satılır.** Lansman
   Eylül/Ekim'e kayarsa (bkz. `Roadmap.md` öteleme kuralı) Katman 1'in
   stratejisi bozulmaz — zeytinyağı/nohut/mercimek/kekik/fındık/ceviz/buğday
   yıl boyu tedarik edilebilir.
3. **Dayanıklı ürün ihtilaf riskini düşürür.** North Star **ihtilafsız**
   tamamlanmış GMV; lojistik (soğuk zincir, hızlı bozulma) henüz
   olgunlaşmamışken taze/kırılgan ürün odaklı bir tarif seti doğrudan bu
   metriği riske atardı.
4. **Kadıköy talep profili.** Zeytinyağlı meze kültürü, vegan/humus/falafel
   patlaması, kahvaltı kültürü, ekşi maya/bakery trendi, kuruyemişli
   tatlılar — 18 tarifin editoryal seçimi bu beş eğilimi doğrudan hedefliyor
   (bkz. tarif başlıkları: Muhammara, Nohut Falafel, Vegan Fındık Kreması,
   Ekşi Mayalı Tam Buğday Ekmeği, Cevizli Kurabiye/Köme).

**Tarif seti = arz büyüme planının aynası:** hangi crop'ta çiftçi kazanmak
istiyorsan o crop'un tarifini yaz ilkesi bu dağılımla uygulandı.

#### Yapılanlar
- **18 özgün tarif** (10 Katman 1, 7 Katman 2, 1 safran) — tamamen özgün
  metin, hiçbir siteden kazıma yok. `author_type='hasat'`,
  `visibility='public'`, `status='published'`, `owner_id=NULL`,
  `source_type='manual'`, ASCII slug.
- **`crop_culinary_meta` seed** — 14 odak crop için `culinary_aliases` +
  `conversion_hints` tam dolduruldu (domates/kekik M2'den genişletildi).
  Kalan 56 crop boş kaldı, M4/M9'a bırakıldı.
- **`recipe_steps.timer_seconds`** — bekleme/pişirme/dinlenme içeren her
  adımda dolu (98 adımın içinde 0 sn'den 259.200 sn'e/3 güne kadar aralık).
- **Görsel altyapı** — `crop-photos` isimlendirme konvansiyonu +
  `default_photo_url` güncelleme SQL'i hazır (bkz. `DB-Schema.md`), henüz
  uygulanmadı (görseller Berkin'in işi).
- **Doğrulama (kural #96)** — bkz. `Build/E2E-QA.md` → S21.

#### Berkin'e kalan işler
- 14 crop görseli + 18 tarif kapak fotoğrafı (liste: `DB-Schema.md`)
- Glossary insan gözden geçirmesi (P22-C'den bekleyen, hâlâ açık — `TODO.md`)
- ~~14 vs 13 sayı tutarsızlığının netleştirilmesi~~ — ✅ Berkin M4-a'da doğruladı (yukarıda)

### M3-D — Mobil UI Görsel Şartnamesi (yeni paralel iş kolu, Berkin onayladı)

**Amaç:** M5/M6'nın iterasyon maliyetini düşürmek. Mobilde Lovable yok, EAS
build dakikalar sürüyor, local inceleme mümkün değil — görsel hedef olmadan
girmek her düzeltmeyi pahalı yapıyor. **BE de-riske etme amacı değil** (o işi
M4 yapıyor).

**Zamanlama:** İlk 2–3 tarif hazır olduktan sonra başlaması şart koşulmuştu
("placeholder metinle pişirme modu tasarlamak işe yaramaz") — M3'ün 18 tarifi
tamamlandıktan sonra yapıldı, gerçek adım uzunlukları ve timer aralığı
(0 sn – 3 gün) şartnameyi doğrudan şekillendirdi.

**Kapsam — tam 5 ekran:** Pişirme modu · Offline durumu · AI import akışı ·
Alt navigasyon · "Talep Et". **Kapsam dışı:** Keşfet, ürün listesi,
siparişlerim, tarif listesi (webdeki muadillerini yakından takip ediyor,
port edilecek).

**Çıktı:** `Build/P23-Mobile-Visual-Spec.md` — görsel şartname, kod değil.
Bu dosyadan referanslanır, serbest dolaşan bir artifact değildir (üçüncü
doğruluk kaynağı riski).

### M4 — Web tarif yüzeyi *(huninin üst ağzı — mobilden ÖNCE)*

> **M4-a / M4-b'ye bölündü (2026-07-30).** Tek turda hem public yüzeyi hem
> Talep Et akışını hem admin heatmap'i açmak riskliydi (üç ayrı yeni yüzey,
> tek PR'da Berkin'in review yükü + Lovable çakışma riski birden büyür).
> Bölünme kural #107 kapsamında değil (görev metni zaten M4-a'yı ayrı
> tanımlıyordu) — burada sadece kayda geçiriliyor.

#### M4-a — Public tarif okuma yüzeyi + DB eki + ölçümleme — ✅ **TAMAMLANDI (2026-07-30)**
- `/tarifler` + `/tarifler/$slug` — **`/buyer/` dışında**, misafire açık, SSR ile SEO'lu (başlık/description/canonical/JSON-LD `Recipe`)
- Malzeme kartı 3 durumu: eşleşti → ürün sayfası + fiyat/min_order · platform crop ama eşleşmiyor → nötr "Hasat'ta henüz yok" (CTA yok) · platform-dışı (tuz/un) → nötr
- `recipe_views` yazımı canlıya alındı (`session_id` + `user_id`), `v_kpi_recipe_funnel_by_recipe` (per-recipe funnel) eklendi
- Detaylar: `Build/DB-Schema.md` → "P23-M4-a", `Build/E2E-QA.md` → S22

#### M4-b — Talep Et akışı + admin heatmap + Gap #9 — ✅ **TAMAMLANDI (2026-07-30)**
- Eşleşmeyen malzeme kartına **Talep Et** CTA'sı — baskın durum tasarımı (68 malzemenin 54'ü), yeni tablo yok (`crop_requests`+`recipe_rfq_links` zaten yetiyordu), guest niyeti `localStorage` + `/login`'in `next` param'ıyla korunuyor
- **"Bu ürün geldiğinde haber ver"** — mevcut `crop_request_match`/`dispatch_sms` deseni yeniden kullanıldı (yeni tablo/event yok), yeni bir `listings` trigger'ı bekleyen eşleşen talepleri bildiriyor
- Admin'de talep ısı haritası (`v_kpi_crop_demand_heatmap`, `/admin/kpi` → "Talep Isı Haritası" sekmesi)
- **BENCHMARK Gap #9 — "parselden tabağa QR görünümü"** kapandı: eşleşen malzemeden mevcut `/batch/$listingId` izlenebilirlik sayfasına link, yeni sistem kurulmadı
- M4-a'nın 3 bulgusu da bu turda düzeltildi: malzeme adlarının cümle-içi küçük harfi, 13/18 tarifte eksik `totalTime` (dinlenme/soğutma/mayalanma/ıslatma süresi), JSON-LD `image` alanının temsili görselle karışması
- Detay: `Build/DB-Schema.md` → "P23-M4-b", `Build/E2E-QA.md` → S23
- **Neden mobilden önce:** SEO 3–6 ayda birikir; içeriksiz ve kullanıcısız bir native app'in store'da değeri yok, 4.2 reddi de en çok o durumda gelir

#### M4-c — `cook_minutes` semantik düzeltmesi + SEO keşfedilebilirliği — ✅ **TAMAMLANDI (2026-07-30) — M4'ün kapanış turu**
- **Kural #107 ihlali bulunup düzeltildi:** M4-b'de bekleme süresi (rest) tutacak kolon olmadığı için sessizce `cook_minutes`'a eklenmişti — muhammara "45 dk pişirme" (gerçeği 15 dk), Cevizli Üzümlü Köme "72 saat pişirme" (gerçeği 20 dk) gösteriyordu. Berkin bulup bildirdi.
- Yeni kolon `recipes.rest_minutes` (nullable, ekleyici); 18 tarifin tamamı adım metninden yeniden sınıflandırıldı — `cook_minutes` en yükseği artık 60 dk (önceki 4.340 dk'dan)
- Frontend: `totalTime` = prep+cook+rest (türetilmiş, kolon değil); üç süre detay sayfasında her zaman ayrı gösteriliyor, tek sayıya toplanmıyor
- SEO: `sitemap.xml` dinamik (18 tarif + public vitrinler, elle güncelleme gerekmiyor); `robots.txt` zaten doğruydu; SSR'da (client-side değil) aynı temel malzemeyi paylaşan tariflere iç link
- Detay: `Build/DB-Schema.md` → "P23-M4-c", `Build/E2E-QA.md` → S24
- **M4 (a+b+c) tamamen kapandı.**

### M5-a — Mobil iskelet + paylaşılan çekirdeğin ikinci hedefi + tesisat — ✅ **TAMAMLANDI (2026-07-30)**
- Expo SDK 57 + Expo Router + Nativewind (marka renkleri `hasat-core/core/design/tokens.ts`'e bağlı) + `expo-build-properties` ile Android API 36
- `hasat-core` → `hasat-mobile` git subtree'si + `sync-to-web.yml`/`drift-check.yml` dual-target + drift'in sürüm-gerisi kör noktası kapandı (bkz. `Shared-Architecture.md`)
- Supabase client (storage adapter parametreli, mobil: `expo-secure-store` tabanlı `LargeSecureStore`) + telefon OTP girişi (mevcut akışın aynısı) + TanStack Query
- Detay + doğrulama tablosu: `TODO.md` → "P23-M5-a" build log

### M5-a-ek — Test altyapısı, bayat tipler, `.env` bekçisi — ✅ **TAMAMLANDI (2026-07-31)**

M5-b'ye (ekran yazma) geçmeden önceki ön koşul turu — M5-a merge edilip
doğrulandıktan sonra bulunan/karara bağlanan dört madde.

#### Test stratejisi kararı — neden gerçek cihaz/Expo Go değil

**Durum (2026-07-31):** Apple Developer bireysel hesabına başvuruldu ($99,
şirketten bağımsız — bkz. `Store-Compliance.md`). Onay 7-10 gün sürmesi
bekleniyor. Onay gelene kadar:

- **Gerçek iPhone'a kurulum mümkün değil.** Bir Expo/React Native app'i
  gerçek bir iOS cihazına kurmak (Expo Go'nun desteklemediği native modüller
  — `expo-secure-store`, `expo-build-properties` vb. kullanıldığı için dev
  client gerekiyor) ya ücretli Apple Developer hesabıyla EAS üzerinden
  provisioning ister ya da **yerel Xcode + manuel imzalama** gerektirir.
  İkinci yol da kapalı: Berkin'in kullanabildiği tek Mac şirket bilgisayarı,
  bu proje için o makinede Xcode/imzalama süreci yönetilmiyor (aynı kısıt
  `Build/Roadmap.md`'de Capacitor'ı eleyen kısıtın aynısı).
- **Elde Android cihaz yok** — Android tarafında da gerçek cihaz testi
  şimdilik mümkün değil.

**Karar (Berkin, 2026-07-30/31):** Onay gelene kadar mobil doğrulama **iOS
Simulator build (EAS, `eas.json`'daki yeni `simulator` profili) + tarayıcı
tabanlı bulut simülatörü (Appetize.io)** ile yapılacak. Bu yol hiçbir Apple
Developer hesabı ya da yerel Mac/Xcode gerektirmiyor — build bulutta
derleniyor, çalıştırma tarayıcıda. Detaylı adım adım QA prosedürü:
`Build/E2E-QA.md` → S25 bölüm B. Simülatörde doğrulanamayan dört şey (push,
gerçek uçak modu, Keychain/SecureStore'un cihazdaki gerçek davranışı,
performans) `TODO.md` → "Apple hesabı gelince koşulacak testler" altında
birikiyor.

#### Bayat tipler bulgusu — kural #111

`hasat-core/core/db/types.ts`'te `recipes.rest_minutes` eksikti — M4-c'de
kolon eklenmiş ama tip üretimi hiç yenilenmemişti. Bayat tipler subtree ile
hem web'e hem mobile inmiş, drift check yeşil kalmıştı çünkü üç kopya
(hasat-core, web, mobil) tutarlı biçimde yanlıştı — drift kontrolü core↔hedef
tutarlılığını denetliyor, DB↔core tutarlılığını denetlemiyordu. Bu turda
canlı şemadan (Supabase MCP ile doğrudan) yeniden üretildi, `hasat-core`'a
commit edildi (dual-target sync PR'ları açacak). Kalıcı çözüm: `hasat-core`
CI'ına `types-freshness.yml` eklendi — `supabase gen types` çıktısını
commit'lenmiş dosyayla günlük karşılaştırıp farklıysa fail ediyor. Detay:
`TODO.md` → kural #111, `hasat-core/README.md` → "Tip tazeliği".

#### `.env` içerik bekçisi

`hasat-mobile/.env` public repoda **bilinçli olarak** takip ediliyor —
içindeki iki değer (Supabase URL + anon publishable key) tasarım gereği
public, silinmesi/gitignore'lanması her klonda uygulamayı çalışmaz hale
getirirdi, hiçbir güvenlik kazancı olmadan. Bunun yerine `hasat-core`'un
drift Action'ına bir içerik denetimi eklendi: her satır `EXPO_PUBLIC_` ile
başlamalı, ayrıca `service_role`/`SECRET`/`PRIVATE`/`TOKEN`/`PASSWORD`
kalıpları geçen bir isim (prefix doğru olsa bile) reddediliyor. Gerekçe:
`Shared-Architecture.md`. Kasten bozulup exit 1 verdiği, sonra geri alındığı
doğrulandı — detay ve bulunan bir sınır (isim-kalıbı denetiminin
tam garanti olmadığı): `Build/E2E-QA.md` → S25.

#### AES anahtarı — doğrulandı, değiştirilmedi

`LargeSecureStore` (`hasat-mobile/src/lib/supabase/large-secure-store.ts`)
şifreleme anahtarı **`expo-secure-store`'da** tutuluyor (`SecureStore.setItemAsync`/`getItemAsync`,
satır 20/25) — Supabase'in resmi Expo deseniyle birebir. Anahtar ne kodda
gömülü ne deterministik türetiliyor (`crypto.getRandomValues` ile her
`setItem`'da yeniden üretiliyor, aynı anahtarla eşleşen şifreli veri
`AsyncStorage`'a yazılıyor). Yani şifreleme dekoratif değil — oturum token'ı
gerçekten AES ile şifreli, anahtarın kendisi Keychain/Keystore'da. Bulgu
raporlandı, kod değiştirilmedi (görev talebi buydu).

### M5-a-ek-2 — Tarayıcıdan Tetiklenebilir EAS Simulator Build Workflow'u — ✅ **TAMAMLANDI (2026-08-03, Claude Code)**

**Sorun:** M5-a-ek'te yazılan EAS talimatları (`TODO.md` → "P23-M5-a-ek")
yerel terminal varsayıyordu (`eas login`, `eas init`, `eas build`). Berkin
şirket Mac'inde bu araç zincirini yönetemiyor, ve talimatı yazan oturumun
ağ politikası `expo.dev`'e erişemiyordu — yani simulator build'ini
tetikleyecek hiçbir yol kalmamıştı, ne Berkin'in makinesinden ne bir Claude
Code oturumundan.

**Bootstrap bulgusu (yeniden kullanılabilir — yerel araç zinciri olmayan bir
kurucu için):** Expo'nun resmî dokümantasyonu hem GitHub App hem CI (Actions)
yolu için "önce bilgisayarından başarılı bir build çalıştır" öneriyor. Ama bu
önerinin sağladığı üç şeyin **hiçbiri terminale bağlı değil**:
- `eas.json`'daki `simulator` build profili zaten dosya olarak vardı (M5-a-ek).
- `projectId`, `eas init`'i çalıştırmadan da Expo panosundan (tarayıcı)
  alınabiliyor — Berkin panoda proje oluşturup ID'yi kopyaladı.
- `bundleIdentifier`/`android.package` `app.json`'a elle yazılabiliyor,
  `eas build` bunu ilk çalıştırmada otomatik keşfetmek zorunda değil.

Yani "önce yerelden çalıştır" bir zorunluluk değil, CI'a geçmeden önce bir
sağlık kontrolü öneriymiş — **non-interactive bir CI build'i doğrudan
kurulabiliyor.** Sonuç: `TODO.md`'deki talimat terminal varsayımı
kaldırılarak tarayıcı akışına çevrildi.

**`app.json` değişiklikleri:**
- `expo.extra.eas.projectId` = `bff1a47c-41d5-42fa-bddc-83320c079253` eklendi.
- `expo.ios.bundleIdentifier` ve `expo.android.package`: `com.hasat.mobile` →
  **`com.hasat.app`** (aynı değer her iki platformda — Android tarafı ileride
  Play submission'ı için hazır bekliyor).

**Bundle identifier kararı — `com.hasat.app` (Berkin onayı):** Hem iOS
`bundleIdentifier` hem Android `package` için aynı değer seçildi. **Kritik
kısıt: bu değer yayınlandıktan (App Store/Play submit) sonra değiştirilemez**
— yeni bir bundle ID teknik olarak **yeni bir uygulama** demektir; mevcut
indirme sayısı, kullanıcı yorumları ve (iOS tarafında) TestFlight geçmişi
yeni ID'ye taşınmaz. Bu yüzden değer submit'ten önce, şimdi sabitlendi.

**`expo.slug` — doğrulanamadı, değiştirilmedi:** `app.json`'daki
`slug: "hasat-mobile"` alanının Expo panosundaki proje
(`bff1a47c-41d5-42fa-bddc-83320c079253`) ile aynı slug'ı taşıyıp taşımadığı
bu oturumdan kontrol edilemedi — ağ politikası `expo.dev`'e erişimi
engelliyor. Slug'a dokunulmadı (görev talimatı: uyuşmazsa dur ve bildir,
kendin değiştirme). **Berkin'in workflow'u ilk çalıştırmadan önce panoda bu
projenin slug'ının gerçekten `hasat-mobile` olduğunu doğrulaması gerekiyor**
— uyuşmazsa build ya yanlış projeye gider ya da EAS hata verir.

**Workflow — `.github/workflows/eas-build-simulator.yml`:** Yalnızca
`workflow_dispatch` (otomatik tetikleyici yok — kota koruması, aşağıda).
Resmî `expo/expo-github-action@v8` (`eas-version: latest`,
`token: secrets.EXPO_TOKEN`, `packager: npm` — repoda `package-lock.json`
var, yarn değil), `npm ci` ile kurulum, ardından
`eas build --profile simulator --platform ios --non-interactive`. Build
bitince artifact linki `$GITHUB_STEP_SUMMARY`'ye yazılıyor (`--json`
çıktısından çıkarılıyor; çıkarılamazsa Expo panosunun builds sayfasına
yönlendiren bir not düşülüyor) — Berkin panoda aramadan doğrudan
Appetize.io'ya (https://appetize.io/upload) yükleyebilsin diye.

**Gereken secret:** `EXPO_TOKEN` — Expo panosundan (Account Settings →
Access Tokens) oluşturulup `hasat-mobile` reposunda Settings → Secrets and
variables → Actions → `EXPO_TOKEN` adıyla eklenmesi gerekiyor (Berkin'in
tek manuel adımı).

**Kota koruması:** Expo'nun ücretsiz katmanı ayda 30 build'e izin veriyor
(iOS için bunların en fazla 15'i), kuyruk 90 dakikayı aşabiliyor, ve her
**başarısız** deneme de kotadan düşüyor. Bu yüzden otomatik tetikleyici
eklenmedi ve `app.json`'ın eksiksiz (dört alan da dolu) olması şart koşuldu
— eksik bir `app.json` ile tetiklenen bir build muhtemelen başarısız olur
ve kotayı boşa harcar. Detay hem workflow yorumunda hem `TODO.md`'de.

**Doğrulama (kural #96):**
| Kontrol | Sonuç |
|---|---|
| `.github/workflows/eas-build-simulator.yml` YAML syntax | ✅ PyYAML ile parse edildi, geçerli |
| Workflow'da yalnızca `workflow_dispatch` tetikleyicisi | ✅ Doğrulandı, başka tetikleyici yok |
| `secrets.EXPO_TOKEN` referansı | ✅ Doğru sözdizimiyle mevcut (`token: ${{ secrets.EXPO_TOKEN }}`) |
| `app.json` geçerli JSON | ✅ `json.load` ile parse edildi |
| `app.json` dört alan (projectId/ios.bundleIdentifier/android.package/slug mevcudiyeti) | ✅ İlk üçü doğru değerle dolu; `slug` mevcut ama Expo panosuyla eşleştiği doğrulanamadı (yukarıda) |
| Gerçek `eas build` çalıştırması | 🔴 **Doğrulanamadı** — bu oturumun ağ politikası `expo.dev`'e erişimi engelliyor (kural #103). Berkin'in GitHub Actions'tan "Run workflow" ile tetiklemesi gerekiyor. |

**Kapsam kuralı tutuldu:** `src/lib/core/` dokunulmadı (kural #105), web
reposuna (`hasat-d2c-marketplace`) dokunulmadı, Supabase şemasına
dokunulmadı, `unit_type` enum'una dokunulmadı. Şema/mimari kararı yok — bu
tamamen CI/build-tetikleme altyapısı; tek karar niteliğindeki değişiklik
(bundle identifier) zaten Berkin onaylıydı (görev talimatı), otonom
alınmadı.

### M5-b — Ekran yazma — 🟡 **UYGULANDI (2026-08-03, Claude Code doğrudan), simülatör/cihaz QA bekliyor**

Kapsam bölündü: **tarif listesi/detayı + offline önbellek bu turda**;
**pişirme modu + AI import M6'ya kaldı** (görev metninin kendi kapsamı da
böyleydi — "'Talep Et' bu turda YOK, M6 veya sonrası"). Play hesap tipi
kararı henüz verilmedi, açık madde olarak kalıyor.

**Tarif ekranları** (`app/home.tsx` liste, `app/recipe/[slug].tsx` detay):
`recipes`(public+published) + web'deki kapak-fallback zincirinin
(kapak → ana malzemenin crop görseli → nötr placeholder) birebir portu;
`prep_minutes`/`cook_minutes`/`rest_minutes` her zaman ayrı gösteriliyor
(P23-M4-c kararı korunuyor), `rest_minutes > 120` dk → "Önceden başlamak
gerekir" rozeti. `rpc_recipe_availability`/`rpc_recipe_shopping_list`
doğrudan çağrılıyor (kural #106 — eşleştirme/dönüşüm client'ta yeniden
yazılmadı). Malzeme kartının 3 durumu (eşleşti/platform crop ama
eşleşmiyor/platform-dışı) web'in birebir portu; eşleşen durumda web'in
"Ürüne Git" linki mobilde **yok** (hedef ekran — Keşfet — M7'ye kadar
yok, kırık bağlantı bırakılmadı), eşleşmeyen durumda "Talep Et" **yok**
(bu turun kapsamı değil). `recipe_views` yazımı taşındı;
`session_id` mobilde `AsyncStorage`'da kalıcı bir UUIDv4 (web'in
`localStorage` + `crypto.randomUUID()`'ının karşılığı, mevcut
`react-native-get-random-values` polyfill'i üzerinden, yeni bağımlılık
yok). Detay: `TODO.md` → "P23-M5-b" build log.

**Offline önbellek (Apple 4.2'nin çekirdeği) — `expo-sqlite`:**
Yalnızca editoryal/durağan veri önbelleklendi (tarif metni, adımlar,
malzeme listesi) — `rpc_recipe_availability`/`rpc_recipe_shopping_list`
(fiyat/stok/min. sipariş) **hiçbir zaman** sqlite'a yazılmıyor ve
cihaz offline'ken bu RPC'ler hiç çağrılmıyor. Karar: bayat fiyat
göstermek yerine hiç göstermemek — "son güncelleme: X" damgası yerine
"çevrimdışı, fiyat/stok bilgisi yok" nötr metni (görev metninin sunduğu
iki seçenekten biri; gerekçe ve alternatif `TODO.md`'de). Tazeleme:
cache-aside (ağ başarılıysa göster + yaz, başarısızsa/offline'sa oku).
Görsel şartnamenin (`P23-Mobile-Visual-Spec.md` → "2. Offline Durumu")
Durum A (önbellek var + offline → üst şerit) ve Durum B (önbellek yok +
offline → tam ekran "Bağlantı yok") birebir uygulandı.

**⚠️ Doğrulanamadı (kural #103):** `expo-sqlite` native bir modül —
bu oturumda simülatör/cihaz erişimi yok, yalnızca `tsc --noEmit` (temiz)
doğrulandı. Gerçek uçak modu testi zaten `TODO.md` →
"Apple hesabı gelince koşulacak testler" altında device-only işaretliydi;
sqlite doğrulaması da oraya eklendi. Detay + tam doğrulama tablosu:
`TODO.md` → "P23-M5-b", `Build/E2E-QA.md` → S26.

**Kural #107 gereği kararı Berkin'e bırakılan iki madde (uygulanmadı):**
mobil test giriş yolu (`123456` OTP web'de çalışıyor, mobilde gerçek
Supabase Auth'a çarpıyor) ve çiftçi rolüyle mobil girişin nasıl
ele alınacağı — üç seçenek + etki analizi `TODO.md`'de.

- **Çıkış:** Uçak modunda app açılıyor ve tarifler görünüyor — *Apple
  4.2'nin asıl testi* — kod tarafı hazır, **gerçek cihaz doğrulaması
  Berkin'e kalıyor.**

### M6 — Native yetenekler
- **Pişirme modu:** adım adım, timer'lar, ekranı uyanık tutma
- **AI import:** metin + yazılı tarif fotoğrafı → private tarif
- **Push:** Android FCM (bağımsız) · iOS APNs (Apple hesabına bağlı, ~1 saatlik iş)
- **Çıkış:** Gerçek cihazda doğrulandı

### M7 — Mobilde marketplace köprüsü + store varlıkları
- Keşfet, ürün detayı, Talep Et, Siparişlerim (**checkout yok**)
- **Store varlıkları burada hazırlanır** (M8'den öne çekildi — hiçbiri hesap gerektirmiyor): gizlilik metni, **uygulama içi hesap silme** (Apple zorunlu), ekran görüntüleri, review notları
- **Çıkış:** Hesap geldiğinde submit tek günlük iş olacak durumda

### M8 — Store submit
- API 36 doğrulaması, iOS submit + review, Play production
- **Çıkış:** iOS + Android canlı

### M9 — Sıraya alındı (silinmedi)
YouTube/link import (hukuki kontrol şartıyla) · yemek fotoğrafından tahmin · HoReCa porsiyon maliyeti hesaplayıcı · abonelik köprüsü (`harvest_subscriptions` × tarif) · bildirim event map konsolidasyonu · organizasyon hesabına geçiş

---

## Şirket gecikirse ne olur

| İş | Durum |
|---|---|
| M1, M2, M3, M4 | ✅ Etkilenmiyor |
| M5, M6 (pişirme modu, AI import, offline) | ✅ Etkilenmiyor |
| M7 (köprü + store varlıkları) | ✅ Etkilenmiyor |
| Android push | ✅ Etkilenmiyor |
| iOS push | ⚠️ Apple hesabına bağlı — **hesap şirketten bağımsız açıldığı için etkilenmiyor** |
| Store submit | ⚠️ Hesaba bağlı — **etkilenmiyor**, sadece satıcı adı kişisel görünür |
| Gerçek ödeme (P17-A/iyzico) | 🔴 Şirkete bağlı — **mobil v1'de checkout olmadığı için uygulamayı bloke etmiyor**, web'de bekler |
| Fatura/e-müstahsil (P17-D) | 🔴 Şirkete bağlı, P23 dışı |

**Sonuç: şirket gecikirse P23'te hiçbir iş durmaz.** Geriye yalnızca web tarafındaki gerçek ödeme ve fatura kalır.

---

## Eski P23 kodlarının eşlemesi

`TODO.md`'deki önceki üç satırlık tanım bu dokümanla değiştirilmiştir.

| Eski kod | Yeni karşılığı | Not |
|---|---|---|
| P23-A — "React Native, mobile-first" | M1 + M5 | **Expo'ya revize edildi** (Mac kısıtı) |
| P23-B — Recipe↔Crop + RFQ otomasyonu | M2 + M4 | Eşleştirme RPC'ye taşındı |
| P23-C — Mobile compliance | M7 + M8 + `Store-Compliance.md` | — |
| *(yoktu)* | M0, M3, M6, M9 | Yeni |

---

## Faz kapanış ritüeli

1. Uygulama biter → **bağımsız doğrulama** (gerçek SQL / `get_diff` / gerçek cihaz — Lovable'ın metnine güvenilmez, kural #96)
2. **Kural #104'e uygun**, kullanıcı-akışı dilinde adım adım QA test case'i hazırlanır → Berkin uygular
3. Claude Code ilgili dokümanlara PR açar → Berkin merge eder
4. "commit ettim" → sonraki taş açılır

**Önceki taşın dokümanı merge edilmeden sonraki taş başlamaz.**
