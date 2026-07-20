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
- [x] **P18-H — Ana Sayfa Retention Katmanı (Çiftçi + Alıcı)** (2026-07-20) — bkz. detay aşağıda

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
- [ ] **[Bu turda bulundu] Lovable proje mesaj kuyruğu duraklatılmış (queue paused, reason: user) olabilir** — bir mesaj "queued but paused" hatası verirse, bu Lovable editöründeki kuyruk durumu kontrol edilmeli; kuyruk açılana kadar mesaj işlenmez.

---

## 🏗️ Lovable Build Sırası

> Sıradaki: **P18-I** (Basit Onboarding Tutorial — Çiftçi). Sonra G → F → E → C → D. Build aracı Lovable MCP olarak devam ediyor (Claude Code'a geçiş denendi, sonra vazgeçildi — bkz. Kararlar). **Faz C / P17** (Eylül). **Faz D** (Piyasa Zaman Serisi backend'i) P18'e paralel, isteğe bağlı.

---

## 🎨 P18 — ARAYÜZ YENİLEME (UI-Only, Backend/DB Değişikliği YOK)

> **Ortak Guardrail (her prompt'a eklenir):** *"Bu görev SADECE frontend/UI katmanını değiştirir. `supabase/migrations/`, `supabase/functions/`, veya herhangi bir DB şema/RLS/trigger dosyasına dokunma. Sadece `src/` altında çalış."* Her build sonrası diff'in yalnızca `src/`'i etkilediği + migration sayısının değişmediği doğrulanır.

### ✅ P18-0 / P18-0.2 — Denetimler *(Tamamlandı)*
(Değişmedi — bkz. önceki sürüm)

### Faz Sıralaması: A ✅ → B ✅ → H ✅ → **I (şimdi)** → G → F → E → C → D

---

### ✅ P18-A — Tema Katmanı + Ortak Component Seti *(Tamamlandı — 2026-07-20, canlı doğrulandı)*
(Değişmedi — bkz. önceki sürüm)

### ✅ P18-B — Çiftçi Ana Sayfa: Sohbet-Önde Giriş Akışı *(Tamamlandı — 2026-07-20, canlı doğrulandı)*
(Değişmedi — bkz. önceki sürüm, alıcı menüsü "Fiyatlar" gizleme notu dahil)

### ✅ P18-H — Ana Sayfa Retention Katmanı (Çiftçi + Alıcı) *(Tamamlandı — 2026-07-20, canlı doğrulandı)*
**Doğrulanan sonuç:**
- `farmer.home.tsx`: `AIBox`'ın üzerine "Bekleyen" kartı eklendi — `useFarmerOffers`/`useFarmerOrders`'tan yanıt bekleyen teklif + hazırlanan sipariş sayısı, her ikisi de sıfırsa kart tamamen gizli, `/farmer/orders`'a link. "Bu Sezon" kartı YTD gelire çevrildi + geçen yıl aynı takvim penceresine göre yüzde kıyası eklendi (geçmiş veri yoksa gösterilmiyor, sahte sayı yok).
- `buyer.discover.tsx`: kategorilerin üzerine "Senin İçin" yatay şeridi eklendi — bekleyen teklif (`useBuyerOffers`), en yakın abonelik teslimatı (`useMySubscriptions`), aktif fiyat alarmı (`usePriceAlerts`). Her kart veri yoksa gizli, şerit tamamen boşsa başlığıyla birlikte gizli. Fiyat alarmı kartı akıllıca `/buyer/reports`'a link veriyor, gizlenen `/buyer/prices`'a değil.
- Token sistemi (`--saffron`/`--gold`/`--sage`/`--hred`) ve 48px dokunma hedefi korunmuş. `tsgo` temiz.

---

### P18-I — Basit Onboarding Tutorial (Çiftçi) *(sıradaki)*
(Değişmedi — bkz. önceki sürüm)

### P18-C, P18-D, P18-E, P18-F, P18-G
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

(1-50 önceki sürümde — devam:)
51. **[Bu turda eklendi] Bir Lovable mesajı "queued but paused" hatası verirse, bu bir işlem hatası değil, editördeki kuyruğun kullanıcı tarafından duraklatılmış olması demektir** — kuyruk açılana kadar beklenmeli, tekrar göndermek yeni bir kuyruk pozisyonu yaratmaktan başka bir şey yapmaz.
52. **[Bu turda eklendi] Build aracı kararları (Lovable ↔ Claude Code) geri alınabilir** — Berkin bir turda Claude Code'a geçmeye karar verip sonraki turda vazgeçebilir; TODO'daki "build aracı" notu her turda güncel karara göre düzeltilmeli, önceki kararın izini bırakmamalı.

---

## 📌 Kararlar

(önceki tablo + eklenenler:)
| **P18-A/P18-B canlı doğrulandı (2026-07-20)** | İkisi de commit SHA + dosya okuma ile doğrulandı, gerçekten uygulanmış. |
| **Build aracı: Lovable'da kalındı (2026-07-20)** | Claude Code'a geçiş bir turda planlandı, sonraki turda Berkin vazgeçti — P18-H ve sonrası Lovable MCP ile devam ediyor. |
| **Mesaj tekrarı yasağı (2026-07-20)** | Bir Lovable mesajının gidip gitmediğinden şüphe duyulduğunda asla tekrar gönderilmeyecek; önce dosya/diff kontrolü yapılacak. Kuyruk duraklatılmışsa (queue paused) bu ayrı bir durum — kullanıcı editörden açmalı. |

---

## 📋 Son Test Sonuçları

### P18-H (2026-07-20) ✅
| Kontrol | Sonuç |
|---|---|
| Çiftçi "Bekleyen" kartı (teklif+sipariş, sıfırsa gizli) | ✅ Dosya okumasıyla doğrulandı |
| Çiftçi YTD gelir + YoY kıyası (veri yoksa gizli) | ✅ |
| Alıcı "Senin İçin" şeridi (teklif/abonelik/alarm) | ✅ |
| Fiyat alarmı kartı kırık linke düşmüyor (`/buyer/reports`'a gidiyor) | ✅ |
| Token/48px kuralına uyum | ✅ |
| `tsgo` | ✅ Temiz |

### P18-A + P18-B (2026-07-20) ✅
(Değişmedi — bkz. önceki sürüm)

### (Önceki tüm test sonuçları — değişmedi, önceki sürümlerde)
