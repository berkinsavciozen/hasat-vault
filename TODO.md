---
title: Hasat — Master Roadmap & Build Log
updated: 2026-07-20
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
- [x] P18-0 Arayüz Denetimi (2026-07-20)
- [x] Faz A — Community Alıcı Sayfası + Gerçek Beğeni Butonu + "Hava Sor" İpucu Kaldırıldı (2026-07-20)
- [x] P18-0.2 — Ana Sayfa Retention + Fiyat Verisi Doğruluk Denetimi (2026-07-20)
- [x] **P18-A — Tema Katmanı + Ortak Component Seti** (2026-07-20)
- [x] **P18-B — Çiftçi Ana Sayfa: Sohbet-Önde Giriş Akışı** (2026-07-20) + alıcı menüsünden "Fiyatlar" gizlendi
- [x] **P18-H — Ana Sayfa Retention Katmanı (Çiftçi + Alıcı)** (2026-07-20)
- [x] **P18-I — Basit Onboarding Tutorial (Çiftçi)** (2026-07-20)
- [x] **P18-G — Ayarlar Yeniden Grupla + Mobil Bildirim Tablosu Düzeltmesi** (2026-07-20) — bkz. detay aşağıda

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
- [ ] Lovable proje mesaj kuyruğu duraklatılmış (queue paused) olabilir — kontrol edilmeli.
- [ ] Kuyruk temizleme sonrası dosya kontrolünde tüm alfabetik aralık taranmalı (bkz. önceki not).
- [ ] **[Bu turda bulundu, kozmetik, düşük öncelik] `farmer.settings.tsx`'te "Çiftlik Bilgileri" accordion'u içindeki `Profil`/`Parsellerim`/`Sertifikalar` `Section`'ları kendi `border bg-card` kabuklarını koruyor** — Accordion'un dış kartıyla birlikte iç içe iki kart görünümü oluşabilir. Onaylanan plan bilerek Section'lara dokunmamıştı (kapsam: sadece wrapper), bu yüzden aktif bug değil — istenirse ayrı bir küçük cila turunda `Section` sarmalayıcıları accordion içinde kaldırılabilir.

---

## 🏗️ Lovable Build Sırası

> Sıradaki: **P18-F** (Piyasa & Performans Görünümü — Fiyatlar + Analitik). Sonra E → C → D. **Faz C / P17** (Eylül). **Faz D** (Piyasa Zaman Serisi backend'i) P18'e paralel, isteğe bağlı.

---

## 🎨 P18 — ARAYÜZ YENİLEME (UI-Only, Backend/DB Değişikliği YOK)

> **Ortak Guardrail (her prompt'a eklenir):** *"Bu görev SADECE frontend/UI katmanını değiştirir. `supabase/migrations/`, `supabase/functions/`, veya herhangi bir DB şema/RLS/trigger dosyasına dokunma. Sadece `src/` altında çalış."* Her build sonrası diff'in yalnızca `src/`'i etkilediği + migration sayısının değişmediği doğrulanır.

### ✅ P18-0 / P18-0.2 — Denetimler *(Tamamlandı)*
(Değişmedi — bkz. önceki sürüm)

### Faz Sıralaması: A ✅ → B ✅ → H ✅ → I ✅ → G ✅ → **F (şimdi)** → E → C → D

---

### ✅ P18-A / P18-B / P18-H / P18-I *(Tamamlandı — canlı doğrulandı)*
(Değişmedi — bkz. önceki sürüm)

### ✅ P18-G — Ayarlar Yeniden Grupla + Mobil Bildirim Tablosu Düzeltmesi *(Tamamlandı — 2026-07-20, canlı doğrulandı)*
**Doğrulanan sonuç:**
- `farmer.settings.tsx`: `Profil`, `Parsellerim`, `Sertifikalar` tek bir `Accordion` (`type="single" collapsible`, default value yok → kapalı başlıyor) içinde "Çiftlik Bilgileri" başlığı altında toplandı. `AccordionTrigger` `min-h-[48px]`. `Banka Bilgileri`, `AI Asistan`, `Bildirim Tercihleri` (link), `Verilerim`, `Hesap` sırası ve içerikleri değişmeden üst seviyede kaldı. Hiçbir state/hook/mutation mantığı değişmedi — sadece JSX yeniden gruplandı.
- `farmer.settings.notifs.tsx`: mobil kart düzeni P18-A'da zaten eklenmişti, bu turda tekrar kontrol edildi ve gereksinimi tam karşıladığı doğrulandı — değişiklik gerekmedi (tekrar/duplikasyon yapılmadı).
- `tsgo` temiz. Lovable planı doğru şekilde önce sadece plan aşamasında durdu, onay sonrası build etti — bu turda plan mode tutarlı çalıştı.

---

### P18-F — Piyasa & Performans Görünümü (Fiyatlar + Analitik) *(sıradaki)*
(Değişmedi — bkz. önceki sürüm, sahte veri yasağı notu dahil)

### P18-C, P18-D, P18-E
(Değişmedi — bkz. önceki sürüm)

---

## 🟠 FAZ D — Piyasa Zaman Serisi Backend'i
(Değişmedi — bkz. önceki sürüm)

---

## 🟡 AĞUSTOS 2026 — Soft Launch
(Değişmedi)

## 🟣 P17 — GÜVEN ÇEKİRDEĞİ
(Değişmedi)

## 🔵 SAFRAN SEZONU / ⬜ SONRAKI FAZLAR
(Değişmedi)

---

## 📋 Lovable Prompt Yazma Kuralları

(1-53 önceki sürümde — devam:)
54. **[Bu turda eklendi] Lovable bazen plan mode'da gerçekten sadece plan üretiyor (build etmiyor) — bu durumda plan içeriği dikkatlice okunup değerlendirilmeli, onay ayrı bir mesajla (`plan_mode=false`, "Onaylıyorum, uygula") verilmeli**, otomatik build'e güvenilmemeli.

---

## 📌 Kararlar

(önceki tablo + eklenenler:)
| **P18-A/B/H/I/G canlı doğrulandı (2026-07-20)** | Beşi de dosya okuma ile doğrulandı, gerçekten uygulanmış. |
| **Build aracı: Lovable'da kalındı (2026-07-20)** | Değişmedi. |

---

## 📋 Son Test Sonuçları

### P18-G (2026-07-20) ✅
| Kontrol | Sonuç |
|---|---|
| Accordion "Çiftlik Bilgileri" — kapalı başlıyor, 48px trigger | ✅ Dosya okumasıyla doğrulandı |
| Profil/Parsellerim/Sertifikalar içerik/mantık değişmemiş | ✅ |
| Diğer bölümlerin sırası korunmuş | ✅ |
| Mobil bildirim kartı (tekrar kontrol, duplikasyon yok) | ✅ |
| `tsgo` | ✅ Temiz |

### P18-I (2026-07-20) ✅
(Değişmedi — bkz. önceki sürüm)

### (Önceki tüm test sonuçları — değişmedi, önceki sürümlerde)
