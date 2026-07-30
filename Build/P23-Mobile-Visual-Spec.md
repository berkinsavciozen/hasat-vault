---
title: Hasat — P23-M3-D Mobil UI Görsel Şartnamesi
updated: 2026-07-30
tags:
  - hasat
  - p23
  - mobile
  - design
  - m3-d
---

# P23-M3-D — Mobil UI Görsel Şartnamesi

> Referans: `Build/P23-Mobile.md` → M3 bölümü, "M3-D" iş kolu.
> Bu dosya tek doğruluk kaynağıdır — Figma/Lovable'da serbest dolaşan bir kopya
> üretilirse bu dosya günceli, diğeri değil.

## Amaç ve kapsam sınırı

M5/M6'nın iterasyon maliyetini düşürmek için var. Mobilde Lovable yok, EAS
build dakikalar sürüyor, local önizleme mümkün değil — görsel hedef olmadan
build'e girmek her düzeltmeyi pahalı yapıyor. **Bu bir backend de-riske etme
çalışması değildir** (o iş M4'te web tarif yüzeyiyle yapılıyor); bu çalışma
yalnızca native UX'in webden gerçekten ayrıldığı ve yanlış yapmanın maliyetli
olduğu **beş ekranı** kapsıyor:

1. Pişirme Modu
2. Offline Durumu
3. AI Import Akışı
4. Alt Navigasyon
5. "Talep Et"

**Kapsam dışı (Design'a girmeyecek):** Keşfet, ürün listesi, siparişlerim,
tarif listesi. Bunlar webdeki muadillerini yakından takip ediyor — Nativewind
+ Expo Router sayesinde port maliyeti düşük, ayrı bir görsel şartnameye
gerek yok (bkz. `Build/P23-Mobile.md` → "Bedeli düşüren üç karar").

**Çıktı kod değildir.** React Native kodu beklenmiyor; M5/M6'da bu şartnameden
gerçek ekrana çeviri adımı olacağı kabul edilmiş durumda (Claude Code %100
mobil işini yazacak — bkz. `Build/P23-Mobile.md` → "Platform kararı").

**Referans veri:** Bu şartnamedeki tüm örnekler P23-M3'te üretilen gerçek
tariflerden alınmıştır (`recipes`/`recipe_steps`/`recipe_ingredients`,
2026-07-30 itibarıyla 18 tarif canlı DB'de). Placeholder metinle pişirme modu
tasarlamanın işe yaramayacağı öngörüsü doğrulandı: gerçek adım uzunlukları
(2 cümleden 1 cümleye), gerçek timer aralığı (0 sn'den 259.200 sn'e/3 güne
kadar) ve gerçek malzeme sayısı (4–9) aşağıdaki kararları doğrudan şekillendirdi.

---

## 1. Pişirme Modu

**Neden native'de farklı:** Web'de "adım adım oku" bir sayfa kaydırmasıdır.
Mobilde mutfakta elleri meşgul bir kullanıcı var — dokunmadan ilerleme
(sesli/otomatik değil ama büyük hedef alanlı), ekranın kararmaması ve
timer'ın arka planda da çalışması gerekiyor. Yanlış yapmanın maliyeti:
kullanıcı elleri hamurlu/yağlıyken telefona dokunmak zorunda kalır ya da
15 dakikalık bir mayalanmayı unutur.

### Durum A — Adım ekranı (timer'sız adım)
```
┌─────────────────────────────┐
│ ✕                    3 / 6  │  ← kapat (adım listesine dön) · adım sayacı
│ ●●●○○○                      │  ← ilerleme çubuğu (dolu=tamamlanan)
│                              │
│   [adım fotoğrafı varsa]    │  ← recipe_steps.photo_url; yoksa crop
│                              │     görseli + "temsili görsel" etiketi
│  "Sarımsak, rendelenmiş     │
│   domates, kekik, tuz ve    │  ← recipe_steps.instruction (büyük punto,
│   karabiberi ekleyip birkaç │     min. 20sp — mutfakta uzaktan okunabilir)
│   dakika daha kavurun."     │
│                              │
│                              │
│  ← Önceki        Sonraki →  │  ← tam genişlik, büyük dokunma alanı
└─────────────────────────────┘
```
Örnek: "Zeytinyağlı Nohut Yemeği" (`zeytinyagli-nohut-yemegi`) adım 4.

### Durum B — Timer'lı adım
Timer, adım metninin **altında** ayrı bir kart olarak durur — metinle
karışmaz, adım geçişinde otomatik sıfırlanmaz (kullanıcı "Sonraki"ye
basmadan timer bitse bile adım ekranında kalınır).
```
┌─────────────────────────────┐
│ ✕                    5 / 6  │
│ ●●●●●○                      │
│                              │
│  "Süzülmüş nohutları         │
│   tencereye alın... kısık   │
│   ateşte pişirin."           │
│                              │
│   ┌───────────────────┐     │
│   │      45:00         │     │  ← timer_seconds=2700 → mm:ss;
│   │   [▶ Başlat]        │     │     büyük rakam, tek dokunuşla başlar
│   └───────────────────┘     │
│                              │
│  ← Önceki        Sonraki →  │
└─────────────────────────────┘
```
**Ekran uyanık tutma:** Timer `Başlat`a basılınca native "keep-awake"
tetiklenir (Expo `expo-keep-awake`) ve **yalnızca o adımdan çıkılınca**
serbest bırakılır — kullanıcı ✕'e basıp listeye dönerse veya "Sonraki"ye
geçerse bir sonraki timer'lı adımda tekrar tetiklenmesi gerekir. Timer
arka plana alınmış uygulamada da (yerel bildirim ile "Süre doldu") çalışmalı
— bu, `Zeytinyağlı Buğday Tanesi Salatası`'ndaki 40 dakikalık haşlama gibi
kullanıcının telefonu bırakıp mutfaktan ayrıldığı senaryolar için zorunlu.

**Uç durum — çok uzun timer:** `Cevizli Üzümlü Köme` adım 6'da
`timer_seconds=259200` (3 gün, kömenin kurutulma süresi). Bu ölçekte
saymaç göstermek anlamsız — `timer_seconds > 3600` olduğunda ekran
mm:ss yerine **"Tahmini süre: 3 gün"** gibi bir açıklama metnine döner,
geri sayım/bildirim tetiklenmez (kullanıcı telefonunu 3 gün açık
tutmayacak). Eşik: 1 saat (3600 sn) üstü = "uzun süreç" modu, altı =
gerçek geri sayım.

**Uç durum — timer yok, adım anlık:** `Kekikli Zeytinyağı Ezmesi` adım
1–2 gibi `timer_seconds=NULL` adımlarda timer kartı hiç render edilmez —
boş/gri bir kart bırakmak "burada bir şey eksik" hissi verir.

### Adım listesi (giriş noktası)
Pişirme moduna girmeden önce tüm adımların özet listesi gösterilir (her
satır: sıra no + ilk cümle + varsa timer ikonu ve süresi), en alt CTA
**"Pişirmeye Başla"**. Bu ekran kapsam dışı değil ama basit bir liste —
ayrı wireframe gerektirmiyor.

---

## 2. Offline Durumu

**Neden native'de farklı:** Web'de offline = beyaz ekran/hata, kabul
edilebilir (kullanıcı zaten masaüstünde, bağlantı genelde geri gelir).
Mobilde kullanıcı mutfakta, bodrum depoda ya da tarlada olabilir — **Apple
4.2'nin asıl testi de bu** (`Build/P23-Mobile.md` → M5 çıkış kriteri: "uçak
modunda app açılıyor ve tarifler görünüyor"). Yanlış yapmanın maliyeti: 4.2
reddi (çalışmayan/boş uygulama).

### Kapsam: ne offline çalışır, ne çalışmaz
| Özellik | Offline'da | Kaynak |
|---|---|---|
| Daha önce açılmış tarif listesi/detayı | ✅ önbellekten | `expo-sqlite` (M5) |
| Pişirme modu (önbellekteki bir tarifte) | ✅ tamamen | timer client-side, DB gerektirmez |
| Yeni tarif arama / henüz görülmemiş tarif | ❌ | ağ gerekli |
| "Talep Et" gönderimi | ❌ (kuyruğa alınır, bkz. aşağı) | ağ gerekli |
| AI import | ❌ | edge function çağrısı gerekli |

### Durum A — Şerit uyarı (bağlantı koptu, önbellek var)
```
┌─────────────────────────────┐
│ 📶✕ Çevrimdışısınız · görü- │  ← üst şerit, kapanmaz, ekranın en üstünde
│ nen tarifler önbellekten     │     sabit; bağlantı dönünce otomatik kaybolur
├─────────────────────────────┤
│  [normal tarif listesi/      │  ← altındaki içerik NORMAL render edilir,
│   detay ekranı, değişmeden]  │     sadece üstte şerit eklenir — ayrı bir
│                              │     "offline ekranı" YOK, mevcut ekranın
│                              │     üstüne binen bir durum
└─────────────────────────────┘
```

### Durum B — Önbellek boş (ilk açılış, hiç ağ görmemiş)
```
┌─────────────────────────────┐
│         📶✕                 │
│                              │
│   Bağlantı yok               │
│   Tarifleri görmek için      │
│   internete bağlanın.        │
│                              │
│      [Yeniden Dene]          │
└─────────────────────────────┘
```
Bu durum yalnızca **hiç veri önbelleklenmemişken** görünür — kural: "boş
önbellek + offline" ayrı bir durumdur, "önbellek var + offline" (Durum A)
ile karıştırılmaz. Bir kullanıcı bir kez online açtıysa bir daha asla bu
ekranı (Durum B) görmemeli.

### Durum C — "Talep Et" offline'da gönderilirse
Form gönderimi engellenmez; **yerel kuyruğa** yazılır ve buton metni
"Kaydedildi, bağlanınca gönderilecek"e döner (toast + buton state'i).
Bağlantı geri geldiğinde otomatik senkronize edilir, kullanıcıya ayrı bir
bildirim gerekmez (sessiz senkron) — form ekranından çıkmış olsa bile.
Gerekçe: "Talep Et" huninin dönüşüm noktası (bkz. bölüm 5); offline
olduğu için talebi kaybetmek, arz büyüme sinyalini kaybetmek demek.

---

## 3. AI Import Akışı

**Neden native'de farklı:** Kamera erişimi ve "çıkar → düzelt → kaydet"
üç adımlı bir akış web'de dosya-yükleme kadar basit değil; mobilde kamera
native bir yetenek ve kullanıcı beklentisi daha yüksek (anlık önizleme,
tekrar çekme). Yanlış yapmanın maliyeti: kullanıcı çektiği fotoğrafın
doğru okunup okunmadığını anlamadan kaydedip bozuk bir tarif biriktirir.

### Akış (4 ekran)

**3a. Kamera / Galeri seçimi**
```
┌─────────────────────────────┐
│ ✕  Tarif Ekle                │
│                              │
│   ┌───────────────────┐     │
│   │   📷 Fotoğraf Çek   │     │  ← kamera açılır (yazılı tarif sayfası/
│   └───────────────────┘     │     el yazısı — M6 kapsamı, bkz. P23-Mobile.md)
│   ┌───────────────────┐     │
│   │  🖼 Galeriden Seç    │     │
│   └───────────────────┘     │
│   ┌───────────────────┐     │
│   │  ✍️ Metin Yapıştır  │     │  ← mode='text', metin kutusu açar
│   └───────────────────┘     │
└─────────────────────────────┘
```

**3b. Çıkarım bekleme durumu**
```
┌─────────────────────────────┐
│  [çekilen/seçilen fotoğraf   │  ← kullanıcı ne gönderdiğini görür,
│   küçük önizleme, üstte]     │     "kayboldu" hissi olmaz
│                              │
│     ⏳ Tarif okunuyor…        │  ← `extract-recipe` çağrısı sürüyor
│                              │
│  [İptal]                     │
└─────────────────────────────┘
```
Süre beklentisi: gerçek edge function testinde (S20-A, `Build/E2E-QA.md`)
metin ~birkaç saniye, görsel (vision+OCR) daha uzun sürebilir — bu ekranda
sabit bir ilerleme çubuğu **kullanılmaz** (yanlış beklenti verir), belirsiz
("indeterminate") bir spinner kullanılır.

**3c. Düzelt/Onayla — kritik ekran**
Çıkarılan tarif **hiçbir alan zorunlu doğrulanmadan doğrudan kaydedilmez**
— kullanıcı her alanı görüp düzenleyebilir. `extraction_confidence` düşükse
(< 0.6, eşik M6'da netleşecek) ilgili alanın yanında bir uyarı rozeti
gösterilir ("emin değiliz, kontrol et"), alan bloklanmaz.
```
┌─────────────────────────────┐
│ ✕  Kontrol Et         Kaydet │
│                              │
│  Başlık: [Kekikli Fırın      │  ← her alan düzenlenebilir input
│           Domates        ]   │
│  Porsiyon: [4]  Süre: [55dk] │
│                              │
│  Malzemeler (5)          [+] │
│   • domates — 4 adet     [✎] │  ⚠️ eğer crop eşleşmediyse
│   • zeytinyağı — 2 yk     [✎] │     (bkz. not) nötr gösterilir,
│   • tuz — 1 çk            [✎] │     hata değil
│  ...                         │
│  Adımlar (4)              [+]│
│   1. "Fırını 200°C'ye..."[✎] │
│  ...                         │
└─────────────────────────────┘
```
**Kritik kural (şemadan doğrudan gelir, DB-Schema.md → "extract-recipe"):**
`recipe_ingredients.crop` bu akışta **daima NULL** gelir — malzeme→crop
eşleştirmesi editoryal, runtime'da fuzzy matching yapılmaz. Bu ekranda
malzeme satırları bu yüzden crop rozetsiz/nötr görünür; bu bir hata değil,
tasarlanmış davranıştır. Kullanıcıya "bu malzemeyi Hasat'ta ara" gibi bir
aksiyon **bu ekranda sunulmaz** (public korpustan farklı, kişisel defter
tarifi — malzeme eşleştirme M4/M9'un kapsamı).

**3d. Kaydedildi**
```
┌─────────────────────────────┐
│         ✅                   │
│   Defterine kaydedildi        │  ← owner_id=kullanıcı, visibility=
│                              │     'private', status='draft'
│   [Tarifi Gör]  [Kapat]      │     (sunucu zorluyor, client override
└─────────────────────────────┘     edemez — extract-recipe kontratı)
```
Bu tarif **asla otomatik public korpusa girmez** — "Kaydedildi" ekranında
"herkese aç" gibi bir kısayol **bilinçli olarak yok**; public'e taşımak
ayrı, editoryal bir işlem (bu akışın kapsamı dışında).

---

## 4. Alt Navigasyon

**Neden native'de farklı:** Web `BuyerShell`'in 5 slotu (bkz.
`Build/Shared-Architecture.md`) masaüstü/mobil-web genişliğinde tasarlandı;
native alt navigasyonda güvenli alan (iOS home indicator), dokunma hedefi
boyutu (min 44×44pt) ve tarif katmanının yeni bir birincil sekme olarak
eklenmesi web'deki 5 slotla birebir örtüşmüyor. Yanlış yapmanın maliyeti:
5 sekmeye sıkıştırılan biri çok küçük dokunma alanı → App Store review'da
"zor kullanılabilir" bulgusu riski, ya da tarif sekmesi hiç yer bulamaz.

### Web BuyerShell (referans, DEĞİŞMEYECEK — sadece karşılaştırma için)
`Keşfet · Taleplerim · Tarifler(M4) · Siparişlerim · Hesabım` — 5 slot.

### Mobil karar: 5 slot, ama farklı öncelik sırası ve ikon-öncelikli tasarım
```
┌───┬───┬───┬───┬───┐
│ 🏠 │ 📖 │ 🔍 │ 📦 │ 👤 │
│Ana│Tarif│Keşfet│Sipariş│Hesap│
└───┴───┴───┴───┴───┘
```
**Sıralama kararı:** Tarifler, Keşfet'in **solunda** — çünkü mobil v1'in
huni girişi tarif katmanı (bkz. `Build/P23-Mobile.md` → "Stratejik
çerçeve": "tarif → kayıt → talep → teklif → sipariş"). Web'de Keşfet
birincil giriş noktasıyken mobilde tarif katmanı önce geliyor; bu bilinçli
bir fark, hata değil. "Taleplerim" ayrı bir sekme **değil** — talepler
"Siparişlerim" sekmesi içinde bir alt-sekme olarak birleşti (mobilde 6.
slot açmamak için, iOS HIG önerisi de 5'i geçmemek yönünde).

**Native özel davranış:** Aktif sekme ikonu dolu (filled), pasifler
outline; seçili sekmeye tekrar dokunma o sekmenin kök ekranına döner
(scroll-to-top + stack reset) — Expo Router'ın native davranışı, ekstra
kod gerektirmiyor ama şartnamede beklenti olarak yazılı durmalı.

**Güvenli alan:** Alt nav yüksekliği + iOS home indicator boşluğu
`SafeAreaView` ile hesaba katılır; Android'de sistem gezinme çubuğu (gesture
nav) ile çakışmayacak şekilde alttan padding bırakılır.

**Checkout yok kararının navigasyona yansıması:** Sepet/ödeme ikonu **alt
navigasyonda hiç yer almaz** (bkz. `Build/P23-Mobile.md` → "Mobil v1
kapsam kararı") — bu, gelecekte checkout eklendiğinde 5 slotun yeniden
tasarlanması gerekeceği anlamına gelir; şimdiden yer ayırmak (boş/pasif
6. slot gibi) **yapılmadı**, YAGNI.

---

## 5. "Talep Et"

**Neden native'de farklı ve yanlış yapmanın maliyeti yüksek:** Bu, huninin
**dönüşüm noktası** (`Build/P23-Mobile.md` → "tarif → kayıt → talep →
teklif → sipariş"; `v_kpi_recipe_funnel`'ın ölçtüğü zincirin can alıcı
adımı). Web'de bu bir form sayfası; mobilde malzeme kartının içinden
tek dokunuşla açılan bir bottom-sheet olması sürtünmeyi düşürür — ama
sürtünmeyi çok düşürmek (örn. tek dokunuşla, onay istemeden gönderme)
yanlış/istemsiz talep riski taşır. Denge: **iki adım, sıfır zorunlu alan
dışında ekstra sürtünme yok.**

### Giriş noktası — malzeme kartı 3 durumu
Tarif detayında her malzeme satırı `rpc_recipe_availability`'nin
`is_matched` alanına göre üç görünümden birinde:
```
✅ domates — eşleşti          → "Ürüne Git"
🔵 safran — Hasat'ta yok       → "Talep Et"
⚪ tuz — platform dışı          → aksiyon yok (nötr, gri)
```

### Adım 1 — Talep formu (bottom sheet)
```
┌─────────────────────────────┐
│  ▁▁▁                         │  ← sürükleme tutamacı
│  Safran talep et             │
│                              │
│  Ne kadar istiyorsun?        │
│  [ 2  ] g                    │  ← recipe_ingredients'ten önerilen miktar
│                              │     ön-dolu (tarifin gerektirdiği miktar,
│                              │     kanonik birime çevrilmiş) — kullanıcı
│                              │     değiştirebilir
│  ☑ Bu ürün geldiğinde        │  ← price_alerts deseniyle aynı, varsayılan
│    haber ver                  │     işaretli (P23-Mobile.md: "talep butonu
│                              │     ölü form olmamalı" şartı)
│                              │
│      [Talebi Gönder]         │
└─────────────────────────────┘
```
**Neden ön-dolu miktar önemli:** Kullanıcı "safran" kelimesini görüp
formu boş bırakıp kapatma ihtimaline karşı — tarif zaten "2 g safran"
diyor, kullanıcıdan sıfırdan miktar hesaplamasını istemek gereksiz
sürtünme. Bu davranış web'de zaten var olan `crop_requests` akışının
mobile taşınmış hali, yeni bir mantık değil.

### Adım 2 — Onay + sonraki adım netliği
```
┌─────────────────────────────┐
│         ✅                   │
│   Talebin iletildi            │
│                              │
│   Safran'ı elinde olan        │  ← beklenti yönetimi ("hızlı teslimat
│   çiftçiler haberdar edildi.  │     değiliz" konumlandırması burada
│   Eşleşme olunca haber        │     da geçerli — bkz. P23-Mobile.md
│   vereceğiz.                  │     "Konumlandırma" bölümü)
│                              │
│      [Tarife Dön]            │
└─────────────────────────────┘
```
`recipe_rfq_links` bu adımda `crop_requests` satırına bağlanır (huni
atfının tek sert bağı, bkz. `Build/DB-Schema.md` → "P23-M2-ek") — bu
şartnamenin kapsamı değil ama ekranın "neden var olduğu" burada.

**Offline davranışı:** Bölüm 2 → Durum C ile aynı (kuyruğa alınır, sessiz
senkron). Ayrı bir offline varyantı bu bölümde tekrar çizilmedi.

---

## Kapsam dışı bırakılanlar — neden port yeterli

| Ekran | Gerekçe |
|---|---|
| Keşfet | Web'deki grid/kart deseni Nativewind ile doğrudan taşınabilir; native-özel bir etkileşim farkı yok |
| Ürün listesi / ürün detay | Aynı — çoklu-batch UI (P21) zaten kart tabanlı, mobilde aynı görsel dil çalışır |
| Siparişlerim | Liste + detay deseni; checkout yok kararı zaten sayfayı basitleştiriyor |
| Tarif listesi | Kart grid — Keşfet'le aynı desen, ayrı şartname gereksiz |

Bu dördü M7'de (`Build/P23-Mobile.md` → "Mobil marketplace köprüsü")
doğrudan web bileşenlerinden Nativewind çevirisiyle üretilecek.

---

## Değişiklik günlüğü
- 2026-07-30: İlk sürüm (P23-M3-D). 18 gerçek tarifin adım/timer/malzeme
  verisiyle kalibre edildi (bkz. `Build/P23-Mobile.md` → M3).
