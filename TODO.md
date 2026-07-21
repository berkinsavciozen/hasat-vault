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
- [x] İzmir hal genişletmesi: 3 → 27 ürün, Q3 backfill, `unit` alanı, UI birim gösterimi — hepsi canlı
- [x] **Fiyatlar liste sayfası: çok-kaynaklı kompakt kartlar + izleme-öncelikli kısa liste** (2026-07-21) — bkz. detay

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
- [ ] `price_points` tablosu hâlâ ölü kod adayı.
- [ ] Lovable kuyruk duraklatma sorunu — tekrarlayan.

---

## 🏗️ Lovable/Supabase Build Sırası

> **P18/P19 bitti. İzmir tarafı tam olgun (27 ürün, Q3 backfill, birimler, kompakt çok-kaynaklı kartlar — hepsi canlı).** Sıradaki: **global commodity API** (Berkin'in hesap açması gerekiyor — en gerçekçi yol) veya İstanbul'un resmi Veri Talebi süreci (Claude'un kontrolü dışında).

---

## 🎨 P18 — ARAYÜZ YENİLEME — **SERİ TAMAMEN TAMAMLANDI ✅**
(Değişmedi — bkz. önceki sürüm)

---

## 🟢 P19 — BORSA DENEYİMİ — **TAMAMEN TAMAMLANDI ✅**
(Değişmedi — bkz. önceki sürüm)

---

## 🟠 P19-C — KAYNAK & ÜRÜN TİPİ GENİŞLETME *(devam ediyor)*

### ✅ Adım 1/1b/1c/1d — İzmir tam olgunlaştı *(Tamamen tamamlandı — 2026-07-21)*
İzmir genişletme (27 ürün), Q3 backfill, `unit` alanı, UI birim gösterimi — hepsi canlı (bkz. önceki sürüm).

**Bu turda eklendi — Fiyatlar liste sayfası UX düzeltmesi (Berkin'in gözlemi üzerine):** Berkin, liste sayfasındaki kartların sadece Hasat fiyatını gösterdiğini, diğer kaynakların ("+{n} kaynak" etiketi arkasında) görünmediğini fark etti. Kontrol edildi, doğrulandı, düzeltildi:
- `PriceSummaryCard` artık her kaynağın fiyatını **kompakt chip'ler** halinde gösteriyor (örn. "Hasat ₺18/kg" · "İzmir Toptancı Hali ₺42/kg" · varsa "Resmi kaynak ₺X/kg"). Hasat verisi yetersizse chip kaldırılmıyor, kesikli çerçeveli "Hasat: yetersiz veri" olarak kalıyor. Sparkline/üretici sayısı detay sayfasına (`/prices/$crop`, zaten iyiydi) bırakıldı — kart artık çok daha kısa.
- Liste sayfası **izleme-öncelikli**: "Ürünlerin"/"İlgilendiğin Ürünler" artık en fazla **4 ürünle** sınırlı (`TIER1_LIMIT`), fazlası "+{n} tane daha — Tüm Piyasa'da gör" linkiyle kontrollü accordion'u açıyor. "İzleme Listesi" sınırsız kalıyor.
- Aramadan yıldızlanan bir ürünün otomatik olarak İzleme Listesi'ne düşmesi kontrol edildi — `useCreatePriceAlert`/`useTogglePriceAlert` zaten `priceAlerts` query'sini invalidate ediyordu, ek kod gerekmedi.
- `tsgo` temiz, dosya okumasıyla tam doğrulandı.

### 🔍 Adım 2 — Diğer kaynak araştırması
(Değişmedi — bkz. önceki sürüm: İstanbul duraklatıldı — sunucu tarafı gerçek bozukluk, Berkin'in tarayıcısı da doğruladı; Konya elendi — 2004'ten kalma durağan arşiv; Ankara bulunamadı; global commodity API en gerçekçi sıradaki adım)

### Güncellenmiş öncelik sırası
1. **Global commodity API** — Berkin'in hesap/key açması bekleniyor.
2. **İstanbul** — resmi Veri Talebi süreci, Claude'un kontrolü dışında.
3. Konya/Ankara yerine başka bir aday (Bursa, Antalya) araştırılabilir — henüz yapılmadı.

---

## 🟡 AĞUSTOS 2026 — Soft Launch
(Değişmedi)

## 🟣 P17 — GÜVEN ÇEKİRDEĞİ
(Değişmedi)

## 🔵 SAFRAN SEZONU / ⬜ SONRAKI FAZLAR
(Değişmedi)

---

## 📋 Lovable/Supabase Prompt Yazma Kuralları

(1-63 önceki sürümde — devam:)
64. **[Bu turda eklendi] Kullanıcı "bu özellik güzel ama şu kısmı beklediğim gibi değil" dediğinde, önce mevcut dosyayı okuyup gözlemini doğrulamak gerekir** — Berkin'in "sadece Hasat fiyatları görünüyor" gözlemi dosya okumasıyla tam doğrulandı, doğrudan varsayılmadı.

---

## 📌 Kararlar

(önceki tablo + eklenenler:)
| **Fiyatlar liste sayfası UX düzeltmesi (2026-07-21)** | Kompakt çok-kaynaklı chip'ler + izleme-öncelikli 4-ürün sınırı, canlı doğrulandı. |

---

## 📋 Son Test Sonuçları

### Fiyatlar Liste Sayfası UX Düzeltmesi (2026-07-21) ✅
| Kontrol | Sonuç |
|---|---|
| Kart her kaynağın fiyatını gösteriyor (Hasat + Resmi + market source'lar) | ✅ Dosya okumasıyla doğrulandı |
| Hasat yetersiz veri durumu chip olarak kalıyor, kaldırılmıyor | ✅ |
| "Ürünlerin" bölümü 4 ile sınırlı, "+n tane daha" linki accordion'u açıyor | ✅ |
| Arama → yıldızlama → otomatik İzleme Listesi'ne düşme | ✅ (mevcut invalidation yeterliydi) |
| `tsgo` | ✅ Temiz |

### (Önceki tüm test sonuçları — değişmedi, önceki sürümlerde)
