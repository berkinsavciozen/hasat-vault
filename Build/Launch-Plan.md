---
title: Hasat — Lansman Planı
updated: 2026-08-06
tags:
  - hasat
  - launch
  - p23
  - roadmap
---

# Lansman Planı — 25 Ağustos 2026 (web marketplace)

> **Kritik yol: web marketplace, 25 Ağustos 2026.** Kapsam web'dir — mobil
> (M8) Ekim'i hedefliyor ama lansman kritik yolunun üzerinde değil (bkz.
> `Build/P23-Mobile.md` → "Şirket gecikirse ne olur").
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
| E3 | Boş durum ekranları | 🤖 Claude Code | 18 Ağu | ⬜ Planlandı |
| **E4 — Çiftçi akışı** | Uçtan uca denetim | 🎯 Orkestratör | 17 Ağu | ⬜ Planlandı |
| E4 | İlan fotoğrafı zorunluluğu | 👤 Berkin | 12 Ağu | 🔴 **Karar bekliyor** — bkz. bölüm 4, madde 2 |
| E4 | "Nasıl başlarım" rehberi (çiftçi onboarding) | 🤖 Claude Code | 18 Ağu | ⬜ Planlandı |
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
| **E7 — Admin/operasyon** | Talep ısı haritası doğrulaması | 🎯 Orkestratör | 22 Ağu | ⬜ Planlandı |
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

## 2. Lansman sonrası milestone tablosu

| Milestone | Tarih | Açıklama / bağımlılık |
|---|---|---|
| **M8-a** — Gerçek cihaz test altyapısı | 6-8 Ağustos 2026 | Apple hesabı onaylandığı (2026-08-05) için başlıyor |
| **M8-b** — Gerçek cihaz doğrulama oturumu | 15 Eylül 2026 | Berkin'e bağlı, lansman sonrası; M5/M6/M7'nin "kod hazır, cihazda doğrulanmadı" işaretli maddeleri (offline erişim, pişirme modu, timer, AI import kamera yolu, native picker/modal akışları) burada koşulur |
| **M8-c** — APNs anahtarı + push doğrulama | 20 Eylül 2026 | Android FCM + iOS APNs gerçek cihaza teslimat testi |
| **M8-d** — Store submit | 30 Eylül 2026 | iOS App Review + Play production başvurusu |
| **Store canlı** | ~15 Ekim 2026 | iOS + Android canlı — milestone |
| **Komisyon açılışı** | Ekim 2026 | **Tüm crop'larda** açılıyor — safran hasat sezonuyla aynı aya denk gelmesi **tesadüf**, karar crop-bağımsız verildi |
| **P17-D — Fatura/e-müstahsil** | Ekim 2026 | Şirket tesciline bloke (BENCHMARK Gap #4) |
| **M9 — 17 madde** | Kasım 2026 | Lansman sonrasına konsolide edilmiş açık madde listesi — tam liste: `TODO.md` → "🟣 M9 — Lansman Sonrası (Konsolide Açık Maddeler)" |
| **Farmer subscription** — ₺99 | Kasım 2026 | |
| **BENCHMARK #11** — onaylı alıcıya vade/cari | Aralık 2026 | Gap #11, P1→P2, şu an "Yapılmadı" |
| **BENCHMARK #12** — hasat öncesi finansman | Ocak 2027 | Gap #12, partner gerektirir, uzun vade |
| **Buyer premium** — ₺299 | Ocak 2027 | |

> M8 tarihlerinin gerekçesi ve M5-M7'nin gerçek tamamlanma durumu:
> `Build/Roadmap.md` → "⏱️ 2026-08-06 güncellemesi". **Önemli:** M7'nin bir
> parçası (M7-b — Keşfet + store varlıkları: gizlilik metni, ekran
> görüntüleri, review notları) bu tabloda M8-a'nın öncesinde bitmesi
> varsayılıyor ama **kesin tarihi yok** — Berkin'den netleşmeli.

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
2. **İlan fotoğrafı zorunlu mu?** E4'teki 12 Ağustos maddesi — zorunlu
   kılınırsa mevcut fotoğrafsız ilanlar/çiftçiler etkilenir, kılınmazsa
   marketplace'te fotoğrafsız ilan kalmaya devam eder.
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
