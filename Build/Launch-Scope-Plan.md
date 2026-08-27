---
title: Hasat — Birleşik Lansman Kapsam Planı (Web + Mobil)
created: 2026-08-18
tags:
  - hasat
  - launch
  - p23
  - planning
status: ONAYLANDI (2026-08-18) — açık kararlar netleşti, implementasyon başladı
---

# Birleşik Lansman Kapsam Planı

> **2026-08-27 UI pointer'ı:** Group 7 / PR #66 `MERGED — APPROVED WITH FOLLOW-UP — ACCEPTED` (merge `1282affa057e1bc0cbd65084404d5bd99674bbc1`). Sıradaki iş Group 9 — Native Buyer App Core Experience, `READY FOR CODEX DISPATCH — NOT STARTED`; implementer Codex, bağımsız reviewer ChatGPT. Group 10 `NOT STARTED`.

> **Karar (Berkin, 2026-08-18):** Web ve mobil ayrı takvimlerde değil,
> **birlikte** lansmanır. Hedef: **App Store + Play submit, 1-15 Eylül
> 2026 penceresi.** Bu doküman, Berkin'in ilettiği 17 maddelik listeyi +
> bu planın kendi önerdiği ekleri, ürün vizyonu ve mevcut mimariye karşı
> analiz ediyor, önceliklendiriyor, kapsamlarını netleştiriyor ve bağımlı
> alanları haritalıyor. **Plan 2026-08-18'de onaylandı ve implementasyon
> başladı.** Güncel UI yürütme kapsamı ve statüsü Drive `04.10` / `04.11`
> kayıtlarından takip edilir; bu belge lansman kapsamı ve tarihsel karar
> gerekçelerini korur.

---

## 0. Gerçekçilik kontrolü — önce bunu okuyun

Bugün **18 Ağustos**. Hedef pencere **1-15 Eylül** — yani **2-4 hafta**.
Bu süre içine giren şey yalnızca Berkin'in listesindeki 17 madde değil,
**zaten var olan mobil borç** da var:

- M7-b (Keşfet + store varlıkları: gizlilik metni, ekran görüntüleri,
  review notları) hâlâ açık.
- M8-a'nın kod tarafı bitti ama **APNs anahtarının EAS'a yüklendiğine dair
  hiçbir kayıt yok**, Android FCM/Firebase kurulumu **hiç başlamadı**
  (`google-services.json` hâlâ repoda yok — az önce tekrar kontrol
  ettim).
- M8-b (gerçek cihaz doğrulama) ve M8-c (push doğrulama) hiç
  başlamadı — bunlar Berkin'in kendi zamanını gerektiriyor, benim
  kod yazmamla hızlanmıyor.
- Şema değişikliği gerektiren her madde kural #115'in dört adımlı
  sırasını (migration → `hasat-core` tip üretimi + PR → sync PR'ları
  **her iki hedefte de merge edilir** → client kod) zorunlu kılıyor —
  her adım Berkin'in PR merge etmesini bekliyor, bu round-trip'ler
  paralelleştirilemiyor.
- Apple review turnaround genelde 24-48 saat ama **red gelirse tam bir
  tur daha** kaybediliyor — bu payı takvime koymadan "1 Eylül'de submit"
  demek riskli.

**Önerim:** Listenin tamamı kesinlikle yapılacak (kapsam kesilmiyor,
kuralımız bu) — ama **hepsinin submit'ten ÖNCE app'in içinde olması
şart değil.** App Store/Play'e bir kere onaylanıp canlıya çıktıktan
sonra, güncelleme (v1.1, v1.2...) çok daha hızlı onaylanıyor (genelde
saatler-1 gün). Bu yüzden aşağıda her maddeyi **"v1.0 — submit'ten önce
şart"** ve **"v1.1 — submit sonrası hızlı takip"** olarak ikiye
ayırıyorum. Bu bir kapsam kesme değil, **sıralama** — hiçbir madde
listeden düşmüyor, yalnızca hangisinin submit'i geciktirmeye değer
olduğuna karar veriyoruz. Bölüm 9'da bunu tek tabloda topluyorum,
onayına sunuyorum.

**Bu planı yazarken gerçek veriye baktım (varsayım değil):** Supabase'e
doğrudan sorgu attım — aşağıdaki bulgular ilk defa burada ortaya çıkıyor
ve önceliklendirmeyi doğrudan etkiliyor:

- `crop_config.default_photo_url` **zaten 27/70 crop için dolu**
  (`Launch-Plan.md`'nin "0/70" notu artık bayat — muhtemelen bu ay
  içinde biri bu SQL'i çalıştırmış). Görsel bucket'ında (`crop-photos`)
  toplam 41 dosya var: 27 crop referans görseli (`{crop}.webp`) + 8
  tarifin görseli (bazılarında yalnızca bir en-boy oranı var, ikisi de
  değil).
- `recipes.cover_photo_url` ise **18 public tarifin 18'inde de NULL** —
  yani görseller bucket'ta duruyor ama tariflere hiç bağlanmamış. Tam
  olarak Berkin'in 1. maddesi.
- `recipes.diet_tags` **zaten bir kolon olarak var VE 18 tarifin
  çoğunda dolu** (`vegan`/`vejetaryen`/`glutensiz` karışımı) — filtre
  maddesinin (13) yarısı sandığımdan çok daha az iş.
- `notifications` tablosu (id, user_id, type, title, body, related_id,
  read_at, created_at) **zaten var** ve 6 farklı trigger fonksiyonu
  (`notify_offer_received`, `notify_offer_accepted`,
  `notify_order_status`, `notify_subscription_changes`,
  `notify_crop_request_fulfilled`, `send_subscription_harvest_reminders`)
  buraya yazıyor — web'deki zil ikonu muhtemelen bu tabloyu okuyor.
  Mobil bildirim merkezi (madde 10) **aynı tabloyu okursa web+mobil
  senkron otomatik gelir**, ayrı bir senkron mekanizması kurmaya gerek
  yok.
- `dispatch_push`/`dispatch_sms`'in event haritası tam olarak şu 13
  event: `new_offer`, `price_alert`, `harvest_time`, `offer_accepted`,
  `offer_countered` (yalnızca push, SMS karşılığı YOK), `payment_confirmed`,
  `order_shipped`, `order_delivered`, `order_cancelled`, `dispute_opened`,
  `crop_request_match`, `subscription_new`, `subscription_accepted`,
  `subscription_rejected`. **Bir tutarsızlık buldum:** `notif_prefs`
  tablosunda `community_push` diye bir kolon var ama `dispatch_push`'ın
  CASE'inde hiçbir `community` event'i yok — yani bu tercih anahtarı
  UI'da gösteriliyorsa bile hiçbir zaman tetiklenmiyor, ölü bir toggle.
  Madde 3'e otomatik giriyor.
- Sipariş akışı **iki ayrı state machine**: `offers.status` (`pending →
  accepted/rejected/counter/pending_farmer/pending_buyer → completed`)
  müzakere fazı, `orders.status` (`preparing → shipped → delivered →
  disputed/completed/cancelled`) teslimat fazı — `orders` teklif kabul
  edilince oluşuyor. Madde 14'ün QA kapsamı ikisini de kapsamalı, tek
  state machine sanıp yarısını atlamak kolay bir hata.

---

## 1. Vizyon hizalaması (kısa hatırlatma)

Tarif katmanının var oluş amacı kendi geliri değil, **tarif → kayıt →
talep → teklif → sipariş** dönüşüm huninisi (`v_kpi_recipe_funnel`).
Aşağıdaki her madde bu hunideki bir noktayı güçlendiriyor mu diye
süzülmeli — güçlendirmeyen bir madde varsa (yok, hepsi güçlendiriyor,
ama kontrol ettim) önceliği düşer. Ayrıca üç sabit ilke geçerli
kalıyor: **mobil v1'de checkout yok** (ödeme web'de), **güven/menşe
tezi** (temsili görsel etiketi, AI içerik açıkça işaretlenir), **kapsam
kesilmez tarih ötelenir.**

---

## 2. Madde madde analiz

Her madde: ne demek, vizyona nasıl bağlanıyor, kapsam (içeride/dışarıda),
bağımlılıklar, web/mobil ayrımı, roller, v1.0 mu v1.1 mi.

**Rol kısaltmaları:** 🤖 Claude Code (kod, GitHub PR) · 🎨 Lovable (web
kod, Lovable ajanı) · 🎯 Orkestratör (bu oturum — plan/denetim/Supabase
MCP/QA senaryosu) · 🧑‍🍳 ChatGPT+Gemini (tarif üretim operasyonu,
Berkin'in hesabı) · 👤 Berkin (manuel/karar/gerçek cihaz test).

### F1 — Bucket'taki görselleri mevcut tariflere/crop'lara bağla — ✅ UYGULANDI (2026-08-18)

**Durum netleşti (yukarıda):** 27 crop zaten bağlı. 8 tarifin görseli
bucket'ta ama **hiçbiri `recipes.cover_photo_url`'e bağlı değil**. 6
tarifte her iki oran (16x9+1x1) tam, 2 tarifte (`firinda-patlican-musakka`
yalnızca 16x9, `safranli-zerde` yalnızca 1x1) eksik.

**Uygulandı (2026-08-18, Supabase MCP ile doğrudan UPDATE — migration
değil, basit veri güncellemesi):** 8 tarifin `cover_photo_url`'ü
bucket'taki gerçek dosya URL'lerine bağlandı (16x9 varsa öncelik ona,
yoksa 1x1'e düşüldü): `cevizli-biber-ezmesi-muhammara`,
`cevizli-kurabiye`, `eksi-mayali-tam-bugday-ekmegi`,
`firinda-patlican-musakka` (yalnızca 16x9 var), `cevizli-elmali-salata`,
`cevizli-uzumlu-kome`, `ev-yapimi-zeytinyagli-domates-sosu`,
`safranli-zerde` (yalnızca 1x1 var). `returning` ile 8/8 satırın
güncellendiği doğrulandı.

**Berkin'i bekleyen (görsel üretimi, F2'nin otomasyonu ya da elle):**
10 tarif hâlâ görselsiz — `incir-receli`, `kekikli-zeytinyagi-ezmesi`,
`koz-biber-patlican-ezmesi`, `mercimek-corbasi`, `nohut-falafel`,
`taze-uzum-cevizli-yesil-salata`, `vegan-findik-kremasi`,
`zeytinyagli-bugday-salatasi`, `zeytinyagli-mercimek-koftesi`,
`zeytinyagli-nohut-yemegi`. Ayrıca 70 crop'un 43'ü hâlâ
`default_photo_url`'süz. `firinda-patlican-musakka` (1x1 eksik) ve
`safranli-zerde` (16x9 eksik) için eksik oranın tamamlanması ayrıca
faydalı olur ama bloke değil.

**Kapsam:** (a) UPDATE SQL ile mevcut 8 tarifi bağla — hero için 16x9'u
`cover_photo_url`'e yaz (eksik olan 2 tarifte var olanı kullan, tek oran
eksik diye bloke etme). (b) Kalan 10 tarif + 43 crop için Berkin'in
görsel üretmesi bekleniyor — bu onun işi, ben yalnızca "eksik liste"yi
netleştiriyorum (bkz. F2). (c) Web + mobil'in kapak-fallback zinciri
(`kapak → ana malzemenin crop görseli → nötr placeholder`) zaten kodda
var, değişiklik gerekmiyor — bağlama tek başına iki yüzeyde de görünür
olacak.

**Bağımlılık:** F2 (otomasyon + kalan görseller) ile aynı bucket/isim
konvansiyonunu paylaşıyor — aynı turda ele alınmalı ki ileride tekrar
iş çıkmasın.

**Roller:** 🎯 (SQL hazırlama + doğrulama) · 🤖 (varsa client-taraflı
fallback kodu, muhtemelen gerekmiyor).

**v1.0 — submit'ten önce şart.** Ucuz, yüksek etkili (SEO + güven +
görsel bütünlük), bugün başlayabilir.

---

### F2 — Otomasyon altyapısı (RecipeAutomation.md'ye göre) hazır olmalı

Bu, önceki turda audit edip Berkin'e ilettiğim planın gerçek
implementasyonu: `recipe_generation_batches/jobs/drafts/qa_results/assets`
tabloları + zincirlenmiş Edge Function aşamaları + admin paylaşılan-anahtar
auth + Gemini görsel üretimi + `hasat-webp.sh` eşdeğeri sunucu-taraflı
pipeline.

**Netleştirilmesi gereken kapsam:** "Altyapımız hazır olmalı" iki farklı
şey olabilir — (a) şema + endpoint'ler var, ChatGPT/Gemini bunları
çağırabiliyor (altyapı hazır, operasyon henüz haftalık ritme girmemiş),
(b) haftada 10 tarif üreten pipeline **fiilen çalışıyor** lansmana kadar.
**Önerim: (a).** (b) submit'i gereksiz riske atar — bu bir üretim hattı,
uygulamanın kendisinin bir parçası değil, Apple/Google review'ında
görünmüyor.

**Bağımlılık:** F1 ile aynı bucket/isim konvansiyonu. Şema tarafı F12
(besin değerleri) ve F13'ün (filtre/etiketleme) AI-destekli
etiketleme ihtiyacıyla **aynı pipeline'ı paylaşabilir** — üçünü ayrı ayrı
kurmak yerine, tek "tarif zenginleştirme" Edge Function zincirinin farklı
aşamaları (görsel · besin değeri · diyet/ekipman/alerjen etiketleme)
olarak tasarlamayı öneriyorum. Bu, F2/F12/F13'ü birbirinden bağımsız üç
proje yerine tek bir altyapının üç kullanımına indiriyor.

**`hasat-webp.sh`'nin sunucu-taraflı eşdeğeri nerede yaşasın — karar
(Berkin sordu, önerim aşağıda):** Bir Supabase Edge Function
(`process-recipe-image` gibi) olarak — Berkin'in yerel makinesinde
kalan bir script değil. Gerekçe: bu pipeline'ı üç taraf çağırabilmeli
— (a) ben/Claude Code (manuel/toplu iş için), (b) ChatGPT'nin
orkestratör agent'ı (haftalık otomasyon sırasında), (c) dolaylı olarak
Gemini'nin ürettiği görsel — Gemini'nin kendisi Hasat'a hiç
bağlanmıyor, görseli üreten ChatGPT tarafı ham görseli bu Edge
Function'a admin-key ile POST ediyor, Function 14% chop + merkezi
kırpma (16:9/1:1) + WebP q=82 + dosya adı normalizasyonu + `crop-photos`
bucket'ına yükleme + manifest güncellemesini yapıyor. Yerel bir script
olsaydı yalnızca Berkin'in makinesi açıkken çalışırdı — otomasyonun
"haftalık, insansız" olma amacını baştan bozardı. Deno'da ImageMagick
yerine saf JS/WASM bir kırpma/kodlama kütüphanesi (ör. `imagescript`
npm paketi, Deno'nun `npm:` özelliğiyle) kullanılabilir — ImageMagick
native binary'si Edge Function ortamında yok.

**Roller:** 🎯 (mimari + admin auth + `hasat-vault`'a doküman) · 🤖
(migration + Edge Function iskeleti + `process-recipe-image` Edge
Function) · 🧑‍🍳 (ChatGPT: Planner/Writer/QA agent'ları, admin-key ile
Hasat endpoint'lerini çağırıyor; Gemini: görsel üretimi, Hasat'a hiç
doğrudan bağlanmıyor — ChatGPT aracılığıyla).

**v1.0: yalnızca şema + admin endpoint iskeleti (altyapı var).**
**v1.1: haftalık üretim ritmi + F12/F13'ün AI-destekli etiketleme
aşamaları.** Submit'i bloklamamalı.

---

### F3 — Push/SMS/WhatsApp Event Haritası (canlı DB'den çıkarıldı, 2026-08-18)

> Kaynak: `dispatch_push`/`dispatch_sms` fonksiyon tanımları + 6 trigger fonksiyonu +
> `cron.job` + `notif_prefs` şeması, tümü Supabase MCP ile doğrudan sorgulandı
> (varsayım değil, gerçek `pg_get_functiondef`/`information_schema` çıktısı).
> Bu turda üç yeni bulgu ortaya çıktı, Berkin'in kararlarıyla çözüldü (bkz. altta).

#### Berkin'in kararları (2026-08-18, ikinci tur)

1. **`price_alert` kaldırılıyor.** Hiçbir trigger/cron/edge function onu tetiklemiyor
   (org genelinde kod araması sıfır sonuç) — `community_push` ile birebir aynı "ölü
   toggle" deseni. `notif_prefs.price_alert_push/sms/whatsapp` düşürülüyor,
   `dispatch_push`/`dispatch_sms`'in CASE'inden çıkarılıyor.
2. **WhatsApp toggle'ı yalnızca gerçekten kolonu olan event'lerde gösterilecek.**
   `price_alert` kaldırıldığı için bu artık yalnızca **2 event** kalıyor: `new_offer`,
   `harvest_time`. ⚠️ **Önemli:** hiçbir yerde bir `dispatch_whatsapp` fonksiyonu yok —
   toggle açık olsa bile şu an bu iki event'te de WhatsApp gitmiyor. UI'da bu toggle'ın
   "yakında" / işlevsiz olduğu görsel olarak belli edilmeli (öneri: devre dışı görünüm +
   kısa not), aksi halde kullanıcı açıp hiç mesaj almayınca yanılgıya düşer.
3. **Üç sessiz geçiş kapatılıyor** — `offer_rejected`, `order_preparing`,
   `order_completed` artık push+SMS gönderiyor (yeni event'ler, aşağıda migration'da).

#### Nihai event tablosu (16 event, price_alert çıkarıldı)

| Event key | Ne zaman | Kime | In-app | Push | SMS | WhatsApp | Kaynak |
|---|---|---|---|---|---|---|---|
| `new_offer` | Buyer teklif oluşturur (INSERT `offers`) | Çiftçi | ✅ | ✅ | ✅ | ✅ | `notify_offer_received` |
| `offer_accepted` | `offers.status`→accepted | ball_side'a göre buyer/farmer | ✅ | ✅ | ✅ | — | `notify_offer_accepted` |
| `offer_countered` | `offers.status`→counter (veya fiyat/miktar değişir) | Karşı taraf | ✅ | ✅ | ⚠️ **yok — bilinçli/açık, aşağıda** | — | `notify_offer_accepted` |
| `offer_rejected` 🆕 | `offers.status`→rejected | Buyer | ✅ | 🆕 | 🆕 | — | `notify_offer_accepted` |
| `payment_confirmed` | `offers.payment_status`→paid | Çiftçi | ✅ | ✅ | ✅ | — | `notify_offer_accepted` |
| `order_preparing` 🆕 | `orders.status`→preparing | Buyer | ✅ (şu an boş gövde — düzeltilecek) | 🆕 | 🆕 | — | `notify_order_status` |
| `order_shipped` | `orders.status`→shipped | Buyer | ✅ | ✅ | ✅ | — | `notify_order_status` |
| `order_delivered` | `orders.status`→delivered | Buyer | ✅ | ✅ | ✅ | — | `notify_order_status` |
| `order_cancelled` | `orders.status`→cancelled | Buyer **+ Çiftçi** | ✅ ikisine | ✅ ikisine | ✅ ikisine | — | `notify_order_status` |
| `dispute_opened` | `orders.status`→disputed | Buyer **+ Çiftçi** | ✅ ikisine | ✅ ikisine | ✅ ikisine | — | `notify_order_status` |
| `order_completed` 🆕 | `orders.status`→completed | Buyer | ✅ (şu an boş gövde — düzeltilecek) | 🆕 | 🆕 | — | `notify_order_status` |
| `crop_request_match` | Crop `crop_config`'te aktif olur, eşleşen bekleyen talep varsa | Talep eden | ✅ | ✅ | ✅ | — | `notify_crop_request_fulfilled` |
| `harvest_time` | Abonelik hasadına 3 gün kala (günlük 07:00 UTC cron) | Çiftçi **+ Buyer** | ✅ ikisine | ✅ ikisine | ✅ ikisine | ✅ ikisine | `send_subscription_harvest_reminders` (`cron.job`: `subscription-harvest-reminders-daily`) |
| `subscription_new` | Yeni abonelik talebi (INSERT, status=pending) | Çiftçi | ✅ | ✅ | ✅ | — | `notify_subscription_changes` |
| `subscription_accepted` | Abonelik pending→active | Buyer | ✅ | ✅ | ✅ | — | `notify_subscription_changes` |
| `subscription_rejected` | Abonelik pending→cancelled **VE `auth.uid() = farmer_id`** ⚠️ | Buyer | ✅ | ✅ | ✅ | — | `notify_subscription_changes` |

##### Kaldırılan

| Event | Neden |
|---|---|
| ~~`price_alert`~~ | Hiçbir tetikleyicisi yok, ölü toggle — kaldırılıyor (karar yukarıda). |

##### Açık/bilinçli bırakılan tek nokta

- **`offer_countered` hâlâ SMS göndermiyor** (yalnızca push+in-app). Berkin bu turda da
  bu maddeye net bir karar vermedi — düşük öncelikli, kapsam dışı bırakılmaya devam
  ediyor, burada açıkça işaretleniyor (kural #107, sessizce kaybolmasın).

##### Dikkat çekilmesi gereken bir davranış (kod değişmeyecek, yalnızca QA'ya not)

- **`subscription_rejected`** yalnızca `auth.uid() = NEW.farmer_id` iken tetikleniyor —
  yani iptal işlemi çiftçinin kendi oturumu üzerinden değil de (ör. bir admin/service-role
  akışıyla) yapılırsa buyer bildirim ALMAZ. F14/F15 QA'sında her iki yol da (çiftçi kendi
  hesabından iptal / farklı bir yoldan iptal) ayrı test edilmeli.

**Kapsam:** (a) event haritası dokümantasyonu (bu bölüm) + migration round 2 (3 sessiz
geçiş kapatıldı, price_alert kaldırıldı) — bunlar benim/🤖 işim. (b) gerçek çok-cihazlı
test matrisi (16 event × push/SMS × farmer/buyer) — **Berkin'in zamanı**, test senaryosu
`Build/E2E-QA-F14-F15.md`'de (kural #104 formatında) hazır. (c) APNs/FCM kredansiyel
durumu — F16 ile birleşik.

**Bağımlılık:** F4 (bildirim tercihleri UI) bu event haritasını doğrudan kullanıyor —
F3 önce netleşmeli. WhatsApp toggle'ı yalnızca `new_offer`/`harvest_time`'da gösterilecek
(karar 2, F4'ün ayrı bir turu).

**Roller:** 🎯 (event haritası dokümanı + QA senaryosu) · 🤖 (migration + kod düzeltmesi)
· 👤 (çok cihazlı gerçek test + Twilio Console/APNs/FCM durum kontrolü).

**v1.0 — şart.** Push/OTP çalışmıyorsa uygulama "tam işlevsel değil"
sayılır, tam olarak Apple'ın aradığı red gerekçesi.

---

### F4 — Bildirim tercihleri sayfaları (mobil+web, buyer+farmer)

`notif_prefs` şeması zaten event-bazlı tam (13 event × kanal). Web'de
bugün bu tercihlere ait bir ayarlar sayfası var mı **doğrulanmadı** —
kontrol edip yoksa Lovable'a, mobilde de yeni bir ekrana ihtiyaç var.

**Kapsam v1.0:** tek liste — her event için push/SMS **+ WhatsApp**
toggle'ları (**karar, Berkin: WhatsApp dahil edilsin** —
`notif_prefs`'teki `*_whatsapp` kolonları zaten var, üçüncü sütun
olarak eklenir), `community_push` satırı hiç gösterilmez (F3 kararı),
rol bazlı yalnızca ilgili event'ler gösterilsin (ör. çiftçiye
`harvest_time` anlamlı, buyer'a `price_alert` anlamlı — ikisi de
`new_offer`/`offer_accepted` görür ama farklı yönde). **v1.1:** kategori
gruplama, açıklama metinleri.

**Bağımlılık:** F3'ün event haritası + community-event kararı burayı
doğrudan besliyor.

**Roller:** 🎨 (web sayfası, Lovable) · 🤖 (mobil ekran) · 🎯 (event→UI
eşleme dokümanı).

**v1.0 — şart** (minimal versiyon).

---

### F5 — Mobilde Hasat tariflerini favorileme + Defterim'de görüntüleme

`recipe_saves` (user_id, recipe_id) zaten var — yeni migration
gerekmiyor. **Karar (Berkin, 2026-08-18): yalnızca kişisel yer imi**
(kalp ikonu, herkese açık bir "N kişi beğendi" sayacı YOK, en basit
doğru yorum onaylandı).

**Kapsam:** Hasat tarifi detay ekranına kalp/favorile butonu · Defterim
tab'ine "Favorilerim" bölümü (mevcut "Tariflerim" bölümünden ayrı).

**Roller:** 🤖 (mobil UI, RLS zaten `recipe_saves` üzerinde muhtemelen
var — kontrol edilecek).

**v1.1 — submit sonrası hızlı takip.** Etkileşim/elde tutma özelliği,
çekirdek işlevsellik değil, review'ı etkilemez.

---

### F6 — Bitmiş yemek fotoğrafı + isim → AI tarif tahmini

Zaten `P23-Mobile.md`'nin kendi M9 listesinde vardı ("Tahmini tarif"
etiketi zorunlu şartıyla) — sen de paralel/blocker-değil dedin, plan
tutarlı. **Tek ekleme önerim:** bu özelliğin çıktısı F12 (besin
değerleri) ve F13 (diyet/alerjen etiketleme) ile aynı "AI-üretimi,
düşük güven, insan onayı gerekebilir" ailesine giriyor — aynı UI
deseni (uyarı rozeti + düzenlenebilir alan) tekrar kullanılmalı, yeni
bir desen icat edilmemeli.

**v1.1/M9 — blocker değil (senin kararın, katılıyorum).**

---

### F7 — Defterime eklediğim kendi tariflerimi editleyebilmeliyim

Şu an yalnızca AI-import sırasındaki "Kontrol Et" ekranı düzenlemeye
izin veriyor (kaydetmeden önce). Kayıttan SONRA tekrar açıp düzenleme
akışı yok. **Kapsam:** aynı "Kontrol Et" bileşenini kayıt-sonrası
düzenleme modunda yeniden kullan (yeni bir ekran yazmak yerine) — hem
daha az kod hem tutarlı UX.

**Bağımlılık:** F12 (besin değerleri) "edit sonrası tekrar hesaplanır"
diyor — bu düzenleme akışının kaydet adımına bir trigger/callback
eklenmesi gerekiyor. F7 önce netleşmeli ki F12 ona bağlanabilsin.

**Roller:** 🤖 (mobil, Defterim-only — web'de kişisel tarif içe aktarma
zaten yok, M9'da kalıyor).

**v1.1.**

---

### F8 — Tarifler share edilebilir olmalı (link)

**Hasat tarifleri (public):** zaten SEO'lu bir slug URL'i var
(`/tarifler/{slug}`) — mobilde native share sheet (`Share.share()`)
ile bu URL'i paylaşmak ucuz bir ekleme, yeni backend gerekmiyor.

**Kişisel (Defterim, private) tarifler — karar (Berkin, 2026-08-18):**
önerdiğim `share_token` modeli onaylandı — tek-kullanımlık olmayan,
tahmin edilemez token'lı salt-okunur link (`recipes.share_token uuid`,
opt-in, `visibility='private'` kalır, link'i bilen görebilir, herkese
açık aramada çıkmaz). Bölüm 4'ün migration'ında zaten vardı, değişiklik
yok.

**Bağımlılık:** Domain henüz yok (madde 17) — paylaşılan link'ler şu an
`hasat.lovable.app` tabanlı olur, domain gelince URL'lerin kırılmadan
değişmesi için **base URL'in tek bir config değerinden okunduğundan**
emin olunmalı (aşağıda F17'de detaylandırıyorum, küçük bir teknik borç
önleme önerisi).

**Roller:** 🤖 (mobil share sheet) · 🎨/🤖 (web share butonu, zaten
public sayfa var) · 🎯 (privacy karar seçenekleri — yukarıda).

**v1.0 (yalnızca Hasat/public tarifler için) — ucuz, hemen yapılabilir.
v1.1 (private/Defterim paylaşımı — migration + karar gerektiği için).**

---

### F9 — Tarif ekle wizard'ında görsel ekleme alanı yok

`recipes.cover_photo_url` zaten var (F1'de kullandığımız kolon) — yeni
kolon gerekmiyor. Gereken: `recipe-step-photos` bucket'ında T4'te
kurulan RLS deseninin (owner-scoped, `parcel-photos` ailesiyle aynı)
bir eşi, tarif kapağı için (`recipe-cover-photos` ya da mevcut bucket'ı
genişletme — küçük bir karar, ben `recipe-step-photos` bucket'ını genel
`recipe-user-photos`'a çevirip hem adım hem kapak fotoğrafını aynı
bucket'ta tutmayı öneriyorum, gereksiz bucket çoğaltmamak için).

**Kapsam:** "Kontrol Et" ekranına (import akışı + F7'nin edit modu)
kapak fotoğrafı seçme/yükleme alanı.

**Bağımlılık:** F7 ile aynı ekranı paylaşıyor, birlikte yapılmalı.

**Roller:** 🤖 (mobil) · 🎯 (bucket/RLS küçük genişletme kararı).

**v1.1.**

---

### F10 — Mobilde notification center + bell, web+mobil full sync

**En büyük iyi haber burada:** `notifications` tablosu zaten var ve web
muhtemelen zaten okuyor (App-Audit.md'de "Zil ikonu, panel, 'Teklifiniz
Kabul Edildi'" ✅ olarak test edilmişti). Mobil aynı tabloyu okursa
(RLS zaten `user_id = auth.uid()` ile scoped olmalı, doğrulanacak)
**senkron otomatik gelir** — ayrı bir eşitleme mekanizması kurmaya
gerek yok, ikisi de aynı tek doğruluk kaynağını okuyor.

**Kapsam v1.0 (lite):** mobilde bell ikonu + basit liste ekranı (tip,
başlık, gövde, `read_at` işaretleme) — mevcut push bildirimleriyle
karışmasın diye push geldiğinde de aynı `notifications` satırına
işaret etsin (zaten trigger fonksiyonları hem `dispatch_push` hem
`insert into notifications` çağırıyor gibi görünüyor, doğrulanacak).
**v1.1:** gruplama/filtre.

**Karar (Berkin, 2026-08-18): "predictive bildirimler" kapsamdan tamamen
çıkarıldı**, şimdilik gerek yok — F10 yalnızca sistem + push + panel
bildirimlerini (mevcut `notifications` tablosu) kapsıyor.

**Roller:** 🤖 (mobil ekran + bell) · 🎨 (web tarafı zaten var,
dokunulmayabilir) · 🎯 (RLS doğrulama).

**v1.0 — lite versiyon şart** (çekirdek işlevsellik parity'si, Apple
reviewer'ı "push geldi ama görülecek yer yok" durumunu fark edebilir).
**v1.1 — gruplama/filtre.**

---

### F11 — Tarifler clone'lanabilir olmalı

Yeni `recipes` satırı: `owner_id=self`, `visibility='private'`,
`author_type='kullanici'`, `source_type` için önerim yeni bir değer
(`'clone'`) — mevcut `manual/text/photo/url` setine ekleniyor, dar
migration. Atıf için opsiyonel `cloned_from_recipe_id uuid` kolonu
öneriyorum (F1/F8/F12/F13 ile aynı migration turuna eklenir).

**Kapsam:** hem Hasat tarifleri hem (F8'in privacy kararına göre)
başkasının paylaştığı tarifler klonlanabilir. `recipe_ingredients`/
`recipe_steps` da kopyalanır (adım fotoğrafları da mı kopyalanır yoksa
referans mı kalır — küçük bir teknik karar, referans kalması daha
ucuz ve yeterli).

**Bağımlılık:** F7 (edit) ile aynı düzenleme ekranını kullanır — klonla
→ direkt edit moduna düşer.

**Roller:** 🤖 (mobil + RPC, kural #106 gereği kopyalama mantığı
Postgres fonksiyonu olarak — client'ta değil).

**v1.1.**

---

### F12 — Besin değerleri AI ile hesaplanmalı (+ mevcutlar backfill)

Yeni migration: `recipes`'e `calories`, `protein_g`, `carbs_g`, `fat_g`,
`fiber_g`, `micronutrients jsonb`, `nutrition_calculated_at
timestamptz`. Hesaplama: malzeme listesi + miktarlar bir LLM'e
(muhtemelen aynı `extract-recipe`'in kullandığı sağlayıcı) verilip
yapılandırılmış çıktı isteniyor — **bu bir tahmin, kesin laboratuvar
değeri değil**, UI'da "tahmini besin değerleri" etiketiyle gösterilmeli
(Recipe-Automation.md'deki gıda güvenliği ilkesiyle aynı aile — tahmini
değerler asla otomatik/sessiz "kesin" gibi sunulmamalı).

**Ne zaman hesaplanır:** (a) yeni tarif kaydında bir kere, (b) F7'nin
edit akışında malzeme/miktar değişince tekrar (kaydet adımına hook),
(c) **backfill**: mevcut 18 Hasat tarifi + ileride otomasyonla gelecek
tarifler için tek seferlik toplu iş.

**Bağımlılık:** F2'nin altyapısıyla (zincirlenmiş Edge Function
deseni) aynı mekanizmayı paylaşabilir — ayrı bir sistem kurmaya gerek
yok, F2'nin pipeline'ına bir "nutrition" aşaması olarak eklenebilir.
F7 (edit) ile kaydet-sonrası tetikleme bağlantısı var.

**Roller:** 🤖 (migration + edge function + backfill job) · web/mobil
UI'da besin değerleri paneli (🎨/🤖).

**v1.1 — submit'i bloklamasın**, yeni bir yetenek, review kapsamında
değil. Ama F2 ile birlikte erken başlanabilir (paralel).

---

### F13 — Hasat tariflerinde filtre butonu + filtre seti

En büyük madde, ama **diyet filtresinin yarısı zaten hazır**
(`diet_tags` dolu). Netleştirilmesi/eklenmesi gerekenler:

1. **Diyet tipleri — karar (Berkin, 2026-08-18): kontrollü sabit liste,
   tam listeyi ben belirledim** (aşağıda). `diet_tags text[]` zaten var,
   yalnızca değer sözlüğü sabitleniyor — migration gerekmiyor, yalnızca
   editoryal disiplin + mevcut 3 değerin (`vegan`/`vejetaryen`/`glutensiz`)
   bu listeyle uyumlu kalması.

   | Slug (DB değeri) | Görünen ad | Not |
   |---|---|---|
   | `vegan` | Vegan | Zaten kullanılıyor |
   | `vejetaryen` | Vejetaryen | Zaten kullanılıyor (doğru yazım budur, "vejeteryan" değil) |
   | `pesketaryen` | Pesketaryen | Senin "Pasketaryen" yazımının doğrusu |
   | `glutensiz` | Glutensiz | Zaten kullanılıyor |
   | `laktozsuz` | Laktozsuz | Glutensiz'in doğal eşi, çoğu kullanıcı ikisini birlikte arar |
   | `akdeniz` | Akdeniz Diyeti | Senin istediğin |
   | `dash` | DASH | Senin istediğin |
   | `dusuk-karbonhidrat` | Düşük Karbonhidrat | Ekledim — çok aranan bir filtre, mevcut malzeme/miktar verisinden türetilebilir |
   | `seker-ilavesiz` | İlave Şekersiz | "bağışıklık/sindirim" isteğini somutlaştırdım — şeker kısıtı en net/ölçülebilir alt-küme |
   | `sindirim-dostu` | Sindirim Sistemi Dostu | Senin "sindirim rahatsızlığı" isteğin — genel/yumuşak bir etiket, aşağıdaki uyarıyla |
   | `bagisikligi-destekleyen` | Bağışıklığı Destekleyen | Senin "bağışıklık" isteğin, aynı uyarıyla |

   **Önemli çekince (eklemem gerekiyordu):** son iki etiket (`sindirim-
   dostu`, `bagisikligi-destekleyen`) sağlıkla ilgili çağrışım yapıyor —
   Hasat bir tıbbi/diyetisyen tavsiyesi vermiyor, bu yüzden filtre
   sayfasında küçük bir dipnot ("bilgilendirme amaçlıdır, tıbbi tavsiye
   yerine geçmez") eklenmesini öneriyorum. Alerjen/hastalık gibi daha
   ciddi sağlık iddiaları bu iki etikete değil, ayrı alerjen filtresine
   (madde 5) gitmeli.

2. **Süre filtresi** (<30dk, <1sa) — yeni kolon gerekmiyor,
   `prep_minutes+cook_minutes` üzerinden türetilen bir sorgu/filtre.
3. **Hasat'ta satılan ürün içeren** — yeni kolon gerekmiyor,
   `recipe_ingredients.crop` + aktif `listings` join'iyle türetilir
   (zaten malzeme kartının "eşleşti" durumunun kullandığı mantığın
   aynısı, kural #106 gereği bir view/RPC olarak).
4. **Mutfak ekipmanı — kontrollü liste (aynı kararla sabitlendi):**
   `required_equipment text[]` (yeni kolon, migration gerekiyor).

   | Slug | Görünen ad |
   |---|---|
   | `firin` | Fırın |
   | `ocak` | Ocak/Tencere |
   | `mikrodalga` | Mikrodalga |
   | `airfryer` | Air Fryer |
   | `blender` | Blender |
   | `mutfak-robotu` | Mutfak Robotu |
   | `duduklu-tencere` | Düdüklü Tencere |
   | `izgara` | Izgara/Barbekü |
   | `ozel-ekipman-gerekmiyor` | Özel Ekipman Gerekmiyor |

5. **Alerjen filtresi** — Recipe-Automation.md denetiminde bulduğum
   `allergen_labels` eksikliği burada da çıkıyor: **hiç kolon yok**,
   migration şart. Alerjen etiketleme özellikle hassas (gıda güvenliği)
   — AI-destekli ön-etiketleme + **insan onayı zorunlu** (Recipe-
   Automation.md'deki "gıda güvenliği alanları süresiz insan onayında
   kalmalı" ilkesiyle birebir). Kontrollü liste öneriyorum: `gluten`,
   `laktoz`, `yumurta`, `fındık-yerfıstığı`, `soya`, `susam`, `deniz-ürünü`.

**UI:** web `/tarifler` filtre çubuğu/sidebar, mobil bottom-sheet ya
da ayrı filtre ekranı (responsive, kolay kullanılabilir — senin
şartın). İkisi de aynı RPC/view'ları çağırmalı (kural #106).

**Roller:** 🤖 (migration + RPC + mobil filtre UI) · 🎨 (web filtre
UI) · 🎯 (kontrollü kelime listesi tasarımı + alerjen insan-onay
akışı) · 🧑‍🍳 (AI ön-etiketleme, F2 pipeline'ı üzerinden).

**v1.0 (dar kapsam):** süre + Hasat-ürünü-içeren + mevcut diyet
etiketleri (3 filtre, migration gerektirmiyor, hızlı). **v1.1:**
ekipman + alerjen (migration + tüm tariflerin etiketlenmesi gerektiği
için daha yavaş, alerjen ayrıca insan-onay akışı istiyor).

---

### F14 — Sipariş status akışları (çiftçi+buyer) kesintisiz olmalı

Yukarıda çıkardığım gibi **iki state machine** var: `offers.status`
(müzakere) ve `orders.status` (teslimat, `preparing→shipped→delivered→
disputed/completed/cancelled`), artı `offers.payment_status` (serbest
metin — gerçek ödeme entegrasyonu gelene kadar muhtemelen manuel/admin
tarafından set edilen bir placeholder, **tam olarak nasıl çalıştığı
doğrulanmalı**, bu maddenin bir parçası). Görev tanımın ("ödeme
altyapısı şimdi gelmeyecek olsa da önü arkası sağlam olmalı") bu
placeholder'ın gerçek ödeme geldiğinde sorunsuz değiştirilebilir
olmasını da kapsıyor — yani QA yalnızca "şu an çalışıyor mu" değil
"yarın ödeme eklenince kırılmayacak mı" sorusunu da sormalı.

**Kapsam:** her iki state machine'in her geçişi (kabul/red/karşı
teklif/iptal/teslim/anlaşmazlık) için hem çiftçi hem buyer tarafında
uçtan uca QA senaryosu (kural #104 formatında, web + mobil), her
geçişte doğru bildirimin (F3'ün event haritasına göre) doğru kişiye
gittiğinin doğrulanması, ve `payment_status` placeholder'ının davranışının
netleştirilmesi/dokümante edilmesi.

**Bağımlılık:** F3 (event haritası) olmadan "doğru bildirim gitti mi"
sorusu cevaplanamaz — F3 önce.

Tam senaryo seti: `Build/E2E-QA-F14-F15.md`.

**Roller:** 🎯 (QA senaryo seti + Supabase MCP ile durum-geçiş
doğrulaması) · 🤖/🎨 (bulunan hataların düzeltmesi, varsa).

**v1.0 — şart.** Bu, uygulamanın çekirdek güven döngüsü; reviewer'ın
en olası test edeceği akış.

---

### F15 — Bugüne kadar doğrulanamayan aşamalar doğrulanmalı

Bu ayrı bir özellik değil, **M5-M8 boyunca birikmiş "kural #103 —
doğrulanamadı" borcunun kapatılması** — offline uçak modu, pişirme
modu gerçek cihaz davranışı, AI import kamera yolu, native
picker/modal akışları, gerçek push teslimatı, gerçek OTP autofill,
`expo-sqlite` gerçek cihaz davranışı, web tarayıcı click-through'ları
(ağ politikası engeliyle bu oturumda hiç yapılamayanlar). Hepsinin tek
bir listesi `TODO.md`'de dağınık duruyor — **ilk adımım bunları tek bir
konsolide checklist'te toplamak** (hangi build log'da, hangi QA
senaryosunda) olacak, böylece Berkin tek oturumda (ya da birkaç
oturumda cihaza göre gruplanmış) hepsini kapatabilir.

Tam senaryo seti: `Build/E2E-QA-F14-F15.md`.

**Roller:** 🎯 (konsolide checklist) · 👤 (fiili gerçek cihaz/tarayıcı
testi — bu adım kimse tarafından devredilemez, gerçek cihaz gerekiyor).

**v1.0 — tanım gereği şart** (submit-hazırlık listesinin kendisi bu).

---

### F16 — Android FCM kurulmalı

Zaten adım adım talimat `Store-Compliance.md`'de var (Firebase Console
→ proje → `com.hasat.app` Android app → `google-services.json` → repo
→ servis hesabı anahtarı → EAS). `app.json` tarafı zaten hazır (M8-a).
**Tek eksik: Berkin'in bu adımları fiilen yapması** — ben yalnızca
kontrol/doğrulama yapabilirim (dosyanın repoya düştüğünü, EAS'a
yüklendiğini teyit ederim).

**Roller:** 👤 (Firebase kurulumu — tarayıcı etkileşimi gerektiriyor,
bu oturumdan yapılamaz) · 🎯 (doğrulama).

**v1.0 — şart, Android submit'i doğrudan bloke ediyor.**

---

### F17 — Marka (logo, landing page, icon, SEO, LLM SEO) — domain olmadan

**Alt maddelere ayırıyorum çünkü sahiplik farklı:**

- **Logo + app icon'lar** — 👤 Berkin (tasarım varlığı, ben üretemem,
  ama App Store/Play submit'i **teknik olarak bloke ediyor** — icon
  olmadan submit formu tamamlanamaz). v1.0 şart.
- **Landing page lansman mesajı** — zaten Launch-Plan.md Epic E2, 🤖
  Claude Code işi, bloke değildi, hâlâ değil. v1.0.
- **Teknik SEO** — `sitemap.xml`/`robots.txt`/JSON-LD `Recipe` zaten var
  (M4-c). F12 (besin değerleri) gelince JSON-LD'ye `nutrition` alanı
  eklenebilir (schema.org Recipe bunu destekliyor) — küçük ek. v1.1.
- **"LLM SEO"** — somutlaştırıyorum: `llms.txt` dosyası (AI arama
  motorlarının sitenin ne olduğunu hızlı anlaması için), temiz/erişilebilir
  SSR içerik (zaten var), FAQPage schema.org işaretlemesi eklenebilir.
  Yeni ve genişleyen bir alan, kimse için "zorunlu" bir standart yok —
  öneri: temel `llms.txt` v1.0'a girsin (ucuz), gerisi v1.1.
- **Domain eksikliği — gerçek risk, açıkça bildiriyorum:** domain
  olmadan (a) paylaşım linkleri (F8), (b) SEO canonical URL'leri, (c)
  e-posta/SMS gönderen kimliği hâlâ `lovable.app`/`supabase.co` tabanlı
  kalıyor. Bunların hepsini **tek bir `PUBLIC_BASE_URL` config
  değerinden** okumaya geçirmeyi öneriyorum (muhtemelen kısmen zaten
  böyle) — domain gelince kod avı yapmadan tek yerden değişsin. Bu,
  domain'i submit'ten önce şart koşmuyor ama **şimdiden bu şekilde
  kurulmasını** öneriyorum (ucuz, ileride büyük zaman kazandırır).

**v1.0:** logo+icon (Berkin) + landing mesajı + temel `llms.txt` +
`PUBLIC_BASE_URL` config disiplini. **v1.1:** derin SEO/LLM-SEO
genişletmesi, domain geldiğinde geçiş.

---

## 3. Önerdiğim ekler (senin listende yoktu, gerekçeli)

1. **Şema migration'larını tek turda birleştir** (F1'in cover_photo
   backfill'i hariç — o migration değil, veri güncelleme): allergen,
   equipment_tags, nutrition kolonları, `share_token`, `cloned_from_recipe_id`
   — hepsi "tarif metadata genişletmesi" ailesi, kural #115'in pahalı
   dört-adımlı döngüsünü 5 kere değil 1 kere yaşamak için tek migration'da
   toplanmalı.
2. **Kontrollü kelime listesi (diyet + ekipman)** editoryal disiplin
   için — serbest metin yerine sabit liste, aksi halde 6 ay sonra
   "vejeteryan"/"vejetaryen" gibi varyant çorbası birikir.
3. **F2/F12/F13'ün AI-etiketleme aşamalarını tek pipeline'da birleştir**
   — üç ayrı "AI tarif işleme" sistemi kurmak yerine, F2'nin zincirlenmiş
   Edge Function deseni görsel+besin değeri+diyet/ekipman/alerjen
   etiketlemesinin ortak taşıyıcısı olsun.
4. **`community` event kararı** (F3'te bulunan ölü toggle) — ya
   event eklenir ya toggle kaldırılır, belirsiz kalmasın.
5. **`PUBLIC_BASE_URL` config disiplini** (F17) — domain geçişini
   ileride tek satırlık değişiklik yapmak için şimdiden.
6. **Alerjen etiketleri için zorunlu insan-onay akışı** — AI
   ön-etiketlesin ama alerjen gibi gıda güvenliği alanı hiçbir zaman
   sessizce otomatik yayınlanmasın (Recipe-Automation.md'deki ilkeyle
   tutarlı, F13'e bağladım).
7. **"Predictive bildirim" tanımının netleştirilmesi** (F10'da açık
   soru olarak bıraktım) — kapsamı bilmeden doğru şekilde
   önceliklendiremiyorum.

---

## 4. Konsolide şema migration planı (tek PR, tek round-trip)

| Kolon | Tablo | Amaç | Bağlı madde |
|---|---|---|---|
| `allergen_labels text[]` | `recipes` | Alerjen filtresi | F13 |
| `required_equipment text[]` | `recipes` | Ekipman filtresi | F13 |
| `calories`/`protein_g`/`carbs_g`/`fat_g`/`fiber_g` numeric, `micronutrients` jsonb, `nutrition_calculated_at` timestamptz | `recipes` | Besin değerleri | F12 |
| `share_token uuid` (nullable, opt-in, unique index) | `recipes` | Private tarif paylaşımı — onaylandı | F8 |
| `cloned_from_recipe_id uuid` (nullable FK) | `recipes` | Klon atfı | F11 |
| `community_push` kolonunu **DÜŞÜR** | `notif_prefs` | Ölü toggle, kullanılmıyor — onaylandı | F3 |

`diet_tags`/`required_equipment` için **yeni kolon yok** (`diet_tags`
zaten var, `required_equipment` yukarıdaki tabloda) — asıl iş kontrollü
kelime listesine göre editoryal/AI-destekli etiketleme (F2 pipeline'ı).

`recipe_generation_*` tabloları (F2) ayrı, ikinci bir migration'da —
farklı bir alt sistem, aynı PR'a karıştırmaya gerek yok.

---

## 5. Bölüm 2 (lansman sonrası milestone tablosu) yeniden değerlendirmesi

Mevcut `Launch-Plan.md` §2'deki M8-b (15 Eyl)/M8-c (20 Eyl)/M8-d (30
Eyl)/Store canlı (~15 Eki) tarihleri **yeni 1-15 Eylül submit hedefiyle
tamamen geçersiz** — hepsi öne çekilmeli. Önerim:

| Yeni sıra | İş | Hedef |
|---|---|---|
| Hemen | F1 (görsel bağlama), F3 (event haritası+2 kod düzeltmesi), F4-lite, F10-lite, F13-dar, F14 QA, F15 konsolide checklist | 18-22 Ağustos |
| Berkin'e bağlı, paralel | F16 (FCM), APNs EAS yükleme, F17 logo/icon | 18-25 Ağustos |
| Migration turu | Bölüm 4'teki tek PR, kural #115 sırasıyla | 22-27 Ağustos |
| M8-b — gerçek cihaz doğrulama | F15 checklist + yeni v1.0 özellikleri | 27-31 Ağustos |
| M8-c — push/OTP gerçek teslimat doğrulama | F3'ün çok-cihazlı testi | 27-31 Ağustos (M8-b ile paralel olabilir) |
| M8-d — Submit | App Store + Play | **1-15 Eylül** |
| Store canlı | Review sonucu | Eylül sonu (tahmini, Apple red ihtimaline pay var) |
| Fast-follow v1.1 | F2 tam ritim, F5/F7/F8-private/F9/F11/F12/F13-geniş | Submit sonrası, ~Eylül sonu-Ekim başı |

Bunu onaylarsan `Build/Roadmap.md`'nin Gantt'ını ve `Launch-Plan.md`
§2'yi bu tabloya göre güncelleyeceğim.

---

## 6. Açık kararlar — ÇÖZÜLDÜ (Berkin, 2026-08-18)

1. `community` event'i eklenmiyor, toggle kaldırılıyor. → F3, Bölüm 4.
2. WhatsApp kanalı bildirim tercihleri sayfasına dahil. → F4.
3. Favoriler yalnızca kişisel yer imi, herkese açık sayaç yok. → F5.
4. Private tarif paylaşımı: `share_token` modeli onaylandı; Hasat
   (public) tarifleri zaten paylaşılabilir. → F8.
5. Diyet/ekipman için kontrollü sabit liste, tam listeler yukarıda
   (F13) belirlendi.
6. Predictive bildirimler kapsam dışı, unutuldu. → F10.
7. `hasat-webp.sh` eşdeğeri: Supabase Edge Function olarak (öneri
   gerekçesiyle yukarıda, F2) — üç tarafın (ben/Claude Code, ChatGPT
   orkestratörü, dolaylı Gemini) erişebilmesi için.

Tek kalan açık soru (bloke etmiyor, ileride netleşebilir): `offer_countered`
event'inin neden SMS karşılığı yok (F3, madde 2) — bilinçli mi
unutulmuş mu, ayrıca sormadım, önemsiz önceliklendirme detayı.

---

## 7. Süreç ve roller (hatırlatma, net olsun)

- **🤖 Claude Code** — `hasat-mobile`/`hasat-d2c-marketplace`/`hasat-core`
  kod değişiklikleri, migration'lar, GitHub commit/PR (senin başka bir
  Claude Code oturumun — bu oturumun bu repolara yazma yetkisi yok, T4
  turunda netleşti).
- **🎨 Lovable** — web tarafında Lovable ajanı üzerinden yürütülen
  değişiklikler (bu oturumun `Lovable` MCP aracı var, kullanılabilir).
- **🎯 Orkestratör (ben)** — planlama, önceliklendirme, Supabase MCP ile
  şema/veri doğrulama, QA senaryosu yazımı, `hasat-vault` doküman
  güncellemesi (yine T4 turunda netleşen kısıtla: bu oturum vault'a da
  doğrudan push edemiyor, senin diğer Claude Code oturumuna prompt
  hazırlıyorum).
- **🧑‍🍳 ChatGPT + Gemini** — tarif içerik üretimi (Planner/Writer/QA
  agent'ları ChatGPT tarafında, görsel üretimi Gemini "nano banana"),
  Hasat'ın admin-key korumalı endpoint'lerini çağırarak — kod
  yazmıyorlar, yalnızca F2'nin altyapısını kullanıyorlar.
- **👤 Berkin** — tasarım varlıkları (logo/icon), gerçek cihaz/tarayıcı
  testi, Twilio/EAS/Firebase konsol işlemleri, PR merge, açık kararlar.

---

## 8. Sıradaki adım

Bu planı onaylarsan (ya da Bölüm 6'daki kararları netleştirirsen):

1. Bu dokümanı + `Launch-Plan.md` §2 + `Roadmap.md` Gantt güncellemesini
   `hasat-vault`'a yazacak bir **Claude Code promptu** hazırlarım (senin
   diğer git-erişimli oturumun çalıştırır, commit+PR açar).
2. Ardından Bölüm 4'teki migration PR'ı (🤖) ve F1'in bağlama SQL'i
   (🎯, doğrudan Supabase MCP ile, migration değil basit UPDATE) ile
   implementasyon başlar — Bölüm 5'teki sırayla.

**Bu planı onaylıyor musun, yoksa önce Bölüm 6'nın bazı maddelerine
netlik mi getirmek istersin?**
