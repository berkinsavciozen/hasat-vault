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
- [x] **P17-B kapsamı audit edildi — sipariş sonrası akış tamamen eksik bulundu** (2026-07-21) — bkz. detay

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

> **P19 tamamen bitti.** Sıradaki: **P17 — Güven Çekirdeği**. Sıralama: **P17-C (karşılıklı değerlendirme) → P17-B (teslim/kargo/iptal/ihtilaf) → P17-A (gerçek escrow, iyzico başvurusuna bağlı) → P17-D/E/F.** P17-B'nin tam kapsamı bu turda audit edildi (aşağı bkz.) — plan hazır, onay bekliyor.

---

## 🎨 P18 — ARAYÜZ YENİLEME — **SERİ TAMAMEN TAMAMLANDI ✅**
(Değişmedi)

## 🟢 P19 — BORSA DENEYİMİ — **TAMAMEN TAMAMLANDI ✅**
(Değişmedi)

## 🟠 P19-C — KAYNAK & ÜRÜN TİPİ GENİŞLETME — **KAPANDI**
(Değişmedi — bkz. önceki sürüm)

---

## 🟣 P17 — GÜVEN ÇEKİRDEĞİ *(başladı — 2026-07-21)*

### Kapsam ve sıralama (onaylandı)
| Kod | Konu | Şiddet | Durum |
|---|---|---|---|
| P17-C | Karşılıklı değerlendirme (rating/review) | P0 | Sıradaki — bağımsız, hemen başlanabilir |
| P17-B | Teslim/kargo/iptal/ihtilaf akışı | P0 | **Audit edildi (aşağı bkz.), plan hazır** |
| P17-A | Gerçek bloke ödeme (escrow) | P0 | iyzico pazaryeri başvurusuna bağlı, beklemede |
| P17-D | Fatura/e-müstahsil | P0 (B2B) | Henüz sırada değil |
| P17-E | Yapılandırılmış RFQ | P1 | Henüz sırada değil |
| P17-F | Tekrar sipariş + şube adresleri | P0 (HoReCa) | Henüz sırada değil |
| P17-G | KPI ölçüm görünümleri | destek | Henüz sırada değil |

### 🔴 P17-B Audit Sonuçları *(Tamamlandı — 2026-07-21, kapsamlı kod+veri incelemesi)*

**Sipariş sonrası akışın (kargo/teslim/iptal/ihtilaf) TAMAMI eksik/kırık bulundu:**

1. **Durum geçişi hiç yok.** `orders.status` sadece `"preparing"`'e geçiyor (3 farklı mutation'da doğrulandı: `useUpdateOfferStatus`, `useSimulatePayment`, `useConfirmTransferReceived`). `"shipped"`/`"delivered"`/`"disputed"`/`"completed"`'a geçiren **hiçbir mutation yok**. Çiftçide "Kargoya Ver" butonu, alıcıda "Teslim Aldım" butonu **yok**.
2. **`disputed` durumu UI'da "preparing" olarak yanlış gösteriliyor (gerçek bug).** `dbToOrder`'daki `statusMap`: `disputed: "preparing"`. Bir sipariş ihtilaflı olsa bile ekranda "Hazırlanıyor" görünür — ihtilaf tamamen gizlenir.
3. **`cancelled` durumu şemada yok.** `order_status` enum = `{preparing, shipped, delivered, disputed, completed}`. `useBuyerAnalytics`'teki `.neq("status","cancelled")` ölü kod — bu değer hiç var olmamış.
4. **Kargo takip no/firma alanı hiç yok**, `orders` tablosunda böyle bir kolon yok.
5. **Gerçek teslimat yöntemi gösterilmiyor (gerçek bug).** `dbToOrder` teslimat yöntemini her zaman sabit `"Kargo"` döndürüyor — alıcının/çiftçinin gerçekte seçtiği yöntemi (örn. "Üreticiden Teslim") görmezden geliyor.
6. **İade/iptal/kısmi red akışı hiç yok** — tablo, mutation, UI'nın hiçbiri.
7. **Buyer sipariş detayında "Satıcıyla Konuş" butonu açık bir placeholder:** tıklanınca "Mesajlaşma yakında..." gösteriyor — üretimde görünen işlevsiz özellik.
8. **110 "delivered" sipariş, `order_timeline` kaydı olmadan mevcut** — gerçek akıştan geçmemiş, direkt seed edilmiş (mock veri, aktif bug değil ama gerçek geçişin hiç test edilmemiş olduğunu doğruluyor).

### P17-B Kapsam Önerisi (onay bekliyor)
1. **Şema:** `orders`'a `tracking_number`, `carrier`, `cancelled_at`, `cancel_reason` kolonları; `order_status` enum'ına `cancelled` eklenir; yeni `disputes` tablosu (order_id, opened_by, reason, evidence_photo_urls, status: open/resolved, resolution, resolved_at, 24 saatlik itiraz penceresi için `window_expires_at`).
2. **Mutation'lar:** `useMarkShipped` (çiftçi, tracking_number+carrier girer), `useConfirmDelivery` (alıcı, fotoğraflı kabul + 24s itiraz penceresi başlar), `useOpenDispute` (alıcı, pencere içinde), `useCancelOrder` (karşılıklı onay veya belirli koşullarda tek taraflı — netleştirilecek).
3. **Bug düzeltmeleri (aynı turda):** `dbToOrder`'daki `disputed` mapping düzeltilir (gerçek "İhtilaflı" durumu gösterilir), teslimat yöntemi gerçek `offer.delivery` değerinden okunur (sabit "Kargo" kaldırılır).
4. **UI:** Çiftçi `OrderCard`'a durum bazlı aksiyon butonu (Hazırlanıyor→Kargoya Ver; Kargoda→bekliyor); alıcı sipariş detayına "Teslim Aldım" (fotoğraflı) + "İhtilaf Aç" butonları, kargo takip no görünümü. "Satıcıyla Konuş" placeholder'ı gerçek bir yönlendirmeyle (mevcut WhatsApp/mesajlaşma akışına) değiştirilir veya kaldırılır.
5. Bu backend değişikliği içeriyor (yeni tablo/kolon/enum) — P18'in "SADECE src/" kuralı burada geçerli değil, P17 fazının kendi guardrail'i: migration'lar sadece ekler, mevcut hiçbir tabloyu/RLS'i bozmaz.

---

## 🟡 AĞUSTOS 2026 — Soft Launch
(Değişmedi)

## 🔵 SAFRAN SEZONU / ⬜ SONRAKI FAZLAR
(Değişmedi)

---

## 📋 Lovable/Supabase Prompt Yazma Kuralları

(1-66 önceki sürümde — devam:)
67. **[Bu turda eklendi] Bir enum'da tanımlı ama koda hiç bağlanmamış değerler (örn. `disputed`) sinsi bir "sahte" bug kaynağı olabilir** — şema "destekliyor görünür" ama gerçekte hiçbir mutation o değeri üretmiyor veya UI o değeri yanlış eşliyor. Bir akışı "eksik" diye işaretlemeden önce enum'un TÜM değerlerinin gerçekten bir mutation tarafından üretilip üretilmediği kontrol edilmeli.
68. **[Bu turda eklendi] `.neq("status", "X")` gibi bir filtre kodda varsa, "X" değerinin gerçekten o enum'da var olup olmadığı doğrulanmalı** — yoksa bu filtre hiçbir şey yapmıyordur, sessiz ölü kod.

---

## 📌 Kararlar

(önceki tablo + eklenenler:)
| **P17 sıralaması onaylandı (2026-07-21)** | C → B → A(iyzico'ya bağlı) → D/E/F/G. |
| **P17-B audit tamamlandı (2026-07-21)** | Sipariş sonrası akış (kargo/teslim/iptal/ihtilaf) tamamen eksik/kırık bulundu, kapsam önerisi hazır, onay bekliyor. |

---

## 📋 Son Test Sonuçları

### P17-B Audit (2026-07-21) 🔴 — Tamamı eksik/kırık
| Kontrol | Sonuç |
|---|---|
| Kargoya verme geçişi (mutation) | ❌ Yok |
| Teslim onayı geçişi (mutation) | ❌ Yok |
| İhtilaf UI'da doğru gösteriliyor mu | ❌ Hayır, "preparing"e yanlış map ediliyor (bug) |
| İptal durumu şemada var mı | ❌ Hayır |
| Kargo takip no/firma alanı | ❌ Yok |
| Gerçek teslimat yöntemi gösteriliyor mu | ❌ Hayır, sabit "Kargo" (bug) |
| İade/kısmi red akışı | ❌ Yok |
| "Satıcıyla Konuş" işlevsel mi | ❌ Hayır, placeholder |

### (Önceki tüm test sonuçları — değişmedi, önceki sürümlerde)
