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
- [x] Fiyatlar liste sayfası: çok-kaynaklı kompakt kartlar + izleme-öncelikli kısa liste
- [x] **Global commodity API araştırması sonuçlandı — P19-C'nin bu kolu durduruldu** (2026-07-21) — bkz. detay

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

> **P18/P19 bitti. İzmir tarafı tam olgun (27 ürün, Q3 backfill, birimler, kompakt çok-kaynaklı kartlar — hepsi canlı).** P19-C'nin diğer kaynak araştırma kolu (İstanbul/Konya/global commodity) şimdilik durduruldu — hiçbiri net bir ilerleme sunmuyor (aşağı bkz.). **Sıradaki gerçek öncelik: Faz C / P17 (Güven Çekirdeği).**

---

## 🎨 P18 — ARAYÜZ YENİLEME — **SERİ TAMAMEN TAMAMLANDI ✅**
(Değişmedi — bkz. önceki sürüm)

---

## 🟢 P19 — BORSA DENEYİMİ — **TAMAMEN TAMAMLANDI ✅**
(Değişmedi — bkz. önceki sürüm)

---

## 🟠 P19-C — KAYNAK & ÜRÜN TİPİ GENİŞLETME *(bu turda kapatıldı — durduruldu)*

### ✅ İzmir tam olgunlaştı + Fiyatlar UX düzeltmesi *(Tamamen tamamlandı)*
(Değişmedi — bkz. önceki sürüm: 27 ürün, Q3 backfill, `unit` alanı, kompakt çok-kaynaklı kartlar)

### 🔴 Global Commodity API — ARAŞTIRMA SONUÇLANDI, DURDURULDU *(2026-07-21)*
Berkin, `commoditic.com`'un ücretli olduğunu bulup **`api-ninjas.com`'da gerçek bir hesap açtı** (3.000 istek/ay ücretsiz katman, kredi kartsız). Gerçek API key ile canlı test edildi:

- **Kapsam sorunu:** API Ninjas'ın ücretsiz katmanı **haftalık dönen küçük bir emtia alt kümesi** sunuyor. Bu hafta ücretsiz olanlar: `crude_oil`, `feeder_cattle`, `gasoline_rbob`, `gold`, `heating_oil`, `lean_hogs`, `live_cattle`. **Hedef 4 ürünümüzün (buğday/mısır/pamuk/ayçiçeği) hepsi premium'a kilitli** — canlı 400 hata ile doğrulandı.
- **Talep sorusu:** Bu sorunu çözsek bile kontrol ettim — `listings` tablosunda buğday/mısır/pamuk/ayçiçeği için **sıfır ilan, sıfır çiftçi** var. Yani şu an bu veriyle doldurulacak gerçek bir "piyasa" yok.
- **Karar: bu araştırma kolu durduruldu.** İki bağımsız sinyal (kapsam uyumsuzluğu + gerçek kullanıcı talebi yok) aynı yöne işaret ediyor — düşük öncelik. Güvenlik temizliği yapıldı: API key içeren geçici probe fonksiyonu (`probe-api-ninjas`) inert bir stub'a döndürüldü (delete tool'u yok, içerik nötrleştirildi).
- **Yeniden değerlendirme koşulu:** Hasat gerçekten buğday/mısır/pamuk/ayçiçeği üreten çiftçileri onboard ederse (örn. Konya/Orta Anadolu bölgesine genişleme), bu konu tekrar açılabilir — o zaman ücretli bir sağlayıcı (Commoditic, CommodityPriceAPI) veya API Ninjas'ın premium katmanı değerlendirilir.

### Genel P19-C Özeti (kapanış notu)
- ✅ İzmir — tam entegre, olgun, 27 ürün.
- 🔴 İstanbul — duraklatıldı (sunucu tarafı gerçek bozukluk, resmi Veri Talebi'ne kadar).
- ❌ Konya — elendi (2004'ten kalma durağan arşiv).
- ❌ Global commodity API — durduruldu (kapsam + talep yok).
- Not bulunamadı: Ankara (spesifik hal verisi yok).

**P19-C artık aktif olarak ilerletilmiyor** — İzmir'in verdiği değer yeterli, diğer kollar gerçek engellerle karşılaştı. GTM önceliği **Faz C / P17**'ye geri dönüyor.

---

## 🟡 AĞUSTOS 2026 — Soft Launch
(Değişmedi)

## 🟣 P17 — GÜVEN ÇEKİRDEĞİ *(sıradaki gerçek öncelik)*
(Değişmedi — bkz. önceki sürüm: P17-A/B/C/D/E/F/G)

## 🔵 SAFRAN SEZONU / ⬜ SONRAKI FAZLAR
(Değişmedi)

---

## 📋 Lovable/Supabase Prompt Yazma Kuralları

(1-64 önceki sürümde — devam:)
65. **[Bu turda eklendi] Bir dış API entegrasyonuna yatırım yapmadan önce, "bu veriyi kim görecek?" sorusu erken sorulmalı** — API Ninjas'ın kapsam sorunuyla karşılaşmadan önce bile, `listings` tablosunda hedef ürünler için hiç çiftçi olmadığı tek bir sorguyla görülebilirdi; iki bulgu birlikte kararı çok daha hızlı netleştirdi.
66. **[Bu turda eklendi] "Ücretsiz katman" ile "ücretsiz katmanda bizim ihtiyacımız olan şey var" farklı şeyler** — bazı API'ler (API Ninjas gibi) gerçekten ücretsiz ve kredi kartsız olabilir, ama ücretsiz katmanın kapsamı (haftalık rotasyon, ürün kısıtı) asıl ihtiyaçla örtüşmeyebilir; hesap oluşturmak yeterli değil, gerçek bir çağrıyla doğrulamak gerekir.

---

## 📌 Kararlar

(önceki tablo + eklenenler:)
| **Global commodity API durduruldu (2026-07-21)** | API Ninjas ücretsiz katmanı hedef ürünleri kapsamıyor + platformda bu ürünler için hiç çiftçi/ilan yok. İki sinyal birlikte düşük öncelik gösteriyor. |
| **P19-C kapandı (2026-07-21)** | İzmir'in verdiği değer yeterli görüldü, kalan araştırma kolları gerçek engellerle karşılaştı. Öncelik Faz C / P17'ye döndü. |

---

## 📋 Son Test Sonuçları

### Global Commodity API — API Ninjas (2026-07-21) ❌ Durduruldu
| Kontrol | Sonuç |
|---|---|
| Hesap gerçek ve ücretsiz mi | ✅ 3.000 istek/ay, kredi kartsız |
| Hedef 4 ürün (buğday/mısır/pamuk/ayçiçeği) ücretsiz katmanda var mı | ❌ Hepsi premium'a kilitli (canlı 400 hatasıyla doğrulandı) |
| Platformda bu ürünler için çiftçi/ilan var mı | ❌ Sıfır (SQL ile doğrulandı) |
| Karar | Durduruldu, güvenlik temizliği yapıldı |

### (Önceki tüm test sonuçları — değişmedi, önceki sürümlerde)
