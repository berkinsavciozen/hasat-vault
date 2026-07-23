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

> **P19 + P17 serisi (B/C/F/E/G) + P20 tamamen bitti, hepsi canlı doğrulandı.** **P17-A ve P17-D şirket kuruluşuna bağlı, bloke.** BENCHMARK Gap listesindeki bağımsız yapılabilecek her şey bitti (bkz. Gap durum tablosu altta). **Sıradaki büyük iş bloğu: P21 (Batch/Vitrin çoklu-batch mimarisi, soft launch öncesi P0) — Berkin'in 22-23.07.2026 el notlarından doğdu, 2026-07-23'te onaylandı ve gerçek kod/DB araştırmasıyla revize edildi, detayları dosyanın sonundaki "Onaylanan Yol Haritası — P21/P22/P23" bölümünde.**

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

## 🟡 AĞUSTOS 2026 — Soft Launch
(Değişmedi)

## 🔵 SAFRAN SEZONU / ⬜ SONRAKI FAZLAR
(Değişmedi)

---

## 📋 Lovable/Supabase Prompt Yazma Kuralları

(1-88 önceki sürümde — devam:)
89. **[Bu turda eklendi] Aynı iş mantığının (örn. "event → tercih kolonu" eşlemesi) iki farklı yerde (SQL fonksiyonu + TypeScript edge function) ayrı ayrı tanımlanması, biri güncellenip diğeri güncellenmediğinde sessizce senkronsuz kalır** — kullanıcı tercihi UI'da açık görünür, backend'in bir katmanı "evet, gönder" der, diğer katman event'i tanımadığı için sessizce iptal eder. Böyle bir çift-mapping deseni varsa, her ikisi de aynı anda güncellenmeli ve kod yorumlarıyla birbirine referans verilmeli.
90. **[Bu turda eklendi] Bir background job/agent tool çağrısı "Error occurred during tool execution" gibi belirsiz bir hata dönerse, işin gerçekten başarısız olduğu varsayılmamalı** — Lovable gibi agent'lar bazen işi arka planda tamamlayıp yine de hata görünümünde bir sonuç dönebiliyor. Bir sonraki adımda dosya/veri durumu bağımsız kontrol edilmeli, sadece hatayı görüp yeniden komut vermek (ve olası çift-çalıştırma riskini almak) yerine önce "gerçekten ne oldu" sorusuna cevap aranmalı.
91. **[Bu turda eklendi] `dispatch_sms`/`send-sms` gibi gerçek dış servise (Twilio) para/mesaj harcayan bir entegrasyon test edilirken, gerçek bir SMS gönderilecek olması açıkça not düşülmeli** — test verisi gibi sessizce oluşturup silinemez, kullanıcının telefonuna gerçek bir mesaj gider. Test öncesi/sonrası tercih durumu (`notif_prefs`) değiştirilip test sonrası eski haline döndürülmeli.
92. **[Bu turda eklendi] "Yeni bir kavram için tablo lazım" varsayımına geçmeden önce mevcut şema sorgulanmalı.** Berkin'in notlarında "Batch kavramı şemada tamamen yok" denmişti; gerçek şemayı sorguladığımda `listings`+`listing_harvest_entries`+`harvest_entries` üçlüsünün bunun temelini zaten attığı görüldü (aynı çiftçinin aynı crop'ta gerçekten birden fazla aktif listing'i mock data'da mevcuttu). Yanlış "sıfırdan yeni" varsayımı, gereksiz yere büyük bir migration'a yönlendirebilirdi. Ders: kavramsal bir boşluk iddiası, kod/şema okunmadan doğru kabul edilmemeli.
93. **[Bu turda eklendi] Bir kolonun DB default'u ile frontend'in o kolonu insert sırasında explicit set edip etmediği farklı şeyler.** `listings.status` default'u `'active'` bulundu ama bunu `'draft'`a çevirmeden önce, mevcut "yeni listing oluştur" formunun `status`'ü explicit gönderip göndermediği kontrol edilmeli — göndermiyorsa default değişikliği mevcut akışı sessizce kırar (yeni listing'ler artık görünmez olur). **[Güncellendi 2026-07-23]** Kontrol edildi: form her zaman explicit `status:'active'` gönderiyor (`queries.ts:806`), DB default'u hiç kullanılmıyor — bu yüzden migration yerine gerçek çözüm form/UX seviyesinde (bkz. P21-A).
94. **[Bu turda eklendi] Bir "boşluk" iddiası kod okunmadan iki kez yanlış çıkabilir — önce "yok" denip sonra "var ama eksik" bulunabilir, üçüncü kez "aslında zaten var, sadece dağınık" çıkabilir.** P21-A için üç farklı katmanda üç farklı gerçek bulundu: (1) DB default'u active ama önemsiz çünkü form onu hiç kullanmıyor, (2) parsel-tetikli otomatik draft-listing mekanizması (`create_draft_listings_for_parcel` trigger'ı) zaten var ama tek-batch mantığında (parcel+crop başına 1 listing, ikincisini engelliyor), (3) stok "azalma trigger'ı" ihtiyacı zaten `enforce_offer_stock` ile karşılanmış (dinamik hesaplama, sayaç değil) ama `offer_items`'a uyarlanması gerekiyor. Ders: bir mimari boşluk hipotezini doğrulamadan planlamaya iki kod-okuma turu (biri "var mı" sorusu, biri "nasıl çalışıyor" sorusu) yatırım yapmak, üçüncü bir yanlış varsayımı da elemiş oluyor.
95. **[Bu turda eklendi] Aynı hesaplama mantığının SQL trigger + frontend hook'ta paralel yazılması (RPC/view paylaşılmadan), birim normalizasyonu gibi ortak bir hatayı iki yerde aynı anda taşıyabilir.** `enforce_offer_stock` (SQL) ve `useListingStock` (frontend) aynı "available = batch_total - reserved" formülünü ayrı ayrı yazmış; ikisi de `harvest_entries.quantity`'nin birimini (`kg` vs `g`) doğrulamıyor. Fix tek bir yerde yapılırsa diğeri sessizce eski/yanlış kalır — P21-D bu ikisini tek kaynaktan (paylaşılan RPC) besleyecek şekilde birleştirmeyi de kapsıyor.

---

## 📌 Kararlar

(önceki tablo + eklenenler:)
| **P17-G tamamen tamamlandı (2026-07-21)** | 20 KPI view'ı + admin dashboard, canlı doğrulandı. Detaylar önceki sürümde. |
| **P20 tamamlandı (2026-07-21)** | SMS bildirim genişletmesi (BENCHMARK Gap #8+#10) — gerçek, önceden var olan ama kırık bir bug (`offer_accepted`/`payment_confirmed` SMS'i hiç çalışmıyordu) bulunup düzeltildi; kargo/teslim/iptal/ihtilaf/RFQ-eşleşme event'leri SMS'e bağlandı; iki senaryo gerçek Twilio SMS'i ile uçtan uca doğrulandı. **BENCHMARK Gap listesindeki bağımsız yapılabilecek her şey artık bitti** — kalanlar (P17-A/D bloke, #9/#11/#12 uzun vadeli/P2) launch'u bloklamıyor. |
| **P21/P22/P23 serisi onaylandı (2026-07-23)** | Berkin'in 22-23.07.2026 el notlarından (GİFTGİ 1-5, BUYER) doğan Batch/Care-Journal/Recipe-App genişlemesi, derinlemesine denetimden geçirildi (mevcut şema sorgulanarak) ve Berkin'in 7 kararıyla kesinleşti. |
| **P21 gerçek koda göre revize edildi (2026-07-23, aynı gün)** | `listings.status` default migration'ı yerine form/UX değişikliği (P21-A); Keşfet'te çoklu-batch görünümü zaten dağınık halde var olduğu için "yeni kapasite" değil "düzenleme" işi olarak revize edildi (P21-B); stok azalma "trigger'ı" zaten `enforce_offer_stock` ile karşılanmış, `offer_items`'a uyarlanacak (P21-D); Vitrin'de çift stok gösterimi (ham+hesaplanmış) düzeltilecek (yeni: P21-G). Detaylar dosyanın sonunda. |

---

## 📋 Son Test Sonuçları

### P20 Tam Doğrulama (2026-07-21) ✅
| Kontrol | Sonuç |
|---|---|
| `send-sms` COL map senkronizasyon bug'ı (`offer_accepted`/`payment_confirmed`) | ✅ Bulundu (SQL ile `dispatch_sms` ve edge function kodu karşılaştırılarak), düzeltildi |
| `notif_prefs` 5 yeni kolon | ✅ `information_schema` ile doğrulandı |
| `notify_order_status` trigger — spesifik mesaj + kargo bilgisi + SMS tetikleme | ✅ Gerçek order'da (delivered) test edildi, in-app bildirim + Twilio SMS doğrulandı |
| `useCreateCropRequest` → `dispatch_sms('crop_request_match')` | ✅ Gerçek crop_request testiyle, Twilio SMS `net._http_response`'ta doğrulandı |
| Bildirim tercihleri UI toggle'ları (farmer+buyer) | ✅ Dosya okumasıyla doğrulandı, doğru kolon eşlemeleri |
| Gerçek Twilio SMS — senaryo 1 (teslim edildi) | ✅ `net._http_response` id=34, status accepted |
| Gerçek Twilio SMS — senaryo 2 (RFQ eşleşme) | ✅ `net._http_response` id=36, status accepted |
| Test verisi/tercih temizliği | ✅ `order_delivered_sms` eski haline döndürüldü, test crop_request silindi |

### P17-G Tam Doğrulama (2026-07-21) ✅
(Değişmedi — bkz. önceki sürüm)

### P17-E Tam Doğrulama (2026-07-21) ✅
(Değişmedi — bkz. önceki sürüm)

### (Önceki tüm test sonuçları — değişmedi, önceki sürümlerde)

----------------------------
New PLAN FROM BERKİN NOTES

# Hasat — Batch/Care Journal/Buyer Genişlemesi: Netleşen Kapsamlı Plan

*Bu plan senin 7 cevabına göre oluşturuldu. Aşağıdaki "🟤 Onaylanan Yol Haritası — P21/P22/P23" bölümünde kalıcı P-serisi formatına işlenmiştir; bu bölüm taslak/tartışma kaydı olarak referans amaçlı korunuyor. P21 alt maddeleri, sonraki turlarda gerçek kod/DB araştırmasıyla revize edilmiştir — güncel hâli dosyanın en sonundadır.*

---

## Cevaplarının özeti ve aldığım kararlar

| # | Senin cevabın | Aldığım mimari karar |
|---|---|---|
| 1 | Care journal ayrı, en kolay/anlaşılır neyse öyle | Ayrı tablo (`care_journal_entries`) + tek "Journal" sayfasında iki sekme |
| 2 | Draft migration'ı bana bırak, önceliklendir | Kontrol ettim, gerçek boşluk doğrulandı → **P0, ilk iş** (sonradan revize: migration değil form/UX işi) |
| 3 | Şimdilik tek offer, tıklayınca batch'ler ayrı görünsün | `offer_items` ara tablosu (yeni) |
| 4 | Sold/expired batch geçmiş siparişten hâlâ görülebilsin | RLS + query katmanında garanti edilecek, madde 4'te detay |
| 5 | Eatr + ReciMe | Araştırdım, aşağıda özellik karşılaştırması var |
| 6 | Aynı çiftçinin aynı crop'ta birden fazla batch'i → Keşfet'te ikisi de görünsün, buyer stok görüp seçsin, toplam fiyat değişsin | Ürün detay sayfası = batch seçici, `offer_items` ile fiyat dinamik hesaplanır |
| 7 | Recipe App şimdilik herkese açık | `buyer_type` segmentasyonu launch'ta yok, ileride veri ile karar verilir |

---

## BÖLÜM A — Şema Değişiklikleri (net karar verildi, implementasyona hazır)

### A.1 — `listings.status` default düzeltmesi — **[İPTAL, bkz. P21-A revize]**
~~`ALTER TABLE listings ALTER COLUMN status SET DEFAULT 'draft';`~~ Kontrol edildi: manuel form her zaman explicit `status:'active'` gönderiyor, DB default'u hiç kullanılmıyor. Gerçek çözüm P21-A'da form/UX seviyesinde revize edildi (bkz. dosya sonu).

### A.2 — Care Journal (rutin bakım) — YENİ tablolar
Bevel modelini (toggle + frequency + threshold) Hasat'ın crop/parsel/batch yapısına uyarlayan 4 yeni tablo:

```sql
-- Global preset + custom entry type tanımları (örn. "Sulama", "İlaçlama", "Soğuk Depo Kontrolü")
CREATE TABLE journal_entry_types (
  code text PRIMARY KEY,              -- sistem-parse edilebilir, snake_case, örn. 'irrigation'
  display_name text NOT NULL,         -- kullanıcıya görünen TR isim, örn. 'Sulama'
  category text NOT NULL,             -- 'care' | 'cold_storage' | 'logistics' | 'safety'
  input_type text NOT NULL DEFAULT 'toggle', -- 'toggle' | 'quantity' | 'note'
  is_preset boolean NOT NULL DEFAULT true,   -- sistem preset'i mi, çiftçinin custom'u mu
  created_by uuid REFERENCES profiles(id),   -- custom ise kim oluşturdu, preset ise NULL
  created_at timestamptz NOT NULL DEFAULT now()
);

-- Crop bazlı default temalar: hangi entry type'lar hangi sıklıkta takip edilecek
CREATE TABLE journal_themes (
  id uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  crop text NOT NULL REFERENCES crop_config(crop),
  entry_type_code text NOT NULL REFERENCES journal_entry_types(code),
  frequency_days integer,             -- kaç günde bir (preset default, örn. sulama=2)
  threshold jsonb,                    -- opsiyonel eşik/hedef değer (örn. {"target_liters": 20})
  is_preset boolean NOT NULL DEFAULT true,
  farmer_id uuid REFERENCES profiles(id), -- NULL=global preset, doluysa çiftçinin override'ı
  created_at timestamptz NOT NULL DEFAULT now(),
  UNIQUE (crop, entry_type_code, farmer_id)
);

-- Merkezi, tek seferlik AI-üretilen glossary (crop × entry_type bazlı tooltip metni)
CREATE TABLE crop_journal_glossary (
  crop text NOT NULL REFERENCES crop_config(crop),
  entry_type_code text NOT NULL REFERENCES journal_entry_types(code),
  glossary_text text NOT NULL,        -- senin domates örneğin gibi, tek paragraf
  generated_by text NOT NULL DEFAULT 'ai', -- 'ai' | 'manual' — şeffaflık için
  updated_at timestamptz NOT NULL DEFAULT now(),
  PRIMARY KEY (crop, entry_type_code)
);

-- Gerçek günlük bakım kayıtları (Bevel'deki "Today's Entries" karşılığı)
CREATE TABLE care_journal_entries (
  id uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  parcel_id uuid NOT NULL REFERENCES parcels(id),
  farmer_id uuid NOT NULL REFERENCES profiles(id),
  crop text NOT NULL,
  entry_type_code text NOT NULL REFERENCES journal_entry_types(code),
  entry_date date NOT NULL DEFAULT CURRENT_DATE,
  status text NOT NULL,               -- 'done' | 'skipped' | 'partial' (toggle'ın 3 hali, Bevel'deki X/-/✓ gibi)
  value jsonb,                        -- input_type='quantity' ise miktar, 'note' ise metin
  listing_id uuid REFERENCES listings(id), -- HANGİ BATCH'E ait — otomatik dolu, editable (4.4 notun)
  created_at timestamptz NOT NULL DEFAULT now()
);
```

**Neden ayrı tablo (senin sorunun cevabı — "en kolay/anlaşılır" hangisi):** `harvest_entries` zaten olay-bazlı, tekil ve stok-etkili bir kayıt (miktar+kalite+maliyet). Bunu toggle-bazlı günlük rutin kayıtlarla (sulama yaptım/yapmadım) aynı tabloya karıştırmak, hem sorgu mantığını (biri stoğa yazar biri yazmaz) hem de UI'yı karmaşıklaştırır. Ayrı tutup **tek "Journal" sayfasında iki sekme** göstermek ("Rutin Bakım" — toggle liste, Bevel tarzı; "Hasat Kayıtları" — mevcut `harvest_entries` formu) çiftçi için en az kafa karıştırıcı çözüm: iki farklı eylem türü (günlük check-in vs. olay kaydı), iki görsel dil, ama aynı sayfa/aynı crop/parsel bağlamı.

### A.3 — Multi-batch offer (senin cevabın: tek offer, batch'ler ayrı görünür)
```sql
CREATE TABLE offer_items (
  id uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  offer_id uuid NOT NULL REFERENCES offers(id) ON DELETE CASCADE,
  listing_id uuid NOT NULL REFERENCES listings(id),
  quantity numeric NOT NULL,
  price_per_unit numeric NOT NULL,    -- batch'ler farklı fiyatlı olabilir (6. cevabın)
  created_at timestamptz NOT NULL DEFAULT now()
);
```
- `offers.listing_id`/`quantity`/`price_per_unit` **geriye dönük uyumluluk için kalır** ama artık "birincil/toplam" değeri temsil eder — tek-batch teklifler `offer_items`'a da 1 satır olarak yazılır (kod tarafında ikisi senkron tutulur, aynı P20'deki `dispatch_sms`/`send-sms` dersini burada tekrar etmemek için tek yazma noktası/trigger önerilir).
- Offer detay sayfasında "batch'lere göre dağılım" gösterimi `offer_items` üzerinden JOIN ile yapılır, her satırda o batch'e tıklanınca `listing_harvest_entries → harvest_entries` (Batch 5.4'teki calendar/list view) açılır.
- `orders` tablosu değişmez (`offer_id` üzerinden zaten tüm zincire erişiyor).
- **[Not, 2026-07-23]** `enforce_offer_stock` trigger'ı şu an `offer.listing_id` (tekil) üzerinden çalışıyor — `offer_items`'a geçince bu fonksiyon **satır bazlı** (her `offer_item.listing_id` için ayrı ayrı) stok kontrolü yapacak şekilde uyarlanmalı. Sıfırdan yazılmayacak, mevcut fonksiyon genişletilecek.

**Buyer tarafında akış (6. cevabın uyarlaması):** Buyer Keşfet'te bir crop+çiftçi kartına tıkladığında, o çiftçinin o crop'taki **tüm aktif batch'leri** (birden fazla `listings` satırı) tek bir ürün detay sayfasında listelenir — her batch kendi stoğu ve (varsa farklı) fiyatıyla görünür. Buyer her batch'ten istediği miktarı seçer, alt toplam **canlı güncellenir** (her satır quantity×price, toplamı sepette gösterilir). "Teklif gönder" dendiğinde tek bir `offers` satırı + N adet `offer_items` satırı oluşur.

### A.4 — Traceability sonrası satış/expire garantisi (senin cevabın: hep görünür kalsın)
Şema tarafında ek bir şey gerekmiyor (zincir zaten silinmiyor), **ama iki şey netleşmeli:**
1. **RLS politikası:** Buyer'ın `listings`/`harvest_entries`/`care_journal_entries`'e okuma erişimi, "bu listing'den bir siparişi var mı" koşuluna bağlı olmalı (aksi halde ya herkese açık olur ki bazı veriler — maliyet gibi — çiftçiye özel kalmalı, ya da hiç erişemez). Önerilen politika: `EXISTS (SELECT 1 FROM offer_items oi JOIN offers o ON o.id=oi.offer_id JOIN orders ord ON ord.offer_id=o.id WHERE oi.listing_id = listings.id AND ord.buyer_id = auth.uid())`.
2. **UI'da "satılmış/süresi geçmiş batch" göstergesi:** Vitrin/Keşfet'te artık görünmesin (status filtrelenir) ama buyer'ın **kendi sipariş geçmişinde** o batch'in detay sayfası hâlâ açılabilsin — bu, listing status'unü kontrol eden filtreyi sadece "keşfet" query'sine uygulayıp "sipariş detayı" query'sine uygulamamak demek (kod seviyesinde ayrı iki sorgu yolu, tek bir "status=active" WHERE'ü her yerde kopyalamamak).

---

## BÖLÜM B — Eatr & ReciMe Araştırması (senin 5. cevabın)

İki uygulamayı da inceledim, ikisi de "Recipe" tarafında ama farklı iki model:

### Eatr (AI Healthy Diet Meal Plan)
- Pişirme süresine göre filtre (15 dk / 1 saat altı)
- Beceri seviyesine göre kişiselleştirme (yeni başlayan → uzman)
- Diyet tercihi filtreleri (keto/vegan/pesketaryen)
- Detaylı besin değeri bilgisi her tarifte
- Adım adım pişirme rehberi
- **Monetizasyon: haftalık/yıllık subscription (₺600-3.500 aralığı), 7 gün ücretsiz deneme** — App Store yorumlarında **iptal/refund şikayetleri belirgin** (kullanıcılar "7 gün ücretsiz" deyip yıllık ücret kesildiğini fark etmemiş). **Ders için not:** Hasat'ın premium/Recipe App aboneliği bu hataları tekrarlamamalı — iptal akışı ve deneme süresi bildirimleri (P20'deki SMS altyapısı burada da kullanılabilir: "deneme süren bugün bitiyor" hatırlatması).

### ReciMe (Recipe Manager)
- **Sosyal medyadan (Instagram/TikTok/YouTube/Pinterest) tarif kaydetme** — Hasat için düşük öncelik, farklı bir problem çözüyor.
- **"Order Groceries – Turn your grocery list into a delivery! Order directly in-app"** — **bu, tam olarak senin 2.2'deki cross-sell önerinle örtüşen özellik**, gerçek bir pazarda kanıtlanmış bir pattern. ReciMe muhtemelen 3. parti bir grocery delivery API'sine bağlanıyor; Hasat'ta bu zaten **kendi envanterimiz** olduğu için 3. parti entegrasyonuna gerek yok, doğrudan Vitrin/Keşfet'e bağlanabilir — bu bizim için ReciMe'den daha güçlü bir konumdayız.
- Malzeme bazlı ölçü dönüşümü (porsiyon başına ingredient scale) — Hasat'ta batch miktar seçimiyle (A.3) doğal olarak örtüşür.
- Cookbook'lara organize etme, cloud sync, çoklu cihaz.

**Sonuç — Recipe App feature hedefi için önerim:** ReciMe'nin "grocery list → sipariş" akışını Eatr'ın "diyet/süre filtreleri + besin değeri" içeriğiyle birleştirmek, ama **sosyal medya import özelliğini** (ReciMe'nin ana değeri) ilk sürümde atlamak — bu Hasat'ın çekirdek değeriyle ilgisiz, büyük bir mühendislik yükü, MVP'ye gerek yok.

---

## BÖLÜM C — Buyer Discovery Akışı (6. cevabının detaylı tasarımı)

**Senaryo:** Ahmet çiftçinin safran'ında 2 aktif batch var (Batch A: 500g @ ₺X, Batch B: 500g @ ₺Y — farklı hasat tarihleri, muhtemelen farklı fiyat).

1. Buyer Keşfet'te "Safran — Ahmet Yılmaz" kartına tıklar (crop+farmer bazında gruplu kart, batch sayısı toplanmış gösterilir: "1kg mevcut, 2 parti").
2. Ürün detay sayfası açılır — her batch kendi kartında: Batch A (500g, ₺X/g, hasat tarihi, kalite), Batch B (500g, ₺Y/g, hasat tarihi, kalite).
3. Buyer her batch için ayrı bir miktar input'u görür (max=o batch'in stoğu). İkisinden de seçebilir.
4. Sayfanın altında **canlı toplam**: `Σ(seçilen_miktar × batch_fiyatı)` — batch'ler farklı fiyatlıysa toplam buna göre değişir (senin "toplam fiyat değişsin" notu).
5. "Teklif Gönder" → `offers` + `offer_items` (A.3'teki şema) oluşur, tek offer, N batch satırı.
6. Offer/Order detay sayfasında (buyer ve farmer tarafında) her batch satırına tıklanınca o batch'in `care_journal_entries`+`harvest_entries` calendar/list view'ı (Çiftçi 5.4/Buyer 5.3) açılır — **her batch'in journal geçmişi kendine özeldir, batch'ler arasında paylaşılmaz.**

---

## Onaylanan Kontrol Noktaları (2026-07-23)

1. ✅ A.2/A.3'teki tablo/kolon adları onaylandı.
2. ✅ P21/P22/P23 kod adlandırması ve sıralaması onaylandı.
3. ✅ P21-A için gerçek kod araştırması yapıldı (bkz. dosya sonu) — migration planı iptal, form/UX çözümüne revize edildi.
4. ✅ Vitrin'de ikinci aktif listing açılırken artık **engelleme + "batch olarak bağla" yönlendirmesi** gösterilecek (yeni: P21-A revize).
5. ✅ Vitrin'deki çift stok gösterimi (ham+hesaplanmış) tek sayıya indirilecek (yeni: P21-G).

----------------------------

## 🟤 Onaylanan Yol Haritası — P21/P22/P23 *(onaylandı 2026-07-23, P21 gerçek kod araştırmasıyla revize edildi — aynı gün)*

> Yukarıdaki "New PLAN FROM BERKİN NOTES" bölümü taslak/tartışma kaydı olarak korunuyor. Bu bölüm, Berkin'in 2026-07-23'te verdiği onay, 7 karar ve ardından yapılan gerçek kod/DB araştırması sonrası **kalıcı, resmi roadmap girişidir.**

### Temel bulgu — Batch sıfırdan değil, hatta kısmen zaten "canlı"

`listings` (Vitrin kartı) + `listing_harvest_entries` (çoktan-çoğa junction, 410 satır) + `harvest_entries` (hasat/bakım kaydı, 406 satır) üçlüsü **Batch kavramının temelini zaten atmış durumda.** Gerçek kod araştırmasıyla üç katman daha netleşti:

1. **Otomatik draft-batch mekanizması zaten çalışıyor** (`create_draft_listings_for_parcel` fonksiyonu, `parcels_after_insert`/`_after_update` trigger'ları) — parsel/crop eklendiğinde otomatik **tek** draft listing açılıyor (quantity=0, fiyat `price_history` ortalamasından seed'leniyor). Aynı parcel+crop için ikincisini açmıyor.
2. **Harvest entry → listing otomatik bağlama zaten var** (`tg_harvest_entries_after_insert_autolink`) — yeni hasat kaydı, aynı farmer+parcel+crop'a sahip **her** draft/active listing'e otomatik bağlanıyor. Birden fazla eşleşen listing varsa hepsine bağlanıyor — belirsizlik kaynağı.
3. **Stok kontrolü zaten var, sayaç değil dinamik hesaplama** (`enforce_offer_stock` SQL fonksiyonu + `useListingStock` frontend hook, ikisi paralel/ayrı yazılmış aynı formül): `available = SUM(bağlı harvest_entries.quantity) − SUM(kabul edilmiş offer'ların miktarı)`, `listings.quantity`'ye sadece fallback olarak düşülüyor.
4. **Manuel "Yeni Ürün" formu her zaman `status:'active'` gönderiyor**, DB default'unu hiç kullanmıyor; **duplicate/ikinci listing engeli yok** — bu yüzden aynı çiftçinin aynı crop'ta birden fazla (bazen amaçsızca) listing'i bugün bile oluşabiliyor.
5. **Buyer Keşfet zaten dedup/gruplama yapmadan tüm active listing'leri ayrı kart olarak gösteriyor** — çoklu-batch görünümü "yeni bir kapasite" değil, var olan dağınık bir davranışın düzenlenmesi.
6. **Vitrin kartında ham `listings.quantity` ile hesaplanmış `available` yan yana, tutarsız şekilde gösteriliyor.**

P21 serisi bu altı bulgunun üstüne **düzenleme, tutarlılık ve UX netliği** ekliyor — çoğu yerde sıfırdan yeni bir mekanizma inşa etmiyor.

### P21 — Batch & Vitrin Çoklu-Batch Mimarisi (P0, soft launch öncesi)

| Kod | Konu | Kapsam | Bağımlılık | Durum |
|---|---|---|---|---|
| P21-A | Batch açma akışı — engelle + bağlamaya yönlendir | Çiftçi aynı parcel+crop için "Yeni Ürün" formunu doldurduğunda, mevcut listing(ler) tespit edilir → **ikinci bir aktif listing açmak engellenir**, yerine "Bu crop için zaten N batch var — yeni hasadını mevcut bir batch'e bağlamak mı istersin, yoksa yeni bir batch mi açmak istersin?" seçimi sunulur. "Yeni batch" seçilirse `status='draft'` ile açılır, çiftçi kendi "✓ Yayınla" akışıyla (zaten var) canlıya alır | Yok — **ilk iş** | ⬜ Planlandı |
| P21-B | Çoklu-batch Keşfet/ürün detay sayfası | Aynı çiftçinin aynı crop'taki tüm aktif batch'leri (listing) Keşfet'te **crop+farmer bazında gruplu tek kart** olarak gösterilir (var olan dağınık kart listesi düzenleniyor, `useActiveListings` sonrası client-side gruplama). Ürün detay sayfası: her batch kendi satırında (`useListingStock` ile hesaplanan available, fiyat, hasat tarihi, kalite), miktar input'u batch başına, **canlı toplam** (Σ miktar×fiyat). Her batch satırına tıklanınca **o batch'e özel** journal log'ları (care_journal_entries + harvest_entries) ayrı ayrı açılır — batch'ler arasında journal geçmişi paylaşılmaz | P21-A | ⬜ Planlandı |
| P21-C | Multi-batch tek offer şeması | Yeni tablo `offer_items` (offer_id, listing_id, quantity, price_per_unit) — Berkin kararı: şimdilik tek `offers` satırı, tıklayınca batch'ler ayrı ayrı görünür. `offers.listing_id/quantity/price_per_unit` geriye dönük uyumluluk için kalır (birincil/toplam değer) | P21-B | ⬜ Planlandı |
| P21-D | Stok kontrolünü `offer_items`'a uyarlama + birim/fallback tutarlılığı | Sıfırdan trigger **yazılmayacak** — mevcut `enforce_offer_stock` fonksiyonu `offer_items` satır bazlı çalışacak şekilde güncellenecek (her `offer_item.listing_id` için ayrı kontrol). Aynı turda: (a) `harvest_entries.quantity` birim normalizasyonu eklenecek (kg/g karışıklığı riski), (b) fallback moduna (bağlı harvest_entries toplamı 0 → `listings.quantity`'ye düşme) görünür/loglanan bir işaret eklenecek — sessiz geçiş olmayacak, (c) SQL ve frontend (`useListingStock`) aynı hesaplamayı **tek bir paylaşılan RPC**'den çekecek şekilde birleştirilecek | P21-C | ⬜ Planlandı |
| P21-E | Traceability RLS garantisi | Berkin kararı: satılmış/expired batch, buyer'ın geçmiş siparişinden hâlâ görülebilsin. RLS politikası: buyer'ın `listings`/`harvest_entries`/`care_journal_entries` okuma erişimi "bu listing'den geçmişte siparişi var mı" koşuluna bağlanır (`offer_items`→`offers`→`orders` zinciri üzerinden). Keşfet query'si status filtreler, sipariş-geçmişi query'si filtrelemez — iki ayrı sorgu yolu | P21-C | ⬜ Planlandı |
| P21-G | Vitrin stok gösterim tutarlılığı | `farmer.storefront.tsx`'teki çift gösterim (ham `listings.quantity` + hesaplanmış `available` yan yana) tek bir sayıya indirilecek — sadece `available` (P21-D'nin paylaşılan RPC'sinden) gösterilecek | P21-D | ⬜ Planlandı |

**Not (rekabet hukuku netliği):** Aynı çiftçinin kendi batch'leri arasında farklı fiyat göstermesi rekabet hukuku riski taşımıyor — riskli olan çiftçiler-arası fiyat şeffaflığı/kolüzyondu (bu yüzden `price_history` aggregated-only tutuluyor), tek çiftçinin kendi ürünü için fiyat farkı göstermesi bambaşka bir konu ve sorunsuz. Berkin'in 6. kararıyla (toplam fiyat batch'lere göre değişsin) bu netleşti.

### P22 — Care Journal (Rutin Bakım) (P1, saffron sezonuna kadar esnek)

Berkin kararı (1. cevap): `harvest_entries`'ten (hasat olayı) ayrı bir tablo — ikisi karışık modeller (biri toggle/günlük, biri olay-bazlı/stok-etkili). Tek "Journal" sayfasında iki sekme: **Rutin Bakım** (yeni, Bevel-tarzı toggle) + **Hasat Kayıtları** (mevcut `harvest_entries` formu).

| Kod | Konu | Kapsam | Bağımlılık | Durum |
|---|---|---|---|---|
| P22-A | Care Journal şeması | 4 yeni tablo: `journal_entry_types` (preset+custom, code/display_name ayrımı — `crop_config.crop`/`display_name` pattern'iyle tutarlı), `journal_themes` (crop×entry_type×frequency×threshold, preset/farmer-override), `crop_journal_glossary` (merkezi, AI-üretilen, tek seferlik tooltip metni, `generated_by` ile şeffaf), `care_journal_entries` (günlük toggle kaydı, `listing_id` FK ile hangi batch'e ait olduğu otomatik-ama-editable) | Yok | ⬜ Planlandı |
| P22-B | Customize Journal ekranı (Bevel referansı) | Preset+custom entry type CRUD, kategori sekmeleri (bakım/soğuk depo/lojistik/güvenlik), her entry type için frequency/threshold modalı, quick-access toggle | P22-A | ⬜ Planlandı |
| P22-C | Crop Glossary üretimi | Her crop için AI ile tek seferlik paragraf-tooltip üretimi (Berkin'in domates örneği referans), "AI tarafından oluşturuldu" şeffaflık notuyla | P22-A | ⬜ Planlandı |
| P22-D | Journal sayfası UI | İki sekme (Rutin Bakım / Hasat Kayıtları), list/calendar view toggle, Parcel/Crop/Entry-type filtreleri (P19 `PricesPageBody` pattern'i yeniden kullanılabilir), overdue kırmızı highlight (bildirim şimdilik yok — Berkin kararı) | P22-B, P22-C | ⬜ Planlandı |
| P22-E | Yeni crop type ekleme wizard'ı | `crop_config` + `crop_market_sources` (P19) + `journal_themes` + otomatik-draft-batch tetikleyicisini **tek akışta** birleştir — ayrı ayrı elle yapılırsa biri unutulur riski var | P22-A | ⬜ Planlandı |

### P23 — Buyer Mobile & Recipe App (P2, ayrı faz — soft launch'u bloklamıyor)

Berkin kararı (7. cevap): Recipe App şimdilik tüm `buyer_type` segmentlerine açık (bireysel+HoReCa ayrımı yok, ileride veri ile karar verilir).

**Referans araştırması (Berkin'in 2 App Store linki, 5. cevap):**
- **Eatr (AI Healthy Diet Meal Plan):** süre/beceri/diyet filtreleri, besin değeri bilgisi, adım-adım pişirme. Abonelik modeli (7 gün deneme + otomatik yıllık ücret) App Store'da ciddi iptal/refund şikayetleri almış — Hasat'ın Recipe App aboneliği bu hatayı tekrarlamamalı, deneme süresi bitişi öncesi SMS hatırlatması (mevcut P20 altyapısı) planlanmalı.
- **ReciMe (Recipe Manager):** "Order Groceries — grocery list'i doğrudan app içinde siparişe çevir" özelliği Berkin'in 2.2 notundaki cross-sell fikriyle birebir örtüşüyor. Hasat kendi envanterine sahip olduğu için (ReciMe'nin 3. parti API'sine ihtiyacı olduğu yerde) doğrudan Vitrin/Keşfet'e bağlanabilir — ReciMe'den yapısal olarak daha avantajlı.
- Sosyal medyadan tarif import (ReciMe'nin ana özelliği) Hasat'ın çekirdek değeriyle ilgisiz, MVP'ye dahil edilmeyecek.

| Kod | Konu | Kapsam | Bağımlılık | Durum |
|---|---|---|---|---|
| P23-A | Buyer mobile + Recipe App keşif/tasarım | React Native, shopping+recipe birlikte mobile-first; Eatr'ın filtre/besin-değeri + ReciMe'nin grocery-to-order akışının Hasat'a uyarlanması | P21 serisi tamamlanmalı (batch verisi stabil olmalı) | ⬜ Planlandı |
| P23-B | Recipe↔Crop eşleştirme + RFQ otomasyonu | Bir tarifin malzemesi Hasat kataloğunda yoksa otomatik `crop_requests` (P17-E) kaydı önerisi | P23-A | ⬜ Planlandı |
| P23-C | Mobile compliance | App Store/Play Store in-app purchase kuralları (₺149 premium riski), KVKK mobile metinleri, OTP/biometric login kararı | P23-A | ⬜ Planlandı |

### Berkin'in 7 kararı (referans, 2026-07-23)
1. Care journal ayrı tablo, tek sayfa iki sekme ile en anlaşılır gösterim (→ P22-A/D)
2. Draft migration'ı Claude kontrol etsin, doğru önceliklendirme ile plana girsin (→ P21-A, gerçek koda göre revize edildi)
3. Multi-batch şimdilik tek offer, tıklayınca batch'ler+journal ayrı görünür (→ P21-C, `offer_items`)
4. Sold/expired batch geçmiş siparişten hâlâ görülebilir (→ P21-E, RLS)
5. Referans app'ler: Eatr (id6479693198) + ReciMe (id1593779280) (→ P23-A araştırma girdisi)
6. Aynı çiftçinin aynı crop'ta çoklu batch'i Keşfet'te ikisi de görünür, buyer stok görüp seçer, toplam fiyat değişir; her batch'in journal log'u kendine özel (→ P21-B)
7. Recipe App şimdilik tüm buyer_type'lara açık (→ P23-A kapsam)
8. **[2026-07-23, ek karar]** Vitrin'de ikinci aktif listing açılırken engellenir, çiftçiye mevcut batch'e bağlama seçeneği sunulur (→ P21-A revize)

### Sıradaki adım
P21-A için Lovable'a `plan_mode=true` ile gönderilecek prompt hazırlanacak: "Yeni Ürün" formuna duplicate-crop kontrolü + "mevcut batch'e bağla / yeni batch aç" seçimi eklenmesi — Berkin onayladığında başlatılacak.
