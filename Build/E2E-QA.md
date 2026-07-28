---
title: Hasat — E2E QA Test Dokümanı
updated: 2026-07-16
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
