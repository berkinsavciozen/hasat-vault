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
