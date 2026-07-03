---
title: Hasat — Finansal Model
updated: 2026-06-30
tags: [hasat, finance, model]
---

# Finansal Model
> v0.5 — Mayıs 2026. 4-kat iç mekan safran sistemi dahil. Progressive not — güncellemeler alta eklenir.

---

## Temel Varsayımlar

### Dijital Ürün
| Parametre | Değer | Güven |
|---|---|---|
| Çiftçi aboneliği | ₺99/ay, %3 churn | Yüksek |
| Alıcı premium | ₺299/ay, %2 churn | Yüksek |
| İşlem ücreti | GMV'nin %2.5'i | Yüksek |
| Ortalama çiftçi GMV/ay | ₺22,500 (M12+) | Orta |
| İşlem yapan çiftçi % | %40 | Orta |
| Çiftçi CAC | ₺120 | Orta |
| Alıcı CAC | ₺450 | Orta |

### Unit Economics — Dijital
| Metrik | Çiftçi | Alıcı |
|---|---|---|
| Fiyat | ₺99/ay | ₺299/ay |
| LTV (24 ay) | ₺2,376 | ₺7,176 |
| CAC | ₺120 | ₺450 |
| LTV:CAC | 19.8× | 15.9× |
| Payback | ~6 hafta | ~7 hafta |
| Brüt margin | ~%88 | ~%85 |

### Çiftlik — Safran (Araştırma Doğrulanmış)
| Parametre | Değer | Güven |
|---|---|---|
| TKDK oranı | %65 (1994 doğum = genç çiftçi) | Yüksek |
| Outdoor Y1 yield (5.5 dönüm) | ~1,925g | Yüksek |
| Safran fiyatı Y1–2 | ₺400/g D2C | Yüksek |
| Safran fiyatı Y3+ | ₺600/g branded | Orta |
| Safran fiyatı Y4+ | ₺700–800/g Hasat premium | Düşük-Orta |

### Çiftlik — Lavanta (1.0 dönüm)
- Multi-stream A+B+C model
- Y3+ net katkı: ~₺228K
- Detay: [[Research/Lavender]]

### Çiftlik — İç Mekan Safran (4-kat)
- 50m² fiziksel → 200m² efektif, 20,000 korm
- Y1 net: +₺95K (bina hariç)
- Y5 gross: ₺3,031K
- Detay: [[Research/Tech-Indoor-Farming]]

### Ek Farm Maliyetleri
| Kalem | Yıllık ₺ |
|---|---|
| Arazi kirası (6.5 dönüm) | 40,000–50,000 |
| Mali müşavir / muhasebe | 12,000–18,000 |
| Araç / ulaşım | 15,000–25,000 |
| TARSİM mahsul sigortası | 8,000–15,000 |
| ISO 3632 kalite testi | 5,000–10,000 |
| Organik sertifikasyon | 10,000–20,000 |
| Ambalaj / etiket | 8,000–15,000 |
| **Toplam** | **₺98,000–153,000/yıl** |

---

## 5-Yıllık P&L — v0.5 (₺K)

*Allokasyon: 50m² iç mekan safran + 5.5 dönüm dış mekan safran + 1.0 dönüm lavanta*

| Akış | Y1 | Y2 | Y3 | Y4 | Y5 |
|---|---|---|---|---|---|
| Abonelik | 95 | 780 | 1,950 | 6,200 | 13,500 |
| İşlem Ücreti | 25 | 520 | 1,480 | 4,800 | 10,800 |
| Alıcı Premium | 0 | 100 | 820 | 1,900 | 3,000 |
| İç Mekan Safran (4-kat) | 240 | 690 | 1,220 | 1,785 | 3,031 |
| Dış Mekan Safran (5.5d) | 682 | 1,600 | 1,975 | 3,333 | 4,015 |
| Lavanta (1.0d) | 9 | 130 | 305 | 334 | 349 |
| **Toplam Gelir** | **1,051** | **3,820** | **7,750** | **18,352** | **34,695** |
| Base OpEx | 1,800 | 2,700 | 3,800 | 6,500 | 9,100 |
| Ek Farm Maliyetleri | 89 | 154 | 200 | 250 | 300 |
| Farm CapEx | 798 | 320 | 10 | 60 | 10 |
| **Net Kâr** | **-1,636** | **+646** | **+3,740** | **+11,542** | **+25,285** |
| **Kümülatif** | -1,636 | -990 | **+2,750** | +14,292 | +39,577 |

---

## Sermaye Planı

| Ay | Olay | Tutar | Kümülatif |
|---|---|---|---|
| M1 | Kurucu tasarruf | +₺1,000K | ₺1,000K |
| M22 | Dijital harcama M1–22 | −₺230K | ₺770K |
| M23 | Kurucu enjeksiyonu | +₺1,000K | ₺1,770K |
| M24–26 | İç mekan kurulum | −₺650K | ₺1,120K |
| M27 | Dış mekan hazırlık | −₺80K | ₺1,040K |
| M28 | Korm + infra (TKDK %65 sonrası) | −₺250K | ₺790K |
| M29 | Lavanta + altyapı | −₺50K | ₺740K |
| **~M30** | **Tüm capex sonrası buffer** | | **~₺740K** |

---

## Aylık Yakma Oranı (M23+, Çiftlikli)
| Kalem | ₺K/ay |
|---|---|
| Yaşam (sen + partner) | 100 |
| Dijital OpEx | 80 |
| Farm OpEx (M28+) | 48 |
| **Toplam** | **228** |
| **Çıkış tetikleyici MRR** | **₺230K** |

---

## Model Versiyon Geçmişi
| Versiyon | Tarih | Değişiklik | Y5 Net |
|---|---|---|---|
| v0.1 | Nis 2026 | Başlangıç (yanlış rakamlar) | — |
| v0.3 | May 2026 | TKDK %65, lavanta rebuild, OpEx eklendi | ₺22M |
| v0.4 | May 2026 | Indoor ₺600/g + ₺10 korm + optimal alloc. | ₺23.8M |
| **v0.5** | **May 2026** | **4-kat indoor → 20K korm** | **₺25.3M** |

---

## Açık Araştırma
- [ ] İç mekan bina kararı: kendi yapımız mı, kiralık mı?
- [ ] Farm ili: TKDK 42-il listesi teyidi (Safranbolu/Karabük uygun mu?)
- [ ] Korm toptan teklifi (500kg+)
- [ ] Organik sertifikasyon M18 öncesi hazır mı?
- [ ] Arazi kirası → bölge netleşince revize
