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
- [x] Supabase schema + seed data · Farmer/Buyer akışları · AI özellikleri · Bildirim sistemi
- [x] Hasat MCP Server (16→21 tool, tümü canlı test edildi) · Rekabet Hukuku Risk Denetimi · Kapsamlı RLS Güvenlik Denetimi
- [x] E2E-QA Dokümanı (`Build/E2E-QA`) · Tedarik Zinciri Görsel Yeniden Tasarımı
- [x] Fiyatlandırma Sistemi Tam Yeniden İnşası · 10+10 Mock Data (2 yıllık) · Alıcı Sayfaları Bug Düzeltmeleri
- [x] Detaylı E2E Devam Turu — 6 Yeni Senaryo (S8-S13) + 4 Kritik "Sahte Veri/Kırık Akış" Bug'ı
- [x] Tam Konuşma Denetimi + BENCHMARK.md + Finansal Model v0.6
- [x] Audit Takibi — MCP Rate Limiting + Tool Genişletmesi + Temizlik
- [x] Sertifika Foto + `useHasat` Taraması + Gelir Modeli Gating Denetimi + Premium Persistence Bug'ı
- [x] Premium Sayfası Yeniden İnşası + Referral Ödül Mekanizması + AI Kredi/Limit Sistemi + Çerez Kontrolü + 5 MCP Tool Canlı Testi
- [x] Bildirim Ayarları DB Bağlantısı + SMS Mimarisi Client'tan DB'ye Taşındı + AI Chat Kalite Denetimi (2026-07-19)
- [x] **P18-0 Arayüz Denetimi** (2026-07-20) — bkz. P18 bölümü aşağıda
- [x] **Faz A — Community Alıcı Sayfası + Gerçek Beğeni Butonu + "Hava Sor" İpucu Kaldırıldı** (2026-07-20) — bkz. detaylı bölüm aşağıda

### P16 — TÜM SERİ TAMAMLANDI ✅
(Detaylar önceki sürümlerde)

---

## 🔎 BENCHMARK RE-AUDIT (Temmuz 2026)

(Değişmedi — bkz. önceki sürüm)

---

## 🔴 ŞİMDİ — Temmuz 2026

### Yasal & Altyapı
- [ ] Şahıs şirketi kur · iyzico başvurusu · Alan adı araştır · Twilio Verify service · **WhatsApp Business başvurusu → hemen başla**
- [ ] **[Audit'te bulundu] Rekabet hukuku uzmanına gerçek danışma** — hâlâ açık, insan aksiyonu.
- [x] ~~Çerez onay banner'ı~~ — Tamamlandı, gerek yok.
- [ ] **[Benchmark re-audit] iyzico başvurusunda pazaryeri modeli + komisyon teklifi talebi** — hâlâ açık, insan aksiyonu.
- [x] ~~Gelir modeli kararlarının üründe doğrulanması~~ — Tamamlandı.
- [ ] **[Benchmark re-audit] Toplu "ilk N kullanıcıya otomatik 6 ay premium" feature-flag mekanizması** — hâlâ açık. *(P18'e dahil değil — bkz. P18 başındaki çapraz referans notu.)*
- [x] ~~Referral suistimal koruması~~ / ~~AI kredi/limit mekanizması~~ — Tamamlandı.

### Düşük öncelikli cila
- [ ] `farmer.journal.new.tsx` "Önce bir parsel ekle" statik metni · `formatCrop()` heuristik · Landing page'de çok fazla rol-seçim butonu
- [ ] MCP yazma tool'larında aralıklı "No approval received" hatası — retry ile geçiyor
- [ ] **Lovable platform bug'ı (bilinen, düzeltilemez):** `plan_mode=false` sinyali agent'ın kod tool'larına bazen yansımıyor.
- [ ] Storage bucket public/private ayarını her zaman canlı doğrula
- [ ] **[Audit'te bulundu] `crop_requests` tablosu için admin inceleme arayüzü** — Claude periyodik kontrol edecek (P17-E'ye kadar)
- [ ] **[Bu turda bulundu, düşük öncelik] `create_subscription` rol-temizliği eksik** — Faz B'de gönderildi, yanıt bekleniyor.
- [ ] **[Bu turda bulundu, kozmetik] `FarmerAIChat.tsx` header'ı "Premium • sınırsız sohbet"** — Faz B'de gönderildi, yanıt bekleniyor.
- [ ] **[Bu turda bulundu] `useHasat.setNotifPref` + store'daki `notifPrefs` slice'ı artık dead code** — Faz B'de gönderildi, yanıt bekleniyor.
- [x] ~~AI chat'te "hava sor" ipucu tamamen kurgusal~~ — **Tamamlandı (2026-07-20):** ipucu kaldırıldı, "fiyat, pazar önerisi, günlük kaydı veya ürün önerisi" ile değiştirildi.

---

## 🏗️ Lovable Build Sırası — TAMAMLANDI ✅

> Sıradaki: **Faz B** (gönderildi, yanıt bekleniyor) → **P18** (paralel yürüyebilir) → **Faz C / P17** (Eylül).

---

### ✅ Faz A — Community Alıcı Sayfası + Gerçek Beğeni + "Hava Sor" Kaldırıldı *(Tamamlandı — 2026-07-20)*

Ağustos E2E kapsamının resmi olarak kapanması için son 2 madde:

**1. Community reply UI denetimi — karma sonuç:** Alıcı tarafında topluluk sayfası **hiç yoktu** (RLS zaten rol-bağımsız izin veriyordu, sadece arayüz hiç yapılmamıştı — "sahte" değil, gerçekten eksik). Çiftçi tarafındaki reply akışı **tamamen gerçekti** (mutation, kalıcılık, moderasyon maskeleme hem post hem reply seviyesinde doğru). **🔴 9. bulgu:** "Beğen" butonu sahteydi — sadece local state, `community_post_likes` tablosu var ama hiç yazılmıyordu. **Düzeltme:** Alıcı community sayfası inşa edildi (paylaşılan `CommunityFeed` component'i factored out), beğeni butonu gerçek tabloya + `on_like_change` trigger'ı ile bakımlı `likes_count` kolonuna bağlandı. Canlı doğrulandı, test verisi temizlendi.

**2. "Hava sor" kararı — kaldırma:** Gerçek bir hava durumu kaynağı olmadığı için (8. bulgu, önceki turda tespit edildi) ipucu metni kaldırıldı, gerçek yeteneklere uygun metinle değiştirildi.

**Sonuç: Ağustos E2E kapsamı resmi olarak tamamlandı.**

---

### ✅ (Önceki tüm tamamlanan işler — Bildirim/SMS, Premium/Referral/AI Limit, Sertifika/useHasat/Gating, Audit Takibi, Detaylı E2E, Alıcı Sayfaları, Mock Data, Fiyatlandırma, Tedarik Zinciri, Landing, RLS, Rekabet Hukuku, MCP Server, Benchmark Re-Audit, P16 serisi)

(Detaylar önceki sürümlerde — değişmedi)

---

## 🧭 SIRADAKİ FAZ PLANI (2026-07-20 itibarıyla)

**Faz A ✅ tamamlandı.** **Faz B** (aşağıdaki 4 madde) gönderildi, Lovable'ın yanıtı bekleniyor:
1. SMS kapsamını genişletme (`offer_accepted_sms`, `payment_confirmed_sms`)
2. `create_subscription` rol-temizliği CHECK'i
3. `useHasat` dead code temizliği
4. Kozmetik "Premium • sınırsız" düzeltmesi

*(5. madde — toplu premium feature-flag — bilinçli olarak Faz B'nin bu turundan çıkarıldı, "ŞİMDİ" bölümünde kendi satırında açık kalıyor.)*

**Faz C (P17, Eylül)** hâlâ Faz A/B bitmeden başlamıyor. **P18 (Arayüz Yenileme)** üçüyle paralel yürüyebilir (UI-only, backend'e dokunmuyor).

---

## 🎨 P18 — ARAYÜZ YENİLEME (UI-Only, Backend/DB Değişikliği YOK)

> **Çapraz referans (2026-07-20):** Faz B'nin 5. maddesi ("toplu ilk N kullanıcıya otomatik 6 ay premium" feature-flag mekanizması) **bilinçli olarak bu seriye dahil edilmedi** — doğası itibarıyla backend/DB tasarımı gerektiriyor (toplu `activatePremium()` çağrısı, "ilk N" tanımı vb.), bu da P18'in kendi "SADECE `src/`" guardrail'ine aykırı olurdu. Bu madde **"ŞİMDİ — Temmuz 2026"** bölümünde kendi satırında açık kalmaya devam ediyor, Faz B'nin diğer maddeleriyle birlikte ya da P18 bittiğinde ayrıca ele alınacak.
>
> **Tetikleyici:** Berkin'in 3 kriterli arayüz modernizasyonu talebi (persona ilgi alanları, persona'nın en sık kullandığı uygulamalar + eylem alma UX sistemleri, AI+WhatsApp-native girdi/çıktı) + ekran görüntüsü üzerinden bulunan somut UX sorunları.
> **Ortak Guardrail (her prompt'a eklenir):** *"Bu görev SADECE frontend/UI katmanını değiştirir. `supabase/migrations/`, `supabase/functions/`, veya herhangi bir DB şema/RLS/trigger dosyasına dokunma. Sadece `src/` altında çalış."* Her build sonrası Claude, diff'in yalnızca `src/`'i etkilediğini + migration sayısının değişmediğini Supabase MCP ile doğrular.

### ✅ P18-0 — Denetim *(Tamamlandı — 2026-07-20, Lovable kredisi kullanmadan)*
Mevcut dosya ağacı, tema katmanı (`styles.css`), farmer shell (`farmer.tsx`), AI chat (`FarmerAIChat.tsx`), Vitrin (`farmer.storefront.tsx`), public storefront (`s.$slug.tsx`), Discover (`buyer.discover.tsx`), Reports (`buyer.reports.tsx`), Fiyatlar (`PricesPageBody.tsx`), Analitik (`farmer.analytics.tsx`), Ayarlar (`farmer.settings.tsx`, `farmer.settings.notifs.tsx`) okundu. Bulgular:
- WhatsApp entry noktası zaten var (`wa.me` linki + `whatsapp-ai-webhook` stub) ama gömülü/küçük.
- AI chat deeplink event sistemi (`hasat:ai-chat:open`) zaten var — quick action'lar bunu kullanmıyor.
- `Sparkline.tsx` component'i **var ama hiçbir yerde kullanılmıyor**.
- `price_alerts` (watchlist) tablosu + hook'ları **var ama UI'da hiç yüzeyde değil**.
- **Mobil bug bulundu:** `farmer.settings.notifs.tsx`'teki 3 kanal kolonu (`w-20` × 3 + gap) 375px ekranda "Olay" etiketini sıkıştırıyor — masaüstü tablo mantığı mobilde bozuluyor.
- "Bildirim Tercihleri" butonu kodda **çalışıyor** (route + hook'lar doğru bağlı) — Berkin'in gördüğü sorun muhtemelen Lovable edit-mode overlay'i, canlı/preview URL'de doğrulanmalı.

### Faz Sıralaması (önerilen): A → B → G → F → E → C → D

---

### P18-A — Tema Katmanı + Ortak Component Seti
**Amaç:** Lavender AI rengini saffron/gold ailesine taşı, WhatsApp yeşilini (`#25D366`) sadece gerçek `wa.me` linklerine ayır, 48px dokunma hedefleri, ve farmer+buyer arasında paylaşılan bir component seti.
**Dokunacağı dosyalar:** `src/styles.css`, `src/components/hasat/ai-chat/FarmerAIChat.tsx`, yeni `src/components/hasat/common/` klasörü.

---

### P18-B — Çiftçi Ana Sayfa: Sohbet-Önde Giriş Akışı
**Amaç:** Quick action'ları form sayfalarına değil, mevcut AI chat deeplink event'ine (`hasat:ai-chat:open`) yönlendir; ana sayfaya kalıcı "Hasadını yaz…" giriş çubuğu ekle.
**Dokunacağı dosyalar:** `src/routes/farmer.home.tsx`, `FarmerAIChat.tsx`.

---

### P18-C — Sipariş Durumu & WhatsApp Deeplink'leri
**Amaç:** Adım timeline'ı öne çıkar, her siparişte "WhatsApp'tan takip et" deeplink'i.
**Dokunacağı dosyalar:** `farmer.orders.index.tsx`, `buyer.orders.$orderId.tsx`, `OrderTimeline.tsx`.

---

### P18-D — Alıcı Raporları/Siparişleri: Tablo Hissini Kart Hissine Çevir
**Amaç:** "Tedarikçi Güveni" ve "Son Siparişler" bölümlerini kart estetiğine çevir.
**Dokunacağı dosyalar:** `buyer.reports.tsx`.

---

### P18-E — Vitrin Kartı: Paylaşılabilir Önizleme
**Amaç:** `/s/$slug` genel vitrini paylaşılabilir hale getir, çiftçinin kendi Vitrin sayfasına canlı önizleme ekle.
**Dokunacağı dosyalar:** `s.$slug.tsx`, `farmer.storefront.tsx`.

---

### P18-F — Piyasa & Performans Görünümü (Fiyatlar + Analitik)
**Amaç:** Fiyatlar ve Analitik sayfalarını borsa/finans hissiyle yeniden tasarla: arama/filtre, sparkline, watchlist.
**Dokunacağı dosyalar:** `PricesPageBody.tsx`, `farmer.analytics.tsx`.

---

### P18-G — Ayarlar Yeniden Grupla + Mobil Bildirim Tablosu Düzeltmesi
**Amaç:** Profil+Parsellerim+Sertifikalar → tek accordion "Çiftlik Bilgileri"; mobil bildirim tablosu düzeltmesi.
**Dokunacağı dosyalar:** `farmer.settings.tsx`.

---

## 🟡 AĞUSTOS 2026 — Soft Launch — ŞİMDİ BURADAYIZ

### E2E Test
- [x] S1-S17 (tüm senaryolar) · Fiyatlandırma/Keşfet/Mesajlar/Raporlar/Abonelikler/Üretici Detay/Fotoğraflar · Sertifika/useHasat/Gating · Premium/Referral/AI Limit · Bildirim ayarları + SMS mimarisi · **Community + hava sor** — **AĞUSTOS E2E KAPSAMI RESMİ OLARAK TAMAMLANDI ✅**

### Teknik Final
- [ ] hasat.lovable.app → custom domain yayını · iyzico canlı ödeme testi
- [x] P16-C tam havale e2e testi — kapandı ✅

---

## 🟣 P17 — GÜVEN ÇEKİRDEĞİ *(Eylül 2026 — Faz C)*

(Değişmedi — bkz. önceki sürüm: P17-A/B/C/D/E/F/G)

---

## 🔵 SAFRAN SEZONU / ⬜ SONRAKI FAZLAR

(Değişmedi — bkz. önceki sürüm)

---

## 📋 Lovable Prompt Yazma Kuralları

(1-45 önceki sürümde — devam:)
46. **[Bu turda eklendi] Bir faz planı yapıldığında, bir sonraki fazın bir maddesinin başka bir seriye "sığmayabileceği" (örn. backend-gerektiren bir madde, UI-only bir seriye) ihtimalini erkenden kontrol et** — çapraz referans notuyla bunu açıkça belirtmek, maddenin iki yerde de kaybolmasını önler.

---

## 📌 Kararlar

(önceki tablo + eklenenler:)
| **Faz A tamamlandı (2026-07-20)** | Ağustos E2E kapsamı resmi olarak kapandı — community (alıcı sayfası + gerçek beğeni), hava sor kaldırıldı. |
| **P18 ↔ Faz B ayrımı (2026-07-20)** | Toplu premium feature-flag mekanizması, backend gerektirdiği için P18'e (UI-only) dahil edilmedi, kendi satırında açık kaldı. |

---

## 📋 Son Test Sonuçları

### Faz A — Community + Hava Sor (2026-07-20) ✅
| Kontrol | Sonuç |
|---|---|
| Alıcı community sayfası (`CommunityFeed` paylaşılan component) | ✅ tsgo temiz |
| Gerçek beğeni butonu (`community_post_likes` + `on_like_change` trigger + `likes_count`) | ✅ Canlı doğrulandı |
| "Hava sor" ipucu kaldırıldı | ✅ tsgo temiz |

### P18-0 Arayüz Denetimi (2026-07-20) ✅
(Değişmedi — bkz. yukarı)

### (Önceki tüm test sonuçları — Bildirim/SMS, Premium/Referral/AI Limit, Sertifika/useHasat/Gating, Audit Takibi, Detaylı E2E, Alıcı Sayfaları, Mock Data, Fiyatlandırma, Tedarik Zinciri, Landing, RLS, Rekabet Hukuku, MCP Server, Benchmark Re-Audit, P16 serisi — değişmedi, önceki sürümlerde)
