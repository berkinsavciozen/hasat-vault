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
- [x] **P17-B — Sipariş sonrası akış (kargo/teslim/iptal/ihtilaf) SIFIRDAN İNŞA EDİLDİ VE CANLI DOĞRULANDI** (2026-07-21) — bkz. detay

### P16 — TÜM SERİ TAMAMLANDI ✅
(Detaylar önceki sürümlerde)

---

## 🔎 BENCHMARK RE-AUDIT (Temmuz 2026)
(Değişmedi)

---

## 🔴 ŞİMDİ — Temmuz 2026
(Değişmedi — bkz. önceki sürüm)

### Düşük öncelikli cila
(Değişmedi — bkz. önceki sürüm)

---

## 🏗️ Lovable/Supabase Build Sırası

> **P19 tamamen bitti. P17-B tamamen bitti ve canlı doğrulandı.** Sıradaki: **P17-C (karşılıklı değerlendirme)**. Ardından **P17-A (gerçek escrow, iyzico başvurusuna bağlı) → P17-D/E/F/G.**

---

## 🎨 P18 — ARAYÜZ YENİLEME — **SERİ TAMAMEN TAMAMLANDI ✅**
(Değişmedi)

## 🟢 P19 — BORSA DENEYİMİ — **TAMAMEN TAMAMLANDI ✅**
(Değişmedi)

## 🟠 P19-C — KAYNAK & ÜRÜN TİPİ GENİŞLETME — **KAPANDI**
(Değişmedi — bkz. önceki sürüm)

---

## 🟣 P17 — GÜVEN ÇEKİRDEĞİ *(devam ediyor — 2026-07-21)*

### Kapsam ve sıralama
| Kod | Konu | Şiddet | Durum |
|---|---|---|---|
| P17-B | Teslim/kargo/iptal/ihtilaf akışı | P0 | ✅ **TAMAMLANDI — canlı doğrulandı** |
| P17-C | Karşılıklı değerlendirme (rating/review) | P0 | **Sıradaki** |
| P17-A | Gerçek bloke ödeme (escrow) | P0 | iyzico pazaryeri başvurusuna bağlı, beklemede |
| P17-D | Fatura/e-müstahsil | P0 (B2B) | Henüz sırada değil |
| P17-E | Yapılandırılmış RFQ | P1 | Henüz sırada değil |
| P17-F | Tekrar sipariş + şube adresleri | P0 (HoReCa) | Henüz sırada değil |
| P17-G | KPI ölçüm görünümleri | destek | Henüz sırada değil |

### ✅ P17-B — TAMAMLANDI *(2026-07-21, kapsamlı canlı doğrulama)*

**Audit bulguları (önceki sürümde detaylı — özet: durum geçişi hiç yoktu, `disputed`→"preparing" bug'ı, `cancelled` şemada yoktu, kargo takip alanı yoktu, teslimat yöntemi sabit "Kargo" bug'ıydı, iade/iptal akışı yoktu, "Satıcıyla Konuş" placeholder'dı).**

**Uygulanan ve dosya okuması + canlı SQL sorgularıyla tek tek doğrulanan çözüm:**

1. **Şema** ✅ — `orders`'a `tracking_number`, `carrier`, `cancelled_at`, `cancel_reason`, `dispute_window_expires_at` kolonları eklendi. `order_status` enum'ına `cancelled` eklendi (artık `{preparing,shipped,delivered,disputed,completed,cancelled}`). Yeni `disputes` tablosu (order_id, opened_by, reason, evidence_photo_urls, status, resolution, resolved_at, window_expires_at) — RLS doğrulandı: SELECT/UPDATE sadece sipariş tarafları, INSERT `opened_by=auth.uid()` + sipariş tarafı kontrolü ile. Yeni `delivery-photos` storage bucket (private). **Veri kaybı yok** — 118 sipariş, 110 delivered aynen duruyor.
2. **Mutation'lar** ✅ — `useMarkShipped` (çiftçi, sadece kendi siparişi + `preparing`→`shipped` guard, tracking/carrier set eder, timeline'a ekler), `useConfirmDelivery` (alıcı, sadece `shipped`→`delivered` guard, opsiyonel fotoğraf yükler, 24s itiraz penceresi başlatır), `useCancelOrder` (sadece `preparing`'ten, taraf kontrolü `.or(buyer_id.eq/farmer_id.eq)`), `useOpenDispute` (pencere kontrolü + taraf kontrolü, kanıt fotoğrafları), `useOrderDispute` (okuma). Hepsi `queries.ts`'te tam okunarak doğrulandı.
3. **Bug düzeltmeleri** ✅ — `dbToOrder`'daki `statusMap`: `disputed: "disputed"`, `cancelled: "cancelled"` (düzeltildi). `delivery: deliveryLabel(offer.delivery)` — artık gerçek seçilen yöntem gösteriliyor (sabit "Kargo" kaldırıldı), `ORDER_SELECT`'e `offer.delivery` eklendi.
4. **UI — Çiftçi** ✅ — `OrderCard`'da `preparing` durumunda "📦 Kargoya Ver" (kargo firması + takip no modalı) ve "İptal Et" (sebep modalı) butonları; `shipped` durumunda takip no gösterimi; `cancelled` durumunda iptal sebebi gösterimi.
5. **UI — Alıcı** ✅ — Sipariş detayında `shipped` durumunda takip no gösterimi; "✅ Teslim Aldım" (opsiyonel fotoğraf, sadece `shipped`'ken) ve "⚠️ İhtilaf Aç" (sebep+fotoğraflar, pencere açıkken) butonları; `disputed`/`cancelled` durumları sebep/detayla gösteriliyor. **"Satıcıyla Konuş" placeholder'ı ("Mesajlaşma yakında...") kaldırıldı** — sadece gerçek çalışan WhatsApp/tel butonları kaldı.
6. `tsgo` temiz (Lovable raporu + kod okumasıyla tutarlı).

**Doğrulama yöntemi:** Bu turda "önce güven, sonra doğrula" değil, tam tersi uygulandı — Berkin'in "hazır" bildirimi sonrası `queries.ts`, `farmer.orders.index.tsx`, `buyer.orders.$orderId.tsx` tam okundu, şema+RLS+storage bucket canlı SQL ile ayrıca doğrulandı. Hepsi rapor edilenle birebir eşleşti.

---

## 🟡 AĞUSTOS 2026 — Soft Launch
(Değişmedi)

## 🔵 SAFRAN SEZONU / ⬜ SONRAKI FAZLAR
(Değişmedi)

---

## 📋 Lovable/Supabase Prompt Yazma Kuralları

(1-68 önceki sürümde — devam:)
69. **[Bu turda eklendi] Kullanıcı "hazır, şu şu yapıldı" dediğinde bile, önce kod/şema/RLS'i kendin okuyup doğrula** — bu turda Berkin'in raporu %100 doğru çıktı, ama bu turdan önceki iki denemede Lovable'ın kendi "tamamlandı" mesajı yarım kalmış işi tam gibi göstermişti; kullanıcının raporu bile bu ayrımı otomatik yapmaz.
70. **[Bu turda eklendi] Yeni bir tablo/mutation eklerken RLS politikalarını sadece var olup olmadığı değil, `qual`/`with_check` içeriğini de okuyarak doğrula** — "politika var" ile "politika doğru sipariş taraflarını kısıtlıyor" farklı şeyler.

---

## 📌 Kararlar

(önceki tablo + eklenenler:)
| **P17-B tamamlandı (2026-07-21)** | Şema+mutation+UI+bug düzeltmeleri, tam canlı doğrulandı (kod okuma + SQL + RLS kontrolü). Sıradaki: P17-C. |

---

## 📋 Son Test Sonuçları

### P17-B Tam Doğrulama (2026-07-21) ✅
| Kontrol | Sonuç |
|---|---|
| Şema (kolonlar, enum, disputes tablosu) | ✅ Canlı SQL ile doğrulandı |
| Veri kaybı yok (118 sipariş, 110 delivered) | ✅ |
| `delivery-photos` bucket (private) | ✅ |
| `disputes` RLS (SELECT/UPDATE/INSERT, taraf kontrolü) | ✅ Policy içeriği okunarak doğrulandı |
| 4 yeni mutation (`useMarkShipped`/`useConfirmDelivery`/`useCancelOrder`/`useOpenDispute`) | ✅ Kod okunarak doğrulandı |
| `disputed`/`cancelled` bug düzeltmeleri | ✅ |
| Gerçek teslimat yöntemi gösterimi (sabit "Kargo" kaldırıldı) | ✅ |
| Çiftçi UI: Kargoya Ver + İptal Et butonları | ✅ |
| Alıcı UI: Teslim Aldım + İhtilaf Aç butonları | ✅ |
| "Satıcıyla Konuş" placeholder kaldırıldı | ✅ |
| `tsgo` | ✅ Temiz |

### (Önceki tüm test sonuçları — değişmedi, önceki sürümlerde)
