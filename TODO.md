---
title: Hasat — Master Roadmap & Build Log
updated: 2026-07-23
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

---

## 🏗️ Lovable/Supabase Build Sırası

> **P19 + P17 serisi (B/C/F/E/G) + P20 + P21 (A, B+C) tamamen bitti, hepsi canlı doğrulandı.** **P17-A ve P17-D şirket kuruluşuna bağlı, bloke.** BENCHMARK Gap listesindeki bağımsız yapılabilecek her şey bitti (bkz. Gap durum tablosu altta). **Sıradaki büyük iş bloğu: P22 (Care Journal / Rutin Bakım, saffron sezonuna kadar esnek) — detayları dosyanın sonundaki "Onaylanan Yol Haritası — P21/P22/P23" bölümünde, P21 artık TAMAMLANDI olarak işaretli.**

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
| 10 | SMS/WhatsApp bildirim genişletmesi | P2 | ✅ **Kapandı (P20)** |
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

## 🔵 P20 — SMS/Bildirim Genişletmesi + Lojistik Bildirimi — **TAMAMEN TAMAMLANDI ✅** *(2026-07-21)*

**Kapsam:** BENCHMARK Gap #8 (lojistik adımı) ve #10 (SMS/WhatsApp bildirim genişletmesi)'nin bağımsız yapılabilecek kısmını kapatmak. Araştırma + backend Claude tarafından doğrudan Supabase MCP ile, frontend Lovable ile yapıldı.

### 🔴 Bulunan gerçek bug (acil düzeltildi)

`public.dispatch_sms()` SQL fonksiyonu `offer_accepted` ve `payment_confirmed` event'lerini doğru tanıyor, `notif_prefs`'teki ilgili tercihi kontrol ediyor, tercih açıksa `send-sms` edge function'ını çağırıyordu — **ama `send-sms`'in kendi `COL` map'i (TypeScript) bu iki event'i tanımıyordu**, `skipped: no-pref-mapping` dönüp sessizce SMS'i atlıyordu. Sonuç: kullanıcı arayüzünde "Teklif Kabul Edildi" / "Ödeme Onaylandı" SMS toggle'ları vardı, kullanıcı açabiliyordu, ama **hiçbir zaman çalışmıyordu**. `dispatch_sms` (SQL) ve `send-sms` (edge function, TS) içinde event→kolon eşlemesi iki farklı yerde ayrı ayrı tutulduğu için birbirinden sapmıştı — klasik "iki kaynak, tek doğruluk" hatası.

**Düzeltme:** `send-sms`'in `COL` map'i genişletildi, artık `dispatch_sms`'in SQL CASE'iyle birebir senkron (10 event: `new_offer`, `price_alert`, `harvest_time`, `offer_accepted`, `payment_confirmed`, `order_shipped`, `order_delivered`, `order_cancelled`, `dispute_opened`, `crop_request_match`). Kod içine iki mapping'in senkron tutulması gerektiğine dair yorum eklendi.

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

## 🟡 AĞUSTOS 2026 — Soft Launch
(Değişmedi)

## 🔵 SAFRAN SEZONU / ⬜ SONRAKI FAZLAR
(Değişmedi)

---

## 📋 Lovable/Supabase Prompt Yazma Kuralları

(1-95 önceki sürümde — devam:)
89. **[Önceki turda eklendi]** Aynı iş mantığının iki farklı yerde (SQL fonksiyonu + TypeScript edge function) ayrı ayrı tanımlanması, biri güncellenip diğeri güncellenmediğinde sessizce senkronsuz kalır.
90. **[Önceki turda eklendi]** Bir background job/agent tool çağrısı belirsiz bir hata dönerse, işin gerçekten başarısız olduğu varsayılmamalı — bağımsız kontrol edilmeli.
91. **[Önceki turda eklendi]** Gerçek dış servise (Twilio) para/mesaj harcayan bir entegrasyon test edilirken, gerçek bir SMS gönderileceği not düşülmeli, test öncesi/sonrası durum eski haline döndürülmeli.
92. **[Önceki turda eklendi]** "Yeni bir kavram için tablo lazım" varsayımına geçmeden önce mevcut şema sorgulanmalı.
93. **[Önceki turda eklendi, bu turda kesinleşti]** Bir kolonun DB default'u ile frontend'in o kolonu insert sırasında explicit set edip etmediği farklı şeyler — kontrol edildi, form her zaman explicit gönderiyordu, gerçek çözüm form/UX seviyesinde oldu (P21-A).
94. **[Önceki turda eklendi]** Bir "boşluk" iddiası kod okunmadan üç farklı yanlış çıkabilir (yok → var ama eksik → aslında var ama dağınık) — plan yapmadan önce birden fazla kod-okuma turu yatırımı üçüncü bir yanlış varsayımı da eleyebilir.
95. **[Önceki turda eklendi]** Aynı hesaplama mantığının SQL trigger + frontend hook'ta paralel yazılması, birim normalizasyonu gibi ortak bir hatayı iki yerde aynı anda taşıyabilir.
96. **[Bu turda eklendi] `plan_mode=true` isteği Lovable tarafında güvenilir bir şekilde uygulanmıyor — agent araştırma yapması istendiğinde bile doğrudan koda geçip commit atabiliyor.** Bu, P21 boyunca en az 4 kez tekrarladı (her seferinde farklı bir konuda: batch açma akışı, buyer çoklu-batch akışı, birim dönüşümü, teslimat/ödeme parity). Ders: `plan_mode=true` sadece bir *tercih* sinyali, bir *garanti* değil — her mesajdan sonra `get_diff`/DB sorgusuyla gerçekten ne değiştiğini bağımsız doğrulamak zorunlu, "sadece plan istemiştim" diye implementasyonu görmezden gelinmemeli.
97. **[Bu turda eklendi] Postgres'te her tip için her aggregate fonksiyon yok — `MIN()`/`MAX()` `uuid` tipinde tanımlı değil.** Bir trigger/fonksiyon `SELECT min(uuid_kolonu)` gibi bir ifade içeriyorsa bu ancak ilgili satır gerçekten insert edilene kadar fark edilmez (migration'ın kendisi hata vermez, sadece trigger tetiklendiğinde patlar) — yeni yazılan her trigger, migration'dan hemen sonra gerçek bir insert ile tetiklenip test edilmeli, "migration başarıyla uygulandı" ile "trigger doğru çalışıyor" aynı şey değil.
98. **[Bu turda eklendi] Bir "tek kaynak, tek doğruluk" öğesi (örn. crop adı) sistemde birden fazla temsille (Title Case görüntü ismi vs. lowercase slug) var olduğunda, hangisinin *kanonik* (DB'ye yazılan) olduğu açıkça seçilmeli — görüntüleme ismi asla yazma değeri olarak kullanılmamalı.** Kanonik formu seçerken sistemin en çok bağımlı olduğu tarafı (burada: `crop_config.crop` slug'ı, çünkü P19'un tüm fiyat/borsa zinciri ona bağlıydı) esas almak, görsel olarak "daha okunaklı" görünen tarafı (Title Case) seçmekten daha güvenli — göz önünde olan her zaman daha az bağımlılığı olan taraf değildir.
99. **[Bu turda eklendi] Bir alanın (burada: birim, g/kg/L/adet) "aynı crop içinde tek bir aile" olduğu varsayımı doğruysa, farklı birimler arasında toplama yaparken göz ardı etmek (sadece "N parti" göstermek) yerine gerçek dönüşüm yapmak tercih edilmeli — ama bu ancak crop_config'te zaten var olan bir kanonik-birim referansı (`default_unit`) varsa güvenli otomatikleştirilir; yoksa önce o referansın var olup olmadığı kontrol edilmeli.**
100. **[Bu turda eklendi] Yeni bir akış (burada: çoklu-batch teklif sayfası) inşa edilirken, o akışın "kardeşi" olan mevcut bir akış (tek-batch teklif sayfası) sadece UI-seviyesinde değil *veri akışı* seviyesinde de (nereye ne zaman insert edildiği, hangi ara adımdan geçtiği) incelenmeli.** Yüzeysel karşılaştırma ("ikisi de miktar+fiyat topluyor") gerçek bir mimari farkı (biri anında insert ediyor, diğeri pending-store+payment-adımı üzerinden gidiyor) gözden kaçırabilir — bu fark, kullanıcı gerçek ekranı gördüğünde (Berkin'in ekran görüntüsü karşılaştırması gibi) ortaya çıktı, kod okumasıyla önceden yakalanabilirdi.

---

## 📌 Kararlar

(önceki tablo + eklenenler:)
| **P17-G tamamen tamamlandı (2026-07-21)** | 20 KPI view'ı + admin dashboard, canlı doğrulandı. |
| **P20 tamamlandı (2026-07-21)** | SMS bildirim genişletmesi — gerçek bir kırık bug bulunup düzeltildi, iki senaryo gerçek Twilio SMS'i ile doğrulandı. |
| **P21/P22/P23 serisi onaylandı (2026-07-23)** | Berkin'in el notlarından doğan Batch/Care-Journal/Recipe-App genişlemesi, derinlemesine denetimden geçirildi ve 7 kararla kesinleşti. |
| **P21 gerçek koda göre revize edildi (2026-07-23)** | Draft migration'ı yerine form/UX çözümü; Keşfet grouping "yeni kapasite" değil "düzenleme" olarak revize edildi. |
| **P21-A TAMAMLANDI (2026-07-23)** | Kontrollü batch açma akışı canlı doğrulandı; `min(uuid)` bug'ı ve crop case-mismatch (Safran/safran karışıklığı, P19'un lowercase-slug mimarisiyle çakışıyordu) bulunup düzeltildi, backfill yapıldı. |
| **P21-B+C TAMAMLANDI (2026-07-23)** | `offer_items` şeması, çoklu-batch Keşfet/ürün sayfası, satır-bazlı stok kontrolü, birim-uyuşmazlık trigger'ı, traceability RLS — hepsi gerçek veriyle (yeterli/yetersiz stok, birim uyuşmazlığı senaryoları) doğrulandı. Ek olarak Berkin'in canlı ekran karşılaştırmasıyla bulunan 3 eksik (teslimat/tarih alanları + ödeme-parity, MCP create-offer'ın offer_items'sız kalması, farmer tarafı parti dağılımı eksikliği) da aynı gün tamamlandı. |

---

## 📋 Son Test Sonuçları

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
(Değişmedi — bkz. önceki sürüm)

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
| P21-H *(yeni, bu turda ortaya çıktı)* | Çoklu-batch akışının teslimat/tarih/ödeme-parity + MCP create-offer + farmer breakdown eksikleri | ✅ TAMAMLANDI |

### P22 — Care Journal (Rutin Bakım) (P1, saffron sezonuna kadar esnek) — ⬜ Planlandı, henüz başlanmadı

Berkin kararı (1. cevap): `harvest_entries`'ten (hasat olayı) ayrı bir tablo. Tek "Journal" sayfasında iki sekme: **Rutin Bakım** (yeni, Bevel-tarzı toggle) + **Hasat Kayıtları** (mevcut `harvest_entries` formu).

| Kod | Konu | Kapsam | Bağımlılık | Durum |
|---|---|---|---|---|
| P22-A | Care Journal şeması | 4 yeni tablo: `journal_entry_types`, `journal_themes`, `crop_journal_glossary`, `care_journal_entries` (`listing_id` FK ile batch bağlantısı — P21 sayesinde artık hangi batch'e bağlanacağı net) | Yok | ⬜ Planlandı |
| P22-B | Customize Journal ekranı (Bevel referansı) | Preset+custom entry type CRUD, kategori sekmeleri, frequency/threshold modalı | P22-A | ⬜ Planlandı |
| P22-C | Crop Glossary üretimi | AI ile tek seferlik paragraf-tooltip üretimi | P22-A | ⬜ Planlandı |
| P22-D | Journal sayfası UI | İki sekme, list/calendar view, filtreler, overdue highlight | P22-B, P22-C | ⬜ Planlandı |
| P22-E | Yeni crop type ekleme wizard'ı | `crop_config` + `crop_market_sources` + `journal_themes` + otomatik-draft-batch'i tek akışta birleştir | P22-A | ⬜ Planlandı |

**Not (P21'den miras):** Crop adlandırması artık her yerde `crop_config.crop` kanonik slug'ı — P22'nin yeni tabloları da `crop text NOT NULL REFERENCES crop_config(crop)` FK'sini olduğu gibi kullanabilir, ek bir case-normalizasyon riski yok (P21-A'da temizlendi).

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

### Sıradaki adım
P22-A için Lovable'a plan/implementasyon isteği gönderilecek: Care Journal şeması (4 tablo) — Berkin onayladığında başlatılacak. **Not:** `plan_mode=true` göndersek de Lovable'ın doğrudan implementasyona geçebileceği unutulmamalı (ders #96) — her turda `get_diff`/DB ile bağımsız doğrulama yapılacak.
