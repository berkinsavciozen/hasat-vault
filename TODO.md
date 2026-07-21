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

---

## 🏗️ Lovable/Supabase Build Sırası

> **P19 + P17-B + P17-C tamamen bitti.** **P17-A VE P17-D ikisi de şirket kuruluşuna bağlı, bloke** (aşağı bkz.). Sıradaki: **P17-F (tekrar sipariş + şube adresleri)** — P0 şiddetinde, hiçbir dış bağımlılığı yok.

---

## 🎨 P18 — ARAYÜZ YENİLEME — **SERİ TAMAMEN TAMAMLANDI ✅**
(Değişmedi)

## 🟢 P19 — BORSA DENEYİMİ — **TAMAMEN TAMAMLANDI ✅**
(Değişmedi)

## 🟠 P19-C — KAYNAK & ÜRÜN TİPİ GENİŞLETME — **KAPANDI**
(Değişmedi — bkz. önceki sürüm)

---

## 🟣 P17 — GÜVEN ÇEKİRDEĞİ *(devam ediyor — 2026-07-21)*

### Kapsam ve sıralama (güncellendi — bağımlılık netleştirildi)
| Kod | Konu | Şiddet | Durum |
|---|---|---|---|
| P17-B | Teslim/kargo/iptal/ihtilaf akışı | P0 | ✅ **TAMAMLANDI** |
| P17-C | Karşılıklı değerlendirme (rating/review) | P0 | ✅ **TAMAMLANDI** |
| P17-A | Gerçek bloke ödeme (escrow) | P0 | 🔴 **Bloke — şirket kuruluşu + iyzico pazaryeri başvurusu gerekiyor** |
| P17-D | Fatura/e-müstahsil | P0 (B2B) | 🔴 **Bloke — e-fatura/e-müstahsil entegrasyonu vergi mükellefiyeti (vergi no) gerektirir, şirket olmadan başlanamaz** (bu turda netleşti) |
| P17-F | Tekrar sipariş + şube adresleri | P0 (HoReCa) | ✅ **Sıradaki — hiçbir dış bağımlılığı yok** |
| P17-E | Yapılandırılmış RFQ | P1 | Henüz sırada değil, bağımsız |
| P17-G | KPI ölçüm görünümleri | destek | Henüz sırada değil, bağımsız |

**Karar (2026-07-21):** Şirket kuruluşu tamamlanana kadar P17-A ve P17-D ikisi de bekletiliyor. Kalan üç bağımsız fazdan (E/F/G) en yüksek şiddetli olan **P17-F**'ye geçiliyor — HoReCa segmentinin retention'ı için kritik ("Cuma listemi tekrarla" — BENCHMARK.md), tamamen yazılım, hiçbir entegratör/API beklemiyor.

### ✅ P17-B / P17-C — TAMAMLANDI
(Değişmedi — bkz. önceki sürüm, kapsamlı canlı doğrulama detayları)

### 🔜 P17-F — Tekrar Sipariş + Şube Adresleri *(sıradaki, plan bekliyor)*
**Kapsam taslağı (onay bekliyor):**
- Yeni `buyer_addresses` tablosu: `buyer_id`, `label` (örn. "Merkez Şube", "Kadıköy Şube"), `address`, `city`, `is_default`.
- Sipariş detayına/listesine "🔁 Tekrar Sipariş Ver" butonu — aynı çiftçi/ürün/miktar/fiyatla yeni bir teklif taslağı açar (mevcut stok/aktif ilan durumuna göre güncellenmiş fiyatla, sabit eski fiyat değil).
- Alıcı profiline/checkout akışına şube adresi seçici eklenir (varsayılan adres + yönetim ekranı).

---

## 🟡 AĞUSTOS 2026 — Soft Launch
(Değişmedi)

## 🔵 SAFRAN SEZONU / ⬜ SONRAKI FAZLAR
(Değişmedi)

---

## 📋 Lovable/Supabase Prompt Yazma Kuralları

(1-72 önceki sürümde — devam:)
73. **[Bu turda eklendi] Bir fazın dış bağımlılığı (örn. iyzico başvurusu) fark edildiğinde, roadmap'teki BENZER bağımlılığı olan diğer fazlar da hemen kontrol edilmeli** — P17-D'nin (e-fatura) de şirket kuruluşuna bağlı olduğu, P17-A'nın bağımlılığı netleşince akla gelip kontrol edildi, TODO'ya böylece iki faz birden düzeltildi.

---

## 📌 Kararlar

(önceki tablo + eklenenler:)
| **P17-A + P17-D bloke (2026-07-21)** | İkisi de şirket kuruluşuna bağlı (escrow: iyzico pazaryeri; fatura: vergi mükellefiyeti). Şirket kurulana kadar bekletiliyor. |
| **Sıradaki: P17-F (2026-07-21)** | Bağımsız, P0 şiddetinde, HoReCa retention için kritik. |

---

## 📋 Son Test Sonuçları
(Değişmedi — bkz. önceki sürüm, P17-C tam doğrulama tablosu)
