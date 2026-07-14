---
title: Hasat — Finansal Model
updated: 2026-07-14
tags: [hasat, finance, model]
---

# Finansal Model
> **v0.6 — Temmuz 2026.** Tam yeniden yazım: gelir modeli redesign (ücretsiz çekirdek + AI premium + komisyon), driver tablosu, PSP/vergi/CAC satırları, Base-Bear-Bull senaryoları, zaman ekseni uzlaştırması, dijital/çiftlik P&L ayrıştırması. Denetim kaynağı: BENCHMARK.md (KPI bölümü) + v0.5 formül denetimi.

---

## 0. Gelir Modeli Kararları (2026-07-14, onaylı)

| Karar | Detay |
|---|---|
| Çekirdek ürün | **Tamamen ücretsiz** — ilan, teklif, pazarlık, sipariş, izlenebilirlik, temel fiyat bandı. Gerekçe: tüm TR rakipleri çiftçiye ücretsiz (BENCHMARK Katman A) |
| AI fiyat bandı (nudge) | **Herkese ücretsiz** — güven tezi gereği; premium'a kilitlenmeyecek |
| Premium tier | **Tek tier, herkese aynı fiyat: ₺149/ay (öneri, test edilecek)** — kapsam: AI asistan, otomasyon, gelişmiş analiz. Aylık **kredi/limit** ile (AI COGS kontrolü) |
| Lansman teşviki | İlk 6 ay premium early adopter'lara **ücretsiz** (feature-flag ile değiştirilebilir olmalı) |
| Referral | 3 yeni kullanıcı getiren kullanıcıya **12 ay premium ücretsiz** |
| Komisyon | GMV üzerinden **%2.5 (Base)** — ana gelir kaynağı |

---

## 1. Driver Tablosu — Dijital (Base)

Tüm gelir satırları buradan türetilir; satır elle yazılmaz. (v0.5'teki %40 işlem-yapan varsayımı ile gelir satırları arasındaki ~3× çelişki böylece kapandı; v0.5'in örtük oranı ~%13-14'tü, v0.6 bunu açıkça %15 olarak sabitler.)

| Driver | Y1 | Y2 | Y3 | Y4 | Y5 | Güven |
|---|---|---|---|---|---|---|
| Kayıtlı çiftçi (yıl sonu) | 500 | 2.000 | 5.000 | 10.000 | 18.000 | Düşük-Orta |
| Ortalama kayıtlı (yıl içi) | 250 | 1.250 | 3.500 | 7.500 | 14.000 | — |
| İşlem yapan % (aylık) | %15 | %15 | %15 | %15 | %15 | Orta |
| Ort. işlem yapan çiftçi | 38 | 188 | 525 | 1.125 | 2.100 | — |
| GMV / işlem yapan / ay (₺K) | 12,0 | 22,5 | 22,5 | 22,5 | 22,5 | Orta |
| **Yıllık GMV (₺K)** | **5.472** | **50.760** | **141.750** | **303.750** | **567.000** | — |
| Take rate | %2,5 | %2,5 | %2,5 | %2,5 | %2,5 | Orta |
| PSP maliyeti (GMV %) | %1,25 | %1,25 | %1,25 | %1,25 | %1,25 | Orta (havale ağırlıklı B2B mix varsayımı — teklif alınacak) |
| MAU (çiftçi %50 aktif + alıcı) | ~300 | ~1.025 | ~2.750 | ~5.750 | ~10.500 | Düşük |
| Premium attach (MAU %) | %4 (M7+) | %4 | %4 | %4 | %4 | Düşük (freemium normu %2-5) |
| Ödeyen premium (referral −%15 sonrası) | ~10 | 35 | 94 | 196 | 357 | — |
| Premium ARPU | ₺149/ay | ₺149 | ₺149 | ₺149 | ₺149 | Düşük — early adopter dönemiyle test |
| AI COGS / premium kullanıcı | ~₺30/ay (kredi tavanlı) | — | — | — | — | Düşük — `ai_usage_tracking` ile kalibre edilecek |
| Çiftçi CAC (harmanlanmış) | ₺250 | — | — | — | — | Düşük (v0.5'teki ₺120 saha maliyetsiz iyimserdi; referral programı düşürücü etken) |

## 2. Unit Economics — Düzeltilmiş

**v0.5 hatası:** LTV düz `fiyat × 24` idi; kendi churn varsayımını ve marjı yok sayıyordu.

**v0.6 formülü:** `LTV = aylık katkı × Σ(1−churn)^t × brüt marj` (24 ay ufku)

| Metrik | Premium abone | İşlem yapan çiftçi (komisyon) |
|---|---|---|
| Aylık katkı | ₺149 | ₺281 (22.500 × net take %1,25) |
| Aylık churn | %4 (sezonluk harmanlanmış) | %2 (platformdan ayrılma) |
| Survival toplamı (24 ay) | 15,6 | 19,1 |
| Brüt LTV | ₺2.324 | ₺5.372 |
| COGS sonrası LTV | ₺1.856 (AI maliyeti düşülmüş) | ₺5.372 (PSP zaten net take'te) |
| CAC | platform CAC'ına biner (ayrı edinim yok) | ₺250 |
| **LTV:CAC** | — | **~21×** |

> Not: v0.5'in 19.8× / 15.9× oranları hem formül hatası hem ücretli-çekirdek varsayımı içeriyordu; karşılaştırılabilir değil. Yeni modelde değer, aboneden değil **işlem yapan çiftçiden** akar — aktivasyon (kayıt → ilk satış) en kritik funnel adımıdır (bkz. BENCHMARK §5.1).

---

## 3. Dijital P&L — Base (₺K, şirket yılı: Y1 = M1-M12, Tem 2026 başlangıç)

| Kalem | Y1 | Y2 | Y3 | Y4 | Y5 |
|---|---|---|---|---|---|
| Komisyon (GMV × %2,5) | 137 | 1.269 | 3.544 | 7.594 | 14.175 |
| Premium (AI) | 9 | 63 | 168 | 350 | 638 |
| **Gelir** | **146** | **1.332** | **3.712** | **7.944** | **14.813** |
| PSP maliyeti (GMV × %1,25) | −68 | −635 | −1.772 | −3.797 | −7.088 |
| AI COGS | −4 | −18 | −47 | −98 | −155 |
| **Brüt kâr** | **74** | **679** | **1.893** | **4.049** | **7.570** |
| Dijital OpEx (infra+ekip) | −180 | −420 | −1.200 | −2.400 | −4.200 |
| Pazarlama / CAC | −60 | −250 | −600 | −1.000 | −1.600 |
| **Dijital EBT** | **−166** | **+9** | **+93** | **+649** | **+1.770** |

> **Stratejik bulgu:** %2,5 take − %1,25 PSP = **%1,25 net take**. Dijital işin kârlılığı v0.5'te abonelik satırının eseriydi; ücretsiz çekirdek kararıyla gerçek tablo bu. Kaldıraçlar: (a) take rate artışı (komisyoncu %9 çıpasına karşı alan var), (b) PSP'nin kısmen alıcıya yansıtılması, (c) işlem yapan % artışı. Üçü de senaryo pivotu.

## 4. Çiftlik P&L — Base (₺K) — zaman uzlaştırması yapıldı

**v0.5 hatası:** Çiftlik geliri P&L'de Y1'de görünürken capex sermaye planında M24-29'daydı (~14× tutarsız dönemler). **v0.6:** Çiftlik-Y1 = **Şirket-Y3** (dikim M27-29 → ilk hasat geliri Y3 içinde).

| Kalem | Y1 | Y2 | Y3 (çiftlik-Y1) | Y4 (çiftlik-Y2) | Y5 (çiftlik-Y3) |
|---|---|---|---|---|---|
| İç mekan safran | — | — | 240 | 690 | 1.220 |
| Dış mekan safran (5.5d) | — | — | 682 | 1.600 | 1.975 |
| Lavanta (1.0d) | — | — | 9 | 130 | 305 |
| Ek farm maliyetleri | — | — | −89 | −154 | −200 |
| Farm CapEx | — | — | −798 | −320 | −10 |
| **Çiftlik EBT** | — | — | **+44** | **+1.946** | **+3.290** |

> ⚠️ **Açık risk:** v0.5'te çiftlik işçilik/girdi OpEx'i "Base OpEx" içinde eriyordu; ayrıştırma yapılmadı. Bu satır eklendiğinde çiftlik EBT'si düşecek — [[Research/Lavender]] ve [[Research/Tech-Indoor-Farming]] üzerinden işçilik detayı çıkarılacak (Açık Araştırma #6).

## 5. Konsolide — Base (₺K)

| | Y1 | Y2 | Y3 | Y4 | Y5 |
|---|---|---|---|---|---|
| Dijital EBT | −166 | +9 | +93 | +649 | +1.770 |
| Çiftlik EBT | — | — | +44 | +1.946 | +3.290 |
| **Konsolide EBT** | **−166** | **+9** | **+137** | **+2.595** | **+5.060** |
| Kurumlar vergisi (%25, zarar mahsuplu) | 0 | 0 | 0 | −644 | −1.265 |
| **Net kâr** | **−166** | **+9** | **+137** | **+1.951** | **+3.795** |
| **Kümülatif net** | −166 | −157 | −20 | +1.931 | +5.726 |

> **v0.5 → v0.6:** Y5 net **₺25,3M → ₺3,8M (Base)**. Fark üç düzeltmenin sonucu: (1) ücretsiz çekirdek → abonelik gelirinin buharlaşması, (2) PSP + vergi + gerçekçi CAC satırları, (3) çiftlik zaman kayması. Model bozulmadı; gerçekçileşti.

---

## 6. Senaryolar (Y5, ₺K)

| Pivot | Bear | Base | Bull |
|---|---|---|---|
| Take rate / PSP | %2,0 / %1,75 | %2,5 / %1,25 | %3,5 / %0,75 |
| İşlem yapan % | %8 | %15 | %20 |
| Kayıtlı çiftçi (M60) | 8.000 | 18.000 | 25.000 |
| Premium attach | %2 | %4 | %8 |
| Çiftlik | 1 yıl gecikme, −%30 verim | plan | plan |
| **Y5 dijital EBT** | **≈ −3.400** | **+1.770** | **≈ +21.000** |
| **Y5 konsolide EBT** | **≈ −2.200** | **+5.060** | **≈ +24.200** |

> **Ders:** v0.5'in ₺25,3M'i aslında **Bull senaryosuydu** ve tek nokta tahmini olarak sunulmuştu. v0.3→v0.5 sürüklenmesinin (22→23,8→25,3M hep yukarı) yapısal nedeni buydu. Bundan sonra her revizyon üç senaryoyu birden güncellemek zorunda.
> **Bear'ın ana mesajı:** düşük take + yüksek PSP kombinasyonunda dijital iş yapısal zarar eder → PSP sözleşmesi ve take rate testi fizibilitenin en kritik iki girdisi.

---

## 7. Sermaye Planı (v0.6 revize)

| Ay | Olay | Tutar | Kümülatif |
|---|---|---|---|
| M1 | Kurucu tasarruf | +₺1.000K | ₺1.000K |
| M1–22 | Dijital net nakit (driver bazlı: −166 Y1, ~breakeven Y2) | −₺170K | ₺830K |
| M23 | Kurucu enjeksiyonu | +₺1.000K | ₺1.830K |
| M24–26 | İç mekan kurulum | −₺650K | ₺1.180K |
| M27 | Dış mekan hazırlık | −₺80K | ₺1.100K |
| M28 | Korm + infra (TKDK %65 sonrası) | −₺250K | ₺850K |
| M29 | Lavanta + altyapı | −₺50K | ₺800K |
| **~M30** | **Tüm capex sonrası buffer** | | **~₺800K** |

> v0.5'teki "dijital M1-22 = ₺230K" satırı P&L'in Y1 OpEx'i (₺1.800K) ile 14× çelişiyordu; v0.6'da her ikisi de aynı driver tablosundan türediği için tutarlı. TKDK onayı gecikirse (Bear) M28 kalemi öz kaynaktan ₺460K'ya çıkar → buffer ₺590K'ya düşer.

## 8. Aylık Yakma (M23+, çiftlikli dönem)

| Kalem | ₺K/ay |
|---|---|
| Yaşam (kurucu + partner) | 100 |
| Dijital OpEx (Y2-3 seviyesi) | 35–100 |
| Farm OpEx (M28+) | 48 |
| **Toplam** | **183–248** |
| **Çıkış tetikleyici MRR+komisyon geliri** | **₺185K** (muhafazakâr eşik) |

---

## 9. Model Versiyon Geçmişi

| Versiyon | Tarih | Değişiklik | Y5 Net |
|---|---|---|---|
| v0.1 | Nis 2026 | Başlangıç (yanlış rakamlar) | — |
| v0.3 | May 2026 | TKDK %65, lavanta rebuild, OpEx | ₺22M |
| v0.4 | May 2026 | Indoor ₺600/g + optimal alloc. | ₺23,8M |
| v0.5 | May 2026 | 4-kat indoor → 20K korm | ₺25,3M |
| **v0.6** | **Tem 2026** | **Gelir modeli redesign (free core + tek tier AI premium ₺149 + komisyon); driver tablosu; LTV formül düzeltmesi; PSP+vergi+CAC satırları; Base/Bear/Bull; zaman uzlaştırma; dijital/çiftlik ayrıştırma** | **₺3,8M Base (−2,2M Bear / +24M Bull, EBT)** |

## 10. Açık Araştırma
- [ ] PSP teklifi: iyzico/PayTR pazaryeri (alt-üye işyeri) komisyon oranı + havale/kart mix → %1,25 varsayımını doğrula
- [ ] Premium ₺149 fiyat testi: early adopter dönemi (M1-6) sonunda ödeme dönüşümü ölçümü + kredi limitinin boyutu
- [ ] AI COGS kalibrasyonu: `ai_usage_tracking` gerçek token verisi → ₺30/ay varsayımı
- [ ] Take rate duyarlılığı saha testi: %2,5 vs %3,5 kabulü (komisyoncu %9 çıpası anlatısıyla)
- [ ] Çiftlik işçilik/girdi OpEx ayrıştırması ([[Research/Lavender]], [[Research/Tech-Indoor-Farming]])
- [ ] İç mekan bina kararı: kendi yapımız mı, kiralık mı?
- [ ] Farm ili: TKDK 42-il listesi teyidi (Safranbolu/Karabük?)
- [ ] Korm toptan teklifi (500kg+)
- [ ] Organik sertifikasyon M18 öncesi hazır mı?
- [ ] Referral programının kötüye kullanım koruması (sahte kayıtla 12 ay bedava premium riski) — ürün tarafına aktarılacak
