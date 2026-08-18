---
title: Hasat — Lansman Planı
updated: 2026-08-10
tags:
  - hasat
  - launch
  - p23
  - roadmap
---

# Lansman Planı — 25 Ağustos 2026 (web marketplace)

> **⚠️ 2026-08-18 güncellemesi — kritik yol değişti.** Web marketplace
> ve mobil (P23) lansmanı artık AYRI takvimlerde değil, **birlikte**
> lansmanıyor. Yeni hedef: App Store + Play **submit 1-15 Eylül
> 2026**, store canlı tahmini Eylül sonu. Önceki "web 25 Ağustos"
> kritik yolu geçersiz. Gerekçe, yeni özellik kapsamı ve tam
> önceliklendirme: `Build/Launch-Scope-Plan.md`. Bu değişikliğin bu
> dosyadaki (§1 Epic tablosu) ve `Build/Roadmap.md`'deki tüm eski
> tarihlere etkisini gözden geçir — bazı Epic satırları hâlâ 25
> Ağustos'a göre yazılmış, `Launch-Scope-Plan.md` §5'teki yeni sırayla
> tutarlı hâle getir ya da en azından "bayat, yeni takvime bkz." notu
> düş (sessizce üzerine yazma, kural #107).

> **Kritik yol (eski, 2026-08-06'dan — artık geçersiz, bkz. yukarıdaki
> 2026-08-18 banner'ı):** ~~web marketplace, 25 Ağustos 2026. Kapsam
> web'dir — mobil (M8) Ekim'i hedefliyor ama lansman kritik yolunun
> üzerinde değil (bkz. `Build/P23-Mobile.md` → "Şirket gecikirse ne
> olur").~~
> Bu doküman 2026-08-06'da oluşturuldu. Bağlam: Apple Developer bireysel
> hesabı 2026-08-05'te onaylandı; şirket tescili henüz yapılmadı (hedef 7
> Ağustos).
> Bu doküman `Build/Roadmap.md`, `Build/P23-Mobile.md`, `Build/Store-Compliance.md`
> ve `TODO.md` ile bağlayıcıdır — çelişki bulunursa burada değil, kaynağında
> not düşülür (bkz. bölüm 3 ve `Build/Roadmap.md` → "⏱️ 2026-08-06 güncellemesi").

**Bu tur da yalnızca doküman değişikliği** — bu dosya dışında `Build/Roadmap.md`
(Gantt + kilometre taşı tablosu, aşağıda gerekçesi açıklanan bir çelişki
nedeniyle) güncellendi; hiçbir kod/DB/migration/edge function değiştirilmedi.

---

## 1. Epic tablosu — lansman öncesi (25 Ağustos'a kadar)

> ⚠️ **Bayat, yeni takvime bkz. (2026-08-18 güncellemesi).** Bu tablodaki
> "25 Ağustos'a kadar" çerçevesi ve bazı deadline'lar (özellikle E7 —
> "Lansman günü izleme planı" 23 Ağu, "İlk 100 kullanıcı kampanyası" 19
> Ağu) artık web-özel 25 Ağustos kritik yoluna göre yazılmış — yeni
> birleşik takvimde bu işler 1-15 Eylül submit penceresine göre yeniden
> sıralanmalı. Bu tablo kural #107 gereği sessizce üzerine yazılmadı;
> Berkin/Claude Code bir sonraki turda bu satırları `Launch-Scope-Plan.md`
> §5'teki yeni sırayla (Bölüm 2, yukarıda) hizalamalı. E1 (görsel
> varlıklar) F1 ile örtüşüyor — F1 zaten yapıldı (bkz.
> `Launch-Scope-Plan.md` F1), bu tablodaki E1 satırları da bayat.

| Epic | Task | Sahip | Deadline | Durum |
|---|---|---|---|---|
| **E1 — Görsel varlıklar** | 14 crop temsili görseli | 👤 Berkin | 14 Ağu | 🔴 Başlanmadı — canlı durum **0/70 crop**'ta görsel var |
| E1 | 18 tarif kapağı — **SEO için işlevsel**: Google Recipe şemasında `image` zorunlu | 👤 Berkin | 14 Ağu | 🔴 Başlanmadı — **0/18 tarif**te kapak var |
| E1 | Bucket'a yükleme + `default_photo_url` bağlama | 🤖 Claude Code | 15 Ağu | ⬜ Görsellere bağlı (14 Ağu'dan sonra başlar) |
| E1 | "Temsili görsel" etiketi doğrulaması | 🎯 Orkestratör | 16 Ağu | ⬜ Yüklemeye bağlı |
| — | *Canlı durum notu* | — | — | **0/70 crop, 0/18 tarif, 0/17 ilan fotoğrafı** — sistemde tek görsel yok |
| **E2 — Misafir/SEO** | Search Console kaydı + sitemap submit | 👤 Berkin | 16 Ağu | ⬜ Planlandı |
| E2 | Rich Results Test (Google) | 👤 Berkin | 17 Ağu | ⬜ Planlandı — görsellere bağlı (E1 tamamlanmadan Recipe şeması eksik kalır) |
| E2 | Landing sayfası lansman mesajı | 🤖 Claude Code | 21 Ağu | ⬜ Planlandı |
| **E3 — Alıcı akışı** | Uçtan uca denetim | 🎯 Orkestratör | 17 Ağu | ⬜ Planlandı |
| E3 | Boş durum ekranları | 🤖 Claude Code | 18 Ağu | ✅ **Yapıldı (2026-08-17, P23-M8-c2/T3)** — üç gerçek bulgu (ikisi web `notFoundComponent`, biri mobil), üçü de "Talep Et" CTA'sı/geri navigasyonuyla düzeltildi; ayrıca kapsam dışı bir bug bulundu (Keşfet kategori filtreleri sonuç listesini etkilemiyor, kural #107 — bkz. `TODO.md` → "P23-M8-c2") |
| **E4 — Çiftçi akışı** | Uçtan uca denetim | 🎯 Orkestratör | 17 Ağu | ✅ **Yapıldı (2026-08-12, P23-M8-c)** — statik kod denetimi (kural #103, gerçek tarayıcı erişimi bu oturumda engelli); ana omurgada (kayıt→parsel→günlük→ilan→teklif yanıtı→sipariş) kırık nokta bulunmadı, iki küçük bulgu raporlandı (`TODO.md` → "P23-M8-c") |
| E4 | İlan fotoğrafı zorunluluğu | 👤 Berkin | 12 Ağu | ✅ **Karar verildi ve uygulandı (P23-M8-c) — zorunlu DEĞİL**, kaydetmeden hemen önce yumuşak `window.confirm` uyarısı ("Fotoğraflı ilanlar daha çok teklif alıyor"). Bkz. bölüm 4, madde 2 (artık çözüldü) |
| E4 | "Nasıl başlarım" rehberi (çiftçi onboarding) | 🤖 Claude Code | 18 Ağu | ✅ **Yapıldı (2026-08-12, P23-M8-c)** — `farmer.home.tsx` boş-durum kartı 3 adımlık rehbere çevrildi (parsel→hasat kaydı→vitrin), her adım gerçek veriyle tamamlandı işaretleniyor |
| E4 | İlk 5-10 gerçek çiftçi kazanımı | 👤 Berkin | 20 Ağu | 🔴 Başlanmadı — bugünkü durum 17 ilan/9 crop, çoğu seed (bkz. bölüm 3, madde c) |
| **E5 — Ödeme/Yasal** | Şirket tescili | 👤 Berkin | 7 Ağu | 🟡 Devam ediyor — hedefe 1 gün kaldı, henüz tamamlanmadı |
| E5 | iyzico onboarding | 👤 Berkin | 8 Ağu | ⬜ Şirket tesciline bloke |
| E5 | Gizlilik metni + KVKK avukat onayı | 👤 Berkin | 20 Ağu | ⬜ Planlandı |
| E5 | Kullanım koşulları / mesafeli satış sözleşmesi | 👤 Berkin | 20 Ağu | ⬜ Planlandı |
| E5 | Rekabet hukuku değerlendirmesi | 👤 Berkin | 20 Ağu | ⬜ Planlandı |
| E5 | P17-A escrow | 🤖 Claude Code | 24 Ağu | 🔴 Bloke — iyzico onayına bağlı |
| **E6 — Veri hijyeni** | Seed/test verisi temizliği | 👤 Berkin | 22 Ağu | 🔴 **Karar bekliyor** — bkz. bölüm 4, madde 4 |
| E6 | `SMS_TEST_OTP_VALID_UNTIL` hatırlatıcısı | 👤 Berkin | 12 Ağu | ⬜ Planlandı (bkz. `Build/Store-Compliance.md` → Bölüm 6) |
| E6 | Glossary insan gözden geçirmesi | 👤 Berkin | 21 Ağu | 🔴 Başlanmadı — `TODO.md`'de "hâlâ açık" olarak kayıtlı (P22-C, AI üretimi, bölgesel doğrulama yapılmadı) |
| **E7 — Admin/operasyon** | Talep ısı haritası doğrulaması | 🎯 Orkestratör | 22 Ağu | ✅ **Yapıldı (2026-08-17, P23-M8-c2/T3)** — `v_kpi_crop_demand_heatmap` canlı `crop_requests` verisiyle bağımsız SQL karşılaştırmasıyla doğrulandı (0 uyuşmazlık); bir tasarım sınırı bulundu ve düzeltilmedi (kural #107 — culinary birimler `total_quantity_normalized`'e dahil edilmiyor, bkz. `TODO.md` → "P23-M8-c2" madde 5) |
| E7 | Lansman günü izleme planı | 🎯 Orkestratör | 23 Ağu | ⬜ Planlandı |
| E7 | İlk 100 kullanıcı kampanyası | 👤 Berkin | 19 Ağu | ⬜ Planlandı |
| **E8 — App Store Connect** *(tümü 👤 Berkin, tarayıcıdan)* | Bundle ID kaydı | 👤 Berkin | 8 Ağu *(önerilen)* | 🟢 Başlanabilir — Apple hesabı 2026-08-05'te onaylandı |
| E8 | Uygulama oluşturma | 👤 Berkin | 8 Ağu *(önerilen)* | 🟢 Başlanabilir — bundle ID'den sonra |
| E8 | App Review Information | 👤 Berkin | ~25 Eylül *(önerilen, M8-d öncesi)* | ⬜ **Bloke — yalnızca uygulama kaydı oluşturulduktan sonra görünür** |
| E8 | APNs anahtarı | 👤 Berkin | 20 Eylül *(M8-c ile aynı)* | ⬜ Planlandı |

> E8'deki tarihler ("önerilen" işaretli olanlar) görev metninde verilmemişti;
> M8-a/M8-c ile tutarlı olacak şekilde önerildi, Berkin onayı bekliyor —
> kesin tarih değil.

---

## 2. Birleşik lansman takvimi (2026-08-18'de güncellendi)

| Aşama | İş | Hedef |
|---|---|---|
| Hemen | F1 (✅ yapıldı) · F3 (event haritası + 2 kod düzeltmesi) · F4-lite (bildirim tercihleri) · F10-lite (mobil bildirim merkezi) · F13-dar (süre + Hasat-ürünü-içeren + mevcut diyet filtresi) · F14 (sipariş akışı QA) · F15 (konsolide doğrulama checklist) | 18-22 Ağustos |
| Berkin'e bağlı, paralel | F16 (Android FCM) · APNs'in EAS'a yüklenmesi · F17 (logo/icon) | 18-25 Ağustos |
| Migration turu | `Launch-Scope-Plan.md` §4'teki tek PR, kural #115 sırasıyla (migration → `hasat-core` tip PR → sync PR'ları her iki hedefte merge → client kod) | 22-27 Ağustos |
| M8-b — gerçek cihaz doğrulama | F15 checklist + yeni v1.0 özellikleri | 27-31 Ağustos |
| M8-c — push/OTP gerçek teslimat doğrulaması | F3'ün çok-cihazlı test matrisi | 27-31 Ağustos (M8-b ile paralel) |
| **M8-d — Submit** | App Store + Play | **1-15 Eylül 2026** |
| Store canlı | Review sonucu | Eylül sonu (tahmini — Apple red ihtimaline pay var) |
| v1.1 fast-follow | F2 (otomasyon tam ritim) · F5 (favoriler) · F7 (kendi tarifini editleme) · F8-private (Defterim paylaşımı) · F9 (wizard görsel ekleme) · F11 (klonlama) · F12 (besin değerleri) · F13-geniş (ekipman+alerjen filtresi) | Submit sonrası, ~Eylül sonu–Ekim başı |

Tam madde bazlı analiz, bağımlılıklar, roller: `Build/Launch-Scope-Plan.md`.

### Eski takvim (2026-08-06, artık geçersiz — bkz. yukarıdaki 2026-08-18 banner'ı)

Aşağıdaki tablo yalnızca referans için tutuluyor (kural #108, doküman
değişiklikleri iz bırakmalı) — 1-15 Eylül submit hedefiyle geçersiz.

| ~~Milestone~~ | ~~Tarih~~ | ~~Açıklama / bağımlılık~~ |
|---|---|---|
| ~~**M8-a** — Gerçek cihaz test altyapısı~~ | ~~6-8 Ağustos 2026~~ | ~~Apple hesabı onaylandığı (2026-08-05) için başlıyor~~ |
| ~~**M8-b** — Gerçek cihaz doğrulama oturumu~~ | ~~15 Eylül 2026~~ | ~~Berkin'e bağlı, lansman sonrası; M5/M6/M7'nin "kod hazır, cihazda doğrulanmadı" işaretli maddeleri (offline erişim, pişirme modu, timer, AI import kamera yolu, native picker/modal akışları) burada koşulur~~ |
| ~~**M8-c** — APNs anahtarı + push doğrulama~~ | ~~20 Eylül 2026~~ | ~~Android FCM + iOS APNs gerçek cihaza teslimat testi~~ |
| ~~**M8-d** — Store submit~~ | ~~30 Eylül 2026~~ | ~~iOS App Review + Play production başvurusu~~ |
| ~~**Store canlı**~~ | ~~~15 Ekim 2026~~ | ~~iOS + Android canlı — milestone~~ |
| **Komisyon açılışı** | Ekim 2026 | **Tüm crop'larda** açılıyor — safran hasat sezonuyla aynı aya denk gelmesi **tesadüf**, karar crop-bağımsız verildi |
| **P17-D — Fatura/e-müstahsil** | Ekim 2026 | Şirket tesciline bloke (BENCHMARK Gap #4) |
| **M9 — 17 madde** | Kasım 2026 | Lansman sonrasına konsolide edilmiş açık madde listesi — tam liste: `TODO.md` → "🟣 M9 — Lansman Sonrası (Konsolide Açık Maddeler)" |
| **Farmer subscription** — ₺99 | Kasım 2026 | |
| **BENCHMARK #11** — onaylı alıcıya vade/cari | Aralık 2026 | Gap #11, P1→P2, şu an "Yapılmadı" |
| **BENCHMARK #12** — hasat öncesi finansman | Ocak 2027 | Gap #12, partner gerektirir, uzun vade |
| **Buyer premium** — ₺299 | Ocak 2027 | |

> Komisyon açılışı, P17-D, M9, Farmer subscription, BENCHMARK #11/#12,
> Buyer premium tarihleri (Ekim 2026+) bu değişiklikten etkilenmiyor —
> bunlar zaten submit sonrası fast-follow zaman çizelgesinde, yeni
> takvimle çelişmiyor, üstü çizilmedi.

> M8 tarihlerinin gerekçesi ve M5-M7'nin gerçek tamamlanma durumu:
> `Build/Roadmap.md` → "⏱️ 2026-08-06 güncellemesi". **Önemli:** M7'nin bir
> parçası (M7-b — Keşfet + store varlıkları: gizlilik metni, ekran
> görüntüleri, review notları) bu tabloda M8-a'nın öncesinde bitmesi
> varsayılıyor ama **kesin tarihi yok** — Berkin'den netleşmeli.

> **⚠️ İsim çakışması bulundu, sessizce üzerine yazılmadı (kural #107):**
> Bu tablodaki **M8-c** ("APNs anahtarı + push doğrulama", 20 Eylül) ile
> 2026-08-12'de yapılan görevin kendi adı olan **P23-M8-c** ("T2 — çiftçi
> mobil rol yönlendirmesi + E4 + Actions build/submit birleştirmesi", bkz.
> `TODO.md` → "P23-M8-c") **aynı kısaltmayı farklı işler için taşıyor.**
> İkisi de bu dokümanda zaten vardı (M8-c burada 2026-08-06'dan beri, T1-T4
> tur yapısı Bölüm 6'da 2026-08-10'dan beri) — bu tur ikisini birleştirmedi,
> sadece fark edilip burada bildiriliyor. Berkin hangi numaralandırmanın
> kalacağına karar vermeli (ör. T2/T3/T4 turlarını M8-c/d/e olarak yeniden
> adlandırmak, ya da push doğrulama milestone'ını farklı bir isimle
> ayırmak) — bu doküman kendi başına yeniden adlandırma yapmadı.

**Actions build+submit birleştirmesi (P23-M8-c, 2026-08-12):**
`eas-build-testflight.yml` artık build'den hemen sonra aynı işte `eas
submit --non-interactive --latest` çalıştırıyor (App Store Connect API Key
Team Key olarak EAS'a kayıtlı olduğu için etkileşimli Apple girişi
gerekmiyor) — Berkin tek "Run workflow" ile hem build alıp hem TestFlight'a
gönderebiliyor, önceki turda (M8-a) ayrı bırakılan terminal adımı ortadan
kalktı. `eas.json` → `submit.ios-testflight.ios.ascAppId` dolduruldu
(`6798678884`). Yukarıdaki milestone tablosundaki **M8-c** (APNs/push
doğrulama, 20 Eylül) ile karıştırılmamalı — bu ayrı bir iş, yukarıdaki
uyarıya bkz. Detay: `TODO.md` → "P23-M8-c".

---

## 3. Takvimin kırılgan noktaları

**(a) Şirket zinciri.** Şirket tescili → iyzico → P17-A escrow tek bir
zincir. **7 Ağustos** (şirket tescili hedefi) kaçarsa iyzico başvurusu
gecikir, iyzico **24 Ağustos**'a (P17-A escrow hedefi) yetişemez —
lansman **ödemesiz** başlar (gerçek para akışı olmadan). Bu, E5'teki dört
maddenin (tescil → iyzico → escrow, + hukuki metinler paralel) hepsinin
tek bir tarihe (7 Ağu) bağlı olduğu anlamına geliyor.

**(b) Görseller — 14 Ağustos.** Bu tek tarihin arkasında üç bağımlılık
zinciri var: (1) görsellerin üretilmesi (32 adet — 14 crop + 18 tarif) →
(2) bucket'a yükleme + `default_photo_url` bağlanması (15 Ağu) → (3)
"temsili görsel" etiketi doğrulaması (16 Ağu) **ve** Search Console/Rich
Results Test SEO submit'i (16-17 Ağu, Google Recipe şemasında `image`
zorunlu olduğu için görsel olmadan geçemez). Görsel üretimi 14 Ağu'yu
kaçırırsa üç bağımlı adım da sağa kayar.

**(c) İlk gerçek çiftçiler — 20 Ağustos.** Bugünkü durum **17 ilan / 9
crop**, ve bunların çoğu seed veri (bkz. E6 — seed temizliği kararı
bekliyor). Gerçek çiftçi kazanımı 20 Ağustos'a yetişmezse, lansmanda
**satılacak gerçek bir şey olmaz** — vitrin dolu görünse bile arkası seed.
Bu madde E4 (çiftçi akışı) ile E6 (seed hijyeni) arasında da bir bağımlılık
yaratıyor: seed veri temizlenmeden gerçek/seed ayrımı görünür değil.

---

## 4. Açık kararlar — Berkin'den bekleniyor

Kural #107 gereği aşağıdakiler **kararlaştırılmadı**, yalnızca seçenekleriyle
sunuluyor:

1. **32 görsel (14 crop + 18 tarif) nasıl üretilecek?** Kendi çekim / stok
   fotoğraf / AI üretimi — üçü de farklı hız/maliyet/telif profiline sahip,
   14 Ağustos'a yetişecek yöntem Berkin'in kendi kapasitesine bağlı.
2. ~~**İlan fotoğrafı zorunlu mu?**~~ — ✅ **Çözüldü (P23-M8-c, 2026-08-12):**
   Berkin kararı **zorunlu değil**. `ListingSheet` (`farmer.storefront.tsx`)
   bir ilan aktif duruma geçmeden hemen önce, fotoğraf yoksa yumuşak bir
   `window.confirm` uyarısı gösteriyor ("Fotoğraflı ilanlar daha çok teklif
   alıyor") — engellemiyor, sadece hatırlatıyor. Detay: `TODO.md` →
   "P23-M8-c".
3. **Şirket tescili gecikirse: ödemesiz lansman mı, lansman ötelemesi mi?**
   Bölüm 3(a)'daki zincirin kırılması durumunda hangi yol izlenecek —
   "kapsam kesilmez, tarih ötelenir" kuralı burada da mı geçerli, yoksa web
   lansmanı için ayrı bir karar mı var.
4. **Seed verisi silinsin mi, gizlensin mi?** E6/bölüm 3(c) — gerçek
   çiftçi/ilan sayısı düşükken seed veriyi tamamen silmek vitrin
   görünürlüğünü de düşürür; gizlemek (ör. bir flag ile) vitrini dolu
   tutar ama gerçek/seed ayrımını UI'da görünmez kılar. İki yaklaşımın da
   riski Berkin'e sunulmalı, burada seçilmedi.

---

## 5. Çapraz referanslar

Bu doküman aşağıdakilerle bağlayıcıdır; her birine bu dosyaya işaret eden
bir not eklendi:

- **`TODO.md`** — M9 listesinin (17 madde) tek doğruluk kaynağı; BENCHMARK
  Gap tablosu (#11, #12); build log'ları (M5-M7 gerçek tamamlanma tarihleri
  buradan alındı).
- **`Build/Roadmap.md`** — Gantt + kilometre taşı tablosu; bu turda
  **2026-08-06'da gerçek duruma göre düzeltildi** (bkz. o dosyadaki "⏱️
  2026-08-06 güncellemesi" bölümü) — aşağıda açıklanan çelişki bu düzeltmeyi
  tetikledi.
- **`Build/P23-Mobile.md`** — M0-M9 kapsam/mimari detayı, "Şirket gecikirse
  ne olur" tablosu.
- **`Build/Store-Compliance.md`** — App Review submit checklist (Bölüm 6),
  M8-c/M8-d'nin dayandığı teknik gereksinimler.

### ⚠️ Bulunan ve çözülen çelişki (silinmeden bildirildi)

Bu dosya hazırlanırken görev metnindeki M8 tarihleri (~30 Eylül store
submit, ~15 Ekim store canlı) `Build/Roadmap.md`'nin **o zamanki** Gantt'ıyla
(19-31 Ekim submit, 31 Ekim store canlı) çelişiyordu. Kural gereği
**sessizce üzerine yazılmadı** — Berkin'e soruldu. Kök neden Berkin'in
yönlendirmesiyle netleşti: M8'in kendisi değil, Roadmap.md'nin M5-M7 için
hâlâ gelecek tarihli görünen Gantt'ı bayattı (M5-M7, M7-b hariç, planın
6-8 hafta önünde zaten tamamlanmıştı). Roadmap.md bu doğrultuda düzeltildi;
bu dosyadaki M8 tarihleri Berkin'in verdiği yeni takvimi yansıtıyor.

---

## 6. T1-T4 tur yapısı (P23-M8-b sonrası, 2026-08-10 eklendi)

S33'ün gerçek cihaz bulguları (Apple 4.2 reddine yol açacak dört blocker +
iki sarkan madde) tek turda kapanamayacak kadar genişti — **T1-T4** dört
ayrı tura bölündü. Bu bölüm hangi maddenin hangi turda olduğunu, mevcut
epic'lerle (E3/E4/E7, yukarıdaki Bölüm 1) nasıl birleştiğini ve Berkin'in
paralel yürüyen işlerini tek yerde topluyor.

> ⚠️ **Doğrulanabilirlik notu:** Görev metni bu turu "S33'ün 12 bulgusu, 3
> sarkan madde, 13 plan maddesi — hiçbiri düşmemeli" diye tanımladı. Bu
> üçlü sayının kaynağı olan orijinal orkestratör dökümü bu repoda (vault'ta)
> ayrı bir belge olarak bulunamadı — yalnızca görev metninin kendisi ve
> aşağıdaki doğrulanabilir kaynaklar mevcuttu: `Build/E2E-QA.md` → S33'ün
> gerçek başarısız adımları (11-14, 36-38, 46 — bkz. S33 sonundaki "Sonuç"
> tablosu) + web'de bağımsız bulunan "çıkış fatal hata" + "parsel ekleme
> kırık" bulguları, ve görev metninin kendi "Dokunulmayacaklar" listesindeki
> 8 T2+ maddesi. Aşağıdaki tur eşlemesi bu doğrulanabilir kaynaklardan
> kuruldu; 12/3/13 sayılarıyla birebir eşleşip eşleşmediği bağımsız olarak
> teyit **edilemedi** — sessizce "eşleşti" varsayılmadı, burada açıkça
> bildiriliyor. Berkin karşılaştırıp fark varsa düzeltmeli.

### T1 — ✅ TAMAMLANDI (2026-08-10, bu tur, `TODO.md` → "P23-M8-b")

**4 blocker + 2 sarkan madde (6 madde), hepsi kök nedenine kadar çözüldü:**

| # | Madde | Tür | Durum |
|---|---|---|---|
| 1 | Uçak modunda giriş ekranı (S33 adım 11-14) | 🔴 Blocker | ✅ Düzeltildi, yeniden test bekliyor |
| 2 | Hesap silme oturum temizliği + web çıkış fatal hatası (S33 adım 46 + bağımsız web bulgusu) | 🔴 Blocker | ✅ Düzeltildi, yeniden test bekliyor |
| 3 | Push gönderim mekanizması hiç kurulmamış (S33 adım 36-38) | 🔴 Blocker | ✅ Kuruldu, Expo'ya gerçek çağrıyla doğrulandı, fiziksel teslimat Berkin'de |
| 4 | Parsel ekleme trigger regresyonu (bağımsız web bulgusu) | 🔴 Blocker | ✅ Düzeltildi, gerçek insert ile doğrulandı |
| 5 | `eas.json` submit profili eksik | 🟡 Sarkan | ✅ Eklendi (`ascAppId` Berkin'i bekliyor) |
| 6 | Sürüm numarası (`0.1.0`→`1.0.0`) | 🟡 Sarkan | ✅ Yapıldı |

**Epic bağlantısı:** T1, Bölüm 1'deki **E3** (Alıcı akışı — uçtan uca denetim)
ve **E4** (Çiftçi akışı — uçtan uca denetim, özellikle parsel ekleme) için
ön koşuldu — ikisi de kırık bir akış üzerinde anlamlı bir denetim
yapamazdı. **E7** (Admin/operasyon) için de kritik: push gönderimi
olmadan "Lansman günü izleme planı" bildirim kanalı eksik sayılırdı.

### T2/T3/T4 — sıraya alındı, henüz tur bazında ayrıştırılmadı

Görev metninin "Dokunulmayacaklar" listesi 8 maddeyi T2+ kapsamına
işaretledi ama bu 8 maddenin T2/T3/T4 arasında nasıl bölüneceğine dair bir
talimat/öncelik sırası **verilmedi** — kural #107 gereği burada bir sıra
uydurulmadı, hepsi tek bir "sıraya alındı" havuzunda listeleniyor. Berkin
önceliklendirmeli:

- ~~Çiftçi rol yönlendirmesi~~ — ✅ **Yapıldı (T2, P23-M8-c, 2026-08-12)**
- ~~`source_recipe_id`~~ (tarif→teklif atfının mobil tarafındaki kalan işi)
  — ✅ **Yapıldı (T3, P23-M8-c2, 2026-08-17)** — mobil zincir kapatıldı,
  web'de aynı desenin hiç kurulmamış olduğu bulundu (kural #107, düzeltilmedi
  — bkz. `TODO.md` → "P23-M8-c2" madde 1)
- Klavye modalı (native picker/modal klavye konumlanma cilası) — **T4, açık**
- ~~Siparişlerim ekranına dokunma~~ (şu an salt okunur, M7-d kararı —
  genişletme T2+) — ✅ **Yapıldı (T3, P23-M8-c2, 2026-08-17)**, yeni
  `app/offer/[id].tsx` detay ekranı; `OrderRow` (Siparişler sekmesi) aynı
  sınıf sorunu taşıyor ama bulgu kapsamı dışındaydı, açık kaldı
- Pişirme modu "Devam Et" (kaldığı yerden devam UX'i) — **T4, açık**
- OTP autofill (`textContentType="oneTimeCode"` zaten var, otomatik
  doldurmanın tam davranışı) — **T4, açık**
- Kalan kayan nokta gösterim yerleri (P23-M7-g'de yalnızca stok/teklif
  ekranları düzeltildi — bkz. `TODO.md` → "P23-M7-g" → "Dokunulmayan") —
  🟡 **Kısmen yapıldı (T3, P23-M8-c2, 2026-08-17):** görev metninin
  adlandırdığı üç kategori (abonelikler, günlük, pazarlık, 6 dosya)
  kapatıldı; M7-g'nin taradığı geri kalan dört yer (`buyer.requests.tsx`,
  `ProvenanceTimeline.tsx`, `farmer.home.tsx`, `Stepper.tsx`) hâlâ açık
- Adım fotoğrafı (pişirme modu adımlarına fotoğraf ekleme) — **T4, açık**

**Epic bağlantısı:** Bu 8 madde büyük ölçüde **E3**/**E4**'ün "boş durum
ekranları" ve genel UX cilası kapsamına giriyor — lansman kritik yolunun
(web, 25 Ağustos) üzerinde değil, M8-b/c/d'nin (mobil, Ekim) bir parçası.

### Berkin'in paralel yürüyen işleri (T1-T4'ten bağımsız, aynı takvimde akıyor)

Bu maddeler zaten Bölüm 1'in Epic tablosunda var — burada yalnızca T1-T4
tur yapısıyla karışmadığı, **ayrı ve paralel** aktığı teyit ediliyor:

| İş | Epic | Not |
|---|---|---|
| Tarif kapakları (18 adet) | E1 | 14 Ağustos hedefi, SEO için işlevsel (Google Recipe şeması) |
| Search Console kaydı + sitemap | E2 | 16 Ağustos hedefi, görsellere bağlı |
| Avukat onayı (KVKK + gizlilik metni) | E5 | 20 Ağustos hedefi |
| Şirket tescili | E5 | 7 Ağustos hedefiydi, bu dosyanın Bölüm 3(a)'sındaki zincirin başı |
| Glossary insan gözden geçirmesi | E6 | 21 Ağustos hedefi, P22-C'nin AI üretimi içeriği |
| İlk gerçek çiftçiler (5-10 kazanım) | E4 | 20 Ağustos hedefi, `Build/Launch-Plan.md` Bölüm 3(c)'deki kırılgan nokta |

**Sonuç:** T1-T4 tamamen mobil/P23-M8 tarafında akıyor (Ekim hedefi); yukarıki
altı madde web lansmanının (25 Ağustos) kritik yolunda. İki tur birbirini
bloklamıyor — Bölüm 2'deki M8-a/b/c/d milestone tablosu bunu zaten
yansıtıyor.
