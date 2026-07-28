---
title: Hasat — P23 Buyer Mobile & Recipe App
updated: 2026-07-28
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

### M3 — İçerik
- **15–20 özgün tarif.** Crop dağılımı mainstream ağırlıklı (domates, elma, fındık, zeytinyağı, kekik, patates); **safran en fazla 1–2 tarif** (crop-agnostic ilkesi — bkz. P25)
- `crop_culinary_meta` seed (70 crop), ~20 crop temsili görseli
- **Glossary insan gözden geçirmesi** (P22-C'den bekleyen madde: AI üretimi, bölgesel doğrulama yapılmadı)
- **Tarif seti = arz büyüme planının aynası:** hangi crop'ta çiftçi kazanmak istiyorsan o crop'un tarifini yaz

### M4 — Web tarif yüzeyi *(huninin üst ağzı — mobilden ÖNCE)*
- `/tarifler` + `/tarifler/$slug` — **`/buyer/` dışında**, misafire açık, SSR ile SEO'lu
- Malzeme kartı 3 durumu: eşleşti → ürün sayfası · yok → **Talep Et** + haber ver · platform-dışı (tuz/un) → nötr
- Admin'de talep heatmap'i
- **BENCHMARK Gap #9 — "parselden tabağa QR görünümü"** buraya bağlandı (şu an sahipsiz duruyor; tarif→malzeme→parsel zinciri tam olarak o özellik)
- **Neden mobilden önce:** SEO 3–6 ayda birikir; içeriksiz ve kullanıcısız bir native app'in store'da değeri yok, 4.2 reddi de en çok o durumda gelir

### M5 — Mobil iskelet
- Expo + Expo Router + Nativewind, telefon OTP (mevcut akışın aynısı), tarif listesi/detayı
- **Offline önbellek** (expo-sqlite)
- Play hesap tipi kararı (personal $25 şimdi mi, organizasyon mu) burada verilir
- **Çıkış:** Uçak modunda app açılıyor ve tarifler görünüyor — *Apple 4.2'nin asıl testi*

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
