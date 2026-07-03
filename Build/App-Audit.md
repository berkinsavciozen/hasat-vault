---
title: Hasat — Uygulama Audit & Bug Takibi
updated: 2026-07-01
tags:
  - hasat
  - audit
  - bugs
---

# Uygulama Audit & Bug Takibi
> Live: https://hasat.lovable.app · Son audit: 25 Haziran 2026

---

## Kritik Buglar

| ID | Bug | Kök Neden | Durum |
|---|---|---|---|
| B1 | Günlük: `[work:ilaçlama][health:3]` ham tag'ler görünüyor | notes renderer parse etmiyor | ⏳ Fix bekleniyor |
| B2 | JournalEntryCard kalite default'u C, A olmalı | parseJournalEntry fallback | ⏳ Fix bekleniyor |
| B3 | Mayıs 2025 orphan entry DB'de (tarih bug öncesi) | Tarih bug öncesi oluştu | 🟡 Manuel SQL ile sil |

**B1+B2 fix Lovable promptu:** `Build/Prompt-Queue.md`'de P16 öncesi sırada.

---

## Orta Seviye Buglar

| ID | Bug | Durum |
|---|---|---|
| B4 | "HASAT DÖNEMİ — KASIM" chip yanlış ay gösteriyor | ⏳ |
| B5 | `safran_soğanı` raw slug görünüyor (Safran Soğanı olmalı) | ⏳ |
| B6 | ISO sertifikası süresi geçmiş ama uyarı yok | ⏳ |
| B7 | Ana Sayfa coach mark AI Box'ı örtüyor (z-index) | ⏳ |

---

## Küçük Buglar

| ID | Bug | Durum |
|---|---|---|
| B8 | Topluluk timestamp format tutarsız (relative vs absolute) | ⏳ |
| B9 | AI Günlük "4 kayıt" / header "7 kayıt" tutarsızlığı | ⏳ |
| B10 | Settings browser tab title "Hasat" yerine "Ayarlar — Hasat" | ⏳ |

---

## GTM Öncesi Eksik Scope

| # | Özellik | Durum | Prompt |
|---|---|---|---|
| E1 | Ürün fotoğrafı upload | ❌ Yok | P16-A |
| E2 | Public vitrin URL | ❌ Yok | P16-B |
| E3 | Gerçek ödeme (iyzico/IBAN) | ❌ Simüle | P16-C |
| E4 | ToS + Gizlilik Politikası | ❌ Yok | P16-D |

---

## Hızlı Kazanımlar (Quick Wins)

| # | İyileştirme | Efor |
|---|---|---|
| Q1 | Slug display: `safran_soğanı` → `Safran Soğanı` | XS |
| Q2 | Journal note parser — tag'leri chip'e çevir | S |
| Q3 | JournalEntryCard quality default → 'A' | XS |
| Q4 | Expired sertifika badge → kırmızı "Süresi Geçti" | XS |
| Q5 | HASAT DÖNEMİ chip'ini gerçek ay ile bağla | S |

---

## Çalışan Özellikler (Audit Özeti — 25 Haziran 2026)

✅ Login (OTP + WhatsApp) · Farmer Dashboard + AI kutu · Günlük (kayıt, ay grubu, AI analiz) · Fiyatlar (5 ürün, grafik, AI nudge) · Vitrin (ürün listesi, AI kutu) · Teklifler/Siparişler (P15 sonrası) · Analitik AI kutu · Topluluk feed · Settings (parseller, sertifikalar, AI kullanım) · AI Chat FAB (WhatsApp deeplink, duplicate detection, tarih fix)

---

## Sonraki Audit

Soft launch öncesinde (Ağustos 2026) gerçek 2 telefonla uçtan uca test yapılacak:
- Farmer: OTP → onboarding → listing (fotoğraflı) → teklif al → kabul et → ödeme onayla
- Buyer: OTP → onboarding → discovery → teklif gönder → müzakere → ödeme

---

## GTM Scope Güncellemesi (Haziran 2026 — Phase 1 Revizyonu)

| # | Özellik | Durum | Prompt |
|---|---|---|---|
| E5 | Parsel fotoğrafı upload + vitrin'de göster | ❌ Yok | P16-A (güncellendi) |
| E6 | Gerçek zamanlı fiyat verisi (manuel kuratör) | ❌ Statik | P16-E |
| E7 | Topluluk mesajlaşma (yorum/reply) | ❌ Feed var, etkileşim yok | P16-F |
| E8 | Çiftçi referral programı UI | ❌ Yok | P16-G |

---

## E2E Test Planı (Claude Chrome ile)

> **Süreç:** Berkin test case listesini onaylar → Claude Chrome ile çalıştırır → eksikleri Lovable'a gönderir → tekrar test eder.

### Test Case Grupları (Onay Bekleniyor)

**Grup 1 — Auth & Onboarding**
- T01: Yeni farmer — OTP ile kayıt (WhatsApp) → onboarding tamamla → dashboard göster
- T02: Yeni buyer — OTP ile kayıt (SMS) → onboarding tamamla → discovery ekranı göster
- T03: Mevcut kullanıcı — tekrar giriş → doğru role yönlendir
- T04: Duplicate phone → uyarı ver, kayıt engelle

**Grup 2 — Farmer Akışı**
- T05: Parsel ekle → fotoğraf yükle → kaydet → listelendiğini doğrula
- T06: Sertifika ekle → süre kontrolü → süresi geçmiş badge doğrula
- T07: Ürün listesi oluştur → fotoğraf yükle → vitrin'de görünmesini doğrula
- T08: Public vitrin URL `/s/{slug}` → giriş yapmadan aç → listing + parsel fotoları görünüyor mu?
- T09: Fiyat sayfası → güncel fiyat görünüyor mu? (P16-E sonrası)
- T10: Topluluk post yaz → başka kullanıcı yorum yapsın → bildirim gel
- T11: Referral kodu kopyala + WhatsApp paylaş butonunu test et

**Grup 3 — Buyer Akışı**
- T12: Discovery → ürün filtrele → farmer profiline git
- T13: Teklif gönder → farmer kabul et → pending_payment durumuna geç
- T14: IBAN köprüsü → "Ödemeyi havale ettim" → farmer "Onayladım" → aktif sipariş
- T15: Buyer Aktif tab → sipariş özeti + farmer iletişim bilgisi görünüyor mu?

**Grup 4 — Teklif/Müzakere (P15)**
- T16: Farmer teklif yap → ball_side doğrulaması
- T17: Son gönderen geri çekme → önceki duruma dön
- T18: Ping-pong kural ihlali girişimi → server-side engel

**Grup 5 — AI & Bildirimler**
- T19: AI analiz kutusu — her sayfada yükleniyor mu? Hata var mı?
- T20: WhatsApp AI chat → mesaj gönder → yanıt al
- T21: In-app bildirim → teklif geldiğinde görünüyor mu?

**Grup 6 — Genel**
- T22: ToS + Privacy sayfaları → footer linkler çalışıyor mu?
- T23: Mobile responsive kontrol (375px viewport)
- T24: Console error taraması — tüm sayfalarda JS hatası var mı?

> Bu test case'leri onaylayınca Claude Chrome ile çalıştırıyor. Onaylamak için: "T01-T24 hepsini çalıştır" veya belirli grupları seç.

---

## E2E Test Sonuçları — 30 Haziran 2026 (Claude Chrome)

### Audit Kapsamı
Farmer flow (Ahmet Yılmaz, 5001234567) + Buyer flow (Zeynep Kaya, 5009876543) tam Chrome E2E testi.

### Test Sonuçları

| Test | Açıklama | Sonuç | Not |
|---|---|---|---|
| T01 | Farmer OTP login | ✅ PASS | /login?role=farmer, OTP 123456 çalıştı |
| T02 | Buyer OTP login | ✅ PASS | /login?role=buyer, WhatsApp seçili geldi |
| T03 | Role routing | ✅ PASS | Farmer → /farmer/home, Buyer → /buyer/discover |
| T04 | Çıkış Yap | ✅ PASS | Settings > Hesap > Çıkış Yap çalışıyor |
| T05 | Farmer dashboard | ✅ PASS | AI kutu, hava durumu, sipariş özeti |
| T06 | Günlük | ✅ PASS | Kayıt, ay grubu; B1 ham tag bug devam ediyor |
| T07 | Fiyatlar | ✅ PASS | Grafik var; B11 birim hatası var |
| T08 | Vitrin | ✅ PASS | Ürün listesi var; E1/E5 fotoğraf yok |
| T09 | Public vitrin `/s/{slug}` | ❌ FAIL | E2 — 404 (implement edilmemiş) |
| T10 | Topluluk | ⚠️ PARTIAL | Feed var; E7 reply/interaksiyon yok |
| T11 | Siparişler/Teklifler | ✅ PASS | P15 state machine çalışıyor |
| T12 | Analytics | ❌ FAIL | B13 — empty state despite veri var |
| T13 | Buyer discovery | ✅ PASS | Keşfet, filtreler, kategori kartları |
| T14 | Teklif Ver akışı | ✅ PASS | Form, miktar stepper, teslimat seçenekleri |
| T15 | Buyer siparişler | ✅ PASS | Tekliflerim/Aktif/Tamamlanan tabs çalışıyor |
| T16 | In-app bildirimler | ✅ PASS | Zil ikonu, panel, "Teklifiniz Kabul Edildi" |
| T17 | ToS/Privacy | ❌ FAIL | E4 — sayfalar yok |
| T18 | Console errors | ✅ PASS | Temiz, hata yok |
| T19 | Mobile (375px) | ⚠️ SKIP | Resize tool Chrome'da etkisiz — 2 telefon testi gerekli |

---

## Yeni Buglar — 30 Haziran 2026 (Buyer Flow Audit)

### Kritik

| ID | Bug | Köken | Durum |
|---|---|---|---|
| B11 | Safran fiyatı ₺850/g gösteriyor (₺850.000/kg = piyasanın 10x'i) — birim hatası | listings tablosunda kg fiyatı gram olarak gösteriliyor | ⏳ Fix bekleniyor |
| B16 | Buyer aktif sipariş kartında satıcı telefon numarası plaintext görünüyor (`+90 542 124 10 11`) | profiles telefon maskelenmemiş | ⏳ Fix bekleniyor — KVKK riski |

### Orta Seviye

| ID | Bug | Durum |
|---|---|---|
| B12 | Farmer paneli Teklifler/Aktif tab'da alıcı adı "Alıcı" generic — buyer profili fetch edilmiyor | ⏳ |
| B13 | Farmer analitik sayfası — aktif sipariş ve 7 günlük kayda rağmen empty state | ⏳ |
| B15 | Teklif Ver > Teslim Tarihi alanı `mm/dd/yyyy` US format — TR için `dd/mm/yyyy` olmalı | ⏳ |
| B18 | Buyer paneli Abonelikler sayfası "🚧 Mesajlar — yakında" yanlış placeholder gösteriyor | ⏳ |
| B19 | Keşfet Kategoriler: Safran "0 ilan" ama altında safran ilanı var — kategori sayacı yanlış | ⏳ |

### Küçük

| ID | Bug | Durum |
|---|---|---|
| B17 | Aktif siparişlerde geçmiş teslim tarihleri ("1 Ocak 2026") — test datasından kalan dummy tarihler | ⏳ |

---

## Sonraki Adımlar (Temmuz 2026)

**Öncelik sırası (Lovable prompt olarak):**
1. B11 + B12 + B15 + B16 + B19 — tek prompt ile fix (birim, buyer adı, tarih format, telefon mask, kategori sayacı)
2. B1 + B2 + B4 + B5 + B6 + B13 + B18 — ikinci fix prompt
3. P16-A (parsel + ürün fotoğrafı)
4. P16-B (public vitrin `/s/{slug}`)
5. P16-C (IBAN payment bridge)
6. P16-D (ToS + Privacy)
7. P16-E (real-time prices)
8. P16-F (community messaging)
9. P16-G (referral program)


---

## GTM Scope Güncellemesi — Temmuz 2026 (Ürün İzlenebilirliği)

| # | Özellik | Durum | Prompt |
|---|---|---|---|
| E9 | Buyer-facing "Ürün Geçmişi" zaman çizelgesi (journal'dan otomatik) | ❌ Yok | P16-H |
| E10 | İzlenebilirlik coverage rozeti (Temel/İyi Belgelenmiş/Tam İzlenebilir) + eksik adım açıklaması | ❌ Yok | P16-H |
| E11 | Farmer-side "buyer bunu görecek" önizleme + eksik kayıt hatırlatması | ❌ Yok | P16-H |

Detay ve açık kararlar: `TODO.md` → P16-H.


---

## GTM Scope Güncellemesi — Temmuz 2026 (Crop Config & Indoor İlgi Formu)

| # | Özellik | Durum | Prompt |
|---|---|---|---|
| E12 | `crop_config` tablosu + B4/B19/P16-E fix'lerinin buna bağlanması | ❌ Yok | P16-I |
| E13 | `parcels.production_method` (indoor/outdoor) | ❌ Yok | P16-I |
| E14 | `/indoor-basvuru` landing page + form + WhatsApp CTA | ❌ Yok | P16-J |

Detay: `TODO.md` → P16-I, P16-J.

---

## Batch 2 Sonuçları — 1 Temmuz 2026

**Chrome E2E ile doğrulandı (Zeynep Kaya buyer + Ahmet Yılmaz farmer session)**

| Fix | Kapsam | Durum |
|---|---|---|
| Vitrin `/s/{slug}` | Public farmer storefront — giriş yapmadan erişilebilir; **E2 GTM scope kapandı** | ✅ |
| ListingSheet description | Edit modunda description prefill, save'de patch | ✅ |
| Stepper min_order | onChange clamping (min=10g, input min="10"); submit guard: qty < min → toast | ✅ |
| Buyer Tamamlanan tab | status filter: `["delivered","completed"]` | ✅ |
| Accept → order create | Teklif kabul → `orders` satırı upsert + `order_timeline` submitted | ✅ (dolaylı) |
| B11 — fiyat birimi | farmer.prices.tsx: price_points her zaman /kg (Zeytinyağı hariç /L) | ✅ |
| B13 — analytics | useFarmerListings + useFarmerOrders gerçek veri; 3 stat kart | ✅ |
| B15 — date picker | shadcn Popover + Calendar + date-fns tr locale; dd.MM.yyyy format | ✅ |
| B18 — abonelikler | Empty state düzeltildi; "Keşfet'e Git" → /buyer/discover | ✅ |

### Bug Status Özeti (1 Temmuz 2026)

**30 Haziran audit bugları — güncel durum:**

| ID | Açıklama | Durum |
|---|---|---|
| B11 | Safran birim hatası ₺850/g | ✅ Düzeltildi — Batch 1+2 |
| B12 | Farmer panelinde alıcı adı "Alıcı" | ✅ Düzeltildi — Batch 1 |
| B13 | Analytics empty state | ✅ Düzeltildi — Batch 2 |
| B15 | Teslim tarihi mm/dd/yyyy | ✅ Düzeltildi — Batch 2 |
| B16 | Telefon numarası plaintext (KVKK) | ✅ Düzeltildi — Batch 1 |
| B17 | Geçmiş dummy teslim tarihleri | ⏳ Test verisi, beklenen |
| B18 | Abonelikler yanlış placeholder | ✅ Düzeltildi — Batch 2 |
| B19 | Keşfet kategoriler sayacı yanlış | ✅ Düzeltildi — Batch 1 |

**İlk audit (Haziran 2026) farmer bugları — hâlâ pending:**

| ID | Açıklama | Durum |
|---|---|---|
| B1 | Journal tag rendering (`[work:ilaçlama]` ham görünüyor) | ⏳ |
| B2 | JournalEntryCard quality default → 'A' | ⏳ |
| B4 | Hasat Dönemi chip yanlış ay | ⏳ |
| B5 | `safran_soğanı` slug display | ⏳ |
| B6 | Süresi geçmiş ISO sertifikası badge | ⏳ |

### Güncelleme: Sonraki Adımlar (1 Temmuz 2026)

1. **P15-Completion** (A1-A4: backfill, server guard, withdraw, buyer aktif tab) → Lovable prompt
2. **Batch 3** — B1/B2/B4/B5/B6 (kalan farmer bugları) → tek prompt
3. **P16-A** — ürün + parsel fotoğrafı upload (Supabase Storage)
4. **P16-C** — IBAN ödeme köprüsü
5. **P16-D** — ToS + Gizlilik Politikası
6. **P16-E** — gerçek zamanlı fiyat verisi
7. **P16-F** — topluluk mesajlaşma (yorum/reply)
8. **P16-G** — çiftçi referral programı

> P16-B ✅ Batch 2'de tamamlandı. P16-A'dan devam edilecek.

### Açık kalan minor issues (blocker değil)

- "Vitrin linkini kopyala" butonu farmer storefront'ta görünmüyor (Lovable implemente etmemiş)
- Farmer kendi `/s/<uuid>`'ini açınca `/farmer/home`'a yönlendiriliyor — guest visitor için çalışıyor, minor edge case
- B17: Aktif siparişlerde geçmiş dummy teslim tarihleri — test datasından kalan


| E15 | AI kutuları (Dashboard/Günlük/Fiyatlar/Vitrin/Analitik/WhatsApp) crop-agnostic refactor | ❌ Yok | P16-I |


---

## GTM Scope Güncellemesi — Temmuz 2026 (Stok, Batch, Fiyat Şeması)

| # | Özellik | Durum | Prompt |
|---|---|---|---|
| E16 | `available_quantity` hesaplama + vitrin/teklif ekranlarında canlı stok gösterimi + stok 0'da giriş bloğu | ❌ Yok | P16-H genişletme |
| E17 | Batch sayfası (günlük entry → linked listing → hasat geçmişi + stok + satış geçmişi, farmer/buyer redaction ayrımı) | ❌ Yok | P16-H genişletme |
| E18 | `price_feed.source_type` + `crop_config.auto_price_source` şema alanları (canlı job yok) | ❌ Yok | P16-I |

## Kritik Bug — B20 (Temmuz 2026, yeni tespit)

| ID | Bug | Kök Neden | Durum |
|---|---|---|---|
| B20 | Stok kontrolü yok — aynı listing'e birden fazla sipariş `pending_payment`/`accepted` durumuna geçebilir (overselling riski) | Offer kabul/ödeme akışında `listings.quantity` veya eşdeğer bir stok kontrolü yapılmıyor | ⏳ P16-H genişletme ile çözülecek — ayrı fix değil |

Sıra: P15-Completion → Batch 3 → (build P16-F civarında) → **P16-H genişletme (stok+batch, B20 dahil)** → P16-G → P16-I (crop_config + production_method + AI refactor + fiyat şema alanları) → P16-J.
