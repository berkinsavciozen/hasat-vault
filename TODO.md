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
- [x] P17-B — Sipariş sonrası akış (kargo/teslim/iptal/ihtilaf) sıfırdan inşa edildi, canlı doğrulandı
- [x] **P17-C — Karşılıklı Değerlendirme Sistemi TAMAMLANDI + 3 gerçek boşluk düzeltildi** (2026-07-21) — bkz. detay

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

> **P19 + P17-B + P17-C tamamen bitti, hepsi canlı doğrulandı.** Sıradaki: **P17-A (gerçek escrow, iyzico başvurusuna bağlı, beklemede) → P17-D/E/F/G.**

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
| P17-C | Karşılıklı değerlendirme (rating/review) | P0 | ✅ **TAMAMLANDI — canlı doğrulandı** |
| P17-A | Gerçek bloke ödeme (escrow) | P0 | iyzico pazaryeri başvurusuna bağlı, beklemede |
| P17-D | Fatura/e-müstahsil | P0 (B2B) | Henüz sırada değil |
| P17-E | Yapılandırılmış RFQ | P1 | Henüz sırada değil |
| P17-F | Tekrar sipariş + şube adresleri | P0 (HoReCa) | Henüz sırada değil |
| P17-G | KPI ölçüm görünümleri | destek | Henüz sırada değil |

### ✅ P17-B — TAMAMLANDI
(Değişmedi — bkz. önceki sürüm, kapsamlı canlı doğrulama detayları)

### ✅ P17-C — Karşılıklı Değerlendirme Sistemi — TAMAMLANDI *(2026-07-21, kapsamlı canlı doğrulama)*

**İlk build (şema + backend + temel UI):**
- **Şema** ✅ — Yeni `reviews` tablosu (order_id, reviewer_id, reviewee_id, reviewer_role, rating 1-5, comment, created_at), unique `(order_id, reviewer_id)`. RLS: SELECT herkese açık (agregat/profil gösterimi için), INSERT sadece `reviewer_id=auth.uid()` + siparişin gerçek tarafı olma + rol-eşleşme kontrolüyle (policy içeriği okunarak doğrulandı — sadece varlığı değil, `with_check` mantığı da kontrol edildi).
- **RPC** ✅ — `get_farmer_rating_summary(_farmer_id)` (sadece `reviewer_role='buyer'` filtreli — alıcıdan çiftçiye).
- **Mutation/query'ler** ✅ — `useCreateReview`, `useOrderReviews`, `useFarmerRatingSummary`, `useFarmerRecentReviews` — hepsi `queries.ts`'te doğrulandı.
- **UI** ✅ — Alıcı sipariş detayında (`buyer.orders.$orderId.tsx`) "Üreticiyi Değerlendir" (yıldız+yorum modalı, zaten değerlendirilmişse "Değerlendirdiniz ✓"); çiftçi sipariş listesinde (`farmer.orders.index.tsx` `OrderCard`) "Alıcıyı Değerlendir" aynı desende; üretici public profilinde (`buyer.producer.$id.tsx`) gerçek ortalama puan + son yorumlar (review yoksa "Henüz değerlendirme yok", sahte veri yok — E2E-QA S16'da kaldırılan sahte "Alıcı Yorumları"nın gerçek karşılığı).

**Berkin'in canlı testte bulduğu 3 gerçek boşluk — hepsi düzeltildi ve doğrulandı:**
1. **Alıcının "Tamamlanan" LİSTE sayfasında (`buyer.orders.tsx`) değerlendirme göstergesi yoktu** — çiftçi tarafında liste sayfasının kendisinde buton varken, alıcı tarafında sadece detay sayfasında vardı (asimetri). **Düzeltme:** `renderDoneOrders` her satırı artık `DoneOrderRow` component'i (satır bazlı `useOrderReviews`/`ReviewModal` ile) — liste satırında doğrudan "⭐ Değerlendir" / "⭐ Değerlendirdiniz" gösteriyor, `e.stopPropagation()` ile satırın kendi navigate'ini bozmadan.
2. **Alıcının puanı hiçbir yerde çiftçiye görünmüyordu** (tek yönlü kalmıştı). **Düzeltme:** Yeni `get_buyer_rating_summary(_buyer_id)` RPC'si (aynı desende, `reviewer_role='farmer'` filtreli) + `useBuyerRatingSummary` hook'u + yeni `BuyerRatingBadge` component'i — `farmer.orders.index.tsx`'teki `OfferCard` ve `OrderCard`'da alıcı adının yanında "⭐ {puan} ({sayı})" gösteriyor, review yoksa hiçbir şey göstermiyor (sahte/varsayılan yok).
3. **Değerlendirme oluşturulunca bildirim gitmiyordu.** **Düzeltme:** `useCreateReview`, mevcut `useCreateReply`'deki best-effort bildirim desenini (try/catch'li, tablo/RLS sorununda sessiz geçen) uyarlayarak `notifications` tablosuna `{type:'review', title:'Yeni değerlendirme', body:'{isim} {puan}/5 puan verdi'}` satırı ekliyor.

**Doğrulama yöntemi:** Her üç düzeltme de dosya okumasıyla (`queries.ts`, `buyer.orders.tsx`, `farmer.orders.index.tsx`) tek tek teyit edildi — Lovable'ın "tamamlandı" özetine güvenilmedi.

---

## 🟡 AĞUSTOS 2026 — Soft Launch
(Değişmedi)

## 🔵 SAFRAN SEZONU / ⬜ SONRAKI FAZLAR
(Değişmedi)

---

## 📋 Lovable/Supabase Prompt Yazma Kuralları

(1-70 önceki sürümde — devam:)
71. **[Bu turda eklendi] "Karşılıklı" (mutual) bir özellik tasarlarken, her iki yönün de SEMANTİK OLARAK ayrı olabileceği unutulmamalı** — `reviewer_role='buyer'` filtreli bir RPC'yi diğer yön için (buyer'ın puanı) doğrudan tekrar kullanmaya çalışmak yanlış sonuç verir; her yön için doğru filtreyle ayrı bir RPC/sorgu gerekir.
72. **[Bu turda eklendi] Bir liste sayfası ile onun detay sayfası arasında aynı aksiyon (örn. "değerlendir") varsa, ikisinde de AYNI görünürlükte olmalı** — sadece detay sayfasına gömmek, kullanıcının o aksiyonu hiç fark etmemesine yol açabilir; liste ve detay arasında UI parity kontrolü yapılmalı.

---

## 📌 Kararlar

(önceki tablo + eklenenler:)
| **P17-C tamamlandı (2026-07-21)** | Şema+RPC+mutation+UI, tam canlı doğrulandı. Berkin'in bulduğu 3 boşluk (liste parity, alıcı rozeti, bildirim) da düzeltildi ve doğrulandı. Sıradaki: P17-A (iyzico'ya bağlı, beklemede) veya P17-D/E/F/G. |

---

## 📋 Son Test Sonuçları

### P17-C Tam Doğrulama (2026-07-21) ✅
| Kontrol | Sonuç |
|---|---|
| `reviews` şeması + RLS (SELECT/INSERT, taraf+rol kontrolü) | ✅ Policy içeriği okunarak doğrulandı |
| `get_farmer_rating_summary`/`get_buyer_rating_summary` RPC'leri | ✅ Canlı test edildi |
| Çiftçi liste sayfası "Alıcıyı Değerlendir" | ✅ |
| Alıcı detay sayfası "Üreticiyi Değerlendir" | ✅ |
| Alıcı LİSTE sayfası değerlendirme göstergesi (parity fix) | ✅ Dosya okumasıyla doğrulandı |
| Çiftçi tarafında alıcı puan rozeti (`BuyerRatingBadge`) | ✅ Dosya okumasıyla doğrulandı |
| Değerlendirme sonrası bildirim (`notifications` insert) | ✅ Dosya okumasıyla doğrulandı |
| Üretici profilinde gerçek puan/yorum, sahte veri yok | ✅ |
| `tsgo` | ✅ Temiz |

### (Önceki tüm test sonuçları — değişmedi, önceki sürümlerde)
