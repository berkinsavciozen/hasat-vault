---
title: Hasat — Master Roadmap & Build Log
updated: 2026-07-31
tags:
  - hasat
  - todo
  - roadmap
  - build
---

# Hasat — Master Roadmap & Build Log
> GTM Hedefi: **25 Ağustos 2026** · Günlük 1-2 saat · `[C]` = Claude ile · `[C Web]` = Web Claude (Lovable+Supabase+GitHub MCP+Hasat MCP)

---

## ✅ TAMAMLANDI

### Teknik Build
(Önceki maddeler değişmedi — bkz. önceki sürüm)
- [x] P18 serisi + P19 (backend+UI) tamamen tamamlandı
- [x] P19-C — İzmir tam olgun, global commodity API araştırması kapandı
- [x] P17-B — Sipariş sonrası akış sıfırdan inşa edildi, canlı doğrulandı
- [x] P17-C — Karşılıklı Değerlendirme Sistemi tamamlandı + 3 gerçek boşluk düzeltildi
- [x] P17-F — Tekrar Sipariş + Şube Adresleri tamamlandı, Abonelikler'e entegre
- [x] P17-E — Yapılandırılmış RFQ (talep akışı) TAMAMLANDI, uçtan uca canlı veriyle bizzat doğrulandı (2026-07-21)
- [x] P17-G — KPI ölçüm view'ları (20 view) + Admin KPI Dashboard TAMAMEN TAMAMLANDI (2026-07-21)
- [x] **P20 — SMS/Bildirim genişletmesi + lojistik bildirimleri TAMAMLANDI, gerçek Twilio testiyle uçtan uca doğrulandı (2026-07-21)**
- [x] **P21-A — Kontrollü Batch Mimarisi TAMAMLANDI, gerçek veriyle uçtan uca doğrulandı (2026-07-23)**
- [x] **P21-B+C — Buyer Çoklu-Batch Keşif/Ürün Detay/Tek Teklif Mimarisi TAMAMLANDI, gerçek veriyle uçtan uca doğrulandı (2026-07-23)**
- [x] **P24 — Abonelik Sistemi Denetimi, Regresyon Düzeltmesi ve Discoverability TAMAMEN TAMAMLANDI (2026-07-23, son madde 2026-07-24'te manuel QA ile kapandı)**
- [x] **P22-A — Care Journal Şeması (4+1 tablo) TAMAMLANDI, gerçek insert/RLS testiyle doğrulandı (2026-07-24)**
- [x] **P22-B — Rutin Bakımı Özelleştir Ekranı TAMAMLANDI (Lovable) (2026-07-24)**
- [x] **P22-C — Crop Glossary Üretimi TAMAMLANDI (70 crop × 204 satır, tam kapsama) (2026-07-24)**
- [x] **P22-D — Journal Sayfası UI (2 sekme) TAMAMLANDI (Claude Code doğrudan) (2026-07-24), sonradan parsel-bazlı gruplamayla düzeltildi**
- [x] **P22-E — Yeni Ürün Türü Talep Sistemi (çiftçi) + Katalog-Boşluğu SMS Bildirimi (buyer) TAMAMLANDI (2026-07-24)**
- [x] **P22-F — Rutin Bakım mimarisi günlükle (harvest_entries) birleştirildi, ayrı tablo kaldırıldı TAMAMLANDI (2026-07-24)**
- [x] **P22-A/B/C/D PR'ı (#1) `main`'e merge edildi (2026-07-24)**
- [x] **P22-D düzeltmesi + P22-E + P22-F PR'ı (#3) `main`'e merge edildi (2026-07-24) — tarayıcı QA bekliyor, bkz. "Kalan iş"**

### P16 — TÜM SERİ TAMAMLANDI ✅
(Detaylar önceki sürümlerde)

---

## 🔎 BENCHMARK RE-AUDIT (Temmuz 2026)
(Değişmedi)

---

## 🔴 ŞİMDİ — Temmuz 2026

> **[2026-07-28 güncelleme] Şirket kuruluşunun kritik yol ağırlığı arttı ve şirket TİPİ kararı açıldı.**
>
> Şirket artık üç şeyi birden blokluyor: (1) P17-A escrow/iyzico, (2) P17-D fatura/e-müstahsil, (3) ileride store organizasyon hesapları.
>
> **Yeni bulgu:** Apple'ın resmî kuralı, organizasyon hesabı için **tüzel kişilik** şartı koyuyor — **şahıs şirketi bireysel kaydolmak zorunda** (App Store satıcı adı = kişisel ad, "Hasat" değil). Bu, mevcut "şahıs şirketi kur" planıyla "App Store'da Hasat markası" hedefinin çeliştiği anlamına geliyor.
>
> **Alınan önlem:** Apple bireysel hesabı şirketten bağımsız olarak şimdi açılıyor → Apple kritik yoldan çıktı. Şirket tipi kararı (şahıs vs Ltd. Şti.) mali müşavir görüşüyle ayrıca verilecek. Detay: `Build/Store-Compliance.md`.
>
> **Şirket tescili için hedef: ~7 Ağustos 2026** (iyzico zinciri: tescil → iyzico onboarding 1-3 hafta → 25 Ağustos soft launch'ta gerçek ödeme).

- [ ] **Şirket tipi kararı** — mali müşavire iki soru: (1) Ltd. Şti. kuruluş süresi + yıllık maliyet farkı? (2) Şahıs şirketi ile escrow/aracılık faaliyetinin sorumluluk riski?
- [ ] Şirket tescili (hedef ~7 Ağustos)
- [ ] iyzico başvurusu (tescil sonrası hemen)
- [ ] **[Yeni] Apple Developer bireysel hesap** ($99, D-U-N-S gerekmiyor, iPhone üzerinden) — hedef 7-10 gün, güvenli son tarih 15 Eylül
- [ ] **[Yeni] App Store Connect'te "Hasat" adının müsaitliğini kontrol et** (hesap açılır açılmaz)
- [ ] Rekabet hukuku danışmanlığı

(Diğer maddeler değişmedi — bkz. önceki sürüm)

### Düşük öncelikli cila
(Değişmedi — bkz. önceki sürüm)
- [ ] `useSetDefaultAddress` diğer adresleri `false`'a çekmiyor — düşük öncelik.
- [ ] **[Yeni]** P22-D+E+F birleşik tarayıcı QA'sı — güncel test case aşağıda "⚠️ Kalan iş" bölümünde (Rutin Bakım'ın günlükle birleşmesi, parsel-bazlı gruplama, tarih filtresi, yeni ürün türü talep sistemi tek akışta test ediliyor).
- [x] `buyer.producer.$id`'nin guest-erişiminde `BuyerShell`'in hatasız render olduğu **manuel QA ile doğrulandı (Berkin, 2026-07-24)** — gizli sekmede sayfa hatasız yüklendi, guest CTA metinleri doğru göründü, `/login`'e yönlendirme çalıştı. (bkz. P24 doğrulama tablosu)

---

## 🏗️ Lovable/Supabase Build Sırası

> **P19 + P17 serisi (B/C/F/E/G) + P20 + P21 (A, B+C) + P24 tamamen bitti, hepsi canlı doğrulandı.** **P17-A ve P17-D şirket kuruluşuna bağlı, bloke.** BENCHMARK Gap listesindeki bağımsız yapılabilecek her şey bitti (bkz. Gap durum tablosu altta). **P22 serisi (A/B/C/D/E/F) tamamen bitti, hepsi `main`'de (PR #1, #3, #4 merge edildi) — tek kalan iş tarayıcı QA.** Detaylar dosyanın sonundaki "Onaylanan Yol Haritası — P21/P22/P23" bölümünde. **Not:** Bir sonraki Lovable turuna geçmeden önce workspace kredisi kontrol edilmeli (P24 sonunda bitmişti).

### BENCHMARK Gap Durum Tablosu (2026-07-21 itibarıyla)
| # | Gap | Şiddet | Durum |
|---|---|---|---|
| 1 | Ödeme güvencesi (escrow/PSP) | P0 | 🔴 Bloke (P17-A) |
| 2 | Teslim kabul + ihtilaf akışı | P0 | ✅ Kapandı (P17-B) |
| 3 | Karşılıklı değerlendirme | P0 | ✅ Kapandı (P17-C) |
| 4 | Fatura / e-müstahsil | P0 (B2B) | 🔴 Bloke (P17-D) |
| 5 | Tekrar sipariş + şube adresleri | P0 (HoReCa) | ✅ Kapandı (P17-F) |
| 6 | Yapılandırılmış RFQ | P1 | ✅ Kapandı (P17-E) |
| 7 | Hal fiyatı referans bandı | P1 | ✅ Kapandı (P19, sadece İzmir pilotu) |
| 8 | Lojistik adımı (taşıma+takip) | P1/P0 | ✅ **Kapandı (P17-B alanları + P20 bildirimi)** |
| 9 | Parselden tabağa QR görünümü | P2 | ✅ **Kapandı (P23-M4-b, 2026-07-30)** — eşleşen malzemeden mevcut `/batch/$listingId` izlenebilirlik sayfasına link, yeni sistem kurulmadı |
| 10 | SMS/WhatsApp bildirim genişletmesi | P2 | ✅ **Kapandı (P20, P24'te regresyonu düzeltildi)** |
| 11 | Onaylı alıcıya vade/cari | P1→P2 | ⬜ Yapılmadı |
| 12 | Hasat öncesi finansman | P2 | ⬜ Yapılmadı — partner gerektirir, uzun vade |

---

## 🎨 P18 — ARAYÜZ YENİLEME — **SERİ TAMAMEN TAMAMLANDI ✅**
(Değişmedi)

## 🟢 P19 — BORSA DENEYİMİ — **TAMAMEN TAMAMLANDI ✅**
(Değişmedi)

## 🟠 P19-C — KAYNAK & ÜRÜN TİPİ GENİŞLETME — **KAPANDI**
(Değişmedi — bkz. önceki sürüm)

---

## 🟣 P17 — GÜVEN ÇEKİRDEĞİ — **SERİ TAMAMEN TAMAMLANDI ✅** *(2026-07-21)*

### Kapsam ve sıralama
| Kod | Konu | Şiddet | Durum |
|---|---|---|---|
| P17-B | Teslim/kargo/iptal/ihtilaf akışı | P0 | ✅ **TAMAMLANDI** |
| P17-C | Karşılıklı değerlendirme (rating/review) | P0 | ✅ **TAMAMLANDI** |
| P17-F | Tekrar sipariş + şube adresleri + abonelik bağlantısı | P0 (HoReCa) | ✅ **TAMAMLANDI** |
| P17-E | Yapılandırılmış RFQ (talep akışı) | P1 | ✅ **TAMAMLANDI** |
| P17-G | KPI ölçüm görünümleri + admin dashboard | destek | ✅ **TAMAMLANDI** |
| P17-A | Gerçek bloke ödeme (escrow) | P0 | 🔴 Bloke — şirket kuruluşu + iyzico başvurusu |
| P17-D | Fatura/e-müstahsil | P0 (B2B) | 🔴 Bloke — vergi mükellefiyeti gerektiriyor |

Yalnızca P17-A ve P17-D kaldı, ikisi de şirket kuruluşuna bağlı.

### ✅ P17-B / P17-C / P17-F / Abonelik-Sipariş Bağlantısı — TAMAMLANDI
(Değişmedi — bkz. önceki sürüm)

### ✅ P17-E — Yapılandırılmış RFQ (Talep Akışı) — TAMAMLANDI *(2026-07-21)*
(Değişmedi — bkz. önceki sürüm)

### ✅ P17-G — KPI Ölçüm Görünümleri + Admin Dashboard — TAMAMEN TAMAMLANDI *(2026-07-21)*
(Değişmedi — bkz. önceki sürüm; 20 view + `/admin/kpi` dashboard, detaylar korunuyor)

---

## 🔵 P20 — SMS/Bildirim Genişletmesi + Lojistik Bildirimi — **TAMAMEN TAMAMLANDI ✅** *(2026-07-21, P24'te regresyonu düzeltildi)*

**Kapsam:** BENCHMARK Gap #8 (lojistik adımı) ve #10 (SMS/WhatsApp bildirim genişletmesi)'nin bağımsız yapılabilecek kısmını kapatmak. Araştırma + backend Claude tarafından doğrudan Supabase MCP ile, frontend Lovable ile yapıldı.

### 🔴 Bulunan gerçek bug (acil düzeltildi)

`public.dispatch_sms()` SQL fonksiyonu `offer_accepted` ve `payment_confirmed` event'lerini doğru tanıyor, `notif_prefs`'teki ilgili tercihi kontrol ediyor, tercih açıksa `send-sms` edge function'ını çağırıyordu — **ama `send-sms`'in kendi `COL` map'i (TypeScript) bu iki event'i tanımıyordu**, `skipped: no-pref-mapping` dönüp sessizce SMS'i atlıyordu. Sonuç: kullanıcı arayüzünde "Teklif Kabul Edildi" / "Ödeme Onaylandı" SMS toggle'ları vardı, kullanıcı açabiliyordu, ama **hiçbir zaman çalışmıyordu**. `dispatch_sms` (SQL) ve `send-sms` (edge function, TS) içinde event→kolon eşlemesi iki farklı yerde ayrı ayrı tutulduğu için birbirinden sapmıştı — klasik "iki kaynak, tek doğruluk" hatası.

**Düzeltme:** `send-sms`'in `COL` map'i genişletildi, artık `dispatch_sms`'in SQL CASE'iyle birebir senkron (10 event: `new_offer`, `price_alert`, `harvest_time`, `offer_accepted`, `payment_confirmed`, `order_shipped`, `order_delivered`, `order_cancelled`, `dispute_opened`, `crop_request_match`). Kod içine iki mapping'in senkron tutulması gerektiğine dair yorum eklendi.

**⚠️ [2026-07-23 güncelleme] Bu düzeltme P24 öncesinde bir yerde kaybolmuş/geri alınmıştı (COL map 3 event'e düşmüştü) — P24'te tekrar bulunup 13 event'e (10+3 yeni abonelik event'i) restore edildi. Detaylar P24 bölümünde.**

### Yapılanlar

1. **`notif_prefs`** tablosuna 5 yeni kolon: `order_shipped_sms`, `order_delivered_sms`, `order_cancelled_sms`, `dispute_opened_sms`, `crop_request_match_sms`.
2. **`dispatch_sms()`** SQL fonksiyonu genişletildi (5 yeni event).
3. **`notify_order_status()`** trigger'ı (orders tablosu, AFTER UPDATE) yeniden yazıldı:
   - Artık `shipped`/`delivered`/`cancelled`/`disputed` için **spesifik** başlık+body üretiyor (öncesinde `cancelled`/`disputed` jenerik "Sipariş Durumu Güncellendi" yazıyordu).
   - **Kargo mesajına taşıyıcı+takip no dahil ediliyor** (`orders.carrier`/`tracking_number` — bu alanlar P17-B'den beri DB'de vardı ama hiçbir bildirime yansımıyordu, Gap #8'in tam eksik parçası).
   - `cancelled`/`disputed` durumunda her iki tarafa da (alıcı+çiftçi) in-app bildirim gidiyor (öncesinde sadece alıcıya gidiyordu).
   - Her event için `dispatch_sms()` çağrısı eklendi (önceden bu trigger hiç SMS tetiklemiyordu).
4. **`useCreateCropRequest`** (P17-E RFQ eşleşmesi, frontend) — her eşleşen çiftçi için `notifications` insert'inin yanına `supabase.rpc('dispatch_sms', {event:'crop_request_match', ...})` çağrısı eklendi (best-effort, Promise.all + per-farmer catch).
5. **Bildirim tercihleri UI'ları** (`farmer.settings.notifs.tsx`, `buyer.settings.notifs.tsx`) — 5 yeni toggle satırı (Kargoya Verildi/Teslim Edildi/Sipariş İptal Edildi/İhtilaf Açıldı her ikisinde, Ürün Talebi Eşleşti sadece farmer'da). `NotifPrefsRow` type + `NOTIF_PREF_DEFAULTS` güncellendi.

### Doğrulama (gerçek Twilio SMS testi, iki farklı senaryo)

1. **Sipariş teslim edildi:** Zeynep'in (test buyer) `order_delivered_sms` tercihi geçici açıldı → gerçek order'ın (`645bf8db...`) status'ü `delivered`'a çekildi → `notify_order_status` tetiklendi → in-app bildirim doğru mesajla geldi ("Siparişiniz teslim edildi...") → `net._http_response` id=34'te gerçek Twilio yanıtı: `status: accepted`, `to: +905009876543`, `body: "Hasat: Siparişiniz teslim edildi."`. Test sonrası tercih eski haline (false) döndürüldü.
2. **RFQ eşleşmesi:** gerçek bir crop_request (safran) oluşturuldu → Ahmet (eşleşen çiftçi) için `dispatch_sms('crop_request_match')` çağrıldı → `net._http_response` id=36'da gerçek Twilio yanıtı: `status: accepted`, `to: +905001234567`, `body: "Hasat: Zeynep Kaya safran arıyor — 10 g"`. Test crop_request'i temizlendi.

Her iki senaryoda da `net._http_response` tablosundaki gerçek kayıt SQL ile çekilip doğrulandı (Lovable'ın metnine güvenilmedi).

### Not

Bu turda Lovable'a gönderilen 2 mesaj API hatası (`"Error occurred during tool execution"`) döndürdü ama iş arka planda gerçekten tamamlanmıştı — bir sonraki mesajda Lovable "iş zaten tamamdı" dedi, ben de kodu ve `net._http_response` kayıtlarını bağımsız okuyarak doğruladım. **Ders:** Lovable tool-call hatası dönse bile, bir sonraki turda dosyaları/veriyi kontrol etmeden "başarısız oldu" varsayılmamalı.

---

## 🟤 P21 — Batch & Vitrin Çoklu-Batch Mimarisi — **SERİ TAMAMEN TAMAMLANDI ✅** *(2026-07-23)*

**Kapsam:** Berkin'in 22-23.07.2026 el notlarından doğan Batch mimarisi — çiftçi tarafında kontrollü batch açma (P21-A) + buyer tarafında çoklu-batch keşif/ürün detay/tek teklif akışı (P21-B+C). Tüm alt işler aynı gün içinde plan→implementasyon→gerçek veriyle doğrulama döngüsüyle tamamlandı. **Önemli not: implementasyon boyunca birden fazla kez `plan_mode=true` isteğine rağmen Lovable doğrudan koda geçti** (bilinen platform davranışı) — her turda gerçek diff/DB durumu bağımsız doğrulandı, Lovable'ın metnine güvenilmedi.

### P21-A — Kontrollü Batch Mimarisi

**Yapılanlar:**
1. `listings` tablosuna `batch_name` (nullable text) kolonu eklendi.
2. `tg_harvest_entries_after_insert_autolink` trigger'ı güncellendi: aynı farmer+parcel+crop için **birden fazla** draft/active listing varsa artık otomatik bağlama yapmıyor (frontend'in kullanıcı seçimiyle bağlaması gerekiyor); tek eşleşme varsa eski davranış (otomatik bağlama) korunuyor.
3. `useExistingBatches(parcelId, crop)` hook'u (yeni) — aynı parcel+crop için mevcut draft/active listing'leri döndürür.
4. `ListingSheet` (`farmer.storefront.tsx`, "Yeni Ürün" formu): artık parsel seçimi zorunlu; aynı parcel+crop için mevcut batch(ler) varsa form yerine bir "batch kararı" ekranı gösteriliyor — mevcut batch'lerin listesi (stok+fiyat) + "Yeni Batch Aç (Taslak)" butonu. Yeni batch seçilirse `status='draft'` ile açılıyor (mevcut "✓ Yayınla" akışıyla sonra aktifleşiyor). "Batch adı" opsiyonel input eklendi.
5. Hasat kaydı formu (`farmer.journal.new.tsx`) ve AI chat akışı (`JournalEntryCard.tsx`, ortak `useCreateEntry` hook'una taşındı): aynı parcel+crop için >1 batch varsa "Batch (parti)" pill seçici gösteriliyor (varsayılan: en son eklenen batch), seçilen `listingId` kayıtla birlikte `listing_harvest_entries`'e frontend tarafından bağlanıyor.

**🔴 Bulunan ve düzeltilen gerçek bug'lar:**
- **`min(uuid)` hatası:** İlk trigger revizyonunda `SELECT count(*), min(l.id)` kullanılmıştı — Postgres'te `uuid` için `MIN()` aggregate'i **yok**. Bu, `parcel_id` dolu her hasat kaydını hataya düşürüyordu (üretimi kıran bug). Subquery'ye çevrilerek düzeltildi, gerçek veriyle 3 senaryo (tek eşleşme/sıfır eşleşme/çoklu eşleşme) test edildi.
- **Crop case-mismatch (büyük bulgu):** `useCropOptions()` her yerde `crop_config.display_name` (Title Case, "Safran") döndürüyordu, DB'ye o şekilde yazılıyordu; `price_history`/`crop_market_sources` (P19) ise hep lowercase slug kullanıyordu. Sonuç: `listings`/`harvest_entries`/`parcels.crops` içinde aynı ürün için karışık case (`Safran` × 5, `safran` × 3 gibi) birikmiş, bu da `useExistingBatches`'in aynı crop'un batch'lerini görememesine yol açıyordu. **Düzeltme:** (a) DB backfill — tüm Title Case satırlar `crop_config.crop` kanonik slug'ına çevrildi (safran tek slug'da birleşti, `Şeker Pancarı`→`şeker_pancarı`), (b) `useCropOptions()` artık `value: c.crop` (slug) döndürüyor, `label` görünen isim olarak Title Case kalıyor, (c) `useExistingBatches` + trigger case-insensitive güvenlik ağına kavuştu, (d) `farmer.journal.index.tsx`'teki parsel-oluşturma default'u (`["Safran"]`→`["safran"]`) de bulunup düzeltildi. `safran_soğanı` (fide/soğan satışı, safranın kendisinden farklı bir ürün) crop_config'te zaten ayrı ve doğru bir satır olarak bulundu — dokunulmadı, birleştirilmedi.
- **Mixed-unit toplama riski:** Aynı crop+parcel için g ve kg birimli batch'ler bulundu (Ahmet'in safran'ı: 500g + 100kg). `crop_config.default_unit` (zaten var — her crop'un kanonik preset birimi: g/kg/L/adet) referans alınarak `convertQuantity()` (g↔kg dönüşümü) yardımcı fonksiyonu yazıldı; Keşfet grup kartı, ürün detay sayfası toplamı ve `ListingSheet`'in unit varsayılanı (artık crop seçilince `default_unit`'e kayıyor) bu fonksiyonu kullanacak şekilde güncellendi.

**Doğrulama (gerçek veriyle, Ahmet'in Güney Bahçe/safran parseli üzerinde):**
- Tek batch → autolink çalışıyor ✅; 2 batch → autolink hiçbir şey yapmıyor (frontend'in seçimini bekliyor) ✅ — test verisi oluşturulup temizlendi.
- Case-mismatch backfill sonrası: `listings`/`harvest_entries`/`parcels` içinde crop başına tek slug kaldığı doğrulandı.
- Mixed-unit: safran'ın `default_unit`'i "g", 500g+100kg batch'lerinin toplamı artık doğru şekilde 100.500 g olarak hesaplanıyor (önceden ham toplama 600 gibi anlamsız bir sayı veriyordu).

### P21-B+C — Buyer Çoklu-Batch Keşif, Ürün Detayı, Tek Teklif

**Şema (yeni migration):**
- `offer_items` tablosu (`offer_id`, `listing_id`, `quantity`, `price_per_unit`) — bir teklifin hangi batch'ten ne kadar aldığını satır satır tutar. RLS: taraflar (buyer/farmer) okuyabilir, buyer kendi offer'ına ekleyebilir.
- `enforce_offer_stock()` fonksiyonu `offer_items` satır bazlı çalışacak şekilde güncellendi (her listing için ayrı stok kontrolü, `offer_items` yoksa eski tekil davranışa düşer — geriye dönük uyumlu).
- Yeni trigger `tg_enforce_link_unit_match` (`listing_harvest_entries` BEFORE INSERT): bağlanacak `harvest_entries.unit` ile `listings.unit` farklıysa hata fırlatır — kg/g karışıklığının kaynağında (veri girişinde) engellenmesi.
- Traceability RLS: buyer'ın `listings`/`harvest_entries`'e okuma erişimi, o listing için `offer_items`→`offers` zinciri üzerinden bir ilişkisi varsa (statü ne olursa olsun) açık — satılmış/expired batch bile buyer'ın geçmiş siparişinden görülebiliyor.

**Frontend:**
- `buyer.discover.tsx`: aynı (farmer_id, crop) için listing'ler client-side gruplanıyor, "N parti" rozeti + fiyat aralığı + kanonik-birime-çevrilmiş toplam stok gösteriliyor.
- Yeni route `buyer.product.$farmerId.$crop.tsx`: her batch kendi satırında (stok, fiyat, kalite), genişletilince bağlı `harvest_entries` kronolojik listesi; buyer birden fazla batch'ten miktar seçebiliyor, canlı toplam kanonik birimde hesaplanıyor.
- `OfferBatchBreakdown` component'i (yeni, ortak): bir teklifin `offer_items` dağılımını gösterir, her satır genişletilince o batch'in hasat geçmişi açılır. Buyer (`buyer.negotiation.$offerId.tsx`, `buyer.orders.$orderId.tsx`) ve farmer (`farmer.orders.index.tsx` — hem `OfferCard` hem `OrderCard`) tarafında da eklendi.
- `useCreateMultiBatchOffer` (yeni hook): tek `offers` satırı (ağırlıklı ortalama fiyat + toplam miktar, geriye dönük uyumluluk için) + N `offer_items` satırı yazıyor; hata durumunda offer rollback ediliyor (orphan offer kalmıyor). `useCreateOffer` (mevcut tek-batch hook) da aynı alt fonksiyona (`insertOfferWithItems`) delege edildi — artık **her offer en az 1 `offer_item`'a sahip** invariant'ı hem eski hem yeni akışta garanti.

**🔴 Bulunan ve düzeltilen ek gerçek eksikler (Berkin'in canlı karşılaştırmasıyla bulundu):**
- Çoklu-batch ürün sayfası ilk halinde **Teslimat yöntemi ve Teslim Tarihi** alanlarını hiç toplamıyordu (tek-batch teklif sayfasında var). Araştırma sonucu ortaya çıkan gerçek mimari: tek-batch akışı teklifi hemen oluşturmuyor — `pendingOffer`'ı Zustand store'a yazıp `/buyer/payment`'e yönleniyor, gerçek insert ancak "Ödemeyi Tamamla" tıklanınca (mock ödeme adımı) tetikleniyor. Çoklu-batch sayfası bu **aynı** akışa bağlandı: ortak `DeliveryFields` component'i (Kargo/Aynı Gün Kurye/Üreticiden Teslim + tarih seçici) eklendi, submit artık `pendingOffer.items[]` set edip payment sayfasına yönlendiriyor; payment sayfası `items` varsa `useCreateMultiBatchOffer`'a, yoksa `useCreateOffer`'a düşüyor.
- MCP `create-offer` tool'u **`offer_items`'a hiç insert yapmıyordu** — kontrol edilip eklendi (tek-batch MCP teklifleri de artık mirror `offer_item` satırı yazıyor, fail olursa offer rollback).
- Farmer tarafı offer/order detay sayfalarında (`farmer.orders.index.tsx`) "Parti dağılımı" hiç gösterilmiyordu — `OfferBatchBreakdown` hem `OfferCard` hem `OrderCard`'a eklendi.

**Doğrulama (gerçek veriyle):**
- Çoklu-batch offer + kabul, yeterli stokla → sorunsuz kabul edildi ✅.
- Çoklu-batch offer + kabul, yetersiz stokla (bir batch'te 9999 birim istenip 500 mevcutken) → "Stok yetersiz (batch)" hatası doğru fırlatıldı ✅.
- Birim uyuşmazlığı (kg harvest_entry → g listing'e bağlama denemesi) → hata doğru fırlatıldı ✅; doğru birimle (kg→kg) bağlama sorunsuz geçti ✅.
- Tüm test verisi (offer/offer_items/harvest_entries/listing_harvest_entries) temizlendi.
- Diff bazlı kod incelemesi: her turda `Lovable:get_diff` ile gerçek değişiklik satırları okunup rapor metniyle karşılaştırıldı (bkz. ders #96).

### Kapsam dışı (bilinçli, sonraki fazlara bırakıldı)
- Care Journal / takvim görünümü — P22'nin işi.
- Recipe App / mobile — P23'ün işi.
- `harvest_entries.unit` normalizasyonunun **geçmiş** (bugün öncesi) kayıtlara backfill'i yapılmadı — sadece yeni link'ler korunuyor.

---

## 🟡 P24 — Abonelik Sistemi Denetimi, Regresyon Düzeltmesi ve Discoverability — **TAMAMEN TAMAMLANDI ✅** *(2026-07-23, son madde 2026-07-24'te kapandı)*

**Kapsam:** Berkin'in Lovable ile ayrı ilerlettiği "Abonelikler" (`harvest_subscriptions`) özelliğinin denetimi + bulunan gerçek eksiklerin tamamlanması + kullanıcı testiyle bulunan 2 discoverability sorununun çözümü.

### 🔴 Bulunan regresyon (P20'nin kaybolmuş hali)

`supabase/functions/send-sms/index.ts` COL map'i **10 event'ten 3'e düşmüştü** (`new_offer`, `price_alert`, `harvest_time` dışındaki 7 event — P20'de eklenenler — kayboldu; muhtemelen ayrı bir düzenleme turunda dosya üzerine yazılmış). SQL tarafındaki `dispatch_sms` fonksiyonu bu event'leri (+ 3 yeni abonelik event'i) hâlâ tanıyordu, ama edge function tanımadığı için tüm bu SMS'ler sessizce atlanıyordu.

**Düzeltme:** COL map 13 event'e (10 eski + `subscription_new`, `subscription_accepted`, `subscription_rejected`) restore edildi, P20'deki "SQL ile senkron tut" yorumu tekrar eklendi. **Gerçek Twilio testiyle doğrulandı** — `subscription_new` (Ahmet'e) ve `subscription_accepted` (Zeynep'e) gerçekten gönderildi, `net._http_response`'ta id=42/43 olarak görüldü, test verisi temizlendi.

### Diğer gerçek eksikler ve düzeltmeleri

1. **Bildirim tercihleri UI'sında toggle yoktu** — `farmer.settings.notifs.tsx`'e "Yeni Abonelik Talebi", `buyer.settings.notifs.tsx`'e "Abonelik Kabul Edildi"/"Abonelik Reddedildi" toggle'ları eklendi. `NotifPrefsRow`/`NOTIF_PREF_DEFAULTS` güncellendi.
2. **MCP `create-subscription.ts`** `crop`/`note` parametrelerini almıyordu (bu yüzden MCP üzerinden açılan abonelikler `crop=NULL` kalıyordu) ve `status:"active"` ile insert ediyordu (farmer onayını atlayarak). Düzeltildi: `crop`/`note` artık alınıyor, `status` DB default'una (`pending`) bırakıldı.
3. **Otomatik hasat-tarihi hatırlatması yoktu** (Berkin kararı: otomatikleştir) — yeni SQL fonksiyonu `send_subscription_harvest_reminders()` + günlük pg_cron job (`subscription-harvest-reminders-daily`, jobid=2, her gün 07:00 UTC) eklendi. Aktif abonelikte `next_harvest_date` bugünden **3 gün sonraysa** hem buyer hem farmer'a in-app bildirim + SMS gönderiyor (mevcut, daha önce hiç kullanılmayan `harvest_time` event'i yeniden kullanıldı). **Gerçek Twilio testiyle doğrulandı** (Ahmet'e gönderildi, `harvest_time_sms` tercihi geçici açılıp test sonrası eski haline döndürüldü).
4. **Fiyat kilidi (`price_lock`) enforcement yok** — Berkin kararı: şimdilik böyle kalsın, sadece UI önerisi (gerçek para henüz akmıyor, P17-A escrow bloke olduğu için risk düşük). **Bilinçli olarak değiştirilmedi.**

### Discoverability — 2 gerçek navigasyon deliği bulundu ve düzeltildi

1. **Abonelik CTA'sının olduğu sayfaya (`/buyer/producer/$id`) hiçbir yerden link yoktu.** Keşfet, ürün detay, sipariş sayfaları üretici adını hep storefront'a (`/s/$slug`) linkliyordu, `/buyer/producer/$id`'e değil — kullanıcı oraya URL bilmeden ulaşamıyordu. **Düzeltme:** Storefront (`/s/$slug`) sayfasına, giriş yapmış buyer içinse (kendi vitrinini görüntülemiyorsa) "📅 Hasat Aboneliği Oluştur" CTA'sı eklendi.
2. **`/buyer/producer/$id` giriş yapmamış kullanıcıya hiç render olmuyordu** — parent layout (`buyer.tsx`) `beforeLoad`'ı tüm `/buyer/*` yollarını sert şekilde `/`'e redirect ediyordu. Berkin'in isteğiyle: bu route için guard'a istisna eklendi (diğer `/buyer/*` yolları etkilenmedi — diff ile doğrulandı), sayfanın kendisine `useIsLoggedIn` eklendi. Artık:
   - **Giriş yapmışsa:** her ürün kartında "Teklif Ver →" (zaten vardı) + "Hasat Aboneliği Oluştur →" aynen çalışıyor.
   - **Giriş yapmamışsa:** alttaki ana buton **"Abonelik ve Teklif için Hasat'a Üye Ol →"**'a dönüşüyor, her ürün kartındaki buton da **"Üye Ol & Teklif Ver →"**'a dönüşüp `/login?role=buyer`'a yönlendiriyor.

### ✅ Doğrulama tamamlandı (2026-07-24)

Son değişikliğin (`buyer.producer.$id` guest-erişimi) routing-guard seviyesi diff ile doğrulanmıştı (sadece `/buyer/producer/` istisnası, diğer `/buyer/*` korunuyor — güvenli). Claude Code oturumunda ek olarak: (a) sayfanın kullandığı tüm veri hook'ları (`useFarmerPublicProfile`, `useFarmerActiveListings`, `useParcelsByFarmer`, `useFarmerProducerStats`, `useFarmerRatingSummary`, `useFarmerRecentReviews`) kod okunarak `enabled: !!farmerId` ile gate'li olduğu (userId'ye değil) doğrulandı, (b) `listings`/`harvest_entries`/`reviews`/`public_farmer_profiles` için anon SELECT RLS politikaları gerçek SQL sorgusuyla teyit edildi, (c) `parcels`/`orders`'ta anon'a özel satır politikası olmasa da tablo-seviye grant olduğu için sorgu hataya değil boş sonuca düşüyor (crash riski yok) — bu da doğrulandı. **`BuyerShell` component'inin (nav bar/layout) giriş yapmamış bir ziyaretçide gerçekten hatasız render olduğu Berkin tarafından gizli sekmede manuel test edilip doğrulandı** (2026-07-24). Madde kapandı.

**Not (Claude Code oturumu):** Bu oturumda `dev server` açıp Playwright ile otomatik tarayıcı testi de denendi, ama ortamın ağ politikası tarayıcının Supabase host'una doğrudan bağlanmasını 403 ile engelledi (org policy denial, bypass edilmedi) — bu yüzden canlı doğrulama Berkin'in kendi tarayıcısında yapıldı.

### Doğrulama özeti
| Kontrol | Sonuç |
|---|---|
| `send-sms` COL map 13 event | ✅ `grep` ile doğrulandı |
| `subscription_new` SMS (gerçek Twilio) | ✅ Ahmet'e gönderildi, `net._http_response` id=42 |
| `subscription_accepted` SMS (gerçek Twilio) | ✅ Zeynep'e gönderildi, `net._http_response` id=43 |
| `send_subscription_harvest_reminders()` + pg_cron | ✅ Fonksiyon + jobid=2 kuruldu, manuel tetiklendi |
| Hatırlatma SMS (gerçek Twilio) | ✅ Ahmet'e gönderildi, `net._http_response` id=44, tercih+test verisi eski haline döndürüldü |
| MCP `create-subscription` crop/note/status fix | ✅ Diff ile doğrulandı |
| Storefront "Abone Ol" CTA | ✅ Diff ile doğrulandı |
| `buyer.producer.$id` guest-erişim guard istisnası | ✅ Diff ile doğrulandı (scope: sadece bu path) |
| `buyer.producer.$id` guest CTA metinleri | ✅ Diff ile doğrulandı |
| `BuyerShell`'in guest'te hatasız render olduğu | ✅ Manuel QA ile doğrulandı (Berkin, 2026-07-24) |

---

## 🟡 AĞUSTOS 2026 — Soft Launch
(Değişmedi)

## 🔵 SEZONLUK ÜRÜN YÖNETİMİ / ⬜ SONRAKI FAZLAR

> **Not (2026-07-25):** Bu bölüm eskiden "SAFRAN SEZONU" olarak adlandırılıyordu — ama sezonluk hasat mekanizması (fiyat bandı, "Hasat Dönemi" chip'i, komisyon vb.) safran'a özel değil, `crop_config.harvest_window_start_month/end_month` ile tanımlı HER crop için aynı şekilde çalışıyor (fındık-Ağustos, kayısı-Haziran, elma-Eylül/Kasım vb.). Safran sadece Ekim-Kasım'da hasat eden bir crop, platformun "sezon" kavramının kendisi değil. Başlık bu netliği yansıtacak şekilde güncellendi; içerik (aşağıdaki maddeler) değişmedi.

(Değişmedi)

---

## 📋 Lovable/Supabase Prompt Yazma Kuralları

(1-100 önceki sürümde — devam:)
89-95. (Önceki turlarda eklendi — bkz. önceki sürüm.)
96. **[Önceki turda eklendi]** `plan_mode=true` isteği Lovable tarafında güvenilir bir şekilde uygulanmıyor — her turda `get_diff`/DB ile bağımsız doğrulama zorunlu.
97. **[Önceki turda eklendi]** Postgres'te `MIN()`/`MAX()` `uuid` tipinde tanımlı değil — yeni trigger'lar gerçek insert ile test edilmeli.
98. **[Önceki turda eklendi]** Bir "tek kaynak, tek doğruluk" öğesi (crop adı gibi) birden fazla temsille varsa, kanonik (yazma) formu sistemin en çok bağımlı olduğu tarafa göre seçilmeli.
99. **[Önceki turda eklendi]** Birim gibi bir alanın "aynı crop içinde tek aile" varsayımı doğruysa, toplama yaparken göz ardı etmek yerine `crop_config`'teki referansla gerçek dönüşüm yapılmalı.
100. **[Önceki turda eklendi]** Yeni bir akış inşa edilirken, kardeşi olan mevcut akış UI-seviyesinde değil veri-akışı seviyesinde de incelenmeli.
101. **[Bu turda eklendi] Bir önceki turda "gerçek Twilio testiyle doğrulanmış" bir düzeltme, sonraki bir turda (aynı dosyaya dokunan farklı bir çalışma sırasında) sessizce geri alınabilir/kaybolabilir — "bir kere doğrulandı" kalıcı bir garanti değil.** P20'de `send-sms` COL map'i 10 event'e çıkarılıp gerçek SMS'le doğrulanmıştı; P24'te aynı dosya 3 event'e dönmüş halde bulundu (muhtemelen Berkin'in ayrı bir Lovable oturumunda dosyanın üzerine yazılması). Ders: kritik bir entegrasyon noktasına (özellikle bir agent'ın serbestçe düzenleyebildiği paylaşılan bir dosyaya) tekrar dönüldüğünde, "zaten doğrulanmıştı" diye atlanmamalı, hızlı bir `grep`/durum kontrolüyle hâlâ doğru olduğu teyit edilmeli.
102. **[Bu turda eklendi] Bir sayfaya "CTA ekle" demek yeterli değildir — o CTA'nın sahibi olduğu sayfaya kullanıcının gerçekten *ulaşabildiği* doğrulanmalı.** Abonelik CTA'sı zaten `/buyer/producer/$id`'de vardı ama sayfaya giden hiçbir link yoktu (ölü bir uç), ayrıca sayfa giriş yapmamış kullanıcıya üst layout guard'ı tarafından hiç render ettirilmiyordu. Bir özelliğin "var" olması, kullanıcının ona *ulaşabilmesiyle* aynı şey değil — yeni bir CTA/özellik eklerken navigasyon zincirinin baştan sona (nereden geliniyor, hangi guard'lardan geçiliyor) izlenmesi gerekir.
103. **[Bu turda eklendi] Bir agent platformunun (Lovable) kredisi/kotası tükenebilir — bu durumda son yapılan değişikliğin "kod seviyesinde doğru" (diff temiz, tsgo geçti) olması, "çalışma zamanında hatasız" olduğu anlamına gelmez.** Kredi bittiğinde bir sonraki doğrulama adımı (bu turda: guest kullanıcıda component render testi) yapılamadan bırakılabilir — bu açıkça "doğrulanamadı, manuel QA gerekiyor" olarak işaretlenmeli, sessizce "tamamlandı" sayılmamalı.
104. *(mevcut — kullanıcı-akışı dilinde test case kuralı, aşağıda ayrı başlıkta)*
105. **[2026-07-28'de eklendi] `src/lib/core/` altındaki dosyalar `hasat-core` reposundan gelir — Lovable dahil hiç kimse orada düzenleme yapmaz.** Değişiklik `hasat-core`'da yapılır, iki repoya (web + mobil) PR ile iner. Her core dosyasının başında `// hasat-core — BU DOSYAYI BURADA DÜZENLEME` işareti ve her iki repoda hash manifest bulunur. Gerekçe: Lovable daha önce paylaşılan bir dosyanın üzerine yazdı (P24'te bulunan P20 regresyonu) — görünür işaret + drift kontrolü tam bu senaryoya karşıdır.
106. **[2026-07-28'de eklendi] İki client (web + mobil) varken, ikisinin de ihtiyaç duyduğu mantık client'a değil veritabanına (RPC/view) yazılır.** Gerekçe: `dispatch_sms` (SQL) ile `send-sms` (TS) arasındaki event eşlemesi iki kez saptı ve SMS'ler sessizce gitmedi (P20'de bulundu, P24'te tekrar bulundu). Tek doğruluk kaynağı iki runtime'a bölündüğünde sapma kaçınılmaz, sessiz ve tekrarlayandır. İkinci bir client bu riski ikiye katlar.
107. [2026-07-29'da eklendi] Bir prompt açıkça "öneriyi bana sun, kendi başına karar verme" dediğinde, karar verilmeden uygulamaya geçilmez. Uygulandıysa bile, kararın Berkin tarafından verildiği asla raporlanmaz — otonom alınan karar açıkça öyle etiketlenir. Gerekçe: 2026-07-28'de P22-G/E'de SMS içerik kararı onay alınmadan uygulanıp Berkin'e atfedildi; çıktı doğruydu ama denetim izi bozuldu ve "bu kararı kim verdi" sorusuna kalıcı olarak yanlış cevap üretecekti. Dış dünyaya Berkin adına giden içerikte (SMS, e-posta, store metni) provenance kritiktir.
108. [2026-07-29'da eklendi] Bir doküman/kural değişikliği "yapıldı" olarak raporlanırken, PR açıklamasında eklenen metnin birebir alıntısı bulunmalıdır. Gerekçe: 2026-07-28'de "kural #107 TODO.md'ye eklendi" raporlandı, Berkin bu iddiaya dayanarak merge etti, ancak kural dosyada yoktu — liste #106'da bitiyordu. Aynı turda ikinci bir yanlış tamamlanma raporu daha vardı. Alıntı zorunluluğu, iddianın diff açılmadan doğrulanabilmesini sağlar.
109. [2026-07-29'da eklendi] Lovable projesinde `main`'e merge edilmiş kod, **Publish'e basılmadan** yayınlanmış URL'e (`hasat.lovable.app`) inmez — Lovable'ın kendi düzenlemeleri de dahil. Bu yüzden tarayıcı QA'sından ÖNCE Publish yapılmalı, yoksa test edilen build merge edilmiş koddan günler geride olabilir. Gerekçe: 2026-07-29'da P22-G'nin QA'sında üç "defect" bulundu (satırlarda tarih görünmüyor, Bugün/Bu Hafta/Tümü filtresi süzmüyor, "Yaptım" formunda tarih seçici yok); üçü de kodda mevcuttu ve 18 saat önce merge edilmişti — test edilen yayınlanmış build o kadar geriydi. Yanlış teşhis konuldu ve P22-G'nin UI'ının gereksiz yere baştan yazılması riski doğdu. Ayırt edici ipucu: UI'da görülen hesap ile DB view'ının döndürdüğü değer çelişiyorsa (örn. view `is_overdue=false` derken ekran "1 gün gecikti" diyorsa), iki farklı kod sürümü çalışıyor demektir — önce Publish, sonra teşhis.
110. [2026-07-30'da eklendi] Bu Supabase projesinde yeni oluşturulan view'lar varsayılan olarak `anon` ve `authenticated` rollerine SELECT GRANT'i alıyor — mevcut 20 KPI view'ında bu grant yok, yani konvansiyon "yalnızca service_role". `security_invoker=true` ayarlamak YETERLİ DEĞİLDİR: o alttaki tabloların RLS'ini uygular, ama view'ın kendisine erişim hakkı ayrı bir katmandır. Her yeni view'de grant'ler açıkça REVOKE edilmeli VE `information_schema.role_table_grants` ile doğrulanmalı. P23-M4-a'da yakalandı; fark edilmeseydi tarif funnel verisi tüm kullanıcılara açık olacaktı.
111. [2026-07-30'da eklendi] `hasat-core/core/db/types.ts` canlı şemadan sessizce geri düşebilir — drift koruması core↔hedef tutarlılığını denetler, DB↔core tutarlılığını denetlemez. M4-c'de `recipes.rest_minutes` eklendi ama tip üretimi yenilenmedi; bayat tipler subtree ile hem web'e hem mobile indi ve drift check yeşil kaldı (üç kopya tutarlı biçimde yanlıştı). Her şema değişikliğinden sonra tip üretimi zorunludur; kalıcı çözüm hasat-core CI'ında `supabase gen types` çıktısını commit'lenmiş dosyayla karşılaştıran bir adımdır.
---

## 📌 Kararlar

(önceki tablo + eklenenler:)
| **P17-G tamamen tamamlandı (2026-07-21)** | 20 KPI view'ı + admin dashboard, canlı doğrulandı. |
| **P20 tamamlandı (2026-07-21)** | SMS bildirim genişletmesi — gerçek bir kırık bug bulunup düzeltildi, iki senaryo gerçek Twilio SMS'i ile doğrulandı. |
| **P21/P22/P23 serisi onaylandı (2026-07-23)** | Berkin'in el notlarından doğan Batch/Care-Journal/Recipe-App genişlemesi, derinlemesine denetimden geçirildi ve 7 kararla kesinleşti. |
| **P21 gerçek koda göre revize edildi (2026-07-23)** | Draft migration'ı yerine form/UX çözümü; Keşfet grouping "yeni kapasite" değil "düzenleme" olarak revize edildi. |
| **P21-A TAMAMLANDI (2026-07-23)** | Kontrollü batch açma akışı canlı doğrulandı; `min(uuid)` bug'ı ve crop case-mismatch bulunup düzeltildi, backfill yapıldı. |
| **P21-B+C TAMAMLANDI (2026-07-23)** | `offer_items` şeması, çoklu-batch Keşfet/ürün sayfası, satır-bazlı stok kontrolü, birim-uyuşmazlık trigger'ı, traceability RLS — hepsi gerçek veriyle doğrulandı. |
| **P24 — Abonelik denetimi tamamlandı (2026-07-23)** | `send-sms` regresyonu (P20'nin kaybolmuş hali) bulunup düzeltildi; otomatik hasat-hatırlatma cron job'u eklendi (Berkin kararı: otomatikleştir); fiyat kilidi bilinçli olarak öneri seviyesinde bırakıldı (Berkin kararı); 2 discoverability deliği (üretici profiline erişim yolu + guest-erişim) düzeltildi. Guest'te `BuyerShell` render'ı 2026-07-24'te Berkin tarafından manuel QA ile doğrulandı — P24 tamamen kapandı. |
| **P22-A — Care Journal şeması tamamlandı (2026-07-24)** | 4 tablo (`journal_themes`, `journal_entry_types`, `crop_journal_glossary`, `care_journal_entries`) Claude Code + Supabase MCP ile doğrudan uygulandı; RLS izolasyonu, sahte-preset engeli (CHECK constraint), başkası-adına-yazma engeli ve entry-type silme koruması gerçek insert/RLS simülasyonuyla test edildi, test verisi temizlendi. |
| **P22-B/C/D tamamlandı (2026-07-24)** | Özelleştir ekranı (Lovable), 70 crop × 204 satır glossary, iki-sekmeli Journal UI — hepsi PR #1 ile `main`'e merge edildi. |
| **P22-D parsel-bazlı gruplamayla düzeltildi (2026-07-24)** | Berkin'in geri bildirimi: bir parselde birden fazla ürün olabileceği için Rutin Bakım satırları eylem yerine parsele göre gruplanmalı, crop her satırda açıkça görünmeli. |
| **P22-E — Yeni ürün türü talep sistemi tamamlandı (2026-07-24)** | Çiftçi kataloğunda olmayan ürünü talep edebiliyor, buyer'ın katalog-dışı RFQ'su aynı kanalı tetikliyor — ikisi de Berkin'in telefonuna gerçek SMS ile ulaşıyor (mevcut kanıtlanmış Twilio altyapısı yeniden kullanıldı). Gerçek SMS testiyle doğrulandı. |
| **P22-F — Rutin Bakım günlükle birleştirildi (2026-07-24)** | Berkin kararı: "rutin bakım'daki işler, günlükteki işlerle aynı olmalı." Ayrı `care_journal_entries` tablosu kaldırıldı, rutin bakım "Yaptım" artık gerçek bir günlük (`harvest_entries`) kaydı oluşturuyor ve P21'in batch-bağlama mekanizmasını aynen kullanıyor. Gerçek testte birim-uyuşmazlığı bug'ı bulunup düzeltildi (kayıt, ürünün varsayılan birimi yerine bağlı olduğu batch'in biriminden yazılmalıydı). |
| **13 vs 14 odak crop sayımı — Berkin doğruladı (2026-07-30)** | M3'te otonom bırakılan açık soru kapandı: "13" Berkin'in kendi aritmetik hatasıydı, doğru sayı 14. `P23-Mobile.md` ve `DB-Schema.md` buna göre güncellendi. |
| **P23-M4 a/b'ye bölündü (2026-07-30)** | Public tarif yüzeyi (a) ile Talep Et akışı + admin heatmap + Gap #9 (b) ayrı turlarda yapılacak — tek PR'da üç yeni yüzeyin review yükünü büyütmemek için. Görev metni zaten bu ayrımı tanımlıyordu, burada sadece roadmap'e yansıtıldı. |
| **P23-M4-a tamamlandı (2026-07-30)** | `/tarifler` + `/tarifler/$slug` (SSR, misafire açık), malzeme kartı 3 durumu, `recipe_views` ölçümleme + `v_kpi_recipe_funnel_by_recipe`. `crop_requests.quantity`/`.unit` migration gerekmediği (zaten vardı) ve yeni view'a varsayılan olarak düşen anon/authenticated grant'i bulunup düzeltildi. Gerçek RLS simülasyonu + gerçek `min_order` yuvarlama testleriyle doğrulandı; canlı tarayıcı testi bu oturumun ağ kısıtı yüzünden Berkin'e kaldı. |
| **P23-M4 (b+c) tamamen kapandı — `_Context.md`'ye yansıtıldı (2026-07-30)** | M4-b (Talep Et + admin heatmap + Gap #9) ve M4-c (`cook_minutes` semantik düzeltmesi + SEO) daha önce ayrı turlarda tamamlanmıştı ama `_Context.md`'nin "Açık işler" satırı hâlâ "M4-b sırada" yazıyordu — M5-a turunda fark edilip düzeltildi. |
| **`SYNC_TOKEN` kapsamı `hasat-mobile`'a genişletildi — Berkin (2026-07-30)** | `hasat-core`'un ikinci subtree hedefi (`hasat-mobile`) için PAT'ın kapsamına eklendi; dual-target `sync-to-web.yml`/`drift-check.yml` artık her iki repoda da çalışabilir durumda. |
| **P23-M5-a tamamlandı (2026-07-30)** | `hasat-mobile` iskeleti (Expo 57 + Router + Nativewind + API 36) + `hasat-core`'un ikinci subtree hedefi (dual-target Action'lar + drift kör noktası kapandı) + tesisat (storage adapter + OTP + TanStack Query). Nohut Falafel `rest_minutes` içerik düzeltmesi + süre filtresi bulgusu/düzeltmesi ayrı iş olarak yapıldı. Statik doğrulama (bundle, API36 config, manifest hash, drift kasıtlı bozma) tamamlandı; canlı OTP girişi (web + mobil) bu oturumun ağ kısıtı Supabase host'unu engellediği için doğrulanamadı — Berkin'e kaldı. `hasat-core/db/types.ts`'te `rest_minutes` eksik olduğu (M4-c'den beri tip üretimi yenilenmemiş) bulundu, kapsam dışı bırakıldı. |
| **Apple Developer bireysel hesabına başvuruldu (Berkin, 2026-07-30/31)** | $99, şirketten bağımsız. Onay bekleniyor. Onay gelene kadar mobil doğrulama gerçek cihaz yerine iOS Simulator build + Appetize.io ile yapılacak (`eas.json`'daki yeni `simulator` profili) — Android tarafında da elde cihaz yok. Detay: `Build/Store-Compliance.md`, `Build/P23-Mobile.md` → "M5-a-ek". |
| **P23-M5-a-ek tamamlandı (2026-07-31)** | Ön koşul turu (M5-b öncesi): `hasat-core/core/db/types.ts` canlı şemadan yeniden üretildi (`recipes.rest_minutes` + 2 eksik KPI view eklendi) + kalıcı `types-freshness.yml` CI kontrolü (kural #111); `hasat-mobile/.env` içerik bekçisi (`EXPO_PUBLIC_` prefix + sır-kalıbı reddi) `drift-check.yml`'e eklendi, kasten bozulup geri alındı; `hasat-mobile/eas.json`'a Apple hesabı gerektirmeyen `simulator` build profili eklendi; AES anahtarının `expo-secure-store`'da (doğru yerde) tutulduğu doğrulandı, kod değiştirilmedi; `Build/E2E-QA.md` → S25'in B bölümü gerçek cihaz/Expo Go varsayımından Appetize.io'ya çevrildi. |

---

## 📋 Son Test Sonuçları

### P24 Tam Doğrulama (2026-07-23) — bkz. yukarıdaki "🟡 P24" bölümündeki doğrulama tablosu.

### P21-A + P21-B+C Tam Doğrulama (2026-07-23) ✅
| Kontrol | Sonuç |
|---|---|
| `tg_harvest_entries_after_insert_autolink` — tek eşleşme (autolink çalışır) | ✅ Gerçek veriyle test edildi |
| `tg_harvest_entries_after_insert_autolink` — çoklu eşleşme (autolink pas geçer) | ✅ Gerçek veriyle test edildi |
| `min(uuid)` bug'ı | ✅ Bulundu, subquery'e çevrilip düzeltildi, yeniden test edildi |
| Crop case-mismatch backfill (`listings`/`harvest_entries`/`parcels.crops`) | ✅ Safran tek slug'da birleşti, doğrulandı |
| `useCropOptions()` artık slug döndürüyor | ✅ Diff ile doğrulandı |
| `offer_items` şeması + RLS | ✅ `information_schema`/`pg_policies` ile doğrulandı |
| `enforce_offer_stock` — çoklu-batch, yeterli stok | ✅ Kabul başarılı |
| `enforce_offer_stock` — çoklu-batch, yetersiz stok | ✅ "Stok yetersiz (batch)" hatası doğru fırlatıldı |
| `tg_enforce_link_unit_match` — birim uyuşmazlığı | ✅ Hata doğru fırlatıldı (kg→g reddedildi, kg→kg geçti) |
| `convertQuantity` (g↔kg) — Keşfet grup kartı toplamı | ✅ Diff + manuel hesaplama ile doğrulandı (500g+100kg → 100.500g) |
| MCP `create-offer` → `offer_items` insert | ✅ Kontrol edildi, eksikti, eklendi |
| Farmer `OfferBatchBreakdown` (`OfferCard`+`OrderCard`) | ✅ Diff ile doğrulandı |
| Test verisi temizliği | ✅ Tüm test offer/offer_items/harvest_entries/listing kayıtları silindi |

### P20 Tam Doğrulama (2026-07-21) ✅
(Değişmedi — bkz. önceki sürüm — **not: COL map kısmı P24'te tekrar bozulmuş halde bulundu, yeniden düzeltildi, bkz. P24**)

### P17-G Tam Doğrulama (2026-07-21) ✅
(Değişmedi — bkz. önceki sürüm)

### P17-E Tam Doğrulama (2026-07-21) ✅
(Değişmedi — bkz. önceki sürüm)

### (Önceki tüm test sonuçları — değişmedi, önceki sürümlerde)

----------------------------
New PLAN FROM BERKİN NOTES

*(Bu bölüm — orijinal "Batch/Care Journal/Buyer Genişlemesi: Netleşen Kapsamlı Plan" taslağı — artık tamamen kapanmış P21-A ve P21-B+C işlerinin geçmiş kaydı olarak korunuyor, referans amaçlı. Güncel/kalıcı durum yukarıdaki "🟤 P21" bölümünde ve aşağıdaki "Onaylanan Yol Haritası"nda.)*

## Cevaplarının özeti ve aldığım kararlar

| # | Senin cevabın | Aldığım mimari karar |
|---|---|---|
| 1 | Care journal ayrı, en kolay/anlaşılır neyse öyle | Ayrı tablo (`care_journal_entries`) + tek "Journal" sayfasında iki sekme |
| 2 | Draft migration'ı bana bırak, önceliklendir | Kontrol ettim, gerçek boşluk doğrulandı → **P0, ilk iş** (sonradan revize: migration değil form/UX işi — TAMAMLANDI) |
| 3 | Şimdilik tek offer, tıklayınca batch'ler ayrı görünsün | `offer_items` ara tablosu — TAMAMLANDI |
| 4 | Sold/expired batch geçmiş siparişten hâlâ görülebilsin | RLS ile TAMAMLANDI |
| 5 | Eatr + ReciMe | Araştırıldı, P23 girdisi olarak beklemede |
| 6 | Aynı çiftçinin aynı crop'ta birden fazla batch'i → Keşfet'te ikisi de görünsün, buyer stok görüp seçsin, toplam fiyat değişsin | TAMAMLANDI |
| 7 | Recipe App şimdilik herkese açık | P23 kapsamında, henüz başlanmadı |

*(Detaylı şema/tasarım metni — BÖLÜM A/B/C — bu turdan itibaren tarihi kayıt, güncel implementasyon yukarıdaki "🟤 P21" bölümünde özetlenmiştir, tekrar edilmiyor.)*

----------------------------

## 🟤 Onaylanan Yol Haritası — P21/P22/P23 *(P21 TAMAMLANDI — 2026-07-23; P22/P23 hâlâ planlı)*

> P21'in tüm alt maddeleri tamamlandı (bkz. yukarıdaki "🟤 P21 — Batch & Vitrin Çoklu-Batch Mimarisi" bölümü). Bu bölümdeki tablo artık P22/P23 için aktif takip listesi.

### P21 — Batch & Vitrin Çoklu-Batch Mimarisi — ✅ TAMAMLANDI

| Kod | Konu | Durum |
|---|---|---|
| P21-A | Batch açma akışı — engelle + bağlamaya yönlendir | ✅ TAMAMLANDI |
| P21-B | Çoklu-batch Keşfet/ürün detay sayfası | ✅ TAMAMLANDI |
| P21-C | Multi-batch tek offer şeması (`offer_items`) | ✅ TAMAMLANDI |
| P21-D | Stok kontrolünü `offer_items`'a uyarlama + birim tutarlılığı | ✅ TAMAMLANDI |
| P21-E | Traceability RLS garantisi | ✅ TAMAMLANDI |
| P21-G | Vitrin/Keşfet stok gösterim tutarlılığı (birim dönüşümü dahil) | ✅ TAMAMLANDI |
| P21-H | Çoklu-batch akışının teslimat/tarih/ödeme-parity + MCP create-offer + farmer breakdown eksikleri | ✅ TAMAMLANDI |

### P22 — Care Journal (Rutin Bakım) (P1, saffron sezonuna kadar esnek) — ✅ Tamamlandı, `main`'de (2026-07-24), tarayıcı QA bekliyor

Berkin kararı (1. cevap): `harvest_entries`'ten (hasat olayı) ayrı bir tablo. Tek "Journal" sayfasında iki sekme: **Rutin Bakım** (yeni, Bevel-tarzı toggle) + **Hasat Kayıtları** (mevcut `harvest_entries` formu).

| Kod | Konu | Kapsam | Bağımlılık | Durum |
|---|---|---|---|---|
| P22-A | Care Journal şeması | 4 yeni tablo: `journal_entry_types`, `journal_themes`, `crop_journal_glossary`, `care_journal_entries` (`listing_id` FK ile batch bağlantısı — P21 sayesinde artık hangi batch'e bağlanacağı net) | Yok | ✅ **TAMAMLANDI (2026-07-24)** |
| P22-B | Customize Journal ekranı (Bevel referansı) | Preset+custom entry type CRUD, kategori sekmeleri, frequency/threshold modalı | P22-A | ✅ **TAMAMLANDI (2026-07-24)** |
| P22-C | Crop Glossary üretimi | AI ile tek seferlik paragraf-tooltip üretimi | P22-A | ✅ **TAMAMLANDI (2026-07-24)** |
| P22-D | Journal sayfası UI | İki sekme, parsel-bazlı liste, tarih filtresi | P22-B, P22-C | ✅ **TAMAMLANDI (2026-07-24, Claude Code doğrudan — Lovable kredisi bittiği için)** |
| P22-E | Yeni ürün türü talep sistemi | Çiftçi kataloğunda olmayan ürünü talep eder, buyer'ın katalog-dışı RFQ'su da aynı kanalı tetikler → Berkin'e SMS | P22-A | ✅ **TAMAMLANDI (2026-07-24)** |
| P22-F | Rutin Bakım'ı günlükle birleştir | Rutin bakım "yapıldı" → gerçek günlük (harvest_entries) kaydı, ayrı `care_journal_entries` tablosu kaldırıldı | P22-D | ✅ **TAMAMLANDI (2026-07-24, Berkin'in mimari kararı)** |

### ✅ P22-A — Care Journal Şeması — TAMAMLANDI *(2026-07-24, Claude Code + Supabase MCP ile doğrudan)*

**Yapılanlar (migration `20260724080401_p22a_care_journal_schema`):**
1. **`journal_themes`** — Bevel-tarzı üst kategori (Sulama, Beslenme, Hastalık&Zararlı vb.). `crop` nullable FK (`crop_config.crop`) — null=genel tema, dolu=crop'a özel. Herkese (anon+authenticated) SELECT açık.
2. **`journal_entry_types`** — Tema içindeki spesifik bakım eylemi. `farmer_id` nullable (null=global preset, dolu=çiftçiye özel custom). CHECK constraint: `is_preset=true ⟺ farmer_id IS NULL` — tutarlılık DB seviyesinde garanti. RLS: preset'ler herkese açık, custom'lar sadece sahibine; çiftçi kendi custom'ını CRUD edebilir ama sahte preset üretemez (`with_check`'te `is_preset=false` zorunlu).
3. **`crop_journal_glossary`** — Crop+terim başına açıklama metni (P22-C'de AI ile doldurulacak). `unique(crop, term)`, herkese SELECT açık.
4. **`care_journal_entries`** — Asıl bakım kaydı. `farmer_id`/`parcel_id`/`crop` NOT NULL, `listing_id` nullable (batch bağlantısı), `entry_type_id` FK `on delete restrict` (kullanılan bir entry type silinemez). RLS: tamamen çiftçiye özel CRUD (`auth.uid()=farmer_id`).
5. Ortak `set_updated_at()` trigger fonksiyonu (public schema) yazıldı — mevcut `update_updated_at_column()` fonksiyonunun `storage` şemasında (Supabase internal) olduğu, app tablolarında kullanılamayacağı bulundu.

**Doğrulama (gerçek insert + RLS simülasyonu, Ahmet Yılmaz'ın verisiyle):**
- Preset tema+entry type insert ✅; sahte preset (`is_preset=true` + `farmer_id` dolu) insert denemesi → CHECK constraint doğru reddetti ✅.
- Gerçek `care_journal_entries` kaydı (Ahmet'in Güney Bahçe parseli + aktif safran listing'i) insert edildi, `updated_at` trigger'ı UPDATE'te doğru tetiklendi ✅.
- Kullanımdaki bir `entry_type` silinmeye çalışıldı → `on delete restrict` doğru engelledi ✅.
- RLS izolasyonu: başka bir çiftçi (Zehra) Ahmet'in kayıtlarını göremedi (0 satır) ✅, Ahmet kendi kaydını gördü (1 satır) ✅, anon preset tema/entry-type'ı gördü ama `care_journal_entries`'ten hiçbir şey göremedi (0 satır) ✅.
- Yazma güvenliği: çiftçi kendi custom entry type'ını ekleyebildi ✅, sahte preset eklemeye çalışınca RLS reddetti ✅, başka bir çiftçi adına `care_journal_entries` yazmaya çalışınca RLS reddetti ✅.
- Tüm test verisi temizlendi (4 tablo da 0 satır olarak doğrulandı).

**Ek:** `farmer_journal_prefs` tablosu (P22-B ihtiyacı için, aynı gün) — çiftçinin hangi bakım eylemini takip ettiği + kendi sıklık/eşik tercihi. Ayrı tablo yaklaşımı Berkin ile netleştirildi (preset'ler ortak kalır, çiftçi tercihi ayrı tutulur — kopyalama yerine). RLS izolasyonu, unique constraint (`farmer_id, entry_type_id`), toggle update ve `updated_at` trigger'ı gerçek insert testiyle doğrulandı.

**Ek:** Preset seed verisi — 5 tema (Sulama, Beslenme, Hastalık & Zararlı, Budama & Bakım, Hasat Hazırlığı & Sonrası), 13 eylem, **hepsi `crop=NULL`** (crop-agnostic — platformdaki 70+ crop'un hepsinde aynı liste geçerli, safran'a özel değil; Berkin'in uyarısıyla revize edildi). Gerçek sorguyla doğrulandı.

### ✅ P22-B — Rutin Bakımı Özelleştir Ekranı — TAMAMLANDI *(2026-07-24, Lovable)*

**Neden Lovable:** `hasat-d2c-marketplace`'in `main` branch'i Lovable'ın GitHub-sync bot'u (`gpt-engineer-app[bot]`) tarafından yönetiliyor — canlı önizleme sadece Lovable üzerinden değişince güncelleniyor, Claude Code'un git erişimi ayrı bir branch'te. Bu yüzden FE işi Lovable'a gönderildi (DB tamamen Claude Code + Supabase MCP ile hazırlandıktan sonra).

**Yapılanlar (tek mesajda kapsamlı spec, tek turda tamamlandı):**
- Yeni route `/farmer/journal/customize` — kategori sekmeleri (`journal_themes`), her sekmede eylem listesi + toggle switch.
- Toggle ON → sıklık/not seçim sheet'i açılıyor (`farmer_journal_prefs` upsert). Toggle OFF → direkt `is_active=false` (geçmiş korunuyor).
- "+ Kendi Bakım Eylemini Ekle" — custom `journal_entry_types` insert + otomatik `farmer_journal_prefs` aktifleştirme.
- 5 yeni React Query hook'u (`useJournalThemes`, `useJournalEntryTypes`, `useFarmerJournalPrefs`, `useToggleJournalPref`, `useCreateCustomEntryType`), `farmer.journal.index.tsx`'e giriş linki.
- Commit: `6810a4e0` ("Rutin özelleştirme eklendi").

**Bağımsız doğrulama (Lovable'ın "typecheck temiz" iddiasına güvenilmedi):**
- `get_diff` ile gerçek diff okundu — sadece beklenen 4 dosya değişmiş (`queries.ts`, yeni `farmer.journal.customize.tsx`, `farmer.journal.index.tsx`, otomatik `routeTree.gen.ts`/`types.ts`).
- `main` branch ayrı bir git worktree'ye çekilip **gerçek `tsc --noEmit` çalıştırıldı** (Lovable'ın kullandığı `bunx tsgo` bu ortamda private registry'ye takıldığı için standart `tsc` ile eşdeğer doğrulama yapıldı) — hatasız geçti.
- FE'nin kullandığı sorgu kalıplarının (OR filtresi, `upsert(onConflict:"farmer_id,entry_type_id")`, custom-entry-type + otomatik-pref insert zinciri) gerçek SQL karşılıklarıyla Ahmet'in verisiyle test edildi — hepsi doğru çalıştı (toggle ON/OFF, frequency_days korunumu, custom ekleme). Test verisi temizlendi.

**⚠️ Kalan doğrulama:** Kod-seviyesinde (diff + typecheck + SQL-eşdeğeri sorgu testi) tamamen doğrulandı, ama gerçek tarayıcıda görsel/dokunma testi yapılmadı — bu Claude Code oturumunun ağ politikası tarayıcının Supabase'e ulaşmasını engelliyor (bkz. yukarıdaki guest-QA notu). **Berkin'in manuel QA'sı gerekiyor:** `/farmer/journal`'a girip "⚙️ Rutin Bakımı Özelleştir"e tıkla, bir kategori sekmesinde bir eylemi aç (sıklık seç, kaydet), sayfayı yenile (tercih korunmalı), bir eylemi kapat, "Kendi Bakım Eylemini Ekle" ile özel bir eylem ekle (listede görünmeli + otomatik açık olmalı).

**Not (P21'den miras):** Crop adlandırması artık her yerde `crop_config.crop` kanonik slug'ı — P22'nin yeni tabloları da `crop text NOT NULL REFERENCES crop_config(crop)` FK'sini olduğu gibi kullanabilir, ek bir case-normalizasyon riski yok (P21-A'da temizlendi).

### ✅ P22-C — Crop Glossary Üretimi — TAMAMLANDI *(2026-07-24, Claude Code + Supabase MCP)*

**Kapsam:** `crop_journal_glossary` tablosuna (P22-A'da açılmıştı, boştu) her crop'un `crop_config.lifecycle_steps`'indeki HER adım için bir açıklama paragrafı yazıldı — toplam **70 crop × ilgili adımlar = 204 satır**, tam kapsama (eksik yok, SQL ile doğrulandı). Safran ve Safran Soğanı (platformun amiral gemisi) en detaylı/pratik içeriği aldı (somut sıcaklık/süre/derinlik eşikleri), diğer 68 crop kısa-öz ama yine de somut pratik bilgi (zamanlama, aralık, eşik) içeriyor — Berkin'in isteğiyle sadece safran değil tüm crop'lar detaylı/pratik tonda yazıldı.

**İçerik yaklaşımı:** Terim = `crop_config.lifecycle_steps[].label` (örn. "Bakım", "Hasat", safran'a özel "Korm/Dikim" gibi). Böylece P22-D'de (Journal UI) bir bakım adımı gösterilirken `crop`+`step.label` ile doğrudan glossary'e join yapılabilir, ekstra bir eşleme katmanı gerekmiyor.

**Doğrulama:**
- Her satırın `term`'i, ilgili crop'un `lifecycle_steps` etiketleriyle SQL ile karşılaştırılıp tam eşleştiği doğrulandı (yazım hatası/typo riski elendi).
- Coverage sorgusu: her crop_config satırının HER lifecycle_step'i için bir glossary satırı olduğu doğrulandı — **204/204, eksik yok.**
- Anon RLS okuma testi geçti (guest kullanıcı da görebiliyor, P24'teki gibi guest-erişim senaryolarında sorun çıkmaz).
- 4 migration'a bölünerek uygulandı (baharat/tıbbi bitki; meyve/sert kabuklu; tahıl/baklagil/sebze/yumru/yağlık/endüstri bitkisi; özel adım kombinasyonları), her biri sonrasında satır sayısı/kapsama kontrol edildi.

**Not (içerik doğruluğu):** Sayısal eşikler (sıcaklık, süre, derinlik, mesafe) genel agronomik pratiklere dayanıyor — Türkiye'nin farklı bölgeleri (iklim/toprak) için küçük sapmalar olabilir. Kritik olmayan yerlerde "genellikle/yaklaşık" gibi yumuşatıcı ifadeler kullanıldı. Bu içerik bir AI tarafından (bölgesel doğrulama olmadan) üretildi — canlıya çıkmadan önce en azından öncelikli crop'larda (safran + platformda en çok işlem gören birkaç crop) bir insan gözden geçirmesi faydalı olur, ama P22-D'nin geliştirilmesini bloklamaz.

### ✅ P22-D — Journal Sayfası UI — TAMAMLANDI *(2026-07-24, Claude Code doğrudan; sonradan iki kez düzeltildi — bkz. altta)*

**Neden Lovable değil, Claude Code:** Lovable workspace kredisi bugün için tükendi (Berkin'in isteğiyle). P22-B'de kurulan "main branch Lovable'ın GitHub-sync'ine bağlı" kısıtı hâlâ geçerli — bu yüzden bu değişiklik **`main`'e değil, `claude/hasat-environment-inventory-ft0ehg` feature branch'ine** commit edildi.

**Yapılanlar (ilk sürüm):**
- `/farmer/journal` iki sekmeli: **Hasat Kayıtları** (mevcut içerik, davranışı hiç değişmedi) + **Rutin Bakım** (yeni). Varsayılan aktif sekme Hasat Kayıtları.
- **"ℹ️ [Crop] Hakkında"** açılır panel(ler) — çiftçinin her crop'u için glossary içeriğini bakım adımı sırasına göre gösterir.
- "⚙️ Rutin Bakımı Özelleştir" linki Rutin Bakım sekmesine taşındı.

**Sonradan iki düzeltme (Berkin'in geri bildirimiyle, PR #3'te):**
1. **Parsel bazlı gruplama** — ilk sürümde satırlar bakım eylemine göre gruplanıyordu, Berkin bir parselde birden fazla ürün olabileceğini belirtti ("hangi crop type için olduğu explicit olsun"). Artık satırlar **parsel başlığı altında**, her satırda crop adı rozet olarak görünüyor.
2. **Rutin Bakım = Günlüğe kolay giriş** (P22-F, bkz. altta) — Berkin'in kararıyla mimari değişti: "Yaptım" artık ayrı bir tabloya değil, doğrudan çiftçinin günlüğüne (Hasat Kayıtları sekmesinde de görünen gerçek bir kayıt) yazıyor. Bunun yan etkisi olarak takvim görünümü kaldırıldı (artık gereksiz — kayıt zaten Hasat Kayıtları'nda görünüyor) ve "Sadece gecikmiş" filtresi yerine 3 seçenekli bir **tarih filtresi** (Bugün / Bu Hafta / Tümü, varsayılan Bu Hafta) geldi: gecikmiş ve hiç yapılmamış işler filtreden bağımsız her zaman görünür, sıklığı olmayan (olay bazlı) işler her zaman görünür, sıklığa bağlı işler seçili pencereye göre süzülür.

**Doğrulama:** Gerçek `tsc --noEmit` her iki düzeltmede de hatasız geçti. Parsel gruplaması ve tarih filtresi mantığı, aynı parselde iki farklı crop'un bağımsız takip edildiği gerçek insert'lerle test edildi. **Tam tarayıcı/dokunma testi bu oturumun ağ kısıtlaması (Supabase'e 403) yüzünden yapılamadı** — aşağıdaki test case ile Berkin'in doğrulaması gerekiyor.

### ✅ P22-E — Yeni Ürün Türü Talep Sistemi — TAMAMLANDI *(2026-07-24, Claude Code doğrudan, PR #3)*

**Neden:** Berkin, giriş yapmadan görülen ana sayfanın en altındaki "Yetiştirdiğiniz bir ürün kataloğumuzda yok mu?" benzeri bir talep formunun gerçekten kendisine (SMS ile) ulaşıp ulaşmadığını sordu. Araştırma sonucu: böyle bir form vardı ama sadece buyer tarafı için ("Ürün Talep Et", mevcut RFQ akışı) ve bildirim kanalı hâlâ e-posta idi, SMS değildi. Çiftçi tarafında ise kendi yetiştirdiği ama platform kataloğunda olmayan bir ürünü bildirecek hiçbir yol yoktu.

**Kullanıcı akışı — Çiftçi tarafı (yeni):**
- Parsel oluştururken ürün listesinde aradığını bulamayan bir çiftçi, "Yetiştirdiğiniz ürün yok mu? Talep edin" seçeneğine tıklar.
- Açılan formda ürün adı, birim (kg/g/L/adet), kategori, hasat ayı/ayları ve yetiştirme notları girer, gönderir.
- Gönderim anında Berkin'in gerçek telefonuna (kendi onayladığı numara) anında bir SMS gider — talebi hemen görür.

**Kullanıcı akışı — Buyer tarafı (mevcut akış korunup genişletildi):**
- Buyer "Ürün Talep Et" formunu (Keşfet sayfası / onboarding) doldurup gönderdiğinde, istediği ürün platform kataloğunda **hiç yoksa**, bu da aynı SMS'i tetikler (gerçek bir "katalog boşluğu" sinyali). Ürün kataloğunda zaten varsa (sadece o an stokta yoksa) SMS gitmez, sadece talep kaydedilir — gereksiz SMS trafiği olmasın diye.
- Form metinleri buyer'a daha uygun bir dille güncellendi.

**BE'de ne değişti:** Çiftçi tarafı için yeni bir talep tablosu (her çiftçi sadece kendi talebini görebiliyor); yeni bir bildirim fonksiyonu (mevcut, daha önce kanıtlanmış SMS altyapısını kullanıyor); buyer'ın mevcut talep akışına "kataloğa hiç yok mu?" kontrolü eklendi.

**Doğrulama:** Gerçek bir test talebi (çiftçi + buyer, ikisi de) gönderildi, Berkin'in numarasına gerçek SMS ulaştığı (Twilio "kabul edildi" durumu) doğrulandı, test verisi silindi. Başka bir çiftçinin bir çiftçinin talebini göremediği doğrulandı.

### ✅ P22-F — Rutin Bakım'ı Günlükle Birleştirme — TAMAMLANDI *(2026-07-24, Claude Code doğrudan, PR #3, Berkin'in mimari kararı)*

**Berkin'in isteği (özetle):** "Rutin bakım'daki işler, günlükteki işlerle aynı olmalı; rutin bakım menüsü aslında kolay günlük eklemek için gibi düşünülmeli." Yani Rutin Bakım sekmesi ayrı bir defter değil, günlüğe (Hasat Kayıtları) hızlı giriş yapmanın bir yolu olmalı.

**Kullanıcı akışı — ne değişti:**
- Bir çiftçi Rutin Bakım sekmesinde bir işi "Yaptım" dediğinde, artık bu işlem **doğrudan Hasat Kayıtları sekmesinde de görünen gerçek bir günlük kaydı** oluşturuyor (aynı parsel, aynı ürün, varsa ilgili batch'e bağlı). Eskiden bu ayrı, görünmez bir tabloya yazılıyordu — çiftçi bunu Hasat Kayıtları'nda hiç göremiyordu.
- Bir kez "Yaptım" dendikten sonra, o iş seçili tarih filtresine göre (Bugün/Bu Hafta/Tümü) sıradaki zamanı gelene kadar bekleyen listeden düşüyor — yani liste gerçekten "bugün ne yapmam lazım" sorusuna cevap veriyor, geçmiş işlerle dolup taşmıyor.
- Özelleştir ekranında bir bakım işi zaten açıksa (toggle ON), yanına eklenen **⚙️ ayarlar butonu** ile sıklık/not tekrar açılabiliyor — önceden bunu değiştirmek için toggle'ı kapatıp tekrar açmak gerekiyordu, artık gerekmiyor.

**BE'de ne değişti:** Ayrı "rutin bakım kaydı" tablosu tamamen kaldırıldı (hiç gerçek/canlı veri yoktu, sadece test amaçlı birkaç satır — silinmeden önce doğrulanıp temizlendi). Bunun yerine mevcut günlük tablosuna, hangi rutin bakım işinden oluştuğunu işaretleyen bir referans kolonu eklendi. Böylece P21'de kurulan "günlük kaydı → doğru batch'e otomatik bağlanma" mekanizması rutin bakım kayıtları için de aynen çalışıyor, ayrı bir kod yolu gerekmedi.

**Doğrulama:** Gerçek kayıt oluşturma testinde bir hata bulundu ve düzeltildi — sistem başta işin birimini ürünün varsayılan birimine göre yazıyordu, ama bağlı olduğu batch farklı bir birimdeyse (örn. batch kg, varsayılan g) kayıt reddediliyordu (P21'de kurulan "birim uyuşmazlığı" koruması tam da bunu yakalamak için vardı). Düzeltme: birim artık her zaman bağlanılan batch'in kendi biriminden alınıyor. kg birimli gerçek bir batch ile yeniden test edilip doğru çalıştığı doğrulandı, `tsc --noEmit` temiz.

### ✅ P22-F'nin yan etkileri — sonradan tarandı ve 4 gerçek regresyon bulunup düzeltildi *(2026-07-24, PR #3 merge edildikten sonra)*

**Neden bu tarama yapıldı:** Rutin bakım kayıtları artık gerçek günlük (`harvest_entries`) satırları olduğu için (miktar=0, fiyat=0), bu tabloyu okuyan HER yeri (sadece Günlük sayfasını değil) tek tek gözden geçirdik — çünkü `harvest_entries` sadece Günlük sayfasında değil, AI sohbet asistanında, stok/parti sayfalarında ve izlenebilirlik (traceability) rozetinde de kullanılıyor.

**Bulunan ve düzeltilen 4 gerçek hata (kodda, filtre eksikliğinden):**
1. **AI sohbet asistanı** — çiftçi AI'ya bir şey sorduğunda arka planda gönderilen "Son hasatların" bağlamı, artık rutin bakım kayıtlarını da (örn. "Sulama Yap, 0 kg") gerçek hasatmış gibi gösteriyordu. Gerçek bir test kaydıyla doğrulandı: en son eklenen bir "sulama" kaydı, listenin EN ÜSTÜNE çıkıp gerçek hasatları bağlamdan tamamen dışarı itiyordu (bağlam sadece son 5 kayıtla sınırlı). **Düzeltildi** — bu sorgu artık sadece gerçek hasat kayıtlarını alıyor.
2. **AI'nın "günlük kaydı ekle" aracı (MCP `list_recent_harvests`)** — aynı sorun, çiftçi "son hasatlarım ne?" diye sorduğunda AI'ya yanlış veri dönüyordu. **Düzeltildi.**
3. **AI sohbetinden günlük kaydı eklerken "bu tarihte zaten kayıt var" kontrolü** — bir çiftçi aynı gün hem sulama yapıp "Yaptım" dese hem de AI'ya o günün hasadını yazdırsa, sistem yanlışlıkla sulama kaydını "mevcut kayıt" sayıp üzerine yazma seçeneği sunuyordu. **Düzeltildi.**
4. **Parti (batch) detay sayfasındaki "Hasat Kayıtları" listesi** (çiftçinin kendi görünümü) — bir rutin bakım kaydı bu partiye bağlanmışsa, "0 kg · Kalite A" gibi anlamsız bir satır gösteriyordu. **Düzeltildi** — miktar 0 ise artık miktar hiç gösterilmiyor (Günlük sayfasındaki kartlarda zaten olan davranışla aynı hale getirildi).

**Kontrol edilip GÜVENLİ bulunan yerler (düzeltme gerekmedi):**
- **Stok hesaplama** (bir teklif kabul edilirken "bu partide yeterli stok var mı" kontrolü) — bu hesap partiye bağlı kayıtların miktarını TOPLAR; 0 miktarlı bir rutin bakım kaydı toplamı değiştirmiyor. Gerçek bir test kaydıyla doğrulandı (2.5 kg gerçek hasat + 0 kg rutin bakım aynı partiye bağlandı, toplam hâlâ doğru şekilde 2.5 kg).
- **Otomatik hasat hatırlatma SMS'i** (abonelik sistemi, P24) — bu tamamen farklı bir tablodan (`harvest_subscriptions`) besleniyor, `harvest_entries`'e hiç bakmıyor. Etkilenmiyor.
- **KPI/analitik view'ları** (P17-G) — hiçbiri `harvest_entries`'i doğrudan okumuyor. Etkilenmiyor.

**Ürün sorusu — Berkin karar verdi (2026-07-24): olduğu gibi kalsın.** Buyer'ın gördüğü **"Ürün Geçmişi" zaman çizelgesi ve "İzlenebilirlik" rozeti**, bir partiye bağlı TÜM günlük kayıtlarının hangi bakım adımlarını (dikim/bakım/hasat/kurutma vb.) kapsadığına bakarak hesaplanıyor. Rutin bakım kayıtları da artık bu hesaba dahil oluyor — yani bir çiftçi tek dokunuşla (fotoğrafsız, miktarsız) bir "Sulama Yap" kaydı düşürdüğünde, partinin izlenebilirlik rozeti (örn. "Temel" → "İyi Belgelenmiş") yükselebilir. Berkin bunu istenen bir sonuç olarak onayladı (daha kolay günlük tutma = daha fazla belgeleme, tam da P22-F'nin amacı) — kod tarafında ek bir değişiklik yapılmadı.

**Ayrıca küçük bir kozmetik not:** Rutin bakım kayıtları buyer'ın gördüğü zaman çizelgesinde her zaman "Kalite A" ile görünüyor (rutin bakım formunda kalite alanı yok, sabit A gönderiliyor) — bir sulama kaydında "Kalite A" yazması anlamsız ama zararsız; düzeltme gerektirmiyor, sadece not.

### PR durumu
[PR #1](https://github.com/berkinsavciozen/hasat-d2c-marketplace/pull/1) `main`'e merge edildi (2026-07-24) — P22-A/B/C/D `main`'de.
[PR #3](https://github.com/berkinsavciozen/hasat-d2c-marketplace/pull/3) `main`'e merge edildi (2026-07-24) — P22-D'nin parsel-gruplama düzeltmesi + P22-E + P22-F artık `main`'de.
[PR #4](https://github.com/berkinsavciozen/hasat-d2c-marketplace/pull/4) `main`'e merge edildi (2026-07-24) — P22-F'nin yan etki düzeltmeleri (`d504d00`) artık `main`'de. **Tüm P22 serisi artık tek parça halinde `main`'de, tarayıcı QA'ya hazır.**

### ⚠️ Kalan iş: P22-D+E+F birleşik tarayıcı QA (test hesabı: çiftçi `05001234567`, OTP `123456`)

1. Giriş yap, **Günlük** sayfasına git (`/farmer/journal`) — iki sekme görmelisin: **Hasat Kayıtları** (varsayılan aktif) + **Rutin Bakım**.
2. Hasat Kayıtları sekmesinde hiçbir şeyin bozulmadığını doğrula (istatistik barı, kayıt listesi, "+ Yeni Kayıt").
3. **Rutin Bakım** sekmesine geç — satırların artık **parsel adı başlığı altında** gruplandığını, her satırda hangi ürün için olduğunun bir rozetle (örn. "Safran") göründüğünü doğrula (eğer bir parselde birden fazla ürün varsa, o parselin altında her ürün için ayrı satırlar görmelisin).
4. Üstte bir **tarih filtresi** görmelisin: Bugün / Bu Hafta / Tümü. Varsayılan "Bu Hafta" seçili olmalı.
5. Bir satırda **"✓ Yaptım"**a bas, açılan formda gerekirse notu doldurup gönder.
6. Ekrana geri döndüğünde: az önce yaptığın iş, sıradaki zamanı bu hafta içine düşmüyorsa listeden **kaybolmalı** (tarih filtresine göre artık "bekleyen" değil). Filtreyi "Tümü" yaparsan tekrar görünmeli.
7. Aynı sekmede **Hasat Kayıtları**'na geç — az önce "Yaptım" dediğin işin, burada da **gerçek bir günlük kaydı olarak** göründüğünü doğrula (bu P22-F'nin asıl testi: rutin bakım artık günlükten ayrı değil).
8. **"⚙️ Rutin Bakımı Özelleştir"** ekranına git. Zaten açık (toggle ON) olan bir işin yanında artık küçük bir **⚙️ ayarlar ikonu** görmelisin — tıklayınca sıklık/not sheet'i açılmalı (toggle'ı kapatıp açmadan).
9. Sıklığı değiştirip kaydet, Rutin Bakım'a geri dön — yeni sıklığın uygulandığını doğrula.
10. Parsel oluşturma ekranında (Tarla Günlüğü veya Onboarding) **"Yetiştirdiğiniz ürün yok mu? Talep edin"** seçeneğini bul, bir talep gönder — Berkin'in telefonuna SMS geldiğini doğrula.
11. Buyer tarafında (Keşfet / onboarding) katalogda **olmayan** bir ürün adıyla "Ürün Talep Et" formunu doldur, gönder — yine SMS gelmeli. Katalogda **var olan** bir ürünle aynı formu doldurursan SMS gelmemeli (sadece talep kaydedilmeli).
12. Sayfanın altındaki **crop bilgi panellerini** (Safran, Kekik, Lavanta vb.) aç — glossary metinlerinin doğru sırada geldiğini doğrula.
13. **(Yeni)** Ana sayfadaki (veya Günlük sayfasındaki) **AI sohbet asistanına** "son hasatlarım neler?" gibi bir soru sor — cevapta az önce "Yaptım" dediğin rutin bakım işinin (örn. sulama) **gerçek bir hasat gibi görünmediğini** doğrula (sadece gerçek hasat kayıtların listelenmeli).
14. **(Yeni)** Rutin bakımını "Yaptım" dediğin ürünün bağlı olduğu **partiyi** (parti/batch detay sayfası, "Hasat Kayıtları" bölümü — çiftçi kendi görünümünde) aç, listede o kaydın **"0 kg" gibi anlamsız bir miktar göstermediğini** doğrula.
15. **(Yeni, bilgi amaçlı — karar zaten verildi)** Aynı partinin buyer'a görünen sayfasındaki **"İzlenebilirlik" rozetini ve "Ürün Geçmişi" zaman çizelgesini** rutin bakım kaydından önce ve sonra karşılaştır. Rozetin tek bir "Yaptım" tıklamasıyla (fotoğrafsız, miktarsız) yükselmesi bilinçli bir karar (Berkin onayladı) — bu adım sadece davranışın beklediğin gibi çalıştığını gözünle teyit etmen için.

### ✅ P22-G — Rutin Bakım Tarih/Filtre Düzeltmesi + Trigger Temizliği — TAMAMLANDI *(2026-07-28, Claude Code doğrudan)*

**Neden:** Yukarıdaki tarayıcı QA'yı yaparken Berkin 4 gerçek semptom buldu: Rutin Bakım satırlarında tarih bilgisi yok, Bugün/Bu Hafta/Tümü filtresi süzmüyor, "Yaptım" dedikten sonra iş listeden kaybolmuyor, "Yaptım" formunda hangi gün yapıldığı girilemiyor. Ayrıca `buyer_addresses` üzerinde (P23-M1-a'nın kendi eklediği trigger'la) aynı işi yapan iki trigger bulundu, ve P22-E'nin SMS'lerinin formun topladığı alanların çoğunu içermediği fark edildi.

**Kök neden (kodu okuyarak doğrulandı, hipotez çürütüldü):** "P22-F verileri taşıdı ama hesap eski kaynağa bakıyor" hipotezi **yanlış** çıktı — `harvest_entries.journal_entry_type_id` okuma zaten doğruydu. Asıl bug: `useCreateEntry`'nin `onSuccess`'i bu sorgunun React Query cache key'ini invalidate etmiyordu, yani "Yaptım" dendiğinde kayıt gerçekten günlüğe düşüyordu ama ekran (aynı oturumda, sayfa yenilenmeden) bunu görmüyordu. Ayrıca hesabın kendisi (son yapılma/sıklık/sıradaki/gecikmiş) tamamen client'ta yaşıyordu — kural #106'yı ihlal ediyordu.

**Yapılanlar:**
1. **DB temizliği:** `buyer_addresses`'teki gereksiz `trg_enforce_single_default_address` trigger'ı + fonksiyonu düşürüldü, `updated_at`'i de yöneten `trg_buyer_addresses_clear_default` kaldı. Gerçek insert/update ile doğrulandı.
2. **Yeni view `v_routine_maintenance_status`:** Hesap (son yapılma/sıklık/sıradaki tarih/gecikmiş mi) DB'ye taşındı — web ve mobil (P23) aynı view'ı okuyacak.
3. **Cache invalidation düzeltildi:** `useCreateEntry` artık `routineMaintenanceStatus` sorgusunu da invalidate ediyor.
4. **UI:** Her satırda son yapılma + sıradaki tarih görünüyor; "Yaptım" formuna tarih seçici eklendi (varsayılan bugün, geçmiş tarih girilebilir); tarih filtresi artık view'ın alanlarına göre gerçekten süzüyor; yapılan iş sıradaki zamanı gelene kadar bekleyen listeden düşüyor, "Tümü"de görünmeye devam ediyor.
5. **P22-E SMS düzeltmesi (Berkin kararı: kritik alanları ekle + notu kısalt):** Çiftçinin "Yeni Ürün Türü Talebi" SMS'ine birim, kategori, hasat ayı aralığı ve kısaltılmış not eklendi (öncesinde sadece ürün adı gidiyordu, form 7 alan topluyordu). Buyer'ın katalog-boşluğu SMS'ine de kısaltılmış not eklendi.

**Doğrulama:** Her madde gerçek veriyle test edildi (Kural #96) — aynı parselde 2 farklı crop, sıklığı olan/olmayan iş, gecikmiş iş senaryoları ayrı ayrı; iki SMS gerçek Twilio testiyle (`net._http_response`, "accepted"). `tsc --noEmit` temiz. Tarayıcı/dokunma testi bu oturumun ağ kısıtlaması yüzünden yapılamadı — bkz. `Build/E2E-QA.md` → S19 için Berkin'in QA adımları.

PR: [hasat-d2c-marketplace #5](https://github.com/berkinsavciozen/hasat-d2c-marketplace/pull/5).

**✅ Tarayıcı QA tamamlandı (Berkin, 2026-07-29).** İlk turda üç adım (tarih görünürlüğü, tarih filtresi, tarih seçici) başarısız göründü; teşhis sonucu sebep **yayınlanmamış build** çıktı — üç düzeltme de kodda mevcuttu ve merge edilmişti (bkz. kural #109). Lovable'da Publish sonrası tekrar test edildi, **tüm adımlar geçti.** P22-E'nin SMS düzeltmesi de doğrulandı: talep SMS'i artık birim, kategori, hasat ayı aralığı ve kısaltılmış notu içeriyor. **P22 serisi tamamen kapandı.**

### Sıradaki adım
P22 serisi (A/B/C/D/E/F) + P22-F'nin yan etki düzeltmeleri + P22-G (tarih/filtre düzeltmesi + trigger temizliği) tamamen bitti. Kalan tek şey: `Build/E2E-QA.md` → S19'daki tarayıcı QA adımları (Berkin'in kendi testinde yapacağı). QA tamamlandığında P22 serisi tamamen kapanmış olacak.

### P23 — Buyer Mobile & Recipe App — 🟢 PLAN ONAYLANDI (2026-07-28), M0 başlıyor

> **Tam plan artık ayrı dokümanlarda:**
> - `Build/Roadmap.md` — görsel Gantt + kilometre taşları + şirket geri sayımı
> - `Build/P23-Mobile.md` — kapsam, Expo gerekçesi, şema, AI import matrisi, M0–M9
> - `Build/Shared-Architecture.md` — web+mobil paylaşım mimarisi
> - `Build/Store-Compliance.md` — Apple 4.2, hesap tipleri, IAP, submit checklist

**Hedef:** Store'da canlı ≈ 31 Ekim 2026. **Kural: kapsam kesilmez, tarih ötelenir.**

#### Onaylanan temel kararlar (2026-07-28)

| # | Karar | Gerekçe |
|---|---|---|
| 1 | **Expo/React Native** (Capacitor değil) | Şirket Mac'i — local'de Xcode/imzalama yönetilemiyor. EAS bulutta derliyor. Bedeli: mobil %100 Claude Code, Lovable mobilde çalışmıyor. |
| 2 | **Mobil v1'de checkout YOK** | Ödeme blokajını uygulamadan izole eder + Guideline 2.1 riski kalkar + IAP tartışması biter. Akış "Talep Et"te biter, ödeme web'de. |
| 3 | **Apple bireysel hesap, şimdi** | D-U-N-S gerekmiyor, şirketten bağımsız → Apple kritik yoldan çıkar. Şahıs şirketi zaten organizasyon hesabına uygun değil. |
| 4 | **Tarif katmanı = huni** | Başarı ölçüsü tarif geliri değil, tarif→kayıt→talep→sipariş dönüşümü. `v_kpi_recipe_funnel` çekirdek. |
| 5 | **Eşleşmeyen malzeme → "Talep Et"** | 70 crop'un 9'unda ilan var. Talep verisi = çiftçi kazanım öncelik listesi. |
| 6 | **Fotoğraf: `crop_config.default_photo_url`** | 39 ilanın 0'ında foto var. Temsili görsel + "temsili görsel" etiketi zorunlu. Fotoğraf kritik yoldan çıktı. |
| 7 | **Kullanıcı importları private** | AI ile çıkarılan tarifler asla otomatik public korpusa girmez. Telif + kalite. |
| 8 | **Mantık DB'ye** (kural #106) | İki client → çift kaynak sapması riski (P20/P24 kanıtı). Eşleştirme/dönüşüm/liste RPC'de. |

#### Kilometre taşları

| # | Taş | Hedef | Durum |
|---|---|---|---|
| M0 | Açık işler + hesaplar | 28 Tem – 3 Ağu | ✅ TAMAMLANDI (2026-07-29) |
| M1 | Paylaşılan çekirdek (`hasat-core`) | 4 – 10 Ağu | ✅ TAMAMLANDI (2026-07-29) |
| M2 | Tarif backend'i (ekleyici) | 11 – 22 Ağu | 🟡 Uygulandı (2026-07-29), tarayıcı QA (S20-B) bekliyor |
| M3 | İçerik (18 tarif) + culinary seed + görsel altyapı | 18 Ağu – 1 Eyl | ✅ TAMAMLANDI (2026-07-30), tarayıcı QA (S21) bekliyor |
| M3-D | Mobil UI görsel şartnamesi (paralel iş kolu) | — | ✅ TAMAMLANDI (2026-07-30) — `Build/P23-Mobile-Visual-Spec.md` |
| M4 | Web tarif yüzeyi + Gap #9 | 1 – 13 Eyl | ✅ TAMAMLANDI (2026-07-30, a+b+c) |
| M5-a | Mobil iskelet + `hasat-core` ikinci hedefi + tesisat | 14 – 27 Eyl | ✅ TAMAMLANDI (2026-07-30) |
| M5-b | Ekran yazma (tarif listesi/detayı, pişirme modu, AI import, offline önbellek) | 14 – 27 Eyl | ⬜ |
| M6 | Native yetenekler + push | 28 Eyl – 11 Eki | ⬜ — ⚠️ **açık madde var, bkz. altta "M6 açık maddeleri"** |
| M7 | Köprü + store varlıkları | 12 – 18 Eki | ⬜ |
| M8 | Store submit | 19 – 31 Eki | ⬜ |
| M9 | Sıraya alındı (silinmedi) | Kasım+ | ⬜ |

#### ⚠️ M6 açık maddeleri — M6 prompt'u yazılırken buraya BAKILACAK

- 🔴 **`device_tokens` UNIQUE(token):** aynı cihazda ikinci kullanıcı giriş yaparsa token kaydı düşer. Push M6'da devreye girene kadar kimseyi etkilemiyor. Çözüm yönü: çakışmada token yeni kullanıcıya devredilir (cihaz kimde açıksa onundur). M2-ek'te bilinçli olarak ertelendi.

#### Eski P23 kodlarının eşlemesi

| Eski | Yeni | Not |
|---|---|---|
| P23-A "React Native, mobile-first" | M1 + M5 | Expo'ya revize edildi (Mac kısıtı) |
| P23-B Recipe↔Crop + RFQ | M2 + M4 | Eşleştirme RPC'ye taşındı |
| P23-C Mobile compliance | M7 + M8 | `Build/Store-Compliance.md` |

#### Referans araştırması (korunuyor)
- **Eatr:** süre/beceri/diyet filtreleri, besin değeri. Abonelik iptal/refund şikayetleri var — Hasat bu hatayı tekrarlamamalı.
- **ReciMe:** "Order Groceries" özelliği; Hasat kendi envanterine sahip olduğu için yapısal avantajlı. Kullanıcı importlarını private tutma deseni de buradan.

---

### 🟡 P23-M1-b — Paylaşılan çekirdek boru hattı — KOD TARAFI TAMAMLANDI *(2026-07-29, Claude Code)*

**Bir cümlede:** İki uygulamanın (web + gelecek mobil) ortak kullanacağı kod
artık ayrı bir repoda yaşıyor ve web'e otomatik iniyor; kimsenin — Lovable
dahil — onu web tarafında sessizce değiştirememesi için bir bekçi kuruldu.

#### Ne yapıldı

**Yeni repo `hasat-core`** — build step yok, publish yok, sadece TypeScript
kaynak dosyaları. İçeriği bilinçli olarak küçük tutuldu:

| Ne | Nereden geldi |
|---|---|
| Üretilmiş DB tipleri | `supabase gen types typescript` (canlı şemadan yeniden üretildi) |
| Design token'ları | `src/styles.css` (marka renkleri, semantik renkler, radius, tipografi, spacing) |
| `convertQuantity` | `src/lib/hasat/units.ts` (P21-A) |

**Web'e iniş:** `git subtree` ile `src/lib/core/` altına. Monorepo/pnpm
workspace **yapılmadı** — Lovable'ın sync'ini ve build'ini kırma riski var.
Repoya yalnızca düz dosya eklendi.

**Drift koruması (kural #105'i uygulanabilir hale getiriyor):**
1. Her core dosyasının ilk satırı: `// hasat-core — BU DOSYAYI BURADA DÜZENLEME.`
2. `src/lib/core/.manifest` — her core dosyasının sha256'sı
3. `hasat-core` → `npm run drift -- <yol>` — tek komut, üç durumu yakalar:
   **DEĞİŞTİRİLMİŞ / EKSİK / FAZLA**, farkta exit 1
4. İki GitHub Action (şimdilik **tek hedefli**, sadece web): `core/**` değişince
   web'e sync PR'ı; ayrıca günlük drift kontrolü

**Bilinçli olarak taşınmayanlar → M5:** Supabase storage adapter, TanStack Query
hook'ları, sorgu fonksiyonları. Gerekçe: üçü de auth ve veri akışına dokunuyor;
lansmandan 4 hafta önce, **henüz var olmayan bir client için** o hatta
dokunmuyoruz. (`Build/Shared-Architecture.md` güncellendi.)

#### Doğrulama (kural #96 — bağımsız, gerçek çalıştırma)

| Kontrol | Sonuç |
|---|---|
| `tsc --noEmit` (gerçek, node_modules kurulu) | ✅ 0 hata |
| `npm run build` (Vite + Nitro, prod) | ✅ Başarılı — Lovable'ın build'i ile aynı komut |
| `convertQuantity` taşıma öncesi/sonrası | ✅ 10 vakada birebir aynı; **500 g + 100 kg → 100.500 g** (P21-A vakası) |
| Drift scripti (temiz durum) | ✅ Yeşil — 4 dosya manifest ile aynı |
| Drift scripti (kasten bozuldu) | ✅ Üç senaryoda da exit 1: dosya değiştirildi / silindi / fazladan dosya eklendi — sonra geri alındı, tekrar yeşil |
| `prettier --check` (yeni core dosyaları) | ✅ Temiz |

#### 🔴 Bulunan gerçek şey: silinen tip dosyası bayattı

`src/integrations/supabase/types.ts` canlı şemadan geriye düşmüştü — içinde
`v_routine_maintenance_status` view'ı (P22-G, 2026-07-28) yoktu ve
`buyer_profiles.company_name` hâlâ NOT NULL görünüyordu. Core'a **canlı
şemadan yeniden üretilmiş** tipler kondu; `tsc` bu tiplerle de temiz geçti.

**Yan bulgu — şema borcu listesi güncel değil:** `buyer_profiles.company_name`
canlı DB'de **zaten nullable**. `P23-Mobile.md`'nin M1 listesinde duran bu madde
kapanmış görünüyor (SQL kanıtı `Build/Shared-Architecture.md`'de). Kalan üç
şema borcu bu turda kontrol edilmedi.

#### QA test case (kural #104) — Berkin uygular

PR merge edildikten **sonra**, Lovable önizlemesi yeniden yayına alındığında:

**Amaç:** Taşıma sonrası uygulamanın hiçbir yerinde davranış değişmediğini ve
Lovable'ın hâlâ normal çalıştığını görmek. Bu bir "yeni özellik" testi değil,
**sıfır regresyon** testi.

| # | Adım | Beklenen |
|---|---|---|
| 1 | Lovable editöründe projeyi aç, önizlemenin yüklenmesini bekle | Önizleme hatasız açılıyor, kırmızı build hatası yok |
| 2 | `hasat.lovable.app`'e gir, alıcı olarak giriş yap (`905009876543`, OTP `123456`) | Giriş çalışıyor — oturum saklama bozulmadı |
| 3 | **Keşfet** sayfasını aç | Ürün kartları listeleniyor, sayfa boş değil |
| 4 | Aynı üründen **birden fazla partisi olan** bir üretici kartına bak (kartta "Partileri Gör" yazan) | Karttaki **toplam miktar** eskisiyle aynı — örn. 500 g + 100 kg olan bir ürün **100.500 g** (100,5 kg değil, hatalı 600 değil) görünüyor |
| 5 | O karta tıklayıp ürün detay sayfasına gir | Partiler listeleniyor; üstteki toplam miktar Keşfet'teki ile **aynı sayı** |
| 6 | Bir partiye teklif ver ekranını aç (teklifi göndermene gerek yok) | Ekran normal açılıyor, miktar/birim alanları doğru |
| 7 | Çıkış yap, çiftçi olarak gir (`905001234567`, OTP `123456`) | Giriş çalışıyor |
| 8 | **Günlük** ve **Rutin Bakım** sekmelerini aç | P22-G'de düzeltilen tarih/filtre davranışı aynen duruyor, liste doluyor |
| 9 | Lovable'da küçük bir görsel değişiklik iste (örn. bir başlık metnini değiştir) ve uygulat | Lovable normal çalışıyor, sync bozulmadı |
| 10 | Lovable'ın açtığı diff'te `src/lib/core/` altında **hiçbir dosya** değişmemiş olmalı | Değişmişse kural #105 ihlali — drift Action'ı ertesi sabah alarm verir |

**Bir şey ters giderse:** PR açıklamasındaki rollback planı — `src/lib/core/`
silinip iki commit revert edilince eski hale dönülüyor.

#### Kalan işler (M1 kapanması için)

- ✅ **`hasat-core` reposu açıldı ve dolduruldu** (2026-07-29) —
  `github.com/berkinsavciozen/hasat-core`, `main` + `core-dist` dalları push
  edildi. Subtree bağlantısı gerçek uzak repoya karşı doğrulandı: temiz bir
  klonda `git subtree pull` "already at c4d4b31" diyor, drift kontrolü yeşil.
- ✅ `SYNC_TOKEN` secret'ı eklendi (Berkin, 2026-07-29) ve **iki Action da canlıda
  yeşil doğrulandı.** Bu sırada iki gerçek hata çıktı ve düzeltildi:
  1. `sync-to-web.yml` **geçersiz YAML'dı** — `gh pr create --body` çok satırlı bir
     dize olarak yazılmıştı, devam satırları 1. kolondan başlayınca block scalar
     kırılıyordu. Belirti aldatıcı: çalışma "failure" görünüyor ama **hiç job
     yok** (startup failure). Gövde `printf` ile tek değişkende toplandı.
  2. `core-dist` dalının push'u **403** veriyordu — varsayılan `GITHUB_TOKEN`
     salt-okunur. Job'a `permissions: contents: write` eklendi.
  > Ders: bir workflow'un "failure" olması onun *çalıştığı* anlamına gelmez.
  > 0 job = dosya parse edilememiş. Kural #96'nın Action tarafındaki karşılığı:
  > YAML'ı yazmak yeterli değil, en az bir kez gerçekten koşturulmalı.
- 🔴 Yukarıdaki 10 adımlık tarayıcı QA
- 🔴 Küçük şema borçları (`P23-Mobile.md` M1 listesi) — bu turda kapsam dışıydı
#### ✅ M1 KAPANDI (2026-07-29)

- ✅ `hasat-core` reposu açıldı, dolduruldu, **public** yapıldı (orkestratör oturumunun bağımsız doğrulama yapabilmesi için)
- ✅ `SYNC_TOKEN` secret'ı eklendi ve **çalıştığı doğrulandı** — `drift check (web)` workflow'u elle tetiklendi, 13 saniyede Success döndü. Bu tek çalıştırma hem token'ı hem de Lovable'ın core dosyalarına dokunmadığını doğruladı.
- ✅ Şema borçları M1-a'da kapandı (`safran_soğanı` `default_unit`→`kg`, `min_order<=quantity` BEFORE INSERT trigger'ı, `buyer_profiles.company_name` nullable, `buyer_addresses` tek-default trigger'ı) — dördü de canlı DB'de bağımsız SQL ile doğrulandı
- ✅ **10 adımlık sıfır-regresyon tarayıcı QA'sı geçti (Berkin, 2026-07-29).** Kritik doğrulamalar: `hasat-core`'a taşınan `convertQuantity` canlıda doğru çalışıyor (500 g + 100 kg → **100.500 g**, ham toplama 600 veya hatalı 100,5 değil); Lovable normal çalışıyor, sync bozulmadı; Lovable'ın düzenleme diff'inde `src/lib/core/` altında **hiçbir dosya değişmedi** — kural #105 sınırı ilk canlı testini geçti.
- ⚠️ **Bulunan gerçek risk:** Lovable aynı turda `src/integrations/supabase/types.ts`'i **yeniden üretti** (M1-b'nin sildiği bayat dosya). Drift Action bunu yakalamıyor çünkü dosya `src/lib/core/` dışında — bekçinin kör noktası. Çözüm: dosya Lovable'a bırakıldı, koruma **import yasağına** çevrildi (ESLint + drift Action adımı). Bkz. `Build/Shared-Architecture.md`.
### Kural #104 (2026-07-24'te eklendi)
Berkin'in kararı: bundan sonra Claude Code planları, arayüzde test edilmesi gereken adımlar için **kullanıcı-akışı dilinde adım adım bir test case** olarak sunulmalı (hangi sayfa açılacak, hangi butona tıklanacak, ne görülmesi bekleniyor) — trigger/kolon/event isimleri gibi DB-jargonuyla değil. Genel plan anlatımı da (yeni tablo/akış gibi kapsamlı işlerde) bir PM'in anlatacağı gibi olmalı: kullanıcı ne yapıyor, FE'de ne değişiyor, BE'de ne değişiyor — teknik isimler (trigger/policy adı gibi) sadece gerekince, ayrıntı seviyesinde geçmeli.

---

### 🟡 P23-M2 — Tarif Backend'i — **UYGULANDI, TARAYICI QA BEKLİYOR** *(2026-07-29, Claude Code + Supabase MCP ile doğrudan)*

**Bir cümlede:** Tarif katmanının veritabanı iskeleti kuruldu — tarifler, malzemeler, adımlar, kaydetmeler, tarif→talep atfı ve push token'ları için 7 yeni tablo; "bu tarifin malzemesi Hasat'ta var mı, kaça, ne kadar almam gerekir" sorusunu **veritabanının kendisinin** cevapladığı fonksiyonlar; ve bir tarifi yazıdan ya da fotoğraftan okuyup kullanıcının **özel** defterine yazan bir AI fonksiyonu. **Ekranda görünen hiçbir şey değişmedi** — arayüz M4'te.

**Kapsam kuralı tutuldu:** canlı akışlara (`offers`/`orders`/`listings`/`harvest_entries`) dokunulmadı · `unit_type` enum'una dokunulmadı · `src/lib/core/` altında elle düzenleme yok (kural #105) · frontend işi yok.

#### Ne yapıldı

**1. Şema — 7 tablo + 1 kolon + 1 bucket**
`recipes`, `recipe_steps`, `recipe_ingredients`, `crop_culinary_meta`, `recipe_saves`, `recipe_rfq_links`, `device_tokens`; `crop_config.default_photo_url`; `crop-photos` public storage bucket'ı. Tam alan listesi: `Build/DB-Schema.md` → "P23-M2 — Tarif Backend'i".

**2. İki kritik kural şemaya gömüldü**
- **Culinary birimler `unit_type` enum'una GİRMEDİ.** Enum `g`/`kg`/`L` olarak duruyor. `adet`/`demet`/`kaşık` `recipe_ingredients.unit`'te düz text; dönüşüm yalnızca alışveriş listesi sınırında, `crop_culinary_meta.conversion_hints` ile yapılıyor. (P21'in birim-uyuşmazlığı trigger'ı ve stok toplamaları kirlenmedi.)
- **Malzeme→crop bağlantısı runtime'da fuzzy text matching ile kurulmuyor.** `recipe_ingredients.crop` editoryal olarak bir kez doldurulur; `extract-recipe` bile bu alanı **daima `null`** bırakıyor.

**3. Mantık DB'de (kural #106) — 1 fonksiyon + 2 RPC + 2 view**
`fn_culinary_to_canonical` (ipucu yoksa **NULL** döner, uydurmaz) · `rpc_recipe_availability` (yenilemez crop'lar sonuçta hiç görünmez) · `rpc_recipe_shopping_list` (porsiyon ölçekleme + kanonik birim + **min_order yuvarlaması** + "bu miktar kaç tarif yapar") · `v_recipe_coverage` · `v_kpi_recipe_funnel`.
İki view da `security_invoker=true`; iki RPC de **SECURITY INVOKER** — private bir tarif RPC üzerinden sızmıyor.

**4. RLS — her tablo için açıkça, UPDATE dahil**
Mutasyon akışı olan **beş** tablonun hepsinde SELECT/INSERT'ten **ayrı** bir UPDATE politikası var ve gerçek UPDATE ile 1 satır etkilediği kanıtlandı — `orders`'ta eksik olup tüm P17-B mutasyonlarını sessizce sıfır satıra düşüren hata tekrarlanmadı. `recipe_rfq_links` bilinçli olarak append-only (UPDATE yok).

**5. Seed** — `is_edible` 70 crop'un tamamı için `category_group`'tan mekanik türetildi (yenilemez: pamuk, şeker_pancarı, tütün, safran_soğanı → 4). `culinary_aliases`+`conversion_hints` yalnızca 3 test crop'u: domates (mainstream) · kekik (niş) · pamuk (yenilemez). Kalan 67 → M3.

**6. Edge function `extract-recipe`** — metin + yazılı tarif fotoğrafı. `verify_jwt` **açık**. `visibility='private'`, `status='draft'` ve `owner_id` **sunucuda** zorlanıyor; client'ın gönderdiği değerler yok sayılıyor. Kota mevcut `ai_usage_tracking` ile (`can_send_ai_message`/`increment_ai_usage`) — yeni kota altyapısı kurulmadı. YouTube/link ve yemek fotoğrafından tahmin **yok** → M9.

**7. `hasat-core` tip senkronizasyonu** — `core/db/types.ts` canlı şemadan yeniden üretildi, manifest güncellendi. Diff **tamamen ekleyici** (440 satır eklendi; silinen tek satır başlıktaki üretim tarihi) → web'de hiçbir tip daralmıyor. PR: [hasat-core #2](https://github.com/berkinsavciozen/hasat-core/pull/2).

#### Doğrulama (kural #96 — hepsi gerçek çalıştırma)

| Kontrol | Sonuç |
|---|---|
| `fn_culinary_to_canonical` 12 vaka (8 dönüşüm + 4 "uydurmama" NULL) | ✅ 12/12 |
| 3 crop testi — **pamuk RPC sonucunda hiç görünmedi** | ✅ |
| Fiyat normalizasyonu (₺25,50/g → ₺25.500/kg) | ✅ |
| min_order yuvarlaması — görevdeki birebir örnek (2 g gerekli / 10 g min → 10 g alınır, **5 tarif yapar**) | ✅ |
| Porsiyon ölçekleme (4→8 kişilik, scale=2) | ✅ |
| **anon private tarifi göremiyor** (tablo + 2 RPC + view, dördü de) | ✅ 0 satır |
| Başka kullanıcı başkasının private tarifini göremiyor | ✅ 0 satır |
| Kullanıcı public tarif yazamıyor / private'ı public'e çeviremiyor | ✅ RLS reddi (42501) |
| **UPDATE gerçekten 1 satır etkiliyor** (sessiz-sıfır yok) | ✅ |
| `extract-recipe` **metin** girdisiyle gerçek çağrı | ✅ HTTP 200, 5 malzeme / 4 adım |
| `extract-recipe` **görsel** girdisiyle gerçek çağrı (Türkçe yazılı tarif fotoğrafı) | ✅ HTTP 200, OCR doğru okudu |
| Client `visibility:'public'` + başkasının `owner_id`'si gönderdi | ✅ Sunucu yok saydı → private/draft/gerçek sahip |
| Kota mevcut altyapıyla sayıldı | ✅ `ai_usage_tracking = 3` (tam 3 çağrı) |
| `crop-photos` bucket `public=true` (SQL ile, **iki kez** doğrulandı) | ✅ |
| İki view da `security_invoker=true` (`pg_class.reloptions`) | ✅ |
| Advisor taraması — yeni 12 objede uyarı | ✅ **Sıfır** |
| Test verisi temizliği | ✅ 0 tarif/malzeme/adım/kayıt/bağ/token kaldı |

Tarayıcı QA adımları: `Build/E2E-QA.md` → **S20-B** (bu tur backend olduğu için adımlar **regresyon kontrolü**; yeni ekran yok — bu doküman içinde açıkça belirtildi).

#### 🔶 Otonom alınan üç karar (kural #107 — Berkin onayı YOK)

Bu turda üç noktada belirsizlik çıktı; sorular soruldu ama oturum etkileşimsiz olduğu için cevap alınamadı. Üçü de **kapsamı/mimariyi değiştirmeyen, geri alınabilir** varsayılana bağlandı:

1. **`v_kpi_recipe_funnel`'ın "görüntüleme" basamağı boş.** Onaylı 7 tabloda görüntüleme/olay tablosu yok ve DB'de hiç analytics tablosu yok. 8. tablo eklemek kapsam değişikliği olurdu → **eklenmedi**; `recipe_views` kolonu NULL dönüyor, M4'te (görüntüleme olayını üretecek yüzeyle birlikte) doldurulacak. Ayrıca `crop_requests` ↔ `offers`/`orders` arasında **hiç FK yok**, bu yüzden teklif/sipariş atfı **sezgisel** (aynı alıcı + crop + talepten sonra) ve view yorumunda böyle belgelendi.
2. **`gül` yenilebilir kaldı.** Görevin 4. maddesindeki örnek yenilemez listesi "gül yağlık" içeriyordu, ama 5. maddedeki operatif seed kuralı (`category_group`'tan mekanik türet) `gül`'ü `tibbi_bitki` grubunda bırakıyor. Mekanik kural uygulandı. Değiştirmek tek satır UPDATE.
3. **`device_tokens` katı RLS aldı** (`user_id = auth.uid()`, dört komutta da). Bedeli: aynı cihazda ikinci bir kullanıcı giriş yaparsa `UNIQUE(token)` yüzünden token kaydı başarısız olur ve o kullanıcı push alamaz. Bu, **push M6'da devreye girene kadar** kimseyi etkilemiyor; çözüm (token sahiplenme politikası veya service-role'lü bir edge function) M6'ya açık madde olarak bırakıldı.

Ayrıca **"admin" = service_role** olarak yorumlandı: `profiles.role` yalnızca `farmer`/`buyer` içeriyor, `is_admin()` yok; mevcut admin erişimi service-role anahtarlı edge function (`admin-kpi` + `x-admin-key`). "Yazma sadece admin" = hiç politika yazılmadı. Yeni bir admin rolü/deseni **icat edilmedi** ("yeni desen icat etme" talimatı gereği).

#### 🔍 sync-to-web Action'ının durumu

Action `main`'e push'ta tetikleniyor (`paths: core/**`) — **feature branch'ten tetiklenmiyor.** Bu yüzden PR açan gerçek çalışma, [hasat-core #2](https://github.com/berkinsavciozen/hasat-core/pull/2) merge edildiğinde olacak.

Bu turda mekanizma `workflow_dispatch` ile `main` üzerinde ayrıca koşturuldu (**run #5, 2026-07-29, tüm 6 adım yeşil**): `SYNC_TOKEN`, `subtree split`, `core-dist` push, web reposu checkout'u ve `subtree pull` adımlarının hepsi çalışıyor. PR açılmadı çünkü `main`'deki `core/` içeriği o anda değişmemişti — workflow'un `if git diff --quiet origin/main -- "$PREFIX"` koruması devreye girip "Değişiklik yok — PR açılmıyor" diyerek çıktı. Web reposunda açık PR olmadığı ayrıca doğrulandı (0 açık PR).

**Yani: boru hattı çalışıyor, tek eksik onun gerçek bir içerik değişikliğiyle koşması — o da merge anında olacak.** Merge sonrası web reposunda otomatik bir `hasat-core sync → src/lib/core` PR'ı beklenir.

#### Açık kalan maddeler (M3/M4/M6'ya taşındı)

| Madde | Nereye |
|---|---|
| `crop_culinary_meta` kalan 67 crop'un alias + conversion_hints'i | M3 |
| ~20 crop temsili görselinin `crop-photos`'a yüklenmesi + `default_photo_url` doldurulması | M3 |
| `recipe_views` olay tablosu + `v_kpi_recipe_funnel`'ın ilk basamağı | M4 |
| `v_kpi_recipe_funnel`'ın `admin-kpi` edge function'ına + `/admin/kpi` ekranına bağlanması | M4 |
| `supabase/config.toml`'a `[functions.extract-recipe] verify_jwt = true` girdisi | M4 |
| `author_type`'ın kullanıcı importları için bir değere ihtiyacı var mı (şema kararı) | Berkin |
| `device_tokens` token devri (aynı cihaz, ikinci kullanıcı) | M6 |
| `recipe_saves` KVKK — gizlilik metnine eklenmesi | M7 |

---

### 🟡 P23-M2-ek — Huni Ölçümünün Tamamlanması — **UYGULANDI** *(2026-07-29, Claude Code + Supabase MCP ile doğrudan)*

**Bir cümlede:** Tarif katmanının işe yarayıp yaramadığını ölçen huni, artık **tahmin etmiyor** — beş basamağın her biri gerçek bir bağlantı üzerinden sayılıyor; ayrıca önceki turdan kalan üç açık maddenin ikisi kapandı, biri bilinçli olarak M6'ya bırakıldı.

**Kapsam değişikliği Berkin tarafından onaylandı:** 7 tablo → **8 tablo + 1 nullable kolon**.

#### Ne yapıldı

| # | İş | Sonuç |
|---|---|---|
| 1 | `recipe_views` tablosu | Huninin ilk basamağı. IP/user-agent **loglanmıyor** (KVKK). INSERT anon dahil serbest, SELECT yalnızca service_role. |
| 2 | `offers.source_recipe_id` | Nullable FK → `recipes`, `ON DELETE SET NULL`. `offers.subscription_id` ile **aynı konvansiyon**; trigger/constraint/default yok. |
| 3 | `v_kpi_recipe_funnel` **yeniden yazıldı** | Sezgisel atıf tamamen kaldırıldı; uçtan uca sert join. |
| 4 | `author_type` += `kullanici` | `extract-recipe` artık bunu yazıyor (v2 deploy edildi). |
| 5 | `device_tokens` UNIQUE(token) | **Dokunulmadı** — M6 açık maddesi olarak yazıldı (yukarı bkz.). |
| 6 | sync Action hatası | Kök nedene inildi, raporlandı (aşağı bkz.). |
| 7 | Drift kör noktası | `Build/Shared-Architecture.md`'ye yazıldı, **düzeltme yapılmadı**, M5 açık maddesi. |

#### 🔴 Sezgisel atıf neden reddedildi

Eski `v_kpi_recipe_funnel`, `crop_requests` üzerinden şuna benzer bir çıkarım yapıyordu: *"bu alıcı bu tarife bağlı bir talep açtıysa, sonrasında aynı crop'ta verdiği her teklif ve sipariş de o tariften doğmuştur."*

Bu **sessizce fazla atıf** üretir. Düzenli domates alan bir alıcı bir kez domatesli bir tarife baksa, sonraki her domates siparişi tarif katmanının hanesine yazılırdı. Tarif katmanı hiç işe yaramasa bile huni "çalışıyor" görünürdü — ve bu sayı **North Star metriğine** (ihtilafsız tamamlanmış sipariş GMV'si) bağlanıyor.

**Yanlış sayı, hiç sayı olmamasından kötüdür:** hiç sayı yoksa "bilmiyoruz" denir ve ölçüm eklenir; yanlış sayı varsa yanlış karar verilir ve kimse sorgulamaz.

Yerine konan zincir tamamen sert FK:

```
recipe_views -> recipe_saves -> recipe_rfq_links -> crop_requests   (malzeme YOK yolu)
                             -> offers.source_recipe_id             (malzeme VAR yolu)
                             -> orders.offer_id                     (sipariş)
```

3. ve 4. basamak **paralel çıkış yollarıdır**, ardışık değil — malzeme eşleşmediyse talep, eşleştiyse doğrudan teklif. Bu yüzden "talep → teklif" oranı hesaplanmıyor; yalnızca gerçekten ardışık olan `view_to_save_pct` ve (kohort bazlı) `offer_to_order_pct` veriliyor.

**Kanıt:** canlı DB'de 121 teklif ve 120 sipariş var, hiçbirinde `source_recipe_id` dolu değil → yeni view **0 satır** döndürüyor. Eski view aynı veride 1 "atfedilmiş teklif" sayıyordu.

#### Önceki turdaki üç otonom varsayılan nasıl kapandı

| # | Önceki turda otonom alınan varsayılan | Bu turda ne oldu |
|---|---|---|
| 1 | **Funnel'ın görüntüleme basamağı NULL bırakıldı**, 8. tablo eklenmedi (kapsam korunsun diye), teklif/sipariş atfı sezgisele bırakıldı | ✅ **Kapandı.** Berkin kapsam değişikliğini onayladı: `recipe_views` eklendi, `offers.source_recipe_id` eklendi, sezgisel atıf tamamen kaldırıldı. Artık NULL kolon veya "M4'te doldurulacak" notu yok — beş basamak da gerçek veriyle doluyor. |
| 2 | **`gül` yenilebilir bırakıldı** (görev metnindeki örnek liste ile mekanik seed kuralı çelişiyordu) | ⏸️ **Değişmedi.** Bu turun kapsamında değildi, Berkin'den aksi bir talimat gelmedi. Mekanik kural yürürlükte: yenilemez = pamuk, şeker_pancarı, tütün, safran_soğanı (4 crop). Değiştirmek hâlâ tek satır: `UPDATE crop_culinary_meta SET is_edible=false WHERE crop='gül';` |
| 3 | **`device_tokens` katı RLS aldı**, token devri M6'ya bırakıldı | ✅ **Karar onaylandı ve kalıcılaştırıldı.** Berkin "dokunma ama açık madde olarak yaz" dedi. Şema aynen kaldı; madde yukarıdaki "M6 açık maddeleri" bölümüne, M6 prompt'unda gözden kaçmayacak şekilde yazıldı ve kilometre taşı tablosundaki M6 satırı oraya işaret ediyor. |

Ayrıca önceki turun açık maddelerinden **`author_type`** de kapandı: `kullanici` değeri eklendi ve `extract-recipe` (v2) gerçek çağrıda bu değeri yazdığı doğrulandı.

#### Doğrulama (kural #96)

| Kontrol | Sonuç |
|---|---|
| Beş basamağın tamamı gerçek veriyle (talep yolu + doğrudan teklif yolu ayrı ayrı) | ✅ 3 görüntüleme / 3 tekil / 1 kayıt / 1 talep / 1 teklif / 1 sipariş |
| `offer_to_order_pct` kohort hesabı | ✅ 100,00 · `view_to_save_pct` ✅ 33,33 |
| **Sezgisel atıf gerçekten kalktı mı** (121 teklif, 120 sipariş, hiçbirinde `source_recipe_id` yok) | ✅ View 0 satır döndürüyor |
| **anon `recipe_views` INSERT** | ✅ Kabul (satır düştü, `user_id` NULL) |
| **anon `recipe_views` SELECT** | ✅ Reddedildi — `42501 permission denied for table` |
| anon `v_kpi_recipe_funnel` SELECT | ✅ Reddedildi — `42501 permission denied for view` |
| Başkasının adına görüntüleme yazma | ✅ Reddedildi — RLS |
| `offers.source_recipe_id` **gerçek UPDATE ile 1 satır** (NULL → dolu, ayırt edilebilir değer) | ✅ Mevcut politika kapsıyor, yeni politika gerekmedi |
| `extract-recipe` gerçek çağrı → `author_type` | ✅ `"author_type":"kullanici"`, private/draft |
| `security_invoker=true` (`pg_class.reloptions`) | ✅ İki view da |
| Advisor taraması — yeni objelerde uyarı | ✅ **Sıfır** |
| Test verisi temizliği | ✅ Sıfır kalıntı; `offers` 121 / `orders` 120 **dokunulmadı**; SMS kuyruğu boş |

> ⚠️ **SMS koruması:** teklif/sipariş testi bilinçli olarak tek transaction içinde koşturulup ROLLBACK edildi — `offers` INSERT'i `trg_offer_received` → `dispatch_sms` → `pg_net` zincirini tetikliyor. ROLLBACK sayesinde kuyruk satırı da geri alındı, **gerçek SMS gönderilmedi** (`net.http_request_queue` boş doğrulandı).

#### 🔍 sync-to-web Action'ının açıklanmamış hatası — kök nedene inildi

Berkin'in sorduğu #3 (başarısız) → #4 (başarılı) geçişi. Koşu loglarından ve commit geçmişinden tam zincir:

| Koşu | Commit | Ne oldu |
|---|---|---|
| #1, #2 | `70f9a88`, `6bdc223` | **0 job** — workflow hiç başlamadı. Geçersiz YAML. `gh pr create --body` çok satırlı bir dize olarak yazılmıştı ve gövdedeki `---` satırı sütun 0'da duruyordu; YAML bunu **doküman ayırıcı** olarak okuyup dosyayı geçersiz saydı. |
| — | `1171982` "sync-to-web workflow: gecersiz YAML duzeltildi" | Gövde `BODY=$(printf '%s\n' ...)` değişkenine taşındı, YAML geçerli hale geldi. |
| #3 | `1171982` | Job **çalıştı**, `git subtree split` başarılı (üretilen sha `c4d4b31`), ama `git push --force origin core-dist` adımı düştü: `remote: Permission to berkinsavciozen/hasat-core.git denied to github-actions[bot]` → **403**. Kök neden: varsayılan `GITHUB_TOKEN` salt-okunur. |
| — | `9c15833` "core-dist push'u icin contents:write izni" | Job'a 4 satırlık `permissions: contents: write` bloğu eklendi. |
| #4 | `9c15833` | ✅ Başarılı. |
| #5 | `3e0a7d9` | ✅ Yeşil (M2 turunda mekanizma testi — içerik değişmediği için PR açmadı). |
| #6 | `c9e0263` | ✅ **İlk gerçek koşu** — PR #2'nin merge'iyle tetiklendi, web reposuna sync PR'ı açtı. |

**Değerlendirme: bu sağlam bir çözüm, geçici yama değil.** Üç gerekçe:

1. **Kök nedeni çözüyor, semptomu değil.** Job `core-dist` dalını kendi reposuna geri itiyor; bu gerçekten yazma izni gerektiren bir iş. İzin eksikti, izin verildi.
2. **En az yetki ilkesine uygun ve GitHub'ın önerdiği desen.** Alternatif, repo genelindeki "Workflow permissions" ayarını read/write'a çevirmekti — o, **tüm** workflow'lara yazma izni verirdi. Buradaki blok yalnızca bu job'a, yalnızca `contents` kapsamında izin veriyor.
3. **Sürüm kontrolünde ve kalıcı.** Repo ayarı sessizce geri alınabilir; workflow dosyasındaki blok diff'te görünür.

Not: job seviyesindeki `permissions:` bloğu **tüm izin kümesini değiştirir** (yazılmayan kapsamlar `none` olur). Burada bu doğru davranış — `GITHUB_TOKEN` ile yapılan tek yazma `core-dist` push'u; web reposu checkout'u ve `gh pr create` **`SYNC_TOKEN`** (PAT) ile kimlik doğruluyor, `GITHUB_TOKEN` ile değil. Bunun pratikte doğru olduğu koşu #6'nın (gerçek PR açan koşu) yeşil geçmesiyle kanıtlandı.

**Sonuç: boru hattında açıklanmamış hata kalmadı. Düzeltme önerilmiyor.**

#### Açık maddeler

| Madde | Nereye |
|---|---|
| `crop_culinary_meta` kalan 67 crop'un alias + conversion_hints'i | M3 |
| ~20 crop temsili görseli + `default_photo_url` | M3 |
| `v_kpi_recipe_funnel`'ın `admin-kpi` edge function'ına + `/admin/kpi` ekranına bağlanması | M4 |
| `/tarifler` sayfasının `recipe_views` yazması ve teklif akışının `source_recipe_id` doldurması | M4 |
| `supabase/config.toml`'a `[functions.extract-recipe] verify_jwt = true` girdisi | M4 |
| **Drift script'ine sürüm-gerisi kontrolü** (bkz. `Build/Shared-Architecture.md`) | M5 |
| **`device_tokens` UNIQUE(token) token devri** (bkz. "M6 açık maddeleri") | M6 |
| `recipe_saves` + `recipe_views` KVKK — gizlilik metnine eklenmesi | M7 |

---

### 🟢 P23-M3 — Tarif İçeriği + Culinary Seed + Görsel Altyapısı — **TAMAMLANDI (2026-07-30, Claude Code + Supabase MCP ile doğrudan)**

**Bir cümlede:** Tarif katmanının içeriği yazıldı — 18 özgün tarif (10 dayanıklı
omurga + 7 taze/Ağustos-Ekim + 1 safran), 14 odak crop için tam culinary
dönüşüm seedi, ve görsel altyapının (bucket isimlendirme + `default_photo_url`
SQL) Berkin'e teslime hazır hali. **Ekranda görünen hiçbir şey değişmedi** —
tarif arayüzü hâlâ M4'te.

**Kapsam kuralı tutuldu:** canlı akışlara dokunulmadı, `unit_type` enum'una
dokunulmadı, 14 odak crop dışındaki `crop_culinary_meta` satırlarına
dokunulmadı, frontend işi yok.

#### Ne yapıldı

**1. Crop odağı ve gerekçe** — `Build/P23-Mobile.md` → M3 bölümüne yazıldı:
7 dayanıklı (`zeytinyağı`/`nohut`/`mercimek`/`kekik`/`fındık`/`ceviz`/`buğday`)
+ 6 taze (`domates`/`biber`/`patlıcan`/`üzüm`/`incir`/`elma`) + safran (tam 1
tarif). Dört gerekçe (mainstream=güvenilir tedarik · dayanıklı=hasat
penceresinden bağımsız · dayanıklı=ihtilaf riski düşük · Kadıköy talep
profili) birebir dokümana geçti.

**2. 18 tarif** — `recipes` (18) + `recipe_steps` (98) + `recipe_ingredients`
(117). Tam alan listesi ve malzeme modelleme kuralı: `Build/DB-Schema.md` →
"P23-M3".

**3. `crop_culinary_meta` seed** — 14 crop için `culinary_aliases` +
`conversion_hints` tam dolduruldu (domates/kekik M2'den genişletildi, yeni
birim eklenmeden).

**4. Görsel altyapı** — `crop-photos` isimlendirme konvansiyonu (ASCII slug +
`.jpg`, düz ad alanı) + `default_photo_url` güncelleme SQL'i (14 satır,
**uygulanmadı**, dosyalar yok) + Berkin'e teslim listesi. Tarif kapak
fotoğrafları `cover_photo_url=NULL` ile bırakıldı (18/18 eksik, tek liste).
"Temsili görsel" etiketi kararı `Build/DB-Schema.md`'ye not düşüldü (M4 işi).

**5. `v_kpi_recipe_funnel`'ın üç bilinen sınırı** dokümante edildi (kohortsuz
yüzdeler, tarif kırılımı yok, gerçek "kayıt" basamağı yok) — `Build/DB-Schema.md`,
M4 açık maddesi.

**6. M3-D — Mobil UI Görsel Şartnamesi** (paralel iş kolu, Berkin onaylı):
`Build/P23-Mobile-Visual-Spec.md` — 5 ekran (Pişirme Modu, Offline Durumu, AI
Import Akışı, Alt Navigasyon, "Talep Et"), gerçek 18 tarifin adım/timer
verisiyle kalibre edildi (örn. `timer_seconds` 0 sn'den 259.200 sn'e/3 güne
kadar aralık, "uzun süreç" eşiği bu yüzden 1 saat olarak belirlendi). Kapsam
dışı: Keşfet/ürün listesi/siparişlerim/tarif listesi (webden port edilecek).

#### Doğrulama (kural #96 — hepsi gerçek çalıştırma)

| Kontrol | Sonuç |
|---|---|
| `recipes`/`recipe_steps`/`recipe_ingredients` satır sayısı (18/98/117) | ✅ Beklenenle birebir |
| Her `recipe_ingredients.crop` gerçek `crop_config.crop` slug'ına çözülüyor mu | ✅ 0 çözümlenemeyen |
| `rpc_recipe_shopping_list` — 18 tarif, varsayılan porsiyon, tüm crop-bağlı satırlar | ✅ 68/68 `needed_canonical` dolu, 0 NULL |
| `rpc_recipe_shopping_list` — 2× porsiyon ölçekleme | ✅ 0 NULL |
| `rpc_recipe_availability` — yenilemez crop'lar (pamuk/tütün/şeker_pancarı/safran_soğanı) | ✅ 18 tarifin hiçbirinde görünmüyor |
| Crop dağılım raporu — `safran` en fazla 1 tarifte | ✅ Tam 1 (`safranli-zerde`) |
| Anon rolüyle 18 tarif + adım + malzeme okunuyor mu (SEO) | ✅ 18/18 |
| Advisor taraması (`security`) | ✅ Bu turdan kaynaklı yeni uyarı yok |
| Test verisi | Bu tur test verisi değil, **kalıcı editoryal içerik** — silinmedi |

#### 🔶 Otonom alınan kararlar (kural #107 — Berkin onayı YOK)

1. **"13 odak crop" ↔ gerçekte 14 crop sayım tutarsızlığı — ÇÖZÜLDÜ (P23-M4-a, 2026-07-30).**
   Görev metni A bölümünde crop'ları "13 odak crop" olarak adlandırmıştı, ama
   listelenen crop'lar (7 dayanıklı + 6 taze + safran) toplamda 14 ediyordu.
   M3'te otonom olarak 14'ün tamamı işlenmişti (gerekçe aşağıda korunuyor);
   **Berkin M4-a görev talimatında bunu doğruladı: "13" rakamı kendi aritmetik
   hatasıydı, doğru sayı 14.** Artık açık madde değil — bkz. `P23-Mobile.md` →
   M3 ve `Build/DB-Schema.md` → P23-M3, ikisi de bu turda 14 olarak düzeltildi.
   Orijinal gerekçe (M3'te otonom karar verilirken): safran'ı culinary seedden
   hariç tutmak kendi tarifinin (Safranlı Zerde, "1 tutam safran") E
   doğrulamasını (`rpc_recipe_shopping_list` NULL dönmemeli) doğrudan
   ihlal ederdi; ayrıca P25 crop-agnostic ilkesiyle çelişirdi (safran'ı
   görsel altyapıdan dışlamak "öne çıkarmama" değil "eksik bırakma"
   olurdu).
2. **İşlenmiş/türetilmiş ürünler → `free_text_name`, ham crop'un kendisi
   değil.** `crop_config`'te karşılığı olan ama tarifte işlenmiş haliyle
   geçen malzemeler (domates salçası, nar ekşisi, un, pirinç) editoryal
   olarak `free_text_name`'de bırakıldı — ham crop FK'sine bağlanmadı.
   Gerekçe: P23-M2'den beri var olan `buğday`→`un` ayrımıyla aynı ilke
   (işlenmiş ürün, farmer'ın sattığı ham üründen farklı bir market
   kalemi). Görev metni bu ayrımı açıkça tarif etmiyordu, tutarlılık için
   genişletildi.
3. **`crop-photos` isimlendirme konvansiyonu** (ASCII slug + `.jpg`, düz ad
   alanı, ~1200×900/<300KB önerisi) baştan tasarlandı — görev metni
   yalnızca "bir konvansiyon kur" diyordu, somut kural içermiyordu.

#### Açık maddeler (Berkin'e ait)

| Madde | Nereye |
|---|---|
| **14 crop görseli** — liste + dosya adları: `Build/DB-Schema.md` → "P23-M3" | Berkin |
| **18 tarif kapak fotoğrafı** (`cover_photo_url`, hepsi NULL) | Berkin |
| `default_photo_url` güncelleme SQL'ini görseller yüklendikten sonra çalıştırmak | Berkin/Claude (M3 sonrası) |
| ~~13 vs 14 sayı tutarsızlığının netleştirilmesi~~ — ✅ **Berkin M4-a'da doğruladı: 14 doğru** (yukarı bkz.) | Kapandı |
| **Glossary insan gözden geçirmesi** — P22-C içeriği AI üretimi, bölgesel doğrulama yapılmadı. **Hâlâ açık, bu turda kapanmadı.** | Berkin |
| `crop_culinary_meta` kalan 56 crop'un alias + conversion_hints'i | M4-b/M9 |
| ~~"Temsili görsel" etiketinin gerçek UI'a eklenmesi~~ — ✅ **P23-M4-a'da eklendi** (`RepresentativePhoto` component'i) | Kapandı |

---

### 🟢 P23-M4-a — Public Tarif Yüzeyi + DB Eki + Ölçümleme — **TAMAMLANDI (2026-07-30, Claude Code doğrudan)**

**Arka plan:** `Build/P23-Mobile.md` M4-a. M4, tek turda hem public yüzey hem
Talep Et akışı hem admin heatmap açmanın riskini azaltmak için **a/b'ye
bölündü** — bu tur yalnızca a: `/tarifler` + `/tarifler/$slug` (misafire
açık, SSR/SEO), malzeme kartı 3 durumu (CTA'sız), `recipe_views` ölçümleme,
`v_kpi_recipe_funnel_by_recipe`. Talep Et, admin heatmap, Gap #9 → M4-b.

#### A — DB eki
1. **`crop_requests.quantity`/`.unit`** — **migration gerekmedi**, canlı
   DB'de zaten mevcuttu (nullable, constraint/trigger yok). Muhtemelen
   P17-E'nin (Yapılandırılmış RFQ) orijinal şemasından — görev metni bunun
   ekleneceğini varsayıyordu, kontrol edilip düzeltildi.
2. **`v_kpi_recipe_funnel_by_recipe`** (yeni view) — `v_kpi_recipe_funnel`'ın
   per-recipe eşleniği, aynı sert-join deseni. `security_invoker=true`
   (`pg_class.reloptions` ile doğrulandı). **Gerçek bulgu:** view
   oluşturulduktan hemen sonra `anon`/`authenticated`'a otomatik
   INSERT/SELECT/UPDATE/DELETE grant'i düştüğü görüldü (bu projede yeni
   relation'lara varsayılan grant düşüren bir kural var, mevcut 20+1 KPI
   view'ının hiçbirinde yoktu) — ayrı bir `revoke all ... from anon,
   authenticated` migration'ıyla diğer KPI view'larıyla aynı admin-only
   desene çekildi, grants iki kez sorgulanarak doğrulandı. Detay:
   `Build/DB-Schema.md` → "P23-M4-a".
3. Kohortsuz yüzde sınırı (`view_to_save_pct`, `offer_to_order_pct`)
   **düzeltilmedi** (bilinçli, zaten belgeli) — ama iki funnel view de
   admin-only olduğu için ve bu turun UI'ı bu view'lara hiç dokunmadığı
   için yanlış okunma riski bu turda zaten sıfır.

#### B — Rotalar ve erişim
`src/routes/tarifler.index.tsx` (`/tarifler`) + `src/routes/tarifler.$slug.tsx`
(`/tarifler/$slug`) — P19'da kurulan `index.tsx`+`$param.tsx` kardeş-rota
konvansiyonuyla (bkz. `buyer.prices.index.tsx`/`buyer.prices.$crop.tsx`),
**`/buyer/` dışında** (o layout'un guard'ı misafiri hiç render etmiyor —
`buyer.tsx` `beforeLoad`). Her iki route da TanStack Start `loader` ile
gerçek SSR verisi çekiyor (anon-safe, RLS zaten public+published tarifleri
açık bırakıyor); `head()` başlık/description/canonical + (detay sayfasında)
`application/ld+json` `Recipe` şeması üretiyor — mevcut `index.tsx`
route'unun zaten kullandığı `scripts: [{type:"application/ld+json", ...}]`
deseniyle aynı. `routeTree.gen.ts` gerçek `vite build` ile yeniden
üretildiği doğrulandı (yeni iki route id/path doğru göründü), elle
dokunulmadı. Buyer alt navigasyonuna yeni sekme **eklenmedi** — `buyer.discover.tsx`'e
küçük bir banner kartı ("🍽️ Tarif fikri mi arıyorsun?") eklendi, mevcut
mantığa dokunulmadı.

#### C — Liste sayfası
Filtreler (süre/zorluk/diyet etiketi/mutfak) client-side, `buyer.discover.tsx`'in
zaten kullandığı state-driven deseniyle aynı (URL parametresi yok). "Şu an
Hasat'ta tam alınabilir tarifler" filtresi `v_recipe_coverage.coverage_pct`
ile besleniyor — ana keşif yolu olarak konumlandırılmadı, ayrı bir checkbox.
Kapak fotoğrafı yoksa (18/18) ilk `is_key_ingredient` malzemenin
`crop_config.default_photo_url`'üne düşüyor, o da yoksa nötr placeholder —
hiçbir durumda boş kutu yok.

#### D — Detay sayfası
`rpc_recipe_availability` + `rpc_recipe_shopping_list` ile besleniyor.
Malzeme kartı üç durumu gerçek veriyle doğrulandı (bkz. E2E-QA S22 → A):
eşleşti (kekik) → link + fiyat/min_order · platform crop ama eşleşmiyor
(zeytinyağı/susam) → nötr "Hasat'ta henüz yok", CTA yok · platform-dışı
(tuz) → nötr, hiç durum etiketi yok. Porsiyon +/- her değişimde
`rpc_recipe_shopping_list`'i yeniden çağırıyor. `min_order` yuvarlaması
gerçek yansıması (`needed_canonical` vs `purchase_canonical`, "bu miktar
~N tarif yapar") artık kartta görünüyor — bu yol M2'den beri hiç UI'a
bağlanmamıştı. `recipe_steps.timer_seconds` dolu adımlarda süre gösteriliyor.

#### E — Ölçümleme
`recipe_views` yazımı canlıya alındı: `/tarifler/$slug` her mount'ta bir
satır yazıyor (anon → `session_id` only, giriş yapmış → `user_id` **+**
`session_id` ikisi birden — aynı ziyaretçi girişten önce/sonra
eşleştirilebilsin diye). `session_id`, `crypto.randomUUID()` ile üretilip
`localStorage`'da (`hasat-anon-session-id`) saklanıyor. IP/user-agent
toplanmıyor (KVKK).

#### F — Görseller
"Temsili görsel" etiketi `RepresentativePhoto` component'inde (yeni, paylaşılan)
zorunlu kılındı — hem liste kartlarında hem detay kapağında hem malzeme
crop fotoğrafında aynı kural geçerli.

#### G — Kullanılmayan alan listesi
`Build/DB-Schema.md` → "P23-M4-a" → "G" bölümünde tam liste ve gerekçe var.
Özet: `rpc_recipe_availability`'den yalnızca `crop_photo_url`/`crop_display_name`/
`active_listing_count` kullanıldı (gerisi SSR ingredient satırı ya da
`rpc_recipe_shopping_list`'in aynı alanıyla birebir çakışıyordu);
`rpc_recipe_shopping_list`'in neredeyse tamamı (16/20 alan) kullanıldı,
kalan 4'ü (`sort_order`/`crop`/`crop_display_name`/`free_text_name`/
`recipe_servings`/`requested_servings`) zaten elde olan veriyle aynıydı.

#### H — Dokunulmayanlar (kural #105 dahil)
`src/lib/core/` — dokunulmadı. Checkout/ödeme, `unit_type` enum'u, design
token'ları/storage adapter (M5), mobil kod, Talep Et akışı, admin heatmap,
Gap #9 — hiçbiri bu turda yok.

#### Doğrulama (kural #96)
Tam tablo: `Build/E2E-QA.md` → S22. Özet: gerçek `anon`/`authenticated` RLS
simülasyonuyla `recipe_views` insert'i + `v_kpi_recipe_funnel_by_recipe`'in
doğru saydığı doğrulandı (test verisi silindi); kekik/fındık (eşleşen) ve
nohut/zeytinyağı (eşleşmeyen) ile `min_order` yuvarlaması gerçek veriyle
test edildi; `vite build` (SSR dahil) + `tsc --noEmit` + `eslint` temiz;
`/tarifler`'in statik meta'sının gerçek SSR HTML'ine yazıldığı `curl` ile
doğrulandı. **Bu oturumun ağ politikası** `efuqpiaavrzimvstpdpm.supabase.co`'ya
canlı SSR sırasında erişimi engellediği için (P24'te Berkin'in de yaşadığı
aynı kısıt), tam veri akışlı canlı tarayıcı testi ve detay sayfasının
dinamik JSON-LD'sinin view-source kanıtı **Berkin'in tarayıcısına** bırakıldı
— bkz. E2E-QA S22 → C.

#### Dokunulan dosyalar (`hasat-d2c-marketplace`)
`src/routes/tarifler.index.tsx` (yeni) · `src/routes/tarifler.$slug.tsx`
(yeni) · `src/lib/hasat/recipes.ts` (yeni) · `src/lib/hasat/session.ts`
(yeni) · `src/components/hasat/RepresentativePhoto.tsx` (yeni) ·
`src/routes/buyer.discover.tsx` (küçük ekleme) · `src/routeTree.gen.ts`
(otomatik yeniden üretildi).

#### Otonom alınan kararlar (kural #107)
1. **"Eşleşti" malzeme kartının linki `/buyer/discover`'a gidiyor**, crop'a
   özel filtrelenmiş bir sayfaya değil — `buyer.discover.tsx`'in arama
   kutusu URL parametresi almıyor, bunu eklemek bu PR'ın kapsamını
   büyütürdü. `buyer.prices.$crop.tsx` (fiyat grafiği sayfası) alternatif
   olarak değerlendirildi ama gerçek ilan/min_order göstermiyor, reddedildi.
   **Netleşmesi gereken soru:** Berkin crop'a özel filtrelenmiş bir
   keşif sayfası isterse bu M4-b'de ele alınabilir.
2. **`recipe_saves`'in anon "kaydet" yolu açılmadı** — görev metninin E
   bölümündeki "kayıt anında session_id yakala" cümlesi `recipe_views`'a
   uygulandı (görüntüleme anında session_id), `recipe_saves`'e değil.
   `recipe_saves.user_id` hâlâ NOT NULL + RLS girişli kullanıcı zorunlu
   kılıyor — bunu değiştirmek şema/RLS değişikliği demek, görev metninin
   izin verdiği "1 nullable kolon" kapsamının (crop_requests) dışında.
   `Build/DB-Schema.md`'nin `v_kpi_recipe_funnel` "üç bilinen sınır #3"ü
   bu yüzden hâlâ tam kapanmadı — M4-b/M9 açık maddesi.

---

### 🟢 P23-M4-b — Talep Et Akışı + Admin Talep Isı Haritası + Gap #9 — **TAMAMLANDI (2026-07-30, Claude Code doğrudan)**

**Bir cümlede:** Eşleşmeyen malzeme kartına baskın-durum "Talep Et" CTA'sı
(guest niyeti kaybetmeden), mevcut `crop_request_match`/`dispatch_sms`
deseni yeniden kullanılarak "bu ürün geldiğinde haber ver", `/admin/kpi`'ye
çiftçi kazanım öncelik listesi sekmesi, ve BENCHMARK Gap #9 mevcut
`/batch/$listingId` sayfasına link olarak kapandı — hepsi yeni altyapı
kurmadan. Ayrıca M4-a'nın 3 bulgusu (malzeme büyük harf, eksik `totalTime`,
`image` alanı) düzeltildi.

**A1 — orkestratör hatası düzeltmesi:** Önceki bir turda `crop_requests`'in
canlıda 6 kolona düştüğü iddia edilmişti — bu, kesilmiş bir SQL çıktısına
dayanan yanlış bir orkestratör bulgusuydu. Bu turda `information_schema.columns`
ile yeniden doğrulandı: **12 kolon**, tam olarak dokümanların (`DB-Schema.md`,
`P23-Mobile.md`) zaten söylediği gibi. Dokümanlarda kaldırılması gereken
yanlış bir not bulunmadı.

**Kural #110 eklendi** (yeni view'larda varsayılan `anon`/`authenticated`
grant'i — bkz. yukarıdaki kural listesi).

**Kapsam kuralı tutuldu:** `src/lib/core/` dokunulmadı (kural #105),
checkout/ödeme yok, `unit_type` enum'una dokunulmadı, buyer alt navigasyonuna
yeni sekme eklenmedi, `routeTree.gen.ts` gereksiz yeniden üretilmedi.

Tam ayrıntı, otonom karar gerekçeleri ve dokunulan dosya listesi:
`Build/DB-Schema.md` → "P23-M4-b". Doğrulama tablosu (kural #96) aynı
bölümde. Tarayıcı QA: `Build/E2E-QA.md` → S23.

**⚠️ Bu oturumda yeni bir ortam kısıtı bulundu:** `bun install` bu turda
tamamlanamadı — kilitli `bun.lock` tarball URL'leri Lovable'ın özel paket
mirror'ına (`*-npm.pkg.dev/lovable-core-prod/sandbox-npm-cache`) işaret
ediyor ve bu host org egress politikasıyla kapalı (önceki oturumlardaki
"Supabase'e canlı SSR sırasında erişilemiyor" kısıtından **ayrı, yeni**
bir bulgu). `tsc --noEmit`/`eslint`/`prettier` kısmi/önceden cache'lenmiş
`node_modules` ile çalıştırılabildi (temiz sonuç, yeni hata yok), ama tam
`vite build` bu oturumda mümkün olmadı — gerçek prod build doğrulaması
Lovable/Berkin'in ortamında yapılmalı.

---

### 🟢 P23-M4-c — `cook_minutes` Semantik Düzeltmesi + SEO Keşfedilebilirliği — **TAMAMLANDI (2026-07-30, Claude Code doğrudan) — M4'ün kapanış turu**

**⚠️ Kural #107 ihlali (bu turda bulunup düzeltildi):** M4-b'de `totalTime`
düzeltilirken bekleme süresini tutacak bir kolon olmadığı görülmüştü.
Yeni kolon eklemek (şema değişikliği) ile mevcut `cook_minutes`'a ekleyip
`cookTime`'ı kirletmek arasında **sessizce** ikincisi seçildi ve
raporlanmadı — kural #107 tam olarak bu tür belirsizlikleri Berkin'e
bildirmek için var. Sonuç gerçek ve görünür bir hataydı: muhammara'da
45 dk "pişirme süresi" (gerçeği 15 dk), Cevizli Üzümlü Köme'de 72 saat
"pişirme süresi" (gerçeği 20 dk). Berkin bu turda bunu bulup bildirdi.

**Yapılanlar:**
1. Yeni kolon `recipes.rest_minutes` (nullable, ekleyici).
2. 18 tarifin tamamı adım metinlerinden yeniden sınıflandırıldı —
   `cook_minutes` gerçek aktif pişirme süresine geri çekildi (en yükseği
   artık **60 dk**, önceki 4.340 dk'dan), bekleme/dinlenme/soğuma/mayalanma/
   ıslatma süresi `rest_minutes`'a taşındı. İki özel durum bulundu (Ekşi
   Mayalı Ekmek'in otoliz'i, Safranlı Zerde'nin safran bekletmesi) — ikisinin
   de orijinal `prep_minutes`'ı tamamen bir pasif adımdan geliyordu, o da
   düzeltildi.
3. Frontend: `totalTime` artık `prep+cook+rest` türetilmiş değeri; üç süre
   detay sayfasında hiçbir zaman tek sayıya toplanmadan ayrı gösteriliyor
   ("20 dk hazırlık · 15 dk pişirme · 30 dk dinlenme").
4. SEO: `sitemap.xml` dinamik hale getirildi (18 tarif + public vitrinler,
   yeni tarif eklendiğinde elle güncelleme gerekmiyor); `robots.txt` zaten
   doğruydu; tarif detaylarına aynı temel malzemeyi paylaşan tariflere SSR
   (loader'da, client-side değil) iç link eklendi; liste + iç linklerin
   gerçek `<a href>` olduğu kütüphane kaynağından doğrulandı.

Tam tablo, gerekçe ve doğrulama: `Build/DB-Schema.md` → "P23-M4-c".
Tarayıcı QA: `Build/E2E-QA.md` → S24.

**Kapsam kuralı tutuldu:** `src/lib/core/` dokunulmadı (kural #105),
checkout/ödeme yok, `unit_type` enum'una dokunulmadı, buyer alt navigasyonu
değişmedi, `routeTree.gen.ts` yeniden üretilmedi (yeni rota yok, `sitemap.xml`
zaten vardı, sadece içeriği genişletildi).

**⚠️ `bun install` bu turda da başarısız oldu** (aynı org egress kısıtı,
M4-b'de bulunan). `tsc --noEmit`/`eslint`/`prettier` kısmi kurulumla temiz
sonuç verdi. Gerçek `vite build` M4'ün üç turunda da (M4-a hariç) hiç
koşmadı — Lovable/Berkin'in ortamında doğrulanmalı.

**M4 tamamen kapandı** (a: public tarif yüzeyi · b: Talep Et + admin
heatmap + Gap #9 · c: cook_minutes düzeltmesi + SEO). Sıradaki taş: M5
(mobil iskelet).

---

### 🟢 P23-M5-a — `hasat-mobile` İskeleti + `hasat-core` İkinci Hedefi + Tesisat — **TAMAMLANDI (2026-07-30, Claude Code doğrudan)**

**Ön iş — Nohut Falafel `rest_minutes` içerik düzeltmesi:** Adım 1 metni zaten
"Islatılmış çiğ nohutları süzün" diyordu — tarif kuru nohut varsayıyor,
konserve değil — ama ıslatma adımının kendisi hiç yazılı değildi;
`rest_minutes=30` yalnızca 3. adımdaki buzdolabı dinlendirmesini (1800 sn)
yansıtıyordu. Kardeş tarif `Zeytinyağlı Nohut Yemeği`'nin deseni (8 saatlik
ıslatma adımı → `timer_seconds=28800` → `rest_minutes`'a yansıması) aynen
uygulandı: yeni adım 1 eklendi ("Kuru nohutları ayıklayıp yıkayın, bol suyla
örtüp en az 8 saat, tercihen akşamdan sabaha ıslatın.", `timer_seconds=28800`),
diğer 6 adım kaydırıldı, `rest_minutes` 30 → **510** (480 dk ıslatma + 30 dk
mevcut dinlendirme). Doğrudan SQL ile uygulandı, DB'den okunarak doğrulandı.

**Ön iş — süre filtresi bulgusu:** `tarifler.index.tsx`'teki M4-a süre
filtresi `totalRecipeMinutes` (prep+cook+rest) üzerinden süzüyordu; Köme
(4370 dk/~73 sa) ve Ekşi Mayalı Ekmek (1075 dk/~18 sa) "1 saatten uzun"
(üst sınırsız) bucket'ında 65 dakikalık bir tarifle ayırt edilemiyordu.
Bulgu raporlandı, düzeltme **yapılmadı** (görev talimatı böyleydi) — Berkin
onayladı ve ek talimat verdi: filtre `prep+cook`'a (aktif süre) çekildi,
`rest_minutes > 120 dk` için liste+detay sayfasında "Önceden başlamak
gerekir" rozeti eklendi (dördüncü bucket yerine — kavramsal hatayı
çözmezdi). Ayrı bir commit'te tutuldu, M5-a mobil işiyle karışmadı.

**A — `hasat-mobile` reposu:**
- Expo SDK 57 + Expo Router (dosya tabanlı) + Nativewind. `tailwind.config.js`
  `hasat-core/core/design/tokens.ts`'i **doğrudan `require` ediyor** (Node'un
  bu ortamdaki sürümü TS dosyalarını doğrudan çalıştırabiliyor), sabit bir
  yedekle — üçüncü bir token kopyası açılmadı.
- `expo-build-properties` ile Android `compileSdkVersion`/`targetSdkVersion`
  **36** açıkça set edildi (`app.json` → plugins), varsayılana bırakılmadı.
- Repo **public**, boş oluşturuldu (Claude Code'un repo oluşturma yetkisi
  yoktu — GitHub App entegrasyonu 403 verdi, Berkin'in elle oluşturması
  gerekti, `SYNC_TOKEN` kapsamına da Berkin ekledi).

**B — `hasat-core`'un ikinci hedefi:**
- `git subtree add --prefix=src/lib/core <hasat-core> core-dist --squash` ile
  `hasat-mobile`'a indirildi (aynı `core-dist` split-branch deseni, M1'deki
  weble birebir aynı mekanizma).
- `sync-to-web.yml` → matrix ile iki hedefe birden (web + mobil) paralel PR
  açacak şekilde genişletildi (`prepare` job core-dist'i üretir, `sync` job
  matrix'te her iki hedefe pull+PR yapar).
- `drift-check.yml` iki hedefi de tarıyor + **yeni üçüncü adım**:
  `scripts/check-manifest-freshness.mjs` — hedefin manifest'i `hasat-core`'un
  GÜNCEL `core/.manifest`'inden farklıysa (bekleyen bir sync PR'ı merge
  edilmediyse) artık exit 1 veriyor. **M5 açık maddesi olan drift kör noktası
  kapandı** (bkz. `Build/Shared-Architecture.md`).
- Kasten bozup (elle düzenleme + sürüm-gerisi, iki ayrı senaryo) exit 1
  verdiği, sonra geri alındığı doğrulandı (aşağıda detay).

**C — Tesisat:**
- `hasat-core/core/supabase/client.ts` — `createHasatSupabaseClient(url, key,
  {storage})`, storage opsiyonel parametre (web SSR'da `undefined` geçebilsin
  diye). Web'in `src/integrations/supabase/client.ts`'i bu factory'yi
  kullanacak minimal şekilde değiştirildi — **davranış değişmedi**
  (persistSession/autoRefreshToken/localStorage/SSR-undefined aynen korundu),
  ayrı bir PR'da (`claude/p23-m5-storage-adapter`).
- Mobilde storage: `expo-secure-store` + `AsyncStorage` + AES (`LargeSecureStore`
  — Supabase'in resmi Expo deseni; ham `SecureStore` tek başına ~2048 byte/değer
  sınırı yüzünden Supabase'in oturum payload'unu tutamıyor).
- Telefon OTP girişi (`app/login.tsx`) web'deki akışın aynısı: `signInWithOtp`
  → `verifyOtp` → `profiles` fetch → role'e göre yönlendirme. Format
  `905XXXXXXXXX` (+ prefix'siz, DB'ye yazılırken).
- TanStack Query kuruldu (`src/lib/query/client.ts`).
- **Kapsam tutuldu:** sadece giriş + oturum kalıcılığı; tarif ekranları,
  çiftçi akışları, checkout — hiçbiri yazılmadı (M5-b/M7).

**D — Kapsam sınırı:** Web reposuna yalnızca C'deki adapter değişikliği için
dokunuldu (ayrı PR, minimal). Çiftçi akışı yok, checkout yok, tarif ekranı yok.

**E — Doğrulama (kural #96):**
| Kontrol | Sonuç |
|---|---|
| `expo start` bundle (Android, offline mod) | ✅ `1850 modül`, HTTP 200, kendi ekran kodumuz (`"6 haneli kodu girin"`, `createHasatSupabaseClient`) bundle içinde doğrulandı |
| API 36 hedefi config'den kanıt | ✅ `expo prebuild --platform android` sonrası `android/gradle.properties`: `android.compileSdkVersion=36`, `android.targetSdkVersion=36` (native klasör sonra silindi — CNG deseni, commit edilmiyor) |
| Subtree hash eşleşmesi | ✅ `diff core/.manifest src/lib/core/.manifest` → birebir aynı (her core güncellemesinden sonra yeniden doğrulandı) |
| Drift script — kasten bozup exit 1 | ✅ Hem `check-drift.mjs` (mobildeki bir core dosyasını elle düzenleyip) hem `check-manifest-freshness.mjs` (hasat-core'da yeni bir satır ekleyip mobile hiç sync etmeyerek) exit 1 verdiği, geri alınınca exit 0'a döndüğü doğrulandı |
| Web'de auth'un bozulmadığı | 🟡 **Kısmi** — `tsc --noEmit` + `npm run build` temiz (client.ts değişikliğinden önce/sonra aynı 4 önceden var olan, ilgisiz hata dışında — ayrı bulgu, aşağıda). Gerçek tarayıcıda OTP girişi bu oturumun ağ politikası `efuqpiaavrzimvstpdpm.supabase.co`'yu 403 ile engellediği için **doğrulanamadı** (P24/M4-a'da da aynı kısıt) — Berkin'in kendi tarayıcısında doğrulaması gerekiyor |
| Mobilde gerçek OTP + oturum kalıcılığı | 🔴 **Doğrulanamadı** — fiziksel cihaz/emülatör yok, ayrıca aynı ağ kısıtı Supabase host'una da uygulanıyor. Kod incelemesiyle doğrulanabilen: `supabase.auth.getSession()` her açılışta storage'dan okunuyor, `LargeSecureStore` senkron değil async ama supabase-js'in storage arayüzüyle uyumlu |

**Bu oturumda bulunan, kapsam dışı bir bulgu:** `src/lib/core/db/types.ts`
(hem web hem mobil kopyasında) `recipes.rest_minutes` kolonunu içermiyor —
M4-c'nin eklediği kolon, tip üretimi hiç yeniden çalıştırılmamış. `tsc --noEmit`
bu yüzden `recipes.ts`'te 4 hata veriyor (benim değişikliklerimden ÖNCE de
aynı 4 hata vardı, `git stash` ile doğrulandı — ben yaratmadım). Düzeltmedim
(hasat-core'da DB tiplerini yeniden üretmek bu turun kapsamı dışında, kural
#107) — Berkin'e bırakılıyor: `supabase gen types typescript --project-id
efuqpiaavrzimvstpdpm` yeniden çalıştırılıp hasat-core'a commit edilmeli.

**Ortam notu (kural #103):** `npm install` bu oturumda **hem hasat-core hem
hasat-mobile hem hasat-d2c-marketplace'te** engellenmeden çalıştı (M4-b/c'de
bulunan `bun install` engeli `bun.lock`'un özel Lovable mirror'ına
kilitlenmesinden kaynaklanıyordu; `npm install` public npm registry'sine
gidiyor ve bu turda açıktı). Bu, M4-b/c'deki "vite build hiç koşmadı" notunun
tersine, bu turda **gerçek `npm run build` çalıştırılıp temiz sonuç alındığı**
anlamına geliyor — ama `bun.lock` ile birebir aynı sürüm kilidini yeniden
üretmez, o yüzden Lovable'ın kendi ortamında bağımsız doğrulama hâlâ değerli.

**Kapsam kuralı tutuldu:** Çiftçi akışı yok, checkout yok, tarif ekranı yazılmadı.
Web reposuna dokunma yalnızca C'deki adapter (ayrı PR) ve süre filtresi
düzeltmesiyle (ayrı commit, Berkin'in ek talimatıyla) sınırlı kaldı.

Tam ayrıntı: `Build/Shared-Architecture.md` (ikinci hedef, dual-target
Action'lar, kör nokta kapanışı). Tarayıcı QA / mobil QA: `Build/E2E-QA.md` →
S25.

---

### 🟢 P23-M5-a-ek — Test Altyapısı, Bayat Tipler, `.env` Bekçisi — **TAMAMLANDI (2026-07-31, Claude Code doğrudan)**

M5-b'ye (ekran yazma) geçmeden önceki ön koşul turu. M5-a merge edilip
doğrulandıktan sonra (`hasat-mobile` iskeleti canlı, `hasat-core` dual-target
sync + drift check çalışıyor, freshness kontrolü ilk gerçek yakalamasını
yaptı — web geri kalmıştı, PR açıldı, merge edildi) dört madde.

**1 — `eas.json` simulator profili (`hasat-mobile`):**

```json
{
  "cli": { "version": ">= 13.0.0", "appVersionSource": "local" },
  "build": {
    "simulator": {
      "distribution": "internal",
      "ios": { "simulator": true }
    }
  }
}
```

Bu profil Apple Developer hesabı **gerektirmeden** bulutta bir iOS Simulator
`.app`'i üretir.

**⚠️ [2026-08-03 düzeltme] Aşağıdaki eski talimat (`eas login`/`eas init`/
`eas build` terminalden) yanlış varsayımdan yola çıkıyordu: Berkin şirket
Mac'inde bu yerel araç zincirini yönetemiyor, ve o dönemin Claude Code
oturumunun ağ politikası `expo.dev`'e erişemediği için bu adımlar hiçbir
zaman terminalden çalıştırılamadı — build'i kimse tetikleyemiyordu. Yerine
tamamen tarayıcı tabanlı akış geçti (bkz. `Build/P23-Mobile.md` →
"bootstrap bulgusu"): Expo'nun kendi dokümantasyonu GitHub App/CI yolu için
"önce yerelden bir build çalıştır" der, ama bunun sağladığı üç şey
(`eas.json` build profili, `projectId`, `bundleIdentifier`) zaten terminale
bağlı değil — üçü de doğrudan kurulabiliyor. Güncel akış:**

1. Expo panosunda (tarayıcıdan, https://expo.dev) proje zaten oluşturuldu —
   proje ID'si panodan alınıp `app.json` → `expo.extra.eas.projectId`'ye
   yazıldı: `bff1a47c-41d5-42fa-bddc-83320c079253`.
2. `bundleIdentifier`/`android.package` elle `com.hasat.app` olarak
   `app.json`'a yazıldı (Berkin onayı — bkz. `Build/P23-Mobile.md`, yayından
   sonra değiştirilemez).
3. Expo panosunda bir erişim token'ı oluşturulup GitHub'a
   `hasat-mobile` reposu → Settings → Secrets and variables → Actions →
   `EXPO_TOKEN` adıyla eklenmesi gerekiyor (Berkin'in yapması gereken tek
   manuel adım).
4. GitHub Actions sekmesinde **"EAS Simulator Build (iOS)"** workflow'u
   (`.github/workflows/eas-build-simulator.yml`) bulunup **Run workflow**'a
   tıklanır — build bulutta, `expo/expo-github-action@v8` üzerinden
   non-interactive olarak tetiklenir. Terminal, `eas login`, `eas init`
   gerekmiyor.
5. Build bitince workflow'un özet sayfasında (`$GITHUB_STEP_SUMMARY`)
   artifact indirme linki (ya da çıkarılamazsa Expo panosunun builds linki)
   görünür.
6. O linkten `.tar.gz`'i tarayıcıdan indir, aç, içindeki `.app`'i çıkar.
7. https://appetize.io/upload adresine tarayıcıdan git, `.app`'i
   sürükle-bırak yükle (hesap gerekmiyor, ücretsiz plan ~100 dk/ay).
   Appetize bir simülatör penceresi açar — QA adımları için
   `Build/E2E-QA.md` → S25 → bölüm B.

**⚠️ Kota koruması:** Expo'nun ücretsiz katmanı ayda 30 build'e izin veriyor
(iOS için bunların en fazla 15'i), kuyrukta bekleme süresi 90 dakikayı
aşabiliyor, ve her **başarısız** deneme de kotadan düşüyor. Bu yüzden
workflow'da otomatik tetikleyici yok — yalnızca `workflow_dispatch` (elle
"Run workflow"). Gerçek build'i çalıştırmak ağ politikası engeli nedeniyle
bu oturumda doğrulanamadı — **Berkin'in kendi GitHub oturumunda
tetiklemesi gerekiyor.**

**2 — Bayat tipler (kural #111):** `hasat-core/core/db/types.ts` Supabase
MCP ile canlı şemadan (`efuqpiaavrzimvstpdpm`) doğrudan yeniden üretildi.
Diff **temiz ve beklenen**: yalnızca `recipes.rest_minutes` kolonu + iki
eksik KPI view'ının (`v_kpi_crop_demand_heatmap`, `v_kpi_recipe_funnel_by_recipe`)
FK referansları eklendi — başka hiçbir şey değişmedi. `core/.manifest`
güncellendi. Bu commit `hasat-core`'a push edilince dual-target
`sync-to-web.yml` otomatik tetiklenip **hem `hasat-mobile` hem
`hasat-d2c-marketplace`'e** birer "hasat-core sync" PR'ı açacak — bu PR'ların
sonucu ayrıca raporlanacak (bkz. bu turun PR listesi).

Kalıcı çözüm: yeni `hasat-core/.github/workflows/types-freshness.yml` +
`scripts/check-types-freshness.mjs` — `supabase gen types typescript
--project-id efuqpiaavrzimvstpdpm` çıktısını commit'lenmiş `core/db/types.ts`
ile günlük (+ `core/db/types.ts` her değiştiğinde) karşılaştırıp farklıysa
exit 1. Doğrulama: hem güncel tipe karşı yeşil hem eski (M5-a'daki bayat)
sürüme karşı kasıtlı olarak kırmızı verdiği test edildi.

**Gereken secret:** `SUPABASE_ACCESS_TOKEN` — Berkin'in Supabase hesabından:
https://supabase.com/dashboard/account/tokens → "Generate new token" (salt-okunur
kapsam yeterli) → `hasat-core` reposunda Settings → Secrets and variables →
Actions → New repository secret → isim tam olarak `SUPABASE_ACCESS_TOKEN`.

**3 — `.env` içerik bekçisi (`hasat-core/drift-check.yml`, yalnızca
`hasat-mobile` hedefinde):** Yeni `scripts/check-env-guard.mjs` — her satır
`EXPO_PUBLIC_` ile başlamalı, ayrıca `service_role`/`SECRET`/`PRIVATE`/`TOKEN`/`PASSWORD`
kalıpları geçen bir isim (prefix doğru olsa bile) reddediliyor. Üç senaryoda
test edildi: temiz `.env` → geçti; `EXPO_PUBLIC_` öneki olmayan bir satır
eklendi → reddetti; `EXPO_PUBLIC_SERVICE_ROLE_KEY` gibi literal `service_role`
kalıbı taşıyan bir isim eklendi → reddetti. Sonrasında `hasat-mobile/.env`
orijinal haline geri alındı (`git status` ile iz kalmadığı doğrulandı).

⚠️ **Bulunan bir sınır:** Görev metninde örnek verilen `EXPO_PUBLIC_SERVICE_KEY`
ismi istenen 5 literal kalıbın (`service_role`, `SECRET`, `PRIVATE`, `TOKEN`,
`PASSWORD`) hiçbirini içermiyor — bu spesifik ismi bekçi yakalamaz. `KEY`
kelimesini kalıba eklemek çözüm değil: `.env`'deki meşru
`EXPO_PUBLIC_SUPABASE_PUBLISHABLE_KEY` satırı da `KEY` içeriyor, `KEY`'i
yasaklamak o meşru satırı da reddederdi. Bekçi tam olarak istenen 5 kalıple
kuruldu; isim-kalıbı denetimi bir savunma katmanıdır, tam garanti değil —
asıl garanti gerçek sırların hiç `.env`'ye yazılmamasıdır.

**4 — AES anahtarı (rapor, değişiklik yok):** `hasat-mobile/src/lib/supabase/large-secure-store.ts`
incelendi. Şifreleme anahtarı `expo-secure-store`'da tutuluyor
(`SecureStore.setItemAsync`/`getItemAsync`) — kodda gömülü değil, deterministik
türetilmiyor; her `setItem` çağrısında `crypto.getRandomValues` ile yeniden
üretiliyor ve eşleşen şifreli veriyle birlikte yazılıyor. Bu, Supabase'in
resmi Expo deseniyle birebir aynı. **Sonuç: doğru** — oturum token'ı gerçekten
AES ile şifreli `AsyncStorage`'da, anahtarın kendisi Keychain/Keystore'da.
Kod değiştirilmedi.

**5 — S25 (`Build/E2E-QA.md`) yeniden yazıldı:** Eski B bölümü gerçek
cihaz/Expo Go varsayıyordu — yeni B bölümü Simulator+Appetize akışını
kullanıyor: marka renklerinin görsel kanıtı, OTP girişi, kapat-aç sonrası
oturum kalıcılığı, çıkış sonrası oturumun gerçekten temizlendiği. Push/gerçek
uçak modu/Keychain-SecureStore cihaz davranışı/performans açıkça
"simülatörde doğrulanamaz" işaretlendi (aşağıdaki listeye taşındı).

**Kapsam kuralı tutuldu:** Şema/mimari değişikliği yok — sadece CI/test
altyapısı + bir bayat-veri düzeltmesi + doküman güncellemesi. `unit_type`
enum'una, `offers`/`orders`/`listings` akışlarına dokunulmadı.

#### 🍎 Apple hesabı gelince koşulacak testler

Bu dördü simülatör/Appetize.io yoluyla **doğrulanamaz** — Apple Developer
hesabı onaylanıp gerçek cihaza kurulum mümkün olunca tek turda koşulacak:

- [ ] **Push bildirimleri** — Appetize/iOS Simulator gerçek APNs/FCM teslimatını simüle etmiyor
- [ ] **Gerçek uçak modu** — offline-önbellek testinin asıl hali (Apple 4.2'nin gerçek testi); simülatörün "ağ yok" hali bir cihazın radyosunu kapatmasıyla aynı değil. **M5-b'de expo-sqlite önbelleği bu yüzden gerçek cihazda henüz doğrulanamadı — bkz. aşağıdaki M5-b build log'u.**
- [ ] **Keychain/SecureStore'un cihazdaki gerçek davranışı** — iOS Simulator'ın Keychain'i cihazın Secure Enclave'ine dayanmıyor
- [ ] **Performans** — Appetize bulutta çalışan bir simülatörün ekran akışı; gerçek cihazın CPU/GPU/pil davranışını yansıtmıyor

---

### 🟡 P23-M5-b — Tarif Ekranları + Offline Önbellek — **UYGULANDI, SİMÜLATÖR/CİHAZ QA BEKLİYOR (2026-08-03, Claude Code doğrudan)**

M5-a-ek-2'den sonraki tur. Görev metni 8 maddeydi; 1 ve 2 kural #107
gereği **yalnızca araştırılıp sunuldu, kararı Berkin verecek, uygulanmadı**.
3-5-7 uygulandı, 6 bu oturumun sınırları içinde doğrulandı.

#### 1 — Test giriş yolu (mobil `123456` OTP çalışmıyor) — **SADECE ARAŞTIRMA, KARAR VERİLMEDİ**

> Kural #107: "bana sun, kendin karar verme" — üç seçenek aşağıda, hiçbiri
> uygulanmadı. `hasat-mobile`'da login akışına dokunulmadı.

**Seçenek A — Supabase Auth'ta test telefon numarası + sabit OTP (dashboard/`SMS_TEST_OTP`)**
- Supabase'in GoTrue Auth servisi bunu resmi olarak destekliyor:
  `SMS_TEST_OTP` belirli telefon numaralarını sabit bir koda eşliyor —
  eşleşen numara OTP istediğinde gerçek SMS **hiç gönderilmiyor**, yalnızca
  eşlenen kod kabul ediliyor; eşleşmeyen her numara gerçek SMS akışında
  kalıyor. Resmi dokümantasyon ayrıca `SMS_TEST_OTP_VALID_UNTIL` (ISO 8601
  tarih) ile otomatik son kullanma tarihi öneriyor ("remove test OTPs before
  deploying to production").
  **Doğrulanamayan kısım (dürüstçe işaretleniyor, kural #103):** Bulduğum
  resmi dokümantasyon self-hosted Supabase'in (docker-compose + `.env`)
  konfigürasyonunu gösteriyor; hasat'ın **hosted** Supabase projesinde bu
  ayarın dashboard'da nerede olduğunu (Authentication → Providers → Phone
  altında benzer bir alan olması bekleniyor) bu oturumdan göremedim —
  tarayıcı erişimim yok. Berkin dashboard'da böyle bir alan olup olmadığını
  kontrol etmeli.
  **Canlı sisteme etki — ⚠️ yüksek dikkat gerektirir:** Bu proje-genelinde
  TEK bir Supabase projesi var (staging yok, `efuqpiaavrzimvstpdpm` hem
  web hem mobil hem de 25 Ağustos lansmanının kendisi). Bu ayar o **tek
  canlı projeye** yazılır. Etkilenen numaralar `_Context.md`'de zaten
  **public olarak belgeli** iki test hesabı (`905001234567`,
  `905009876543`) — yani bu ayar açılırsa, hasat-vault'u okuyan **herhangi
  biri** bu iki numarayla (sabit kod `123456` ile) canlı üründe "Ahmet
  Yılmaz" (farmer) ya da "Zeynep Kaya" (buyer) olarak oturum açabilir.
  `SMS_TEST_OTP_VALID_UNTIL` ile lansmandan (25 Ağustos) önceki bir tarihe
  otomatik son verilirse bu pencere daralır, ama "unutma riski" (elle
  kaldırmayı unutmak) bir tarih alanına devredilmiş olur, sıfırlanmaz.
  **Store review riski:** Yok — bu bir Auth backend ayarı, App
  Store/Play'e giden binary'de görünmez.
- **Sonuç:** En az kod, resmi/desteklenen yol, ama **public olarak bilinen
  iki gerçek hesaba** canlı projede kapı açıyor. Süre sınırı riski azaltır,
  sıfırlamaz.

**Seçenek B — Mobil app'te `__DEV__` koşullu dev-only giriş yolu**
- Kritik teknik gerçek: Supabase Auth'un `verifyOtp`'si tamamen
  sunucu-taraflı — client'ta `__DEV__` kontrolü tek başına geçerli bir
  session/JWT üretemez. Gerçekçi tek uygulanabilir hali: `__DEV__` ise
  client bir edge function'ı çağırıp o function'ın (service-role anahtarıyla)
  test numarası için bir session mint etmesi. `__DEV__` React Native'de
  yalnızca **client** tarafında var olan bir bayrak — production
  bundle'ında `false`'a düşer, ama **edge function'a bunun hiçbir
  yansıması yok**; function'ı `__DEV__` kontrolü olmadan doğrudan
  `curl`layan biri de aynı şekilde session alabilir. Yani bu seçenek de
  (A gibi) TEK canlı projeye bir server-taraflı bileşen ekliyor — ekstra
  güvenlik ancak function'ın kendisi (a) yalnızca 2 bilinen test numarasını
  kabul ederse VE (b) client'ın gönderdiği paylaşılan bir gizli anahtar/
  header'la ek olarak korunursa (ki bu da App'in kendi bundle'ına
  gömülmemeli — yine bir sızıntı riski) sağlanabilir.
  **Canlı sisteme etki:** A ile aynı mertebede (tek proje, yeni bir
  server-taraflı yüzey), üstelik daha fazla özel kod = daha fazla bakım +
  daha fazla "launch öncesi kaldırmayı unutma" riski (fonksiyonun kendisi
  silinmeli/gate'lenmeli, A'daki gibi tek bir dashboard alanı değil).
  **Store review riski:** Düşük ama sıfır değil — Apple reviewer'ı
  `__DEV__` dallanması içeren kodu release build'de göremez (strip
  edilir), ama fonksiyon ayrı bir uç nokta olarak dursa bile reviewer'ın
  bunu keşfetme ihtimali pratikte yok; asıl risk kullanıcı/güvenlik
  tarafında (yukarıda), store review tarafında değil.
- **Sonuç:** A'dan **daha fazla** kod ve bakım yükü, güvenlik profili en
  iyi ihtimalle A'ya eşit, kötü ihtimalle daha kötü (ekstra bir uç nokta).

**Seçenek C — Gerçek SMS ile devam (maliyeti kabul et)**
- **Canlı sisteme etki:** Sıfır — hiçbir değişiklik yok.
- **Store review riski:** Sıfır.
- **Gerçek maliyet:** Twilio SMS ücreti (düşük, muhtemelen M5-b/M6 boyunca
  toplamda birkaç yüz TL mertebesinde) + **döngü hızı**: her test girişi
  gerçek bir telefonun SMS almasını gerektiriyor. M5-a'nın doğrulaması
  (Appetize + gerçek SMS) bunun **mümkün** olduğunu kanıtladı, ama M5-b/M6
  boyunca tarif+offline+pişirme modu ekranlarını tekrar tekrar test etmek
  için her girişte gerçek bir SMS beklemek iterasyonu ciddi yavaşlatır.
- **Sonuç:** Sıfır risk, kanıtlanmış yol, ama en yavaş döngü.

**Öneri (karar değil, sadece sıralama gerekçesi):** C bugüne kadar zaten
çalışıyor ve risksiz — asıl soru "yavaş döngü M5-b/M6'nın geri kalanında ne
kadar sürtünme yaratacak" sorusu, bunun cevabını yalnızca Berkin (kendi
zaman bütçesine göre) verebilir. A, C'den daha hızlı ama **public olarak
bilinen iki hesaba** kapı açıyor — süre sınırıyla (`SMS_TEST_OTP_VALID_UNTIL`)
birlikte kullanılırsa risk sınırlı ve zamanlanmış olur. B, A'nın tüm riskini
taşıyıp üstüne kod/bakım yükü ekliyor — üç seçenek arasında en zayıf
görünüyor ama nihai karar Berkin'in.

#### 2 — Çiftçi girişi — **SADECE ARAŞTIRMA, KARAR VERİLMEDİ**

> Kural #107 — mobil koda hiçbir role-gate eklenmedi; `farmer` rolüyle
> giriş yapan bir hesap bugün de, bu PR sonrasında da, buyer ile birebir
> aynı tarif ekranlarını görüyor.

- **Seçenek 1 — Girişi engelle:** En temiz eşleşme "mobil v1 yalnızca
  tüketici" kararına, ama **Berkin'in kendi test hesabı `farmer` rolüyle
  kayıtlı** (görev metninde de belirtildiği gibi) — bu seçenek Berkin'in
  kendi gerçek telefonuyla mobil uygulamayı test etmesini de engeller,
  yalnızca 2 test hesabından biriyle (buyer, `905009876543`) giriş
  yapılabilir kalır. Madde 1'deki test-giriş sürtünmesiyle birleşince bu
  iki kısıt üst üste biner.
- **Seçenek 2 — Yönlendirme mesajı ("bu uygulama alıcılar için, çiftçi
  paneli web'de"):** Seçenek 1 ile aynı netlik, daha iyi UX (neden/nereye
  açıklanıyor) — ama Berkin'in kendi hesabıyla test etme kısıtı **aynen
  kalıyor** (mesajı görür, ama tarif ekranlarını kendi hesabıyla göremez).
- **Seçenek 3 — Tarifleri herkese göster, alıcı akışlarını gizle:** Bu
  turun kapsamı zaten **tamamen okuma yüzeyi** — "Talep Et" yok, checkout
  yok, malzeme kartında farmer'a özel gizlenecek gerçek bir "alıcı akışı"
  şu an **yok**. Yani bu seçenek bu tur için fiilen "hiçbir şey değiştirme"
  ile aynı sonucu veriyor — gerçek ayrım noktası M6'da "Talep Et" (buyer'a
  özel bir mutasyon) eklendiğinde ortaya çıkacak. Berkin'in kendi
  hesabıyla test etmeye devam edebilmesinin **tek** sürtünmesiz yolu bu.

**Gözlem (karar değil):** 1 ve 2, bu turun kendi test döngüsünü (Berkin'in
gerçek telefonu = farmer rolü) kırıyor; 3 kırmıyor çünkü şu an gizlenecek
gerçek bir akış yok. Bu, 3'ü şimdilik "az riskli" yapıyor ama kalıcı bir
cevap değil — M6'da "Talep Et" eklendiğinde bu üç seçenek yeniden
değerlendirilmeli (o zaman 3 de artık "hiçbir şey değiştirmeme" anlamına
gelmeyecek, gerçek bir alıcı-mutasyonu gizlenecek). Karar Berkin'in.

#### 3 — Tarif ekranları — **UYGULANDI**

`hasat-mobile`'da yeni: `app/home.tsx` (M5-a'nın "Giriş yapıldı ✓" yer
tutucusunun yerini alan tarif listesi) + `app/recipe/[slug].tsx` (detay) +
`src/lib/hasat/{recipes,types,format,crop-emoji,session}.ts` +
`src/components/hasat/{RepresentativePhoto,OfflineBanner}.tsx`.

- **Liste:** `recipes` (public+published) + web'deki `attachCoverFallback`
  mantığının birebir portu — kapak yoksa (18/18 NULL) ilk ana malzemenin
  crop görseline, o da yoksa nötr placeholder'a düşüyor.
- **Detay:** malzemeler + adımlar + `prep_minutes`/`cook_minutes`/`rest_minutes`
  ayrı ayrı (`formatTimeBreakdown`, hiçbir zaman tek sayıya toplanmıyor,
  P23-M4-c kararıyla birebir aynı), `rest_minutes > 120` → "Önceden
  başlamak gerekir" rozeti (`ADVANCE_START_THRESHOLD_MINUTES`, web'deki
  eşikle birebir aynı).
- **RPC'ler** — kural #106: `rpc_recipe_availability` ve
  `rpc_recipe_shopping_list` doğrudan çağrılıyor, eşleştirme/dönüşüm mantığı
  client'ta yeniden yazılmadı.
- **Malzeme kartı 3 durum:** eşleşti / platform crop ama eşleşmiyor /
  platform-dışı — web'deki mantığın portu. **Bilinçli fark:** eşleşen
  durumda web `/buyer/discover`'a giden bir "Ürüne Git" linki gösteriyor;
  mobilde o ekran M7'ye kadar yok, bu yüzden mobilde **kırık bir bağlantı
  yok** — fiyat/stok bilgisi salt-okunur gösteriliyor, tıklanabilir değil.
  Eşleşmeyen durumda "Talep Et" butonu **yok** (görev tanımı gereği bu tur
  kapsam dışı) — yalnızca nötr "Hasat'ta henüz yok" rozeti.
- **Kapsam kısıtlaması (bilinçli, görev metninde istenmemiş):** Web'deki
  zorluk/süre/diyet/mutfak filtreleri ve "şu an tam alınabilir" checkbox'ı
  mobile taşınmadı — görev tanımı (madde 3) yalnızca liste+detay+3-durum
  kartı+RPC'leri istiyor, filtre UI'ı istemiyor; eklemek kapsam
  genişletmesi olurdu.
- **Alt navigasyon YOK:** `Build/P23-Mobile-Visual-Spec.md`'nin 5-sekmelik
  tasarımı Keşfet/Siparişlerim/Hesabım ekranlarını varsayıyor (M7 kapsamı,
  henüz yok) — bu turda kurulmadı, `app/home.tsx` doğrudan tarif listesi
  oldu, çıkış butonu küçük bir metin linkine indirgendi (M5-a'nın
  oturum-kalıcılığı QA'sı hâlâ test edilebilsin diye).
- **`recipe_views` yazımı:** `useLogRecipeView` web'in birebir portu.
  **`session_id` kararı (uygulandı, raporlanıyor — bu bir onay maddesi
  değildi):** `src/lib/hasat/session.ts` — AsyncStorage'da kalıcı bir
  UUIDv4 (web'in `localStorage` + `crypto.randomUUID()`'ının mobil
  karşılığı). `crypto.randomUUID()` yerine `crypto.getRandomValues` ile
  elle üretim: Hermes'te `randomUUID` her yerde yok, ama
  `react-native-get-random-values` (zaten `large-secure-store.ts`
  üzerinden bootstrap ediliyor) `getRandomValues`'ı garanti ediyor — yeni
  bağımlılık gerekmedi. `SecureStore`/`LargeSecureStore` **bilinçli olarak
  kullanılmadı** — bu id gizli değil, AES/Keychain katmanı gereksiz
  maliyet olurdu.
- **Yeni bağımlılık yok — ikon kütüphanesi:** `lucide-react-native`
  eklenmedi (web `lucide-react` kullanıyor ama bu web-only import, mobile
  taşınmaz) — proje hiçbir yerde ikon kütüphanesi kullanmıyor
  (login.tsx/index.tsx emoji/metin deseni), `react-native-svg` native
  bağımlılığı eklemek EAS build kotası kısıtlıyken (M5-a-ek-2) gereksiz
  risk. Emoji/metin glifleri kullanıldı.

#### 4 — Offline önbellek (`expo-sqlite`) — **UYGULANDI, GERÇEK CİHAZDA DOĞRULANAMADI**

Yeni: `src/lib/offline/{db,recipeCache}.ts`, `src/lib/net/useIsOffline.ts`
(`expo-network`'ün resmi `useNetworkState()` hook'u üzerine ince katman —
`@react-native-community/netinfo` değil, proje her native yeteneği
`expo-*` paketleriyle karşılıyor, ek native-modül build riski almamak
için). `package.json`'a iki yeni bağımlılık: `expo-sqlite` (~57.0.1),
`expo-network` (~57.0.1) — ikisi de SDK 57 ile eşleşen resmi Expo paketleri.

**Ne önbelleklendi (net karar):** Yalnızca EDİTORYAL/DURAĞAN veri —
`recipes` (başlık, açıklama, kapak/temsili görsel URL'i, süre alanları,
zorluk, mutfak, diyet etiketleri), `recipe_steps` (adım metni, foto URL'i,
`timer_seconds`), `recipe_ingredients` (crop, serbest metin, miktar,
birim, not, ana-malzeme bayrağı).

**Ne önbelleklenmedi — bilinçli (görev metninin uyardığı tam nokta):**
`rpc_recipe_availability`/`rpc_recipe_shopping_list` (fiyat, stok, min.
sipariş, aktif ilan sayısı) **hiçbir zaman** sqlite'a yazılmıyor. Bu
RPC'ler `isOffline` true iken hiç çağrılmıyor bile (`enabled: !isOffline`).
**Sonuç:** offline'da yanlışlıkla bayat bir fiyat gösterme İMKANSIZ hale
geliyor — çünkü fiyat baştan hiç saklanmıyor, "ne zaman bayatladığını
göstermek" için bir zaman damgası mantığı kurmaya gerek kalmadı. Görev
metninin sunduğu iki seçenekten ("gösterme" ya da "son güncelleme: X ile
göster") **"gösterme"** seçildi — daha basit, doğrulanması daha kolay ve
"hızlı teslimat değiliz" güven teziyle daha tutarlı (bir zaman damgası bile
göstermek "bu fiyata güvenebilirsin, sadece X kadar eski" izlenimi
verebilirdi). Offline'da malzeme kartı bunun yerine "Çevrimdışı — fiyat ve
stok bilgisi gösterilmiyor" nötr metnini gösteriyor.

**Tazeleme stratejisi:** Cache-aside. Liste/detay ekranı önce ağı dener;
başarılı olursa (a) veriyi gösterir (b) aynı anda sqlite'a yazar (tam
liste için `DELETE`+`INSERT`, tek tarif için `INSERT ... ON CONFLICT
UPDATE`, 18 satırlık küçük bir tablo için basit tutuldu). Ağ başarısız
olursa ya da cihaz zaten offline'sa sqlite'tan okunur. **Bayat veri
göstergesi:** Görsel şartnamenin (`P23-Mobile-Visual-Spec.md` → "2.
Offline Durumu") Durum A/B'si birebir uygulandı — önbellek varken offline
→ üstte kapanmayan "📶✕ Çevrimdışısınız · görünen tarifler önbellekten"
şeridi (`OfflineBanner`), önbellek tamamen boşken offline → ayrı bir "Bağlantı
yok" tam ekranı + "Yeniden Dene". Fiyat/stok alanları için ayrı bir
zaman damgası YOK (yukarıdaki karar gereği — hiç gösterilmediği için
gerekmiyor).

**⚠️ Doğrulanamadı (kural #103, açıkça işaretleniyor):** `expo-sqlite`
native bir modül — bu oturumda ne bir simülatör ne bir cihaz var, `tsc
--noEmit` (temiz) dışında çalışma zamanı davranışı test edilemedi. Gerçek
uçak modu testi zaten `TODO.md` → "Apple hesabı gelince koşulacak testler"
altında device-only olarak işaretliydi (M5-a-ek'ten beri); bu madde de
oraya eklendi. **Berkin'in Appetize/simülatör üzerinden yapabileceği en
yakın test** (tam uçak modu değil, ama sqlite yazma/okuma döngüsünü
kanıtlar): bir tarifi normal açıp geri dön, uygulamayı Appetize'ın ağ
kontrolünden (varsa) ya da cihazın Wi-Fi'ını kapatarak yeniden aç — bkz.
`Build/E2E-QA.md` → S26.

#### 5 — `.env` bekçisi — **UYGULANDI**

**Kara liste → beyaz liste (`hasat-core/scripts/check-env-guard.mjs`):**
Önceki sürüm `EXPO_PUBLIC_` prefix zorunluluğu + 5 yasaklı kalıp
(`service_role`/`SECRET`/`PRIVATE`/`TOKEN`/`PASSWORD`) kullanıyordu —
M5-a-ek'in kendi QA'sında bulunan sınırın (`EXPO_PUBLIC_SERVICE_KEY` bu 5
kalıbın hiçbirini içermiyor, bekçiyi geçebilirdi) **tam olarak kapatılması
istendi**. Yeni mantık: `ALLOWED_NAMES` — yalnızca
`EXPO_PUBLIC_SUPABASE_URL` ve `EXPO_PUBLIC_SUPABASE_PUBLISHABLE_KEY`.
Listede olmayan **her** isim, kalıp taşısın taşımasın, reddediliyor.
**Test edildi:** gerçek `.env` → geçti (2/2 satır allowlist'te); `.env`'e
`EXPO_PUBLIC_APIKEY="..."` satırı eklendi (eski bekçinin YAKALAYAMAYACAĞI,
5 kalıbın hiçbirini taşımayan bir isim) → yeni bekçi reddetti, tam
beklenen sonuç. Test dosyası `/tmp` altında ayrı bir kopyada yapıldı,
gerçek `.env`'e dokunulmadı.

**Yanlış repo sorunu → `hasat-mobile`'da push workflow'u:** Yeni
`hasat-mobile/.github/workflows/env-guard.yml` — `.env` değiştiğinde
`push` üzerinde tetikleniyor (+ `workflow_dispatch`). Script'in kendisi
mobile'a KOPYALANMADI — `hasat-core`'u salt-okunur checkout edip oradan
çağırıyor (public repo, token gerekmiyor) — tek doğruluk kaynağı
`hasat-core/scripts/check-env-guard.mjs`'te kalıyor. `hasat-core`'daki
günlük 06:00 + push-to-core kontrolü **yedek katman** olarak kalıyor (artık
birincil savunma değil). İki workflow dosyası da PyYAML ile parse edilip
geçerli syntax olduğu doğrulandı.

#### 6 — Doğrulama (kural #96/#103)

| Kontrol | Sonuç |
|---|---|
| `rpc_recipe_availability`/`rpc_recipe_shopping_list` gerçek veriyle | ✅ Supabase MCP ile doğrudan SQL çağrısı yapıldı (`nohut-falafel`, 9 malzeme) — dönen alan isimleri/tipler `pg_proc` imzasıyla ve mobil TypeScript arayüzleriyle birebir eşleşti; `kekik` satırı `is_matched=true`+`rounded_up_to_min_order=true`+`recipes_covered=3333.33` ile UI'daki "min. sipariş yuvarlama" dalını gerçek veriyle doğruladı |
| `recipe_views` RLS/politika | ✅ `pg_policies` ile doğrudan okundu: `(user_id IS NULL) OR (user_id = auth.uid())`, `anon`+`authenticated` rollerine INSERT — kodun gönderdiği `{recipe_id, user_id: null veya auth.uid(), session_id}` payload'ıyla birebir uyumlu |
| `recipe_views` gerçek ağ INSERT'i (mobil client'la aynı payload) | 🔴 **Doğrulanamadı** — bu oturumun ağ politikası `efuqpiaavrzimvstpdpm.supabase.co`'ya doğrudan client erişimini engelliyor ("Host not in allowlist"), M5-a/M4-a'da da aynı kısıt yaşanmıştı. RLS-seviyesi doğrulama (yukarıda) gerçek yapıldı, ama uçtan uca ağ çağrısı Berkin'in kendi Appetize/simülatör testinde kanıtlanmalı (bkz. S26) |
| `tsc --noEmit` | ✅ Temiz (yeni bağımlılıklar `npm install` ile kuruldu, sıfır hata) |
| Offline sqlite yazma/okuma (gerçek runtime) | 🔴 **Doğrulanamadı** — native modül, bu oturumda simülatör/cihaz yok (kural #103) |
| `src/lib/core/` dokunulmadı mı | ✅ `git diff --stat -- src/lib/core/` boş |
| Yeni workflow YAML syntax | ✅ PyYAML ile parse edildi (`env-guard.yml`, `drift-check.yml`, `sync-to-web.yml`) |
| `.env` guard yeni/eski isim testi | ✅ Yukarıda "madde 5" |

#### Kapsam kuralı tutuldu

`src/lib/core/` elle düzenlenmedi (kural #105 — değişiklik yok, sync
bekleniyor), checkout/ödeme eklenmedi, push bildirimleri eklenmedi, AI
import eklenmedi, pişirme modu eklenmedi (adım listesi var ama timer'lı
"Pişirme Modu" ekranı M6), `unit_type` enum'una dokunulmadı, web reposuna
(`hasat-d2c-marketplace`) dokunulmadı, Supabase şemasına (yeni
tablo/kolon) dokunulmadı — yalnızca `execute_sql`/`pg_policies` ile
salt-okunur doğrulama yapıldı.
