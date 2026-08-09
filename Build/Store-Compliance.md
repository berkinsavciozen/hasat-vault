---
title: Hasat — App Store & Play Store Uyumluluk
updated: 2026-08-09
tags:
  - hasat
  - mobile
  - compliance
  - appstore
  - playstore
---

# App Store & Play Store Uyumluluk

> Kaynak doğrulaması: 2026-07-28 (Apple resmî dokümantasyonu + Play Console Help)
> Store politikaları değişebilir — submit öncesi (M8) yeniden kontrol edilmeli.

---

## 1. Hesap stratejisi

> ⚠️ **Tekrar vurgulanıyor (2026-07-31) — bu proje boyunca İKİ KEZ karıştırıldı:**
> **Apple Developer bireysel hesabı şirket tescilinden tamamen bağımsızdır.**
> Bu iki karar birbirine bağlı DEĞİL: (1) şirket kuruluşu (şahıs şirketi vs.
> Ltd. Şti., hâlâ açık, mali müşavir bekleniyor), (2) Apple Developer hesabı
> (bireysel, $99, D-U-N-S gerekmiyor, **2026-07-30/31'de başvuruldu** —
> başvuru şirket tescilini beklemedi ve beklemiyor). Şirket tescili gecikse
> bile Apple hesabı süreci etkilenmez; App Store'da satıcı adı sadece
> kişisel görünür (bkz. aşağıdaki tablo). Bu ayrımı her okuyan netleştirmeli
> — aşağıdaki bölümler bunu tekrar tekrar detaylandırıyor çünkü konu daha
> önce iki kez karıştı.

### Karar (2026-07-28): Apple bireysel hesap, ŞİMDİ

**Gerekçe:** Bireysel kayıt **D-U-N-S gerektirmiyor** ve şirket tescilinden tamamen bağımsız. Bu tek hamle Apple'ı kritik yoldan çıkarıyor.

| | Bireysel | Organizasyon |
|---|---|---|
| D-U-N-S | Gerekmiyor | Zorunlu (1–4 hafta) |
| Şirket şartı | Yok | **Tüzel kişilik** zorunlu |
| App Store satıcı adı | Kişisel yasal ad | Şirket adı ("Hasat") |
| Ücret | $99/yıl | $99/yıl |
| Ekip üyesi davet | Hayır | Evet |

**Hedef tarih:** 7–10 gün içinde açılmış olmak. **Güvenli son tarih: 15 Eylül 2026** (gerçek ihtiyaç ~28 Eylül, M6'da iOS push için APNs anahtarı).

**Durum (2026-07-31):** Başvuru yapıldı (Berkin, 2026-07-30/31) — şirketten
bağımsız olarak. Onay bekleniyor. Onay gelene kadar mobil doğrulama gerçek
cihaz yerine iOS Simulator + Appetize.io ile yapılıyor (bkz. `P23-Mobile.md`
→ "M5-a-ek" ve `Build/E2E-QA.md` → S25).

**Durum güncellemesi (2026-08-05/09, P23-M8-a):** Hesap **onaylandı**
(2026-08-05). Bundle ID `com.hasat.app` Apple Developer'da kayıtlı, **Push
Notifications capability açık**. App Store Connect'te uygulama oluşturuldu
— **satıcı/uygulama adı "Hasat-AI"** ("Hasat" alınmıştı, bkz. Bölüm 7 →
"'Hasat' adı App Store'da alınmış" riski, gerçekleşti). App Review
Information dolduruldu; reviewer test hesabının **girişi** (OTP ile oturum
açma) gerçek bir mobil build'de denendi ve çalıştı (Berkin raporu) — bu,
Bölüm 6'daki "Reviewer test hesabıyla uçtan uca gezinti" maddesinin
**yalnızca giriş kısmını** kapsıyor; pişirme modu/AI import/teklif
oluşturma dahil tam akış hâlâ ayrı bir madde (bkz. `Build/E2E-QA.md` →
S33, adım "Reviewer hesabıyla uçtan uca gezinti"). Gerçek cihaza dağıtım
altyapısı (bu bölümün devamı) ve APNs/FCM kurulum adımları P23-M8-a'da
eklendi — kod tarafı hazır, kredansiyel yükleme ve gerçek cihaz
doğrulaması Berkin'e kalıyor (kural #103).

**Pratik notlar:**
- Kayıt **iPhone üzerinden** (Apple Developer uygulaması, kimlik doğrulama süreç boyunca aynı cihazda kalmalı). Mac şirket bilgisayarı olduğu için telefon en temiz yol.
- Hesap açılınca App Store Connect'te **"Hasat" adının müsaitliğini hemen kontrol et** — alınmışsa alternatif için erken haber almak iyi.
- Hesap gelir gelmez **~1 saatlik doğrulama işi:** EAS'ın Apple kimlik bilgileriyle konuşup sertifika/provisioning üretebildiğini test et. Bunu M6'da değil, hemen yap.

### Gerçek cihaza dağıtım yolu — TestFlight (P23-M8-a, 2026-08-09)

**Karar: (b) TestFlight.** Görev metninde iki seçenek sunulmuştu; gerekçeli
değerlendirme aşağıda, görev talimatı gereği ("gerekçeli olarak birini
seç ve uygula") karar bu turda uygulandı — kural #107 kapsamında değil,
çünkü talimat açıkça karar vermeyi istedi ("bana sun, kendin karar verme"
değil, tam tersi).

| Kriter | (a) EAS internal distribution | (b) TestFlight |
|---|---|---|
| Cihaz kaydı | `eas device:create` — **interaktif terminal akışı** gerekiyor, GitHub Actions'tan tetiklenemez | Gerekmiyor |
| Kurulum karmaşıklığı | Berkin'in terminalinden `eas device:create` çalıştırması + telefonda bir sayfa açması + her yeni test cihazı için tekrarı gerekiyor | TestFlight uygulamasını telefona kurup davete/internal tester bağlantısına tıklamak yeterli |
| M8-d (submit) ile paylaşım | Ayrı bir build tipi (`distribution: internal`, ad-hoc imza) — submit için **yine ayrı bir store build'i** gerekecek, altyapı iki kez kurulur | Aynı yol (`distribution: store`, prodüksiyon imzası) — M8-d'de doğrudan submit edilecek build ile **aynı profil** |
| Build süresi/kota | Aynı EAS iOS kuyruğu, aynı 15/ay kota | Aynı |
| Apple hesabı zaten var (App Store Connect'te "Hasat-AI" oluşturuldu) | Kullanılmıyor — ad-hoc profil ayrı bir yol | Doğrudan kullanılıyor, ikinci bir kurulum yok |

**Sonuç:** (b) hem kurulumu daha az adımlı hem de M8-d'nin altyapısını
şimdiden kurmuş oluyor — orkestratörün eğilimiyle aynı sonuca varıldı, ama
gerekçe kurulum karmaşıklığı + altyapı tekrarı önleme üzerinden, sadece
"öneri buydu" değil. (a)'nın tek avantajı (build başına bekleme
gerektirmeden anlık kurulum) submit-sonrası App Store Connect işleme
süresiyle (genelde birkaç dakika–yarım saat) karşılaştırıldığında önemsiz.

**Uygulanan altyapı (`hasat-mobile`):**
- `eas.json` → yeni `ios-testflight` profili (`distribution: "store"`,
  `autoIncrement: true`) — mevcut `simulator` profili silinmedi.
- `eas.json` → yeni `android-device` profili (`distribution: "internal"`,
  `buildType: "apk"`) — Android'de UDID kaydı kavramı yok, doğrudan APK
  sideload ile gerçek cihaza kurulabiliyor; S33'ün Android FCM adımı için
  gerekli.
- `.github/workflows/eas-build-testflight.yml` (yeni, `eas-build-simulator.yml`
  bozulmadı) — yalnızca `workflow_dispatch`, aynı kota-koruma deseni.
  Yalnızca **build** yapıyor; **submit** ayrı, tek seferlik ve interaktif
  (aşağıda).
- `.github/workflows/eas-build-android-device.yml` (yeni) — aynı desen,
  Android APK.

**Berkin'in yapacakları — adım adım:**
1. GitHub → `hasat-mobile` reposu → **Actions** sekmesi → **"EAS TestFlight
   Build (iOS)"** workflow'u → **"Run workflow"** butonu → `main` dalıyla
   çalıştır.
2. Build bitince (kuyruk + derleme genelde 10–30 dk) workflow'un özet
   sayfasında (Actions → ilgili run → Summary) build linki görünür.
3. Kendi terminalinden (Berkin'in terminali var, bu adım interaktif olduğu
   için Actions'tan çalıştırılamıyor):
   ```
   cd hasat-mobile
   eas submit -p ios --profile ios-testflight --latest
   ```
   İlk çalıştırmada Apple ID (veya App Store Connect API key) ile giriş
   isteyecek — App Store Connect'te "Hasat-AI" uygulaması zaten var,
   bundle ID `com.hasat.app` ile otomatik eşleşmeli, ekstra bir uygulama
   oluşturmaya gerek yok.
4. App Store Connect → **TestFlight** sekmesi → build "İşleniyor"dan
   "Hazır"a geçince (birkaç dakika – yarım saat) kendi Apple hesabını
   **Internal Testing** grubuna ekle (App Store Connect'te zaten hesap
   sahibi/team member olduğun için otomatik uygun olman gerekiyor,
   ayrıca davet göndermene gerek kalmayabilir — ekranda "Test Bilgisi"
   doldurman istenebilir, iç test için App Review'a gitmez).
5. Telefonuna **TestFlight** uygulamasını App Store'dan kur, bildirimi/linki
   aç, "Yükle"ye bas.
6. Uygulama telefona kurulunca `Build/E2E-QA.md` → **S33**'ü koş.

**Android (FCM/genel gerçek cihaz testi için, opsiyonel — TestFlight'tan
bağımsız):**
1. GitHub → `hasat-mobile` → Actions → **"EAS Android Device Build (APK)"**
   → Run workflow.
2. Build bitince özet sayfasındaki APK linkini Android telefonda aç, indir,
   kur (bilinmeyen kaynak izni istenebilir).

**Bu oturumda doğrulanamayan kısım (kural #103):** Gerçek `eas build`/`eas
submit` çalıştırması — bu oturumun ağ politikası `expo.dev`'e erişimi
engelliyor (aynı kısıt M5-a-ek-2'den beri geçerli). Yalnızca YAML
sözdizimi (`PyYAML`) ve `eas.json`/`app.json` JSON geçerliliği + `tsc
--noEmit` bu oturumda doğrulandı, aşağıdaki tabloya bkz.

### APNs — kredansiyel yükleme (Key ID `246F7SPF74`, Team ID `XM562PFC7F`)

**Adım adım (Berkin, tarayıcıdan — interaktif, bu oturumda yapılamaz):**
1. [expo.dev](https://expo.dev) → hesabına giriş → **hasat-mobile** projesi
   → **Credentials** sekmesi.
2. **iOS** → **Push Notifications** → "Add a Push Notifications Key" (veya
   mevcut bir anahtarı bağla).
3. `.p8` dosyasını yükle, **Key ID**: `246F7SPF74`, **Team ID**:
   `XM562PFC7F` gir.
4. Kaydet — bundan sonra `ios-testflight` profiliyle yapılan her build bu
   anahtarı otomatik kullanır (build sırasında ayrıca bir şey seçmen
   gerekmiyor, EAS bundle ID + capability'den anahtarı otomatik eşliyor).

Bu adım **tarayıcı üzerinden** de yapılabiliyor — `eas credentials` CLI'ı
şart değil, M5-a-ek-2'de bulunan "terminal zorunlu değil" deseniyle
tutarlı (Berkin şirket Mac'inde CLI araç zincirini yönetemiyor).

**Sandbox/Production sorusu — doğrulandı:** Apple'ın token-tabanlı APNs
kimlik doğrulaması (`.p8` anahtar) **environment'a özel değildir** — aynı
anahtar hem `api.sandbox.push.apple.com` hem `api.push.apple.com`'a karşı
geçerli bir JWT üretir (bu, eski sertifika-tabanlı `.p12` modelinin
aksine, Apple'ın 2016'dan beri değişmeyen resmi token-tabanlı kimlik
doğrulama tasarımı). Hangi environment'ın kullanılacağını anahtar değil,
**uygulamanın imzalandığı provisioning profile**'daki `aps-environment`
entitlement'ı belirler: geliştirme profili → sandbox, dağıtım (App
Store/TestFlight) profili → production. `ios-testflight` profili
(`distribution: "store"`) EAS'ta prodüksiyon imzası ve dolayısıyla
`aps-environment: production` ile build alır — bu yüzden **ikinci bir
APNs anahtarına gerek yok**, aynı `246F7SPF74` hem sandbox (gelecekteki
development build'ler) hem production (TestFlight/App Store) için
kullanılabilir. Berkin'in gördüğü "Sandbox" etiketi muhtemelen henüz
hiçbir prodüksiyon-imzalı build yapılmamış olmasından kaynaklanıyor —
kodda bir eksiklik değil. **Bu, Apple'ın resmi, uzun süredir değişmeyen
platform davranışına dayanan bir çıkarım; gerçek bir prodüksiyon push
teslimatıyla bu oturumda doğrulanamadı (kural #103) — S33'te doğrulanacak.**

### Android FCM — Firebase kurulumu (Berkin, tarayıcıdan)

Firebase projesi henüz yok. Adım adım:
1. [Firebase Console](https://console.firebase.google.com) → **Add
   project** → isim (örn. "Hasat") → Google Analytics'i istersen kapat
   (gerekmiyor) → oluştur.
2. Proje içinde **Add app** → **Android** ikonu → paket adı olarak
   **birebir** `com.hasat.app` gir (bundle identifier ile aynı olmalı,
   uyuşmazsa push token alınamaz) → uygulama takma adı (opsiyonel) →
   kaydet.
3. **`google-services.json`**'ı indir → `hasat-mobile/google-services.json`
   olarak repoya ekle (kod tarafında `app.json` zaten bu yolu bekliyor —
   aşağıya bkz.). Bu dosya **gizli değil** — Google'ın resmi kılavuzuna
   göre client config dosyasıdır, `.env`'deki Supabase anon key kararıyla
   aynı gerekçeyle (`Build/P23-Mobile.md` → "M5-a-ek" → ".env içerik
   bekçisi") **bilinçli olarak public repoda** tutulacak; API key'in
   kendisi paket adı/imza kısıtlarıyla korunuyor.
4. Firebase Console → proje ayarları (⚙️) → **Service accounts** sekmesi →
   **Generate new private key** → indirilen JSON'ı **repoya EKLEME** (bu,
   `google-services.json`'ın aksine gerçek bir sunucu sırrı — FCM V1 API'yi
   Hasat adına kullanabilen bir servis hesabı anahtarı).
5. [expo.dev](https://expo.dev) → **hasat-mobile** → **Credentials** →
   **Android** → **FCM V1 Service Account Key** → adım 4'te indirilen
   JSON'ı yükle. (Bu da tarayıcı üzerinden, CLI gerekmiyor.)
6. `android-device` profiliyle build alındığında `google-services.json`
   (adım 3) uygulamaya gömülür, push gönderiminde de adım 5'teki servis
   hesabı kullanılır.

**Kod tarafında yapılan değişiklik:** `hasat-mobile/app.json` →
`expo.android.googleServicesFile: "./google-services.json"` eklendi.
Dosyanın kendisi henüz repoda **yok** (Berkin adım 3'ü yapana kadar) —
bu, yalnızca Android push testini etkiler; iOS build'leri ve mevcut
Android UI/uçak modu/timer testleri bu dosya olmadan da çalışır (alan
yalnızca Android build zamanında okunuyor, `tsc`/Metro'yu etkilemiyor —
`tsc --noEmit` bu turda temiz kaldığı doğrulandı).

**Bu oturumda doğrulanamayan kısım (kural #103):** Firebase projesi
oluşturma, `google-services.json`/servis hesabı anahtarı yükleme ve gerçek
FCM teslimatı — hepsi interaktif tarayıcı/hesap işlemleri, Berkin'e
kalıyor.

### ⚠️ Şahıs şirketi organizasyon hesabına uygun DEĞİL

Apple'ın resmî dokümanı: organizasyon kaydı için işletmenin **tüzel kişilik** (şirket, limited şirket vb.) olarak tanınması gerekiyor. **Şahıs şirketi / tek kişilik işletme statüsündekiler bireysel kaydolmak zorunda.** DBA, ticari unvan ve şube kabul edilmiyor.

Yani `TODO.md`'deki "şahıs şirketi kur" planı ile "App Store'da Hasat markası" hedefi **çelişiyor**. Ltd. Şti. seçilirse organizasyon hesabı mümkün.

Apple, bireysel üyelikten organizasyon üyeliğine geçişe izin veriyor — yani **şimdi bireysel açmak ileride organizasyona geçmeyi engellemiyor.**

### Google Play — karar M5'e ertelendi

| | Personal ($25) | Organization |
|---|---|---|
| D-U-N-S | Gerekmiyor | Zorunlu |
| **12 tester × 14 gün kapalı test** | **Zorunlu** (13 Kasım 2023 sonrası açılan personal hesaplar için) | **Muaf** |

12 tester bulmak Hasat için zor değil (ilk 100 kullanıcıya 6 ay premium veren bir ürün; çiftçi/alıcı ağı hazır). Ama uygulamayı sonradan personal'dan organization hesabına taşımak sancılı — bu yüzden karar M5'te, şirket durumu netleştiğinde verilecek.

**Personal seçilirse:** kapalı testi M5'te başlat, 14 gün M6/M7 boyunca paralel akar, kritik yola girmez.

---

## 2. Apple Guideline 4.2 — Minimum Functionality

**En yüksek red riski bu.** Reviewer cihazı **uçak moduna alıyor**; uygulama boş beyaz ekran veya tarayıcı hatası gösterirse doğrudan "web wrapper" olarak işaretleniyor.

Uyarıcı bir emsal: offline video indirme + push + favoriler eklenmiş bir uygulama yine de reddedilmiş; Apple'ın gerekçesi *"favorileri Safari'de de bookmark'layabilirler, yeterince özgün deneyim değil"* olmuş. **Yani zayıf native özellikler yetmiyor.**

### Hasat'ın 4.2 savunması

Tarif katmanı bu testi geçmek için **doğal ve güçlü** özellikler getiriyor — bunlar "bookmark" gibi zayıf değil, webde gerçekten yapılamayan şeyler:

| Özellik | Neden savunulabilir | Taş |
|---|---|---|
| **Offline tarif erişimi** | Uçak modu testinin doğrudan cevabı | M5 |
| **Pişirme modu** — adım adım, timer'lar, ekranı uyanık tutma | Cihaz donanımı, webde yok | M6 |
| **AI import — kamera ile tarif fotoğrafı** | Kamera + cihaz üzeri akış | M6 |
| **Push bildirimleri** | Sipariş/talep/sezon | M6 |

> **Not:** Salt marketplace wrapper'ı büyük ihtimalle reddedilirdi. Tarif katmanı native app'i mümkün kılan şeydir.

### Durum tablosu (2026-08-03, M6 sonrası) — kod hazır mı, doğrulandı mı

⚠️ **İki ayrı soru, karıştırılmamalı:** "kod yazıldı mı" ile "gerçek cihazda
çalıştığı görüldü mü" aynı şey değil. App Review notlarına yalnızca
**doğrulanmış** özellikler güvenle yazılabilir; aşağıdaki 🟡 satırlar submit
öncesi gerçek cihazda koşulmadan nota girmemeli.

| Native yetenek | Kod | Gerçek cihazda doğrulandı mı |
|---|---|---|
| Offline tarif listesi + detayı (`expo-sqlite`, 18/18 tarif arka planda önbelleğe alınıyor) | ✅ M5-b + M5-b-ek | 🟡 Hayır — simülatörde Wi-Fi kapatma yaklaşık test; **gerçek uçak modu yalnızca cihazda** |
| Pişirme modu (tam ekran, adım adım) | ✅ M6 | 🟡 Hayır — simülatör/cihaz yok |
| Timer (zaman-damgası tabanlı, arka planda doğru) | ✅ M6 | 🟡 Hayır — **arka plan doğruluğu tam olarak cihazda ölçülmeli** |
| Süre dolunca yerel bildirim | ✅ M6 | 🟡 Hayır — planlama kodu doğru, teslimat cihaz işi |
| Ekranı uyanık tutma (`expo-keep-awake`, yalnızca pişirme modunda) | ✅ M6 | 🟡 Hayır — kararmama VE çıkışta bırakılma (pil) ölçülmedi |
| AI import — **metin** girdisi | ✅ M6 | ✅ **Evet, uçtan uca gerçek `extract-recipe` çağrısıyla** (sunucu tarafı; ekrandaki akış cihazda koşulmadı) |
| AI import — **galeri** (yazılı tarif fotoğrafı, galeriden seçilerek) | ✅ M6 | ✅ **Evet — Appetize'da galeriden seçilen gerçek bir tarif fotoğrafıyla uçtan uca doğrulandı (Berkin, 2026-08-04)** |
| AI import — **kamera** (yazılı tarif fotoğrafı, canlı çekim) | ✅ M6 | 🔴 Hayır — `expo-image-picker`'ın kamera akışı simülatör/Appetize'da gerçek çekim üretmiyor, gerçek cihaz bekliyor |
| Push token kaydı + `device_tokens` devri | ✅ M6 (`rpc_register_device_token`) | ✅ DB tarafı gerçek SQL ile · 🔴 gerçek token/teslimat hayır (kredansiyel + cihaz yok) |
| Push **teslimatı** (Android FCM / iOS APNs) | 🟡 P23-M8-a'da altyapı hazırlandı, kredansiyel henüz yüklenmedi | 🔴 Hayır |
| Teklif oluşturma (`rpc_create_offer` — çoklu-parti, ürün/parti detay ekranı) | ✅ P23-M7-a | ✅ RPC gerçek SQL/RLS simülasyonuyla · 🟡 Ekrandaki akış (routing, miktar clamp, teslimat seçimi) cihazda koşulmadı |

**Kredansiyel engelleri (kod değil, hesap işi) — P23-M8-a durumu (2026-08-09):**
- **Android:** FCM V1 servis hesabı anahtarı + `google-services.json` — Firebase
  projesi henüz açılmadı. Apple'dan bağımsız, **şimdi yapılabilir.** Adım
  adım talimat + gerekli kod değişikliği (`app.json` →
  `googleServicesFile`) tamamlandı, bkz. yukarıda "Android FCM — Firebase
  kurulumu".
- **iOS:** APNs anahtarı **Berkin'de mevcut** (Key ID `246F7SPF74`, Team ID
  `XM562PFC7F`, `.p8` dosyası) — Apple hesabı 2026-08-05'te onaylandığı
  için artık engel değil, yalnızca **EAS'a yüklenmesi** kaldı (interaktif,
  tarayıcıdan — bkz. yukarıda "APNs — kredansiyel yükleme"). Sandbox/
  production için ayrı anahtar gerekmediği doğrulandı (aynı bölüm).
- **İkisi de** gerçek cihaza dağıtım altyapısı olmadan test edilemiyordu —
  bu artık çözüldü (TestFlight + Android APK build profilleri, yukarıda).
  Kalan iş tamamen Berkin'in kredansiyel yükleme + gerçek cihaz adımları.

### Submit sırasında App Review notlarına yazılacak liste (M8'de kopyalanacak)

> Aşağıdaki taslak İngilizce yazılacak; buradaki amaç hangi maddelerin
> gireceğini sabitlemek. **Bir madde ancak yukarıdaki tabloda gerçek cihazda
> doğrulandıktan sonra bu listeye alınır** — doğrulanmamış bir özelliği
> reviewer denerken çalışmazsa 4.2 riski azalmaz, artar.

1. **Offline recipe access** — uçak modunda uygulama açılıyor, 18 tarifin
   listesi VE daha önce hiç açılmamış bir tarifin adım+malzemeleri
   görünüyor (cihaz üzeri SQLite önbelleği).
2. **Cooking mode** — tam ekran adım adım pişirme; adım başına geri sayım;
   timer arka planda ve uygulama kapatılıp açıldıktan sonra da doğru
   (zaman damgası tabanlı); süre dolunca yerel bildirim.
3. **Screen keep-awake** — yalnızca pişirme modunda; çıkışta bırakılıyor.
4. **Camera-based recipe import** — yazılı bir tarifin (kitap sayfası, el
   yazısı) fotoğrafı çekilip cihazdan gönderiliyor, yapılandırılmış tarife
   çevriliyor, kullanıcı kaydetmeden önce her alanı düzeltebiliyor.
5. **Push notifications** — sipariş/talep/sezon bildirimleri.
6. **Bu uygulamada ödeme/checkout ekranı yoktur** (Guideline 2.1 + IAP
   tartışmasını baştan kapatan not) — akış "Talep Et" veya teklif
   oluşturmada bitiyor (P23-M7-a'dan sonra ikisi de native).
7. **Test hesabı** — reviewer'a **gerçek SMS ile değil**, Supabase Auth'un
   resmî "Test OTP" özelliğiyle tanımlı sabit bir telefon numarası +
   rastgele (tahmin edilemez) 6 haneli bir kod verilir (bkz. P23-M7-c,
   `TODO.md`, 2026-08-05). Gerekçe: Apple inceleme ekibi Türk numarasına
   SMS alamıyor ve mobil client gerçek `verifyOtp` akışını çağırıyor
   (sahte/mock bir kod kabul etmiyor, M5-a'da doğrulandı) — bu yüzden
   reviewer'ın giriş yapabilmesinin **tek** yolu, canlı Supabase Auth'un
   o numara için gerçekten sabit bir kodu kabul etmesidir.

   **⚠️ Teşhis düzeltmesi (P23-M7-d, 2026-08-05):** M5-a/M5-b'de "`123456`
   web'de çalışıyor, mobilde Supabase Auth'a çarpıyor" diye kayda geçmişti —
   bu web/mobil istemci ayrımı **yanlıştı**. Gerçek neden istemciden bağımsız:
   Supabase Auth'taki test-OTP ayarı zaten kuruluydu ama
   `SMS_TEST_OTP_VALID_UNTIL` 1 Ağustos 2026'da dolmuştu, bu yüzden hiçbir
   istemcide çalışmıyordu; Berkin 2026-08-05'te süreyi Eylül'e uzattı ve eski
   test numaraları (`905001234567`, `905009876543`) mobilde de çalıştı. Bu
   düzeltme M7-c'nin **sonucunu değiştirmiyor** — reviewer için ayrı,
   gerçek kullanıcı verisi taşımayan bir numara + zorunlu süre yönetimi
   (`SMS_TEST_OTP_VALID_UNTIL`) hâlâ doğru iş; yalnızca gerekçesi "mobil web'e
   göre farklı bir OTP mekanizması kullanıyor" değil, "tek bir paylaşılan
   Supabase Auth ayarının süresi sessizce dolabiliyor" oldu. Detay:
   `TODO.md` → "P23-M7-d" build log.

   **Reviewer demo hesabı kuruldu (buyer, izole, gerçek veri yok)** —
   isim, adres, birkaç kaydedilmiş tarif dahil, reviewer pişirme modu, AI
   import ve teklif oluşturma akışlarını gerçek veriyle deneyebilir (aksi
   halde 4.2 savunmasının tamamı hiç görülmeden inceleme biter). **Telefon
   ve OTP App Store Connect → App Review Information alanında tutulur, bu
   repoda saklanmaz** — hesap boş olsa da RLS altında gerçek yazma yetkisi
   var (bkz. Bölüm 7 — Bilinen riskler), bu yüzden çift bu public repoda
   duramaz. `SMS_TEST_OTP_VALID_UNTIL` ile zorunlu time-box + App Review
   onayından sonra dashboard'dan kaldırma: bkz. Bölüm 6 ve `TODO.md` →
   P23-M7-c açık maddeleri.
8. **Native offer creation** (P23-M7-a) — reviewer'ın "Sipariş Ver"e basıp
   Safari'ye atılmadığı, teklifin uygulama içinde (çoklu-parti, teslimat
   seçimi, teslim tarihi dahil) oluşturulduğu not — bu doğrudan 4.2
   savunmasının bir parçası (aşağıdaki "Web/mobil özellik ayrımı"na bkz.).

### Web/mobil özellik ayrımı — App Review notlarına girecek çerçeve (P23-M7-a)

4.2 savunmasının özeti App Review notlarına şu formda girecek: **"web
[X]'i listeliyor, [Y] yalnızca uygulamada çalışıyor."** Örnekler:

- Web tarifleri ve malzeme eşleşmesini listeliyor (SSR, SEO için); **çalışan
  timer ve ekranı uyanık tutan pişirme modu yalnızca uygulamada.**
- Web teklif oluşturmayı da destekliyor (aynı `rpc_create_offer`, kural
  #106); **ama teklif oluşturma artık mobilde de native** — reviewer 4.2
  testinde (uçak modu + "Sipariş Ver" akışı) bir web sayfasına
  düşürülmüyor, IAP'a da düşürülmüyor (3.1.3(e), Bölüm 4).
- Web'de pazarlığın devamı (karşı teklife cevap) ve sipariş takibi var;
  bunlar mobilde bu turda YOK, ilgili yerlerde "web'de devam et"
  yönlendirmesi olacak — **ama bu yönlendirmeler yalnızca satın alma
  SONRASI noktalarda** (bir teklif zaten oluşturulduktan sonra, çiftçi
  karşı teklif verdiğinde veya sipariş takibinde). Teklifin kendisini
  oluşturmak (huninin "satın alma" adımı) hiçbir zaman web'e düşmüyor —
  4.2 riski tam olarak bu adımda ("Sipariş Ver"e basınca Safari'ye
  atılmak) yoğunlaşırdı, P23-M7-a bunu kapattı. Uygulamanın kendi değeri
  (tarif + pişirme + teklif oluşturma) her zaman baskın kalıyor, web
  yönlendirmeleri asla ana huniyi kesmiyor.

**Submit sırasında:** App Review notlarına bu native özellikleri **açıkça listele.** 4.2 itirazlarında fark yaratıyor.

---

## 3. Guideline 2.1 — App Completeness

**Karar: mobil v1'de checkout ekranı YOK.** Çalışmayan/mock bir "Ödemeyi Tamamla" ekranı "tamamlanmamış uygulama" reddine yol açabilir. Akış "Talep Et" / teklif oluşturmada biter, ödeme web'e devredilir.

Yan fayda: ödeme altyapısı (P17-A/iyzico, şirkete bağlı) gecikirse **uygulama bloke olmaz.**

---

## 4. In-App Purchase (IAP)

| Satılan | IAP zorunlu mu | Karar |
|---|---|---|
| Fiziksel ürün (tarım ürünü) | Hayır — fiziksel mal muaf | Zaten mobil v1'de ödeme yok |
| Premium abonelik (dijital hizmet) | Evet — %15–30 kesinti | **Mobil v1'de premium SATILMAYACAK**, sadece web'de |

### 3.1.3(e) — fiziksel mal kuralı (P23-M7-a'da netleştirildi, 2026-08-04)

Apple Guideline 3.1.3(e) "Physical Goods and Services": kargoyla/elden teslim
edilen fiziksel bir malın (bu durumda: tarım ürünü) satışında **IAP
kullanmak yasaktır**, ödeme uygulama dışına (dış ödeme yöntemi, banka
transferi, iyzico vb.) çıkarılmak **zorundadır**. Bu, dijital içerik satan
uygulamalara uygulanan "anti-steering" kısıtının (3.1.3, kullanıcıyı uygulama
dışı ödemeye yönlendiren bir link/buton bile koyamama) **tam tersi** —
3.1.3(e) kapsamındaki uygulamalar için Apple dış ödemeye **yönlendirmeyi
serbest bırakıyor**, hatta pratikte bunu bekliyor.

**Hasat'a etkisi:** P23-M7-a'da mobile eklenen teklif oluşturma akışı bu
yüzden Guideline 2.1/3.1.3 riski taşımıyor — teklif oluşturmanın kendisi bir
ödeme değil (mobil v1'de checkout hâlâ yok, bkz. Bölüm 3), ama ileride
gerçek ödeme (P17-A/iyzico) mobile eklenirse bile bu IAP tartışmasını hiç
açmayacak: tarım ürünü fiziksel mal olduğu için IAP zaten muaf, dış ödemeye
(iyzico) yönlendirmek Apple'ın kendi kuralına uygun. Karıştırılmaması gereken
nokta: bu muafiyet yalnızca **fiziksel mal** (üretici→alıcı tarım ürünü)
içindir — premium abonelik (dijital hizmet) hâlâ IAP'a tabi, tablodaki karar
değişmedi.

---

## 5. Zorunlu teknik gereksinimler

| Gereksinim | Detay | Taş |
|---|---|---|
| **Uygulama içi hesap silme** | Apple zorunlu tutuyor. **Uygulandı (P26, 2026-08-04)** — `rpc_delete_own_account`, web + mobil aynı RPC'yi çağırıyor. DB/RPC seviyesinde gerçek insert + impersonation testiyle doğrulandı; gerçek tarayıcı/cihaz click-through'u kural #103 kısıtı yüzünden doğrulanamadı — bkz. `TODO.md` → "P26" | ✅ M7 (erken çekildi) |
| **Android 16 / API 36 hedefi** | 31 Ağustos 2026'dan itibaren yeni uygulama ve güncellemeler için zorunlu. Expo SDK sürümünün bunu desteklediği M0'da doğrulanmalı | M0 / M8 |
| **Gizlilik metni + veri beyanı** | `recipe_saves`, push token'ları, kamera erişimi → KVKK + store privacy label'ları | M7 |
| **Sign in with Apple** | Yalnızca üçüncü taraf sosyal login sunuluyorsa zorunlu. Hasat **telefon OTP** kullanıyor → muhtemelen muaf, submit öncesi teyit | M8 |
| **Ekran görüntüleri + açıklama** | Tüm gerekli cihaz boyutları | M7 |
| **App Review notları** | Native özelliklerin açık listesi (4.2 savunması) + test hesabı bilgisi | M8 |

---

## 6. Submit öncesi kontrol listesi (M8)

> **[2026-08-05 eklendi]** Cihaz-bağımlı testlerin ayrıntılı/genişletilebilir listesi
> `TODO.md` → "🍎 Apple hesabı gelince koşulacak testler" başlığında tutulur (bu
> doküman değil — isim burada geçse de liste orada yaşıyor); aşağıdaki kontrol
> listesi submit-günü özetidir. M9 konsolidasyon taramasında (2026-08-05) her iki
> liste karşılaştırıldı ve iki eksik bulunup her ikisine de eklendi: prefetch
> atlama davranışı, native picker/modal/Linking akışları.

- [ ] Uçak modu testi: uygulama açılıyor, kaydedilmiş tarifler görünüyor (**daha önce hiç açılmamış bir tarifin adımları da** — M5-b-ek'in bulk prefetch'i)
- [ ] Pişirme modu + timer gerçek cihazda çalışıyor (**timer arka planda doğru sayıyor**, ekran kararmıyor, çıkışta keep-awake bırakılıyor)
- [ ] AI import (metin + fotoğraf) gerçek cihazda çalışıyor (metin yolu M6'da sunucu tarafında doğrulandı; **kamera yolu cihaz bekliyor**)
- [ ] Push bildirimi gerçek cihaza ulaşıyor (iOS + Android) — **önce FCM V1 anahtarı (Android) ve APNs anahtarı (iOS) EAS'a yüklenmeli** (adım adım: yukarıda "APNs — kredansiyel yükleme" ve "Android FCM — Firebase kurulumu", P23-M8-a); dağıtım altyapısı (TestFlight + Android APK build profilleri) hazır, `Build/E2E-QA.md` → S33'te koşulacak
- [ ] App Review notları listesi yalnızca gerçek cihazda doğrulanmış maddelerden oluşuyor (bkz. bölüm 2 → "Durum tablosu")
- [x] Uygulama içi hesap silme çalışıyor — DB/RPC seviyesinde doğrulandı (P26, 2026-08-04); gerçek tarayıcı/cihaz click-through'u submit öncesi Berkin tarafından yapılmalı (kural #103)
- [ ] **[2026-08-05 eklendi]** Hesap silme — gerçek cihaz/tarayıcı click-through ("HESABIMI SİL" yazma onayı dahil) ayrı, açık bir madde olarak koşuldu (yukarıdaki [x] yalnızca DB/RPC seviyesini kapsıyor)
- [ ] **[2026-08-05 eklendi]** Prefetch atlama davranışı gerçek cihazda doğrulandı (`cached_recipe_detail_meta` — önbellek tam ve 24 saatten yeniyse tarama başlamıyor)
- [ ] **[2026-08-05 eklendi]** Native picker/modal/Linking akışları (`CropPickerModal`, teslim tarihi preset chip'leri, web/native yönlendirme geçişleri) gerçek cihazda denendi
- [ ] Hiçbir yerde ödeme/checkout ekranı yok
- [ ] Gizlilik metni yayında ve uygulamadan erişilebilir
- [ ] API 36 hedefleniyor
- [ ] Test hesabı (telefon + OTP) review notlarında
- [ ] **[P23-M7-c, 2026-08-05]** Reviewer test hesabıyla (test telefon
      numarası + rastgele OTP, App Store Connect → App Review Information'da)
      **gerçek bir mobil build'de** uçtan uca giriş denendi — pişirme modu,
      AI import, teklif oluşturma dahil (Berkin, submit gününden ÖNCE; bkz.
      `TODO.md` → P23-M7-c açık madde 1)
- [ ] **[P23-M7-c, 2026-08-05, ZORUNLU]** Apple onayından **sonra** Supabase
      Dashboard'daki test-OTP satırı kaldırıldı (`SMS_TEST_OTP_VALID_UNTIL`
      bir yedek, elle kaldırma unutulmamalı; bkz. `TODO.md` → P23-M7-c açık
      madde 2)
- [ ] **[P23-M7-d, 2026-08-05]** `SMS_TEST_OTP_VALID_UNTIL` süresi her
      uzatıldığında yeni bitiş tarihinden ~1 hafta önceye bir takvim
      hatırlatıcısı konuldu — süre sessizce dolarsa test hesapları (web +
      mobil, istemciden bağımsız) hata mesajı vermeden "kod hatalı/süresi
      dolmuş" gibi genel bir mesajla kilitleniyor; bkz. `TODO.md` →
      "P23-M7-d" madde 6
- [ ] **[P23-M7-d, 2026-08-05]** Lovable Cloud'a `SUPABASE_URL` +
      `SUPABASE_SERVICE_ROLE_KEY` ortam değişkenleri eklendi — eksik olduğu
      sürece yeni alıcı web onboarding'i "Premium deneme başlatılamadı"
      hatası gösteriyor (kayıt kırık değil, ama 25 Ağustos lansmanında ilk
      saniyede kırmızı hata; bkz. `TODO.md` → "P23-M7-d" madde 7)
- [ ] Native özellik listesi review notlarında
- [ ] Store politikaları yeniden kontrol edildi (bu doküman güncel mi?)

---

## 7. Bilinen riskler

| Risk | Etki | Azaltma |
|---|---|---|
| 4.2 reddi | 1–2 hafta gecikme | Native özellik paketi + açık review notları; red gelirse itiraz + eksik özellik ekleme |
| "Hasat" adı App Store'da alınmış | Marka karmaşası | Hesap açılır açılmaz kontrol et (M0) |
| Play personal → 12 tester bulunamaz | Production gecikir | M5'te başlat, 14 gün paralel akar; tester ağı hazır |
| Apple hesap doğrulaması takılır | Push + submit gecikir | 7–10 gün içinde başvur, ~7 hafta tampon var |
| Şahıs şirketi ile organizasyon hesabı alınamaz | Satıcı adı kişisel görünür | Kabul edilebilir; ileride Ltd. Şti. ile dönüşüm mümkün |
| **[P23-M7-c, 2026-08-05] Reviewer mobil test girişi yapamıyor** (Türk numarasına SMS ulaşmıyor, sahte OTP mobilde çalışmıyor — M5-a'da doğrulandı) | **Kesin red** — pişirme modu, AI import, teklif oluşturma (4.2 savunmasının tamamı) hiç görülmeden inceleme biter | Supabase Auth test telefon numarası + rastgele (tahmin edilemez) OTP (yalnızca bu numarayı etkiliyor, gerçek kullanıcıların SMS akışı değişmiyor) + arkasında dolu bir demo hesap + submit öncesi gerçek mobil build'de doğrulama (Bölüm 6). Detay ve doğrulama: `TODO.md` → P23-M7-c |
| **[P23-M7-c, 2026-08-05] Reviewer demo hesabının RLS altında gerçek yazma yetkisi var** (boş olması onu zararsız yapmıyor — telefon+OTP ele geçerse gerçek bir `buyer` gibi işlem yapılabilir) | Sahte teklif → gerçek bir çiftçiye **gerçek Twilio SMS** gider (maliyet + çiftçi güveni); sahte talep admin ısı haritasını (`v_kpi_crop_demand_heatmap`) kirletir; `ai_usage_tracking` kotası tüketilebilir | Tahmin edilemez rastgele 6 haneli OTP + telefon/OTP bu repoda **tutulmuyor** (App Store Connect → App Review Information'da) + zorunlu time-box (`SMS_TEST_OTP_VALID_UNTIL`) + Apple onayından sonra test-OTP ayarının dashboard'dan kaldırılması (Bölüm 6) |
