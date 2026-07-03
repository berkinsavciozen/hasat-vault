---
title: Hasat — İç Mekan Tarım Teknolojisi
updated: 2026-06-30
tags: [hasat, research, indoor, farming, tech]
---

# İç Mekan Tarım Teknolojisi
> Progressive not — yeni araştırma her zaman yeni `## [Tarih] — [Konu]` başlığıyla alta eklenir.

---

## Mayıs 2026 — İç Mekan Safran Sistemi (4-Kat)

### Neden Safran CEA İçin İdeal?
| Faktör | Safran Avantajı |
|---|---|
| Bitki boyu | 15–20 cm → 4 kat mümkün |
| Işık ihtiyacı | Düşük PPFD (gölge bitkisi) |
| **Karanlık evre** | **~7 ay LED gerektirmez** (dormancy + chilling) |
| Kat başına yükseklik | **50 cm** (LED + clearance dahil) |
| Primary product | Stigma — küçük hacim, çok yüksek değer |

### 4-Kat Sistem Hesabı (50m² fiziksel → 200m² efektif)

**Kat başına yükseklik dağılımı:**
- Tepsi (substrat + korm): 12 cm
- Bitki (yaprak maks.): 20 cm
- LED + montaj: 5 cm
- Clearance: 13 cm
- **Toplam: 50 cm/kat**

**Tavan gereksinimine göre kat sayısı:**
| Tavan | Kat | Efektif Alan |
|---|---|---|
| 2.5m | 3 kat | 150m² |
| **3.0m** | **4 kat** | **200m²** ✅ |
| 3.5m | 5 kat | 250m² |

> ⚠️ Kritik: Bina minimum 3.0m tavan şartı — kiralama kararında belirleyici.

### Elektrik Maliyeti — Karanlık Evre Avantajı
Safran LED'e yalnızca ~5 ay ihtiyaç duyar:
- Vejetatif: Aralık–Mart (4 ay)
- Çiçeklenme: Ekim–Kasım (1.5 ay)
- Dormancy + chilling: Nisan–Eylül (**~6.5 ay — LED YOK**)

| Bileşen | Yıllık Maliyet |
|---|---|
| LED (200m² × 40W/m² × 12h × 150 gün × ₺3/kWh) | ₺52,000 |
| HVAC (yıl boyu) | ₺35,000–45,000 |
| IoT + pompa + diğer | ₺5,000 |
| **Toplam elektrik (4 kat)** | **₺92,000–102,000/yıl** |

### Gelir Modeli — 20,000 Korm, 200m² Efektif

**Stream 1 — D2C Stigma**
| Yıl | Yield | Fiyat/g | Gelir |
|---|---|---|---|
| Y1 (M27) | 400g | ₺600 | ₺240,000 |
| Y2 | 600g | ₺650 | ₺390,000 |
| Y3 | 800g | ₺700 | ₺560,000 |
| Y4 | 900g | ₺750 | ₺675,000 |
| Y5 | 1,000g | ₺800 | ₺800,000 |

**Stream 2 — Korm Satışı (Platform D2C)**
| Yıl | Üretilen | Replant | Satış | Gelir |
|---|---|---|---|---|
| Y1 | 20,000 | 20,000 | 0 | ₺0 |
| Y2 | ~50,000 | 20,000 | 30,000 | ₺300,000 |
| Y3 | ~75,000 | 20,000 | 55,000 | ₺660,000 |
| Y4 | ~112,500 | 20,000 | 92,500 | ₺1,110,000 |
| Y5 | ~168,750 | 20,000 | 148,750 | ₺2,231,250 |

**Konsolide İç Mekan P&L (₺K)**
| | Y1 | Y2 | Y3 | Y4 | Y5 |
|---|---|---|---|---|---|
| Stigma | 240 | 390 | 560 | 675 | 800 |
| Korm | 0 | 300 | 660 | 1,110 | 2,231 |
| **Brüt** | **240** | **690** | **1,220** | **1,785** | **3,031** |
| OpEx | 145 | 160 | 175 | 190 | 205 |
| **Net (bina hariç)** | **+95** | **+530** | **+1,045** | **+1,595** | **+2,826** |

*Y1'den net pozitif. 5Y kümülatif net (bina hariç): ₺6,091K.*

### Capex — 4-Kat (50m² Fiziksel)
| Kalem | ₺K |
|---|---|
| LED grow lights (200m² eff.) | 240 |
| Metal raf / dikey kurulum | 110 |
| HVAC iklim kontrolü | 75 |
| NFT / substrat sistemi | 90 |
| IoT sensörler + kontrol | 22 |
| İlk korm (20,000 × ₺2.3) | 46 |
| Elektrik altyapısı + montaj | 67 |
| **TOPLAM** | **~₺650K** |

*ROI: +₺230K ek capex (2-kata göre) → Y2'de +₺305K ek net → geri dönüş < 1 yıl.*

### Unit Economics / m² (200m² Efektif)
| Yıl | Brüt/m² | OpEx/m² | Net/m² |
|---|---|---|---|
| Y1 | ₺1,200 | ₺725 | ₺475 |
| Y2 | ₺3,450 | ₺800 | ₺2,650 |
| Y3 | ₺6,100 | ₺875 | ₺5,225 |

### Gerçek Dünya Benchmark
| | Setup | Y1 Korm | Y1 Yield |
|---|---|---|---|
| Konya pilot (TR) | 14m² multi-layer | 10,000 | 200–250g |
| Bizim model | 50m² × 4-kat | 20,000 | 400g |

### Açık Araştırma
- [ ] 🔴 Bina tavan yüksekliği teyidi: min 3.0m
- [ ] 🔴 Bina kararı: kendi yapımız mı, kiralık mı? (₺0 vs ₺96K/yıl)
- [ ] 🟡 Korm toptan teklifi (20,000 adet) — indirim beklentisi
- [ ] 🟡 LED tedarikçi teklifi 4-kat sistem için
- [ ] 🟢 Konya pilot ziyaret — yield doğrulama
