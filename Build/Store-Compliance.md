---
title: Hasat — App Store & Play Store Uyumluluk
updated: 2026-07-31
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

**Pratik notlar:**
- Kayıt **iPhone üzerinden** (Apple Developer uygulaması, kimlik doğrulama süreç boyunca aynı cihazda kalmalı). Mac şirket bilgisayarı olduğu için telefon en temiz yol.
- Hesap açılınca App Store Connect'te **"Hasat" adının müsaitliğini hemen kontrol et** — alınmışsa alternatif için erken haber almak iyi.
- Hesap gelir gelmez **~1 saatlik doğrulama işi:** EAS'ın Apple kimlik bilgileriyle konuşup sertifika/provisioning üretebildiğini test et. Bunu M6'da değil, hemen yap.

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

---

## 5. Zorunlu teknik gereksinimler

| Gereksinim | Detay | Taş |
|---|---|---|
| **Uygulama içi hesap silme** | Apple zorunlu tutuyor. Mevcut web akışında var mı — **doğrulanmalı** | M7 |
| **Android 16 / API 36 hedefi** | 31 Ağustos 2026'dan itibaren yeni uygulama ve güncellemeler için zorunlu. Expo SDK sürümünün bunu desteklediği M0'da doğrulanmalı | M0 / M8 |
| **Gizlilik metni + veri beyanı** | `recipe_saves`, push token'ları, kamera erişimi → KVKK + store privacy label'ları | M7 |
| **Sign in with Apple** | Yalnızca üçüncü taraf sosyal login sunuluyorsa zorunlu. Hasat **telefon OTP** kullanıyor → muhtemelen muaf, submit öncesi teyit | M8 |
| **Ekran görüntüleri + açıklama** | Tüm gerekli cihaz boyutları | M7 |
| **App Review notları** | Native özelliklerin açık listesi (4.2 savunması) + test hesabı bilgisi | M8 |

---

## 6. Submit öncesi kontrol listesi (M8)

- [ ] Uçak modu testi: uygulama açılıyor, kaydedilmiş tarifler görünüyor
- [ ] Pişirme modu + timer gerçek cihazda çalışıyor
- [ ] AI import (metin + fotoğraf) gerçek cihazda çalışıyor
- [ ] Push bildirimi gerçek cihaza ulaşıyor (iOS + Android)
- [ ] Uygulama içi hesap silme çalışıyor
- [ ] Hiçbir yerde ödeme/checkout ekranı yok
- [ ] Gizlilik metni yayında ve uygulamadan erişilebilir
- [ ] API 36 hedefleniyor
- [ ] Test hesabı (telefon + OTP) review notlarında
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
