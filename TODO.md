---
title: Hasat — Master Roadmap & Build Log
updated: 2026-07-21
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

> **P19 + P17 serisi (B/C/F/E/G) + P20 tamamen bitti, hepsi canlı doğrulandı.** **P17-A ve P17-D şirket kuruluşuna bağlı, bloke.** BENCHMARK Gap listesindeki bağımsız yapılabilecek her şey bitti (bkz. Gap durum tablosu altta). Sıradaki büyük karar: yeni bir P-serisi mi (P2 gap'ler: parselden-tabağa QR, vade/cari, hasat öncesi finansman — hepsi uzun vadeli/partner gerektiren), yoksa doğrudan Ağustos soft-launch hazırlığına mı geçilecek.

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

---

## 📌 Kararlar

(önceki tablo + eklenenler:)
| **P17-G tamamen tamamlandı (2026-07-21)** | 20 KPI view'ı + admin dashboard, canlı doğrulandı. Detaylar önceki sürümde. |
| **P20 tamamlandı (2026-07-21)** | SMS bildirim genişletmesi (BENCHMARK Gap #8+#10) — gerçek, önceden var olan ama kırık bir bug (`offer_accepted`/`payment_confirmed` SMS'i hiç çalışmıyordu) bulunup düzeltildi; kargo/teslim/iptal/ihtilaf/RFQ-eşleşme event'leri SMS'e bağlandı; iki senaryo gerçek Twilio SMS'i ile uçtan uca doğrulandı. **BENCHMARK Gap listesindeki bağımsız yapılabilecek her şey artık bitti** — kalanlar (P17-A/D bloke, #9/#11/#12 uzun vadeli/P2) launch'u bloklamıyor. |

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

*Bu plan senin 7 cevabına göre oluşturuldu. TODO.md'ye henüz işlenmedi — onayını bekliyor. Onayladıktan sonra P-serisi olarak (P21, P22...) TODO.md'ye geçireceğiz.*

---

## Cevaplarının özeti ve aldığım kararlar

| # | Senin cevabın | Aldığım mimari karar |
|---|---|---|
| 1 | Care journal ayrı, en kolay/anlaşılır neyse öyle | Ayrı tablo (`care_journal_entries`) + tek "Journal" sayfasında iki sekme |
| 2 | Draft migration'ı bana bırak, önceliklendir | Kontrol ettim, gerçek boşluk doğrulandı → **P0, ilk iş** |
| 3 | Şimdilik tek offer, tıklayınca batch'ler ayrı görünsün | `offer_items` ara tablosu (yeni) |
| 4 | Sold/expired batch geçmiş siparişten hâlâ görülebilsin | RLS + query katmanında garanti edilecek, madde 4'te detay |
| 5 | Eatr + ReciMe | Araştırdım, aşağıda özellik karşılaştırması var |
| 6 | Aynı çiftçinin aynı crop'ta birden fazla batch'i → Keşfet'te ikisi de görünsün, buyer stok görüp seçsin, toplam fiyat değişsin | Ürün detay sayfası = batch seçici, `offer_items` ile fiyat dinamik hesaplanır |
| 7 | Recipe App şimdilik herkese açık | `buyer_type` segmentasyonu launch'ta yok, ileride veri ile karar verilir |

---

## BÖLÜM A — Şema Değişiklikleri (net karar verildi, implementasyona hazır)

### A.1 — `listings.status` default düzeltmesi (P0, İLK İŞ)
```sql
ALTER TABLE listings ALTER COLUMN status SET DEFAULT 'draft';
```
**Önce yapılması gereken kontrol (Lovable'a sorulacak):** Mevcut "yeni listing oluştur" formunun/akışının `status` alanını explicit olarak `'active'` set edip etmediği kontrol edilmeli. Eğer form zaten explicit `status: 'active'` gönderiyorsa, DB default değişikliği hiçbir şeyi bozmaz (sadece "hiç status göndermeyen" gelecekteki insert'ler için güvenlik ağı olur). Eğer form hiç status göndermiyorsa (DB default'una güveniyorsa), bu değişiklik **mevcut "listing oluştur → hemen görünür" davranışını kırar** — form'un kendisine de bir "Yayınla" adımı eklenmesi gerekir. **Bu yüzden migration'dan önce Lovable'a bu soruyu sormak implementasyonun 0. adımı.**

### A.2 — Care Journal (rutin bakım) — YENİ tablolar
Bevel modelini (toggle + frequency + threshold) Hasat'ın crop/parsel/batch yapısına uyarlayan 3 yeni tablo:

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
6. Offer/Order detay sayfasında (buyer ve farmer tarafında) her batch satırına tıklanınca o batch'in `care_journal_entries`+`harvest_entries` calendar/list view'ı (Çiftçi 5.4/Buyer 5.3) açılır.

---

## BÖLÜM D — Önerilen P-Serisi Sıralaması (TODO.md'ye işlenmeye hazır taslak)

| Kod | Konu | Kapsam | Öncelik | Bağımlılık |
|---|---|---|---|---|
| **P21-A** | Draft mode düzeltmesi | A.1 migration + Lovable'a "status nerede set ediliyor" sorusu + form güncellemesi | P0, ilk iş | Yok |
| **P21-B** | Batch/Vitrin çoklu-batch görünürlüğü | Keşfet'te crop+farmer gruplu kart, batch seçici ürün detay sayfası (Bölüm C) | P0 | P21-A |
| **P21-C** | Multi-batch offer şeması | `offer_items` tablosu + offer/order detay sayfasında batch breakdown UI | P0 | P21-B |
| **P21-D** | Stok azalma tetikleyicisi | `offer_items` kabul edildiğinde ilgili `listings.quantity` otomatik düşsün (trigger) | P0 | P21-C |
| **P21-E** | Traceability RLS garantisi | A.4'teki RLS politikası + "satılmış batch hâlâ görülebilir" testi | P0 | P21-C |
| **P22-A** | Care Journal şeması | A.2'deki 4 tablo (migration) | P1 | Yok (paralel gidebilir) |
| **P22-B** | Journal Entry Types yönetimi (Bevel-tarzı Customize Journal ekranı) | Preset+custom CRUD, kategori sekmeleri, frequency/threshold modal | P1 | P22-A |
| **P22-C** | Crop Glossary (AI-üretilen, merkezi) | Tek seferlik AI-generation + tooltip UI | P1 | P22-A |
| **P22-D** | Journal sayfası UI (2 sekme: Rutin Bakım + Hasat Kayıtları) | List/Calendar view, filtreler, overdue highlight | P1 | P22-B, P22-C |
| **P22-E** | Yeni crop type ekleme wizard'ı | crop_config + crop_market_sources + journal_themes + default batch tetikleyicisini tek akışta birleştir | P1 | P22-A |
| **P23-A** | Buyer mobile + Recipe App keşif/tasarım | Bölüm B'deki feature hedefi, UI tasarımı | P2 | P21 serisi tamamlanmalı (batch verisi stabil olmalı) |
| **P23-B** | Recipe↔Crop eşleştirme + crop_request otomasyonu | Recipe malzemesi Hasat'ta yoksa otomatik RFQ (P17-E) tetikleme | P2 | P23-A |
| **P23-C** | Mobile (React Native) + App Store/Play Store compliance | In-app purchase, KVKK, OTP/biometric | P2 | P23-A |

**Not:** P21 serisi (Batch) soft launch'tan önce (Ağustos) bitmesi gereken kısım. P22 (Care Journal) saffron sezonuna kadar (Ekim) esnek. P23 (Recipe/Mobile) ayrı bir faz, soft launch'u bloklamıyor.

---

## Onay Bekleyen Son Kontrol Noktaları

1. Yukarıdaki 4 yeni tablo (A.2) ve 1 yeni tablo (A.3) isimlerini/kolon adlarını onaylıyor musun, yoksa değiştirmek istediğin bir isimlendirme var mı?
2. P21/P22/P23 kod adlandırması ve sıralaması uygun mu, yoksa mevcut P20'den sonra farklı numaralandırma tercih eder misin?
3. Onaylarsan bir sonraki adım: **P21-A'yı** (draft mode) Lovable'a `plan_mode=true` ile gönderip gerçek frontend davranışını (status nerede set ediliyor) öğrenmek — bunu şimdi başlatabilirim.

Onayını verdiğinde bu planı TODO.md'nin "New PLAN FROM BERKİN NOTES" bölümünün altına, kalıcı P-serisi formatında işleyeceğim.
