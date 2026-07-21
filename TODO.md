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
- [x] Alıcı Bildirim Tercihleri sayfası eklendi + `buyer.account.tsx` bayat veri bug'ı düzeltildi
- [x] Abonelik → Sipariş bağlantısı (`subscription_id`) eklendi + escrow yanlış vaadi düzeltildi
- [x] **Abonelik CTA'sı üretici profilinde yukarı taşındı + abonelik-sipariş bağlantısı gerçek verilerle bizzat test edildi** (2026-07-21) — bkz. detay

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

> **P19 + P17-B + P17-C + P17-F tamamen bitti, hepsi canlı doğrulandı.** Abonelik↔sipariş bağlantısı + üretici profili CTA konumu düzeltildi. **P17-A ve P17-D şirket kuruluşuna bağlı, bloke.** Sıradaki: **P17-E (yapılandırılmış RFQ)** veya **P17-G (KPI görünümleri)**.

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
| P17-B | Teslim/kargo/iptal/ihtilaf akışı | P0 | ✅ **TAMAMLANDI** |
| P17-C | Karşılıklı değerlendirme (rating/review) | P0 | ✅ **TAMAMLANDI** |
| P17-F | Tekrar sipariş + şube adresleri + abonelik bağlantısı | P0 (HoReCa) | ✅ **TAMAMLANDI** |
| P17-A | Gerçek bloke ödeme (escrow) | P0 | 🔴 Bloke — şirket kuruluşu + iyzico başvurusu |
| P17-D | Fatura/e-müstahsil | P0 (B2B) | 🔴 Bloke — vergi mükellefiyeti gerektiriyor |
| P17-E | Yapılandırılmış RFQ | P1 | Sırada, bağımsız |
| P17-G | KPI ölçüm görünümleri | destek | Sırada, bağımsız |

### ✅ P17-B / P17-C / P17-F / Abonelik-Sipariş Bağlantısı — TAMAMLANDI
(Değişmedi — bkz. önceki sürüm)

### ✅ Abonelik CTA Konumu + Bizzat Canlı Veri Testi *(2026-07-21)*

**Berkin'in "abonelik butonu görünmedi" gözlemi üzerine:**
- Kod incelendi: `buyer.producer.$id.tsx`'teki "Hasat Aboneliği" kartı **koşulsuz render ediliyordu, bug değildi** — ama sayfanın en altında, "Değerlendirmeler"den sonra konumlanmıştı, uzun bir profilde gözden kaçması kolaydı.
- **Düzeltme:** Kart, hero/istatistik alanının hemen altına ("Genellikle 24 saat içinde yanıtlar" rozetinin altına, "Verim Geçmişi"nden önce) taşındı. Dosya tam okunarak tekilliği (mükerrer olmadığı) ve doğru sıra teyit edildi.

**Berkin'in "ikinciyi de sen test et" isteği üzerine — Claude'un bizzat yaptığı canlı veri testi:**
- Zeynep'in Ahmet'le olan **gerçek** aktif bir aboneliği (`ee34d29b-...`) kullanılarak, gerçek bir safran ilanı üzerinden, `subscription_id` bağlantılı **gerçek bir teklif** oluşturuldu (Supabase MCP ile doğrudan).
- Bu teklif, çiftçi tarafının kullandığı **tam sorgu şekliyle** (`useFarmerOffers`'ın join yapısı) geri okundu — `subscription_id` doğru geldi, `buyer_name`/`crop` doğru geldi. Yani "🔁 Abonelik Siparişi" rozetinin gerçekten görüneceği kanıtlandı.
- Test verisi hemen sonra temizlendi (kalıcı test/mock verisi bırakılmadı).

---

## 🟡 AĞUSTOS 2026 — Soft Launch
(Değişmedi)

## 🔵 SAFRAN SEZONU / ⬜ SONRAKI FAZLAR
(Değişmedi)

---

## 📋 Lovable/Supabase Prompt Yazma Kuralları

(1-77 önceki sürümde — devam:)
78. **[Bu turda eklendi] "Buton görünmedi" gibi bir rapor geldiğinde, kodun render koşulunu kontrol etmeden önce SAYFA SIRASINI/konumunu kontrol et** — bu turda buton kodda koşulsuzdu, sorun render mantığında değil, JSX'teki fiziksel konumundaydı (sayfanın en altı).
79. **[Bu turda eklendi] "Sen test et" istendiğinde, statik kod okuması yetmez — gerçek test hesaplarıyla gerçek bir satır oluşturup uygulamanın kullandığı TAM sorgu şekliyle geri okumak, tarayıcı erişimi olmadan en güçlü doğrulama yöntemidir; test verisi hemen sonra temizlenmeli.**

---

## 📌 Kararlar

(önceki tablo + eklenenler:)
| **Abonelik CTA konumu düzeltildi (2026-07-21)** | Sayfanın en altından, hero/istatistik alanının altına taşındı — bug değil, görünürlük sorunuydu. |
| **Abonelik-sipariş bağlantısı bizzat canlı veriyle test edildi (2026-07-21)** | Gerçek Zeynep-Ahmet aboneliği + gerçek teklif + gerçek sorgu şekliyle doğrulandı, test verisi temizlendi. |

---

## 📋 Son Test Sonuçları

### Abonelik CTA + Canlı Veri Testi (2026-07-21) ✅
| Kontrol | Sonuç |
|---|---|
| CTA kartı tekil (mükerrer yok) | ✅ Dosya okumasıyla doğrulandı |
| Yeni sıra (rozet → Abonelik → Verim Geçmişi → ...) | ✅ |
| `tsgo` | ✅ Temiz |
| Gerçek Zeynep-Ahmet aboneliğiyle gerçek teklif oluşturuldu | ✅ |
| Çiftçi sorgu şekliyle geri okunduğunda `subscription_id` doğru geldi | ✅ |
| Test verisi temizlendi | ✅ |

### (Önceki tüm test sonuçları — değişmedi, önceki sürümlerde)
