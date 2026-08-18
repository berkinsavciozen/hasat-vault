# F14 — Sipariş/Teklif Akışı QA Senaryoları (kural #104 formatı)

> İki ayrı state machine var: `offers.status` (müzakere) ve `orders.status`
> (teslimat, `orders` yalnızca teklif kabul edilince oluşur). Her senaryo hem
> çiftçi hem buyer tarafında, gerçek cihaz/tarayıcıda, F3'ün event tablosuna göre
> hangi bildirimin gitmesi gerektiğini de kontrol ediyor. **Gerçek push/SMS
> teslimatı bu oturumdan doğrulanamaz (kural #103)** — Berkin'in kendi
> hesabı/telefonuyla koşması gerekiyor.

## Bölüm A — `offers.status` (müzakere fazı)

**S-F14-1 — Yeni teklif → çiftçi bildirimi**
1. Buyer hesabıyla bir ilana teklif gönder (miktar+fiyat gir, "Teklif Gönder").
2. Çiftçi hesabıyla (aynı anda ikinci cihaz/oturum) uygulamayı aç.
3. Beklenen: çiftçinin bildirim zilinde "Yeni Teklif" görünür; çiftçinin telefonuna
   push gelir; çiftçinin telefonuna SMS gelir (üçü de `notif_prefs`'te açıksa).
4. Beklenen: buyer tarafında teklif "Beklemede" durumunda görünür.

**S-F14-2 — Çiftçi kabul eder → buyer bildirimi**
1. Çiftçi hesabıyla bekleyen teklifi "Kabul Et".
2. Beklenen: buyer'a "Teklifiniz Kabul Edildi" — in-app + push + SMS.
3. Beklenen: `orders` tablosunda yeni bir satır oluşur, `status='preparing'`.
4. Beklenen (F3 düzeltmesinden sonra): buyer'a ayrıca `order_preparing` bildirimi
   de gider (anlamlı gövdeyle, boş değil) — bu yeni, önceden hiç gitmiyordu.

**S-F14-3 — Çiftçi karşı teklif yapar → buyer bildirimi**
1. Çiftçi hesabıyla teklife farklı fiyat/miktarla "Karşı Teklif Yap".
2. Beklenen: buyer'a "Karşı Teklif" — in-app + push. **SMS gitmeyecek (bilinçli, F3'te
   açık bırakıldı)** — bunu bir hata sanıp rapor etme, bilinen bir durum.
3. Buyer karşı teklifi kabul eder → Beklenen: çiftçiye "Teklifiniz Kabul Edildi" —
   in-app + push + SMS, `orders` oluşur.

**S-F14-4 — Çiftçi reddeder → buyer bildirimi (F3 düzeltmesinden sonra yeni davranış)**
1. Çiftçi hesabıyla bekleyen teklifi "Reddet".
2. **Düzeltme öncesi beklenen (eski davranış, referans için):** yalnızca in-app,
   push/SMS yok.
3. **Düzeltme sonrası beklenen (bu turun hedefi):** buyer'a "Teklif Reddedildi" —
   in-app + push + SMS.
4. Bu senaryoyu migration round 2 merge olmadan ÖNCE bir kez, merge olduktan
   SONRA bir kez koş — davranış farkını gözle teyit et.

**S-F14-5 — Ödeme onaylanır → çiftçi bildirimi**
1. Buyer ödemeyi tamamlar (`offers.payment_status`→`paid`).
2. Beklenen: çiftçiye "Ödeme Alındı" — in-app + push + SMS.

## Bölüm B — `orders.status` (teslimat fazı)

**S-F14-6 — Kargoya verildi**
1. Çiftçi hesabıyla siparişi "Kargoya Verildi" işaretle (varsa kargo firması +
   takip no gir).
2. Beklenen: buyer'a in-app + push + SMS, gövdede kargo firması/takip no varsa görünür.

**S-F14-7 — Teslim edildi**
1. Sipariş "Teslim Edildi" olarak işaretlenir (çiftçi ya da otomatik).
2. Beklenen: buyer'a in-app + push + SMS ("itiraz penceresi" notuyla).

**S-F14-8 — İptal (her iki taraftan da dene)**
1. Buyer siparişi iptal eder (iptal nedeni gir).
2. Beklenen: **hem buyer hem çiftçiye** in-app + push + SMS.
3. Aynı senaryoyu çiftçi tarafından başlatarak da tekrarla — sonuç aynı olmalı.

**S-F14-9 — İhtilaf açılır**
1. Buyer ya da çiftçi bir siparişte "İhtilaf Aç".
2. Beklenen: **her iki tarafa da** in-app + push + SMS.

**S-F14-10 — Tamamlandı (F3 düzeltmesinden sonra yeni davranış)**
1. Sipariş `completed` durumuna geçer.
2. **Düzeltme öncesi:** yalnızca boş gövdeli in-app kaydı.
3. **Düzeltme sonrası:** buyer'a anlamlı gövdeyle in-app + push + SMS.

## Bölüm C — Abonelik (harvest_time / subscription_*)

**S-F14-11 — Abonelik talebi → kabul → red, iki farklı iptal yolu**
1. Buyer çiftçiden düzenli tedarik talep eder → çiftçiye in-app+push+SMS
   (`subscription_new`).
2. Çiftçi kabul eder → buyer'a in-app+push+SMS (`subscription_accepted`).
3. Ayrı bir abonelikte: çiftçi **kendi hesabından, kendi oturumuyla** talebi
   reddeder → buyer'a in-app+push+SMS (`subscription_rejected`) gitmeli.
4. ⚠️ **Bilinen kenar durum:** iptal çiftçinin kendi oturumu dışında bir yoldan
   (ör. bir arka-uç/admin işlemiyle) yapılırsa `auth.uid() = farmer_id` şartı
   sağlanmadığı için buyer bildirim ALMAZ — bu bir hata değil, DB fonksiyonunun
   mevcut davranışı; test ederken hangi yoldan iptal ettiğini not et.
5. Hasat tarihine 3 gün kala (cron, günlük 07:00 UTC) — hem çiftçi hem buyer'a
   "Hasat Yaklaşıyor" gitmeli; bunu gerçek zamanlı test etmek zor, en pratik yol
   Supabase'de bir abonelik satırının `next_harvest_date`'ini bugün+3'e manuel
   set edip fonksiyonu (`send_subscription_harvest_reminders()`) elle çağırmak.

## Bölüm D — Bildirim tercihleri kapatıldığında

**S-F14-12 — Toggle kapalıyken bildirim GİTMEMELİ**
1. Herhangi bir kullanıcının `notif_prefs`'inde `new_offer_push`'ı kapat.
2. O kullanıcıya bir teklif gönder.
3. Beklenen: in-app kayıt yine oluşur (`notifications` tablosu her zaman yazılır),
   ama push GİTMEMELİ. Aynısını SMS için de dene.
4. Bu, "toggle gerçekten işe yarıyor mu" sorusunun tek somut testi — F4'ün UI'ı
   doğru görünse bile arkadaki toggle çalışmıyorsa fark edilmez.

---

# F15 — Konsolide "Daha Önce Doğrulanamayan" Checklist'i

> M5-M8 boyunca `TODO.md`'de dağınık biriken, kural #103 gereği bu oturumdan
> (ağ politikası / simülatör-cihaz yokluğu) doğrulanamayan maddelerin tek listesi.
> Her biri **kod seviyesinde** doğrulandı/mantığı okundu — yalnızca gerçek çalışma
> zamanı davranışı Berkin'in kendi cihazı/tarayıcısıyla teyit edilmeli. Gruplandı,
> tekilleştirildi (aynı madde build log'larında onlarca kez tekrar geçiyordu).

## 1. Mobil — offline / önbellek
- Uçak modunda uygulamayı aç → tarif listesi görünüyor mu (Apple 4.2'nin asıl testi)?
- Uçak modunda **daha önce hiç açılmamış** bir tarife dokun → adımlar+malzemeler
  görünüyor mu (bulk `expo-sqlite` prefetch'in kanıtı)?
- Prefetch atlama davranışı: önbellek dolu ve 24 saatten yeniyse tarama gerçekten
  atlanıyor mu?
- Yetim `cached_recipe_steps`/`cached_recipe_ingredients` satırları gerçekten
  temizleniyor mu (liste yeniden çekilince)?

## 2. Mobil — pişirme modu native davranışı
- Timer arka plana alınıp/uygulama kapatılıp açıldıktan sonra doğru zamanı
  gösteriyor mu (zaman damgası tabanlı, tick değil)?
- `expo-keep-awake` ekranı gerçekten açık tutuyor mu?
- Süre dolunca yerel bildirim + (ön plandaysa) titreşim geliyor mu?
- "Devam Et" (kaldığı yerden devam) gerçek cihazda doğru adıma dönüyor mu?

## 3. Mobil — AI import (kamera yolu)
- Yazılı tarif fotoğrafı çekip AI çıkarımı çalıştırma uçtan uca (kamera izni →
  çekim → `extract-recipe` çağrısı → düzenlenebilir sonuç) gerçek cihazda çalışıyor mu?
- Düşük güven (`extraction_confidence < 0.6`) uyarısı gerçekten görünüyor mu?
- Kota aşımı mesajı anlaşılır şekilde gösteriliyor mu?

## 4. Push/SMS/OTP gerçek teslimat
- Expo'dan `status:"ok"` dönen push'lar gerçekten Berkin'in telefonuna ulaşıyor mu?
- Bildirime dokununca doğru ekrana yönlendiriyor mu?
- OTP autofill (`textContentType="oneTimeCode"`) gerçek cihazda otomatik dolduruyor mu
  (yalnızca ilk haneyi değil, tamamını)?
- Test-OTP ayarı uygulandıktan sonra **listelenmeyen** numaralar hâlâ gerçek SMS
  akışında mı (Supabase'in dokümante davranışı, bu projenin panosunda gözle
  teyit edilmedi)?
- APNs anahtarının EAS'a fiilen yüklendiği + Android FCM/`google-services.json`
  kurulumunun tamamlandığı (F16 ile aynı madde, burada da referans).

## 5. Native UI / navigasyon
- Native picker/modal'larda klavye içeriğin altında kalmıyor mu (T4'te düzeltildiği
  iddia edildi — `KeyboardAvoidingScreen`'e taşınan 7 ekranın hepsinde tek tek kontrol
  edilmeli)?
- Sipariş durumu değişince mobil ekranda gerçek zamanlı/yenilemeyle görünüyor mu?
- Bildirim zili → doğru ekrana yönlendirme (F10'un yeni mobil bell'i geldiğinde
  ayrıca test edilmeli).

## 6. Web — tarayıcı click-through (bu oturumun ağ politikası engelliyor)
- Gerçek tarayıcıda OTP girişiyle giriş (web + mobil).
- "Çıkış" ve hesap silme sonrası oturum temizliği — fatal hata var mı?
- Boş durum ekranlarının CTA'ları (Keşfet kategori filtreleri dahil).
- Teklif oluşturma uçtan uca (buyer tarayıcıdan).
- Banned/oturum-sonlandı döngüsü — saatler süren token expiry senaryosu.

## 7. CI/araç (kod değil ama build'i etkiliyor)
- `hasat-mobile`'da `eslint` bu oturumun ağ politikası config indirmesini
  engellediği için hiç çalıştırılamadı — Berkin'in kendi ortamında/CI'da
  bir kez çalıştırıp temiz olduğunu teyit etmesi gerekiyor.
- Gerçek `eas build`/`eas submit` çalıştırması (Actions'tan "Run workflow").

## 8. Kapsamlı referans
- `Build/E2E-QA.md` → S26, S27, S28, S29, S31, S33 senaryolarının tamamı (49 adımlık
  S33 dahil) hâlâ gerçek cihazda hiç koşulmadı — bu checklist onların özet/gruplanmış
  hâli, ayrıntılı adım adım metin için oradaki senaryo numaralarına bakılmalı.

**Öneri (Berkin'e):** Bu checklist'i tek oturumda değil, cihaza göre grupla —
(a) iOS gerçek cihaz oturumu (Bölüm 1-2-3-5'in çoğu), (b) Android gerçek cihaz
oturumu (FCM + aynı testler tekrar), (c) tarayıcı oturumu (Bölüm 6), (d) CI/Actions
oturumu (Bölüm 7). M8-b/M8-c'nin 27-31 Ağustos penceresine bu dört ayrı oturum
olarak plan yapmak, hepsini tek seferde "bitirmeye çalışmaktan" daha gerçekçi.
