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
- [x] **P18-I — Basit Onboarding Tutorial (Çiftçi)** (2026-07-20) — bkz. detay aşağıda

### P16 — TÜM SERİ TAMAMLANDI ✅
(Detaylar önceki sürümlerde)

---

## 🔎 BENCHMARK RE-AUDIT (Temmuz 2026)
(Değişmedi)

---

## 🔴 ŞİMDİ — Temmuz 2026
(Değişmedi — bkz. önceki sürüm)

### Düşük öncelikli cila
(Değişmedi — bkz. önceki sürüm, `price_points` temizlik notu dahil)
- [ ] Lovable proje mesaj kuyruğu duraklatılmış (queue paused) olabilir — "queued but paused" hatasında Lovable editöründeki kuyruk durumu kontrol edilmeli.
- [ ] **[Bu turda bulundu] Kuyruk "silinip" tekrar mesaj gönderildiğinde, önceki kuyruktaki mesajın arka planda daha önce işlenmiş olabileceği unutulmamalı** — dosya kontrolü yaparken TÜM ilgili dosya adlarının alfabetik konumu doğru sayfada aranmalı (bu turda `OnboardingTour.tsx` "O" harfiyle başladığı için yanlış sayfa kontrol edilmiş, dosya var olduğu halde "yok" sanılmıştı — şans eseri zararsız çıktı, ama dikkat edilmeli).

---

## 🏗️ Lovable Build Sırası

> Sıradaki: **P18-G** (Ayarlar Yeniden Grupla + Mobil Bildirim Tablosu Düzeltmesi). Sonra F → E → C → D. **Faz C / P17** (Eylül). **Faz D** (Piyasa Zaman Serisi backend'i) P18'e paralel, isteğe bağlı.

---

## 🎨 P18 — ARAYÜZ YENİLEME (UI-Only, Backend/DB Değişikliği YOK)

> **Ortak Guardrail (her prompt'a eklenir):** *"Bu görev SADECE frontend/UI katmanını değiştirir. `supabase/migrations/`, `supabase/functions/`, veya herhangi bir DB şema/RLS/trigger dosyasına dokunma. Sadece `src/` altında çalış."* Her build sonrası diff'in yalnızca `src/`'i etkilediği + migration sayısının değişmediği doğrulanır.

### ✅ P18-0 / P18-0.2 — Denetimler *(Tamamlandı)*
(Değişmedi — bkz. önceki sürüm)

### Faz Sıralaması: A ✅ → B ✅ → H ✅ → I ✅ → **G (şimdi)** → F → E → C → D

---

### ✅ P18-A / P18-B / P18-H *(Tamamlandı — canlı doğrulandı)*
(Değişmedi — bkz. önceki sürüm)

### ✅ P18-I — Basit Onboarding Tutorial (Çiftçi) *(Tamamlandı — 2026-07-20, canlı doğrulandı)*
**Doğrulanan sonuç:**
- `src/components/hasat/OnboardingTour.tsx`: portal tabanlı spotlight tur component'i — hedef elemanın etrafında kesilmiş overlay (`box-shadow` spotlight tekniği), `getBoundingClientRect` ile ölçüm + resize/scroll dinleyicileri, viewport dışına taşmayı önleyen clamp, Escape ile atlama, `role="dialog"`/`aria-modal`/`aria-labelledby`.
- `src/lib/hasat/onboarding-tour.ts`: `FARMER_TOUR_STEPS` (5 adım: AIBox, chat input, WhatsApp, Vitrin sekmesi, Fiyatlar sekmesi) + `FARMER_TOUR_STORAGE_KEY`.
- `farmer.home.tsx`: `data-tour` attribute'ları eklendi, flag set değilse otomatik başlatma, `hasat:tour:restart` event dinleyicisi.
- `farmer.tsx`: hem sidebar hem mobil "Daha" sheet'inde `HelpCircle` ikonlu "Nasıl Çalışır?" butonu — flag'i sıfırlayıp turu yeniden açıyor (gerekirse önce `/farmer/home`'a yönlendirip sonra tetikliyor). Vitrin/Fiyatlar sekmelerinde `data-tour` attribute'ları doğru yerde.
- Token sistemi + 48px dokunma hedefi korunmuş. `tsgo` temiz.

---

### P18-G — Ayarlar Yeniden Grupla + Mobil Bildirim Tablosu Düzeltmesi *(sıradaki)*
(Değişmedi — bkz. önceki sürüm)

### P18-C, P18-D, P18-E, P18-F
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

(1-52 önceki sürümde — devam:)
53. **[Bu turda eklendi] Kuyrukla ilgili bir "silme/temizleme" işlemi sonrası dosya kontrolü yaparken, önceki (görünüşte başarısız) gönderimlerin arka planda işlenmiş olabileceği ihtimali göz önünde tutulmalı** — read-only doğrulama tek bir sayfa/aralıkla sınırlı kalmamalı, ilgili tüm dosya adları alfabetik konumlarına göre doğru şekilde aranmalı.

---

## 📌 Kararlar

(önceki tablo + eklenenler:)
| **P18-A/P18-B/P18-H/P18-I canlı doğrulandı (2026-07-20)** | Dördü de dosya okuma ile doğrulandı, gerçekten uygulanmış. |
| **Build aracı: Lovable'da kalındı (2026-07-20)** | Değişmedi. |

---

## 📋 Son Test Sonuçları

### P18-I (2026-07-20) ✅
| Kontrol | Sonuç |
|---|---|
| `OnboardingTour.tsx` — spotlight, clamp, Escape, a11y | ✅ Dosya okumasıyla doğrulandı |
| `onboarding-tour.ts` — 5 adım + storage key | ✅ |
| `farmer.home.tsx` — otomatik başlatma + restart event dinleyicisi | ✅ |
| `farmer.tsx` — sidebar + mobil "Nasıl Çalışır?" butonu, `data-tour` attributeları | ✅ |
| Token/48px kuralına uyum | ✅ |
| `tsgo` | ✅ Temiz |

### P18-H (2026-07-20) ✅
(Değişmedi — bkz. önceki sürüm)

### (Önceki tüm test sonuçları — değişmedi, önceki sürümlerde)
