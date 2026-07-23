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

# Hasat — Çiftçi & Buyer Update Kapsamı Konsolide Tablosu

*Kaynak: 22.07.2026 tarihli el notları (GİFTGİ 1-5, BUYER, HASAT tarihli sayfa). Bu tablo finalize etme ve önceliklendirme için çalışma dokümanıdır.*

## ÇİFTÇİ TARAFI

| # | Segment | Madde | Alt Madde | Mevcut Durum / Ne Anlama Geliyor | Eklenebilir Not (Claude önerisi) | Önerilen Öncelik | İlişkili P-Serisi / Not | Durum |
|---|---------|-------|-----------|-----------------------------------|-----------------------------------|-------------------|--------------------------|-------|
| 1.1 | Journal Enhancement | Her crop type için özel default journal draft | — | Şu an journal_entries düz kayıt, crop-type'a özel default şablon yok | Draft'ın kaynağı: Hasat editörü mü sağlayacak, çiftçi mi özelleştirecek? | P1 | Yeni — mevcut journal şemasının üstüne | Planlanmadı |
| 1.2 | Journal Enhancement | Crop Type Hasat Glossary (dikey template) | 1.2.1 Parsel >1 ise Tabs ile ayrım | Parselde birden fazla crop varsa glossary görünümü ayrışmalı | Glossary içeriği versiyon kontrolü gerekebilir | P1 | Yeni | Planlanmadı |
| 1.3 | Journal Enhancement | Her crop/journal type için glossary çalışma mantığı | — | Henüz tanımsız — sadece "nasıl olacak" sorusu | Glossary'nin çiftçi tarafından mı yoksa merkezi mi yönetileceği netleşmeli | P2 | Yeni | Netleştirme gerekiyor |
| 1.4 | Journal Enhancement | Yeni crop type ekleme akışı | Örnek: Ayas Domates | Crop type ekleme UI'ı zaten var mı kontrol edilmeli | Yeni crop type eklerken market_sources/crop_market_sources ile de senkron olmalı (P19 borsa şemasıyla çakışabilir) | P1 | P19 ile kesişim | Kontrol gerekiyor |
| 1.5 | Journal Enhancement | Farklı crop type'lar için default Journal Theme add/edit | — | Journal Theme kavramı hiç yok, yeni | — | P1 | Yeni | Planlanmadı |
| 1.6 | Journal Enhancement | Her crop type için Calendar view | — | Şu anki journal UI'da calendar/list toggle yok | 3. segmentteki calendar view ile aynı bileşen olabilir, ayrı geliştirilmemeli | P2 | 3. segmentle birleştirilebilir | Planlanmadı |
| 2.1 | Journal Entry Types & Themes | Standard presets + custom entry type seçenekleri | — | Şu an tek tip journal entry var | — | P1 | Yeni | Planlanmadı |
| 2.2 | Journal Entry Types & Themes | Cold storage/paketleme & lojistik/güvenlik entry type'ları | — | Yeni kategori seti | Bu kategoriler P17-D (invoice/e-müstahsil) ile veri bağlantısı kurabilir (lojistik kaydı → fatura) | P2 | P17-D ile bağlantılı olabilir | Planlanmadı |
| 2.3 | Journal Entry Types & Themes | Theme attribute'ları: entry type, frequency, duration (opsiyonel) | Preset + custom | Yapısal şema değişikliği gerektirir | — | P1 | Şema migration'ı gerekir | Planlanmadı |
| 2.4 | Journal Entry Types & Themes | Akış: Entry Types → Preset/Custom Themes (crop bazlı) → kullanıcı parsel-crop themes (dikey) → multi-parsel tabs | — | Tüm zincir yeni | UI akışı P17-A/B gibi çok adımlı flow'lara benziyor, aynı wizard pattern kullanılabilir | P1 | Yeni mimari | Planlanmadı |
| 2.5 | Journal Entry Types & Themes | Yeni parsel/crop type eklenince default Theme otomatik oluşsun | — | Otomasyon gerektirir (trigger veya edge function) | P19'daki sync-izmir-hal-prices gibi bir trigger/cron pattern uygulanabilir | P2 | Teknik pattern: mevcut trigger'lara benzer | Planlanmadı |
| 3.1 | Journal Sayfası UI | Default görünüm: theme'e göre upcoming task list view | — | Şu anki journal sayfası bu görünümde değil | — | P1 | Yeni UI | Planlanmadı |
| 3.2 | Journal Sayfası UI | Crop/parsel için dikey bileşen, chip'li attribute gösterimi, ayrık/birleşik seçenek | — | Yeni component | — | P2 | Yeni UI | Planlanmadı |
| 3.3 | Journal Sayfası UI | Filtreler: Parcel (All), Crop Type (All), Journal entry type | — | Filtre mantığı P19'daki fiyat sayfası filtrelerine benzer şekilde tasarlanabilir | Aynı filtre component'i P19'daki PricesPageBody'den yeniden kullanılabilir mi kontrol edilmeli | P1 | Kod tekrar kullanımı fırsatı | Planlanmadı |
| 3.4 | Journal Sayfası UI | time ≥ today AND overdue highlight (dropdown) | — | Yeni mantık | Overdue highlight'a ek olarak bildirim/reminder mekanizması hiç bahsedilmemiş | P2 | **Eksik: notification** | Netleştirme gerekiyor |
| 3.5 | Journal Sayfası UI | Calendar view: aylık/haftalık/günlük, filtreler korunur, sadece date filter değişir | — | P19'daki PriceChart'ın zaman aralığı sekmeleri (3 Ay/6 Ay/1 Yıl) mantığına benzer ama farklı granülarite | Aynı takım (Son 3 Ay/6 Ay/1 Yıl) mantığı burada da uygulanabilir mi, yoksa native calendar mı gerekiyor netleşmeli | P1 | P19 pattern referansı | Planlanmadı |
| 4.1 | Batch Logic (temel) | Her HASAT = Batch (journal type) | — | Batch kavramı şemada tamamen yok | — | P0 | Yeni — kök kavram | Planlanmadı |
| 4.2 | Batch Logic (temel) | Her Batch: ID + isim input + mapping | — | Yeni tablo gerektirir (`batches`?) | — | P0 | Şema tasarımı gerekiyor | Planlanmadı |
| 4.3 | Batch Logic (temel) | Her crop type'ın farklı batch'leri olabilir | — | 1-N ilişki: crop_type → batches | — | P0 | Şema tasarımı gerekiyor | Planlanmadı |
| 4.4 | Batch Logic (temel) | Yeni parsel=crop type eklenince Batch otomatik oluşsun | — | 2.5 ile aynı otomasyon paterni | Journal Theme otomasyonuyla aynı trigger'da birleştirilebilir | P1 | 2.5 ile birleştirilebilir | Planlanmadı |
| 4.5 | Batch Logic (temel) | Journal & Batch, Parcel'e bağlı, DB'de senkron edit | — | Foreign key ilişkileri netleşmeli | — | P0 | Şema tasarımı | Planlanmadı |
| 4.6 | Batch Logic (temel) | Her zaman önce Draft mode | — | Yayınlama öncesi taslak zorunluluğu | Draft→Publish state machine, mevcut orders.status enum pattern'i gibi tasarlanabilir (P17-B'de orders.status genişletildi, benzer yaklaşım) | P1 | P17-B pattern referansı | Planlanmadı |
| 5.1 | Batch / Stok & Vitrin | Yeni Hasat journal entry eklenince stok artsın | — | Stok hareketi journal'a bağlı otomasyon | Stok azalma tarafı (satış/sipariş sonrası) da simetrik olarak tanımlanmalı, sadece artış yazılmış | **Eksik: stok azalma tetikleyicisi** | P0 | Netleştirme gerekiyor |
| 5.2 | Batch / Stok & Vitrin | Batch view = Vitrin | — | Vitrin'in veri kaynağı batch bazlı olacak şekilde değişir | — | P0 | Mevcut Vitrin'in üstüne yeniden mimari | Planlanmadı |
| 5.3 | Batch / Stok & Vitrin | Crop type'ın birden fazla batch'i varsa isim+ID ile liste | — | Vitrin kart yapısı değişir (tekli üründen batch listesine) | Rekabet hukuku mimarisiyle (aggregated-only price history, min-5-farmer threshold) çakışmamalı — batch bazlı görünürlük bireysel fiyat feed'ine dönüşmemeli | P0 | **Competition law riski — dikkat** | Hukuki kontrol gerekiyor |
| 5.4 | Batch / Stok & Vitrin | Batch'e tıklayınca aynı filtrelerle Journal Logs calendar/list view | — | 3. segmentteki UI'nın batch-detay versiyonu | Batch bazlı traceability gösterimi, Hasat'ın "trust infrastructure" tezine hizmet eder, P17-C (review) ile bağlanabilir | P1 | 3. segment + P17-C bağlantısı | Planlanmadı |

## BUYER TARAFI

| # | Segment | Madde | Alt Madde | Mevcut Durum / Ne Anlama Geliyor | Eklenebilir Not (Claude önerisi) | Önerilen Öncelik | İlişkili P-Serisi / Not | Durum |
|---|---------|-------|-----------|-----------------------------------|-----------------------------------|-------------------|--------------------------|-------|
| 1.1 | Kapsam Keşfi | Buyer için tüm use case'ler (actions, I/O, tablolar, servisler, fonksiyonaliteler) | — | Henüz tanımsız, keşif aşaması | Mevcut buyer flow'unun (browse/order/subscription/review) hangi kısmının aynı kalıp hangisinin yeni olacağı önce ayrılmalı, scope creep riski var | P0 | Önce netleştirme gerekiyor | Netleştirme gerekiyor |
| 1.2 | Kapsam Keşfi | Recipe App consumers için unique value | — | Tanımsız, iş fikri seviyesinde | — | P2 | Ayrı ürün keşfi | Netleştirme gerekiyor |
| 2.1 | Mobile/Recipe App Mimarisi | Aynı DB, enhanced servisler mobile'a adapte → React Native (shopping side) | — | Backend aynı, frontend yeni platform | — | P1 (soft launch sonrası olabilir) | Mobile roadmap | Planlanmadı |
| 2.2 | Mobile/Recipe App Mimarisi | Aynı DB + yeni tablolar/servisler/auth/AI/dokümantasyon (Recipe App için) | — | Recipe App tamamen ayrı bir ürün hattı gibi görünüyor | Recipe App'in gelir modeli finansal modelde (v0.6) yok — ayrı gelir kalemi mi, yoksa retention aracı mı belirlenmeli; crop→recipe eşleştirme için yeni bir curation/veri seti gerekir | P2 | **Finansal modelle bağlantısı yok — eksik** | Netleştirme gerekiyor |
| 3.1 | UI/UX | Yeni UI/UX sayfaları | — | Mobile-first tasarım gerekiyor | Mobile tasarım dili mevcut Lovable web tasarım sistemiyle tutarlı mı olacak, erken netleşmeli | P2 | frontend-design skill referansı olabilir | Netleştirme gerekiyor |
| 3.2 | UI/UX | Yeni theme referansları seçilmeli | — | Görsel kimlik kararı | — | P2 | — | Planlanmadı |
| 4.1 | Prod. Marketplace Netleştirmesi | Aynı DB, mobile'a taşınacak | — | Mimari netleştirme | — | P1 | — | Planlanmadı |
| 4.2 | Prod. Marketplace Netleştirmesi | Yeni özellikler eklenecek | — | Kapsam TBD | — | P2 | — | Netleştirme gerekiyor |
| 4.3 | Prod. Marketplace Netleştirmesi | iOS & Android gereksinimleri kontrol edilecek | — | Platform-spesifik kısıtlar henüz araştırılmadı | App Store/Play Store compliance: in-app purchase kuralları premium/subscription (₺149/ay) için geçerli olabilir; KVKK/gizlilik metinleri mobile için ayrı gerekebilir; OTP mı kalacak yoksa biometric/social login eklenecek mi | P1 | **₺149 premium ile in-app purchase kuralı çakışması riski** | Hukuki/teknik araştırma gerekiyor |
| 5.1 | Batch Görünümü & Talep Akışı | Crop type'ın birden fazla batch'i varsa Vitrin'de isim+ID liste | — | Çiftçi tarafı 5.3 ile aynı veri, buyer görünümü | — | P0 | Çiftçi 5.3 ile aynı geliştirme | Planlanmadı |
| 5.2 | Batch Görünümü & Talep Akışı | Örnek senaryo: 12 birim istek, Batch1=10, Batch2=5 | 5.2.1 Batch1 yetersizse Batch2'den ekleme seçeneği; 5.2.2 Hiçbiri yetmezse "request more" butonu | Multi-batch sepete ekleme mantığı, sipariş modelinde değişiklik gerektirir | "Request more" butonu P17-E (Structured RFQ) ile aynı mekanizma mı olacak yoksa ayrı basit form mu — ikisi çakışmamalı, tek akışa indirilmeli | P0 | **P17-E ile çakışma riski — netleştirilmeli** | Netleştirme gerekiyor |
| 5.3 | Batch Görünümü & Talep Akışı | Batch'e tıklayınca journal loglarının calendar/list view'ı | — | Çiftçi 5.4 ile aynı UI, buyer'a salt-okunur gösterim | Buyer'a batch bazlı traceability gösterimi, sipariş sonrası "hangi hasattan geldi" bilgisini review/dispute akışına (P17-B/C) bağlayabilir | P1 | P17-B/C bağlantısı | Planlanmadı |

## ÇAPRAZ / ROL-BAĞIMSIZ (22.07.2026 — Soft Launch Checklist)

| # | Madde | Mevcut Durum | Eklenebilir Not | Öncelik | Durum |
|---|-------|--------------|-------------------|---------|-------|
| C1 | Soft launch'a kadar ne eksik? (feature-wise) | Sadece soru, henüz liste yok | Yukarıdaki tüm çiftçi/buyer maddeleri bu listeye girdi adayı — hangileri Ağustos'tan önce, hangileri sonra netleşmeli | P0 | Bu tablo bu sorunun cevabına temel oluşturuyor |
| C2 | Soft launch'a kadar maliyet ne olacak? | Tanımsız | Finansal modelde (v0.6) bu kapsam genişlemesi (Batch, Recipe App, mobile) henüz yok — v0.7 revizyonu gerekebilir | P0 | Netleştirme gerekiyor |
| C3 | Soft launch sırasında capital ihtiyacı | Tanımsız | C2 ile bağlı | P0 | Netleştirme gerekiyor |
| C4 | Marketing & Brand eksikleri | Tanımsız | Recipe App/mobile gibi yeni segmentler marka mesajını etkiler, önce kapsam netleşmeli | P1 | Netleştirme gerekiyor |
| C5 | Legal wise & Accounting eksikleri | Tanımsız | Batch/Vitrin'in competition law riski (5.3) ve mobile in-app purchase kuralları (Buyer 4.3) bu listeye doğrudan giriyor | P0 | Netleştirme gerekiyor |

---

## Önceliklendirme Özeti (P0 = soft launch kritik, P1 = önemli ama esnek, P2 = sonraya bırakılabilir)

- **P0 (Ağustos soft launch öncesi karar gerekli):** Batch temel mimarisi (4.1-4.6), Stok/Vitrin entegrasyonu (5.1-5.4), Buyer batch görünümü ve talep akışı (5.1-5.3), Competition law kontrolü, Soft launch checklist'in tamamı
- **P1 (Yapılabilir ama saffron sezonuna kadar erteneilebilir):** Journal Enhancement (1.x), Journal Entry Types/Themes (2.x), Journal UI (3.x), Mobile marketplace netleştirmesi
- **P2 (Ayrı faz — muhtemelen Ekim-Kasım sonrası):** Recipe App / mobile genişleme (Buyer 1.2, 2.2, 3.x), yeni UI/tema seçimi

**Kritik açık sorular (karar bekliyor):**
1. Batch bazlı Vitrin görünümü competition law mimarisiyle nasıl uyumlu olacak? (Çiftçi 5.3)
2. "Request more" butonu P17-E (RFQ) ile aynı mı, ayrı mı? (Buyer 5.2.2)
3. Recipe App/mobile genişleme Ağustos soft launch kapsamında mı, yoksa ayrı faz mı? (Cross C1)
4. Stok azalma (satış sonrası) tetikleyicisi hiç tanımlanmamış — sadece artış var (Çiftçi 5.1)
