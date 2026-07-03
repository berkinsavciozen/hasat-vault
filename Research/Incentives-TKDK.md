---
title: Hasat — Teşvik & TKDK Araştırması
updated: 2026-06-30
tags: [hasat, research, tkdk, incentives]
---

# Teşvik & TKDK Araştırması
> Progressive not — yeni araştırma her zaman yeni `## [Tarih] — [Konu]` başlığıyla alta eklenir.

---

## Nisan 2026 — TKDK Temel Araştırması

### TKDK Nedir?
- Tarım ve Kırsal Kalkınmayı Destekleme Kurumu
- AB IPARD fonlarını yönetiyor
- Başvuru penceresi: **yılda 1 kez, Ağustos–Eylül**
- Onay süresi: ~7 ay
- Ödeme modeli: Post-expense reimbursement (önce harca, sonra belgele, sonra al)

### Finansal Etki
| Senaryo | Korm maliyeti | Fark |
|---|---|---|
| TKDK ile (%65 hibe) | ~₺57,750 | — |
| TKDK olmadan | ₺165,000 | ₺107,250 fazla |
| **Tasarruf** | **~₺107K** | Bonus — farm TKDK'sız da viable |

Not: %75 hibe oranı organik sertifika + yenilenebilir enerji bileşeni eklenirse mümkün. Default model %65 (1994 doğum = genç çiftçi).

### Kritik Kural
⚠️ **Korm alımı TKDK sözleşme imzasından SONRA yapılmalı.** Ön satın alma = hibe hakkı kaybı.

### Zaman Çizelgesi
| Ay | Milestone |
|---|---|
| M15 | TKDK danışmanı bul |
| M16 | Proje yazımı başlar |
| M17 | Evraklar + proje tamamlandı |
| **M18 (Ağustos)** | **🔴 BAŞVURU YAP** — kaçırılırsa +12 ay |
| M25 | Onay bekleniyor |
| M26 | Sözleşme imzası |
| M27 | Korm sipariş (sözleşme sonrası) |
| M28 | Ekim |
| M29+ | Harcama belgesi ile ödeme talepleri |

### Gerekli Belgeler
- ÇKS (Çiftçi Kayıt Sistemi) kaydı ✅ zorunlu
- Tapu veya kira sözleşmesi (≥5 yıl)
- İş planı
- Organik tarım sertifikası (başvuru varsa %65→%75 artış)
- TKDK 42-il listesinde olma zorunluluğu ⚠️ doğrulanmadı

### Açık Risk
**R9:** Hedef il (Safranbolu/Karabük) 42-il listesinde mi? → **Doğrulanmadı. En kritik açık item.**

---

## Açık Araştırma
- [ ] TKDK 42-il listesini al, Karabük/Safranbolu'nun listede olduğunu doğrula
- [ ] Organik tarım sertifikasyon süreci: ne kadar sürer, ne kadar maliyetli?
- [ ] TARSİM mahsul sigortası: safran kapsama alınıyor mu?
- [ ] Coğrafi İşaret (Safranbolu safranı, 2024): kendi üretimine uygulanır mı?


---

## Temmuz 2026 — Journal'ın İkincil Kullanımı: Sertifikasyon Dokümantasyonu

Pain-point analizinde (`Research/Market.md`, Temmuz 2026) "TKDK/organik sertifikasyon bürokrasisi" düşük near-term addressability ile Phase 2+ olarak işaretlenmişti — Hasat şu an TKDK'ya kendi çiftliği için başvuran taraf, başkasına bu süreci çözen taraf değil.

Berkin'in eklediği nüans: generator/raporlama özelliğini şimdi build etmesek de, journal veri modelini bu ihtiyacı destekleyecek genişlikte tasarlamak neredeyse maliyetsiz — ve ileride hem kendi organik sertifikasyonumuz hem de diğer çiftçiler için "Hasat journal'ından tek tıkla ÜKD/sertifikasyon raporu" özelliğinin önünü açar. Şema tarafı: `Build/DB-Schema.md` → "Journal'ı Organik Sertifikasyon/ÜKD Muadili Olarak Tasarlamak".
