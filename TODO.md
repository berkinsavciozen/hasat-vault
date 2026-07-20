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
> GTM Hedefi: **25 Ağustos 2026** · Günlük 1-2 saat · `[C]` = Claude ile · `[C Web]` = Web Claude (Lovable+Supabase+GitHub MCP+Hasat MCP) · `[CC]` = Claude Code (yerel repo)

---

## ✅ TAMAMLANDI

### Teknik Build
(Önceki maddeler değişmedi — bkz. önceki sürüm)
- [x] P18-0 Arayüz Denetimi (2026-07-20)
- [x] Faz A — Community Alıcı Sayfası + Gerçek Beğeni Butonu + "Hava Sor" İpucu Kaldırıldı (2026-07-20)
- [x] P18-0.2 — Ana Sayfa Retention + Fiyat Verisi Doğruluk Denetimi (2026-07-20)
- [x] **P18-A — Tema Katmanı + Ortak Component Seti** (2026-07-20) — bkz. detay aşağıda
- [x] **P18-B — Çiftçi Ana Sayfa: Sohbet-Önde Giriş Akışı** (2026-07-20) — bkz. detay aşağıda
- [x] **Alıcı menüsünden "Fiyatlar" gizlendi** (2026-07-20) — route korunarak, sadece nav'dan kaldırıldı

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

---

## 🏗️ Lovable/Claude Code Build Sırası

> Sıradaki: **P18-H** (Ana Sayfa Retention Katmanı) — bu fazdan itibaren Claude Code ile yapılıyor (bkz. aşağı, ayrı prompt). Sonra P18-I → G → F → E → C → D. **Faz C / P17** (Eylül, ayrıca değerlendirilecek). **Faz D** (Piyasa Zaman Serisi backend'i) P18'e paralel, isteğe bağlı.

---

## 🎨 P18 — ARAYÜZ YENİLEME (UI-Only, Backend/DB Değişikliği YOK)

> **Ortak Guardrail (her prompt'a eklenir):** *"Bu görev SADECE frontend/UI katmanını değiştirir. `supabase/migrations/`, `supabase/functions/`, veya herhangi bir DB şema/RLS/trigger dosyasına dokunma. Sadece `src/` altında çalış."* Her build sonrası diff'in yalnızca `src/`'i etkilediği + migration sayısının değişmediği doğrulanır.

### ✅ P18-0 / P18-0.2 — Denetimler *(Tamamlandı)*
(Değişmedi — bkz. önceki sürüm)

### Faz Sıralaması: A ✅ → B ✅ → **H (şimdi)** → I → G → F → E → C → D

---

### ✅ P18-A — Tema Katmanı + Ortak Component Seti *(Tamamlandı — 2026-07-20, canlı doğrulandı)*
**Doğrulanan sonuç:** `FarmerAIChat.tsx` FAB/mesaj balonu/gönder butonu `var(--gold)`'a taşındı, WhatsApp yeşili (`#25D366`) sadece gerçek `wa.me` linkinde kalmış. Header butonları (`History`/`Plus`/`X`) `min-h-[48px] min-w-[48px]`. `src/components/hasat/common/` altında `StatCard.tsx`, `SectionCard.tsx`, `PhotoListingCard.tsx`, `index.ts` oluşturuldu (henüz hiçbir sayfaya migrate edilmedi — sonraki fazlar kullanacak). `farmer.settings.notifs.tsx`'teki duplicate "Hasat Zamanı" olayı da bu turda temizlendi (plan dışı, küçük, gerçek bir bug'dı).

### ✅ P18-B — Çiftçi Ana Sayfa: Sohbet-Önde Giriş Akışı *(Tamamlandı — 2026-07-20, canlı doğrulandı)*
**Doğrulanan sonuç:** `farmer.home.tsx`'e "Hasadını yaz veya WhatsApp'tan gönder…" giriş çubuğu eklendi (boş prefill ile `hasat:ai-chat:open` event'ini tetikliyor + yanında gerçek `wa.me` linkine giden WhatsApp ikonu). "Hasat Kaydet" quick action'ı ve boş-durum CTA'sı artık `/farmer/journal/new` formuna değil, `prefill: "Hasat kaydı eklemek istiyorum: "` ile sohbete açılıyor. "Vitrine Ekle"/"Alıcı Bul" aynen kaldı.

**Yan not:** Aynı turda, Berkin'in isteği üzerine `buyer.tsx`'teki `tabs`/`mobileTabs` dizilerinden "Fiyatlar" girdisi kaldırıldı (route dosyası korunarak) — alıcılar artık Fiyatlar sayfasına menüden erişemiyor.

---

### 🔜 P18-H — Ana Sayfa Retention Katmanı (Çiftçi + Alıcı) *(sıradaki — Claude Code ile yapılacak)*
**Amaç:** Her iki personanın da uygulamayı açmak için günlük bir sebebi olsun.
**Kapsam:** Çiftçi home'a bekleyen teklif/sipariş kartı + geçen sezona göre kıyas; alıcı Discover'ın üstüne kişisel "Senin İçin" şeridi (bekleyen teklif, abonelik teslimatı, fiyat alarmı). Detay için aşağıdaki Claude Code prompt'una bkz.
**Dokunacağı dosyalar:** `farmer.home.tsx`, `buyer.discover.tsx`.

---

### P18-I, P18-C, P18-D, P18-E, P18-F, P18-G
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

## 📋 Lovable / Claude Code Prompt Yazma Kuralları

(1-48 önceki sürümde — devam:)
49. **[Bu turda eklendi] P18-H'den itibaren build aracı Lovable MCP'den Claude Code'a geçti** — Claude Code'a gönderilen prompt'lar, Lovable session'ının hiçbir sohbet geçmişine erişimi olmadığı için **tamamen kendi kendine yeterli** olmalı: stack, dosya yolları, ilgili hook isimleri, guardrail ve doğrulama adımları prompt'un içinde tekrar yazılmalı.
50. **[Bu turda eklendi] Lovable'ın `plan_mode=true` iken bile bazen direkt build ettiği doğrulandı (2 kez, P18-A ve buyer-menü+P18-B turlarında)** — bu, "plan mode inconsistency" olarak Berkin tarafından işaretlendi. Kural: bir mesajın işlenip işlenmediğinden şüphe duyulduğunda ASLA tekrar gönderilmez; önce ilgili dosyalar/diff read-only olarak kontrol edilir.

---

## 📌 Kararlar

(önceki tablo + eklenenler:)
| **P18-A/P18-B canlı doğrulandı (2026-07-20)** | İkisi de commit SHA + dosya okuma ile doğrulandı, gerçekten uygulanmış. |
| **Build aracı değişikliği (2026-07-20)** | P18-H'den itibaren Claude Code kullanılacak — Lovable credit tasarrufu ve daha stabil plan/build ayrımı için. Prompt'lar kendi kendine yeterli yazılacak. |
| **Mesaj tekrarı yasağı (2026-07-20)** | Bir Lovable mesajının gidip gitmediğinden şüphe duyulduğunda asla tekrar gönderilmeyecek; önce dosya/diff kontrolü yapılacak. |

---

## 📋 Son Test Sonuçları

### P18-A + P18-B (2026-07-20) ✅
| Kontrol | Sonuç |
|---|---|
| AI FAB/balon/gönder butonu gold'a taşındı mı | ✅ Dosya okumasıyla doğrulandı |
| WhatsApp yeşili sadece gerçek `wa.me` linkinde mi | ✅ |
| `common/` component'leri oluşturuldu mu | ✅ StatCard, SectionCard, PhotoListingCard, index.ts |
| 48px dokunma hedefleri (header butonları) | ✅ |
| Çiftçi home chat-first giriş çubuğu | ✅ Canlı doğrulandı |
| Alıcı menüsünden Fiyatlar gizlendi mi | ✅ tabs/mobileTabs/moreItems'da yok, route korunuyor |

### (Önceki tüm test sonuçları — değişmedi, önceki sürümlerde)
