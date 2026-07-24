---
title: Hasat — Master Roadmap & Build Log
updated: 2026-07-24
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
(Değişmedi — bkz. önceki sürüm, "Şahıs şirketi kur" hâlâ işaretsiz)

### Düşük öncelikli cila
(Değişmedi — bkz. önceki sürüm)
- [ ] `useSetDefaultAddress` diğer adresleri `false`'a çekmiyor — düşük öncelik.
- [ ] **[Yeni]** P22-D+E+F birleşik tarayıcı QA'sı — güncel test case aşağıda "⚠️ Kalan iş" bölümünde (Rutin Bakım'ın günlükle birleşmesi, parsel-bazlı gruplama, tarih filtresi, yeni ürün türü talep sistemi tek akışta test ediliyor).
- [x] `buyer.producer.$id`'nin guest-erişiminde `BuyerShell`'in hatasız render olduğu **manuel QA ile doğrulandı (Berkin, 2026-07-24)** — gizli sekmede sayfa hatasız yüklendi, guest CTA metinleri doğru göründü, `/login`'e yönlendirme çalıştı. (bkz. P24 doğrulama tablosu)

---

## 🏗️ Lovable/Supabase Build Sırası

> **P19 + P17 serisi (B/C/F/E/G) + P20 + P21 (A, B+C) + P24 tamamen bitti, hepsi canlı doğrulandı.** **P17-A ve P17-D şirket kuruluşuna bağlı, bloke.** BENCHMARK Gap listesindeki bağımsız yapılabilecek her şey bitti (bkz. Gap durum tablosu altta). **P22 serisi (A/B/C/D/E/F) tamamen bitti, hepsi `main`'de (PR #1 ve PR #3 ikisi de merge edildi) — tek kalan iş tarayıcı QA.** Detaylar dosyanın sonundaki "Onaylanan Yol Haritası — P21/P22/P23" bölümünde. **Not:** Bir sonraki Lovable turuna geçmeden önce workspace kredisi kontrol edilmeli (P24 sonunda bitmişti).

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
| 9 | Parselden tabağa QR görünümü | P2 | ⬜ Yapılmadı — landing v2 ile senkron |
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

## 🔵 SAFRAN SEZONU / ⬜ SONRAKI FAZLAR
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

### PR durumu
[PR #1](https://github.com/berkinsavciozen/hasat-d2c-marketplace/pull/1) `main`'e merge edildi (2026-07-24) — P22-A/B/C/D `main`'de.
[PR #3](https://github.com/berkinsavciozen/hasat-d2c-marketplace/pull/3) `main`'e merge edildi (2026-07-24) — P22-D'nin parsel-gruplama düzeltmesi + P22-E + P22-F artık `main`'de.

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

### Sıradaki adım
P22 serisi (A/B/C/D/E/F) tamamen bitti, hepsi `main`'de (PR #1 ve PR #3 ikisi de merge edildi, 2026-07-24). Kalan tek şey: yukarıdaki tarayıcı QA test case'i (Berkin'in kendi testinde yapacağı). QA tamamlandığında P22 serisi tamamen kapanmış olacak.

### P23 — Buyer Mobile & Recipe App (P2, ayrı faz — soft launch'u bloklamıyor) — ⬜ Planlandı, henüz başlanmadı

Berkin kararı (7. cevap): Recipe App şimdilik tüm `buyer_type` segmentlerine açık.

**Referans araştırması (Berkin'in 2 App Store linki):**
- **Eatr:** süre/beceri/diyet filtreleri, besin değeri bilgisi. Abonelik iptal/refund şikayetleri var — Hasat bu hatayı tekrarlamamalı.
- **ReciMe:** "Order Groceries" özelliği Berkin'in cross-sell fikriyle örtüşüyor, Hasat kendi envanterine sahip olduğu için yapısal avantajlı.

| Kod | Konu | Kapsam | Bağımlılık | Durum |
|---|---|---|---|---|
| P23-A | Buyer mobile + Recipe App keşif/tasarım | React Native, shopping+recipe birlikte mobile-first | P21 tamamlandı ✅ (artık başlanabilir) | ⬜ Planlandı |
| P23-B | Recipe↔Crop eşleştirme + RFQ otomasyonu | Malzeme Hasat'ta yoksa otomatik `crop_requests` önerisi | P23-A | ⬜ Planlandı |
| P23-C | Mobile compliance | App Store/Play Store in-app purchase, KVKK, OTP/biometric | P23-A | ⬜ Planlandı |

### Kural #104 (2026-07-24'te eklendi)
Berkin'in kararı: bundan sonra Claude Code planları, arayüzde test edilmesi gereken adımlar için **kullanıcı-akışı dilinde adım adım bir test case** olarak sunulmalı (hangi sayfa açılacak, hangi butona tıklanacak, ne görülmesi bekleniyor) — trigger/kolon/event isimleri gibi DB-jargonuyla değil. Genel plan anlatımı da (yeni tablo/akış gibi kapsamlı işlerde) bir PM'in anlatacağı gibi olmalı: kullanıcı ne yapıyor, FE'de ne değişiyor, BE'de ne değişiyor — teknik isimler (trigger/policy adı gibi) sadece gerekince, ayrıntı seviyesinde geçmeli.
