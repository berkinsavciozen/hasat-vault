---
title: Hasat — Master Roadmap & Build Log
updated: 2026-07-10
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
- [x] Supabase schema + seed data · Farmer/Buyer akışları (bireysel dahil) · AI özellikleri · Bildirim sistemi
- [x] P15 Teklif/Müzakere state machine + P15-Completion
- [x] Auth/profiles fix'leri (orphan cleanup, ON CONFLICT düzeltmesi)
- [x] Mobil responsiveness auditi · Landing page v2 (premium "güven altyapısı")
- [x] **Hasat MCP Server** — `hasat.lovable.app/mcp`, OAuth 2.1 (Supabase issuer), 16 tool
- [x] **Rekabet Hukuku Risk Denetimi + Düzeltmeleri**
- [x] **Kapsamlı RLS Güvenlik Denetimi + 4 Kritik/Orta Düzeltme**
- [x] **E2E-QA Dokümanı** (`Build/E2E-QA`) — Hasat MCP + Supabase MCP otomasyon mimarisi
- [x] **Landing Page Kopya + SEO + AI-SEO Denetimi** (bkz. detaylı bölüm aşağıda)
- [x] **Tedarik Zinciri Görsel Yeniden Tasarımı** (2 iterasyon — bkz. aşağıda)

### P16 — TÜM SERİ TAMAMLANDI ✅
(Detaylar önceki sürümlerde)

### Araştırma & Planlama
- [x] Landing page tasarım auditleri v1/v2 + **kopya/SEO denetimi (v3)**
- [x] Rekabet Kurumu risk denetimi
- [x] MCP server kullanım planı

---

## 🔴 ŞİMDİ — Temmuz 2026

### Yasal & Altyapı
- [ ] Şahıs şirketi kur · iyzico başvurusu · Alan adı araştır · Twilio Verify service
- [ ] **WhatsApp Business başvurusu → hemen başla**

### Düşük öncelikli cila
- [ ] `farmer.journal.new.tsx` "Önce bir parsel ekle" statik metni
- [ ] `formatCrop()` heuristik (crop_config'ten canlı okumuyor)
- [ ] Landing page'de çok fazla rol-seçim butonu (karışma riski, bir testte yaşandı)
- [ ] MCP yazma tool'larında aralıklı **"No approval received"** hatası — retry ile geçiyor
- [ ] `profiles`/`certifications` güvenlik düzeltmeleri için canlı exploit testi henüz yapılmadı (kod seviyesinde doğrulandı, düşük öncelik)
- [ ] **Lovable platform bug'ı (bilinen, bizim tarafımızdan düzeltilemez):** `plan_mode=false` (build moduna geçiş) sinyali bazen agent'ın kendi kod düzenleme tool'larına (code--line_replace vb.) yansımıyor — agent "hâlâ plan mode kısıtlaması var" diye fark edip bir sonraki turda aynı mesajın tekrar gönderilmesini istiyor. Bu oturumda 3+ kez yaşandı, her seferinde 1 ekstra mesajla çözüldü, veri kaybı/tutarsızlık yaratmadı. Lovable'a feedback olarak bildirildi (2026-07-10).

---

## 🏗️ Lovable Build Sırası — TAMAMLANDI ✅

> P16 serisi + mobil audit + bireysel alıcı + landing v2/v3 + MCP server + rekabet hukuku + güvenlik denetimi bitti. Sıradaki: **Ağustos E2E test kapsamının tamamlanması** (buyer-tarafı özellikler: sertifika, community reply, AI chat, bildirimler).

---

### ✅ Tedarik Zinciri Görsel Yeniden Tasarımı *(Tamamlandı — 2026-07-10, 2 iterasyon)*

Berkin'in canlı gözden geçirmesi sonucu iki ayrı sorun bulundu ve çözüldü:

**Sorun 1 — Kavramsal karışıklık:** "Geleneksel" ve "Hasat ile" toggle durumları iki farklı metriği aynı görsel dille (bar+%) gösteriyordu, kafa karıştırıyordu. **Çözüm:** Her iki durum da aynı metriğe ("alıcının ödediğin ₺100'ün üreticiye kalan payı") indirildi — Hasat ile artık sadece 2 düğüm (Çiftçi 100% → Alıcı 95%, "Hasat" kendi barı olan bir düğüm değil, bir mekanizma). Üstte büyük, animasyonlu **%14 vs %95** karşılaştırma kartı eklendi (`CountUp` component'i).

**Sorun 2 — Görsel "wow" eksikliği + teknik kırılganlık:** Toggle kaldırılıp iki zincir yan yana/üst üste gösterilmesi istendi, animasyonla güçlendirilmesi istendi.
- **1. deneme:** SVG + SMIL `<animate>` particle'ları (geleneksel zincirde küçülen/soluklaşan ₺ parçacıkları, Hasat'ta kesintisiz akan parçacıklar). **Tarayıcıda kırıldı** — kök neden: `preserveAspectRatio="none"` ile non-uniform stretch edilen viewBox içinde SMIL hesaplamaları React re-render'larla çakışıp particle'ları görünmez alt-piksel boyutuna küçültüyordu.
- **2. deneme (kalıcı çözüm):** SVG/SMIL tamamen kaldırılıp **sadece düz CSS**'e geçildi — her zincir tek, kompakt bir yatay çubuk (6 sütunlu grid'den ~%50 daha az düşey yer), segmentler kademeli soluklaşan renkle "erime" hissi veriyor, tek bir ₺ işaretli `<span>` marker `@keyframes` ile (`left` + `transform: scale`) geziniyor — geleneksel'de her sınırda küçülüyor, Hasat'ta sabit boyutta akıyor. `prefers-reduced-motion` desteği korundu.

**Öğrenilen ders:** SVG SMIL animasyonları, özellikle `preserveAspectRatio="none"` ile stretch edilmiş viewBox'larda ve React'in re-render döngüsüyle etkileşimde güvenilmez olabilir — karmaşık/çok-parçalı animasyon ihtiyaçlarında düz CSS `@keyframes` (tek element, `left`/`transform`) daha sağlam bir birinci tercih.

---

### ✅ Landing Page Kopya + SEO + AI-SEO Denetimi *(Tamamlandı — 2026-07-10)*

Kullanıcı talebiyle tüm landing page metni denetlendi: çiftçi/basit alıcı dilinde akıcı Türkçe, SEO avantajı, AI/LLM SEO (Generative Engine Optimization), ilk gelen ziyaretçiler için açıklayıcılık, ve "prototip/test" hissi vermeme.

**Dil/akıcılık düzeltmeleri:** Hero alt metni (abstrakt→somut), meta description (garip cümle yapısı düzeltildi), "Adil Fiyat" kart metni (istatistik jargonu→sade dil), AI chat balonu ("→" yerine "'den...'ye"), Nav CTA tutarlılığı ("girişi"→"-yim", Hero ile eşleşti).

**Ton değişikliği (dürüstlük korunarak):** "Örnek ilan"/"Örnek senaryo" gibi özür diler tondaki etiketler → "Gösterim" gibi kendinden emin, ürün-tanıtımı tonunda etiketlere çevrildi — hiçbir sahte veri sinyali kaldırılmadı, sadece dili güçlendirildi. Ayrıca aynı örnek kullanıcının (Ahmet/Safranbolu/Safran) 4 farklı bölümde tekrarlanması "tek örnek var, gerisi boş" hissi verdiği için çeşitlendirildi: Hero+Timeline Safran/Safranbolu kalırken, İzlenebilirlik mockup'ı Zeytin/Ayvalık'a, Çiftçi Hikayesi Lavanta/Isparta'ya çevrildi (gerçekçi mevsim tarihleriyle).

**Teknik SEO:** Title tag arama niyetine uygun hale getirildi ("Hasat \| Çiftçiden Doğrudan Alışveriş — İzlenebilir Tarım Pazarı"), `ValuePillars`'a eksik olan `<h2>` eklendi (H1→H3 boşluğu kapatıldı), boş `alt=""` etiketleri dolduruldu, **JSON-LD structured data** eklendi (Organization + FAQPage şemaları).

**AI/LLM SEO (Generative Engine Optimization):** Yeni bir **SSS (FAQ) bölümü** eklendi (6 soru-cevap: Hasat nedir, komisyon, izlenebilirlik hesaplama, bireysel alıcı, ödeme, çiftçi başlangıcı) — hem geleneksel SEO (featured snippet potansiyeli) hem AI arama modellerinin (ChatGPT, Perplexity vb.) net, alıntılanabilir gerçekler bulması için. FAQPage JSON-LD ile eşleştirildi.

**Tümü tek bir plan-mode onayı sonrası tek build'de uygulandı, tsgo temiz.**

---

### ✅ Kapsamlı RLS Güvenlik Denetimi + Düzeltmeleri *(Tamamlandı — 2026-07-10)*

(Detaylar önceki sürümde — 2 kritik + 2 orta bulgu, JWT impersonasyonuyla canlı exploit testi, S2 regresyon testi sıfır hatayla geçti)

---

### ✅ Rekabet Hukuku Risk Denetimi + Düzeltmeleri *(Tamamlandı — 2026-07-09)*

(Detaylar önceki sürümde)

---

### ✅ Hasat MCP Server Kurulumu + Genişletmesi *(Tamamlandı — 2026-07-09/10)*

(Detaylar önceki sürümde — 16 tool, OAuth 2.1, S1/S2 canlı test)

---

### ✅ Landing Page v2 — Görsel Düzeltmeler *(Tamamlandı — 2026-07-09)*

(Detaylar önceki sürümde — 8 madde)

---

## 🟡 AĞUSTOS 2026 — Soft Launch — ŞİMDİ BURADAYIZ

### E2E Test — devam ediyor
- [x] Farmer/Buyer onboarding E2E · Landing page E2E · Draft-İlan pipeline E2E
- [x] S1, S2, S3, S5, S7 — bkz. `Build/E2E-QA`
- [ ] Kalan kapsam: sertifika (foto upload), community+reply UI, AI chat kalitesi, bildirimler, referral tam akış — MCP tool'u olmayan alanlar

### Teknik Final
- [ ] hasat.lovable.app → custom domain yayını · iyzico canlı ödeme testi
- [x] P16-C tam havale e2e testi — S2 ile kapandı ✅

---

## 🔵 SAFRAN SEZONU / ⬜ SONRAKI FAZLAR

(değişmedi, bkz. önceki sürümler)

---

## 📋 Lovable Prompt Yazma Kuralları

(1-21 önceki sürümde — devam:)
22. **SVG SMIL animasyonlarına dikkat** — özellikle `preserveAspectRatio="none"` ile non-uniform stretch edilen viewBox'larda ve React re-render'larıyla etkileşimde güvenilmez/kırılgan olabiliyor. Karmaşık/çok-elemanlı animasyon ihtiyaçlarında önce düz CSS `@keyframes` (tek element, `left`/`transform`/`opacity`) denenmeli — daha sağlam, daha az bağımlılık.
23. **Landing/pazarlama metni yazdırırken** dürüstlük ilkesini (sahte veri/sosyal kanıt yok) korumakla "kendinden emin ton" arasında gerginlik olabilir — çözüm dürüstlük sinyalini kaldırmak değil, **tonunu** değiştirmek (özür diler dilden → ürün-tanıtımı diline).
24. **Bilinen Lovable platform hatası:** `plan_mode` API parametresi bazen agent'ın kod düzenleme tool'larının izin durumuna senkronize yansımıyor — build moduna geçiş mesajı bazen 2 kez gönderilmesi gerekiyor. Bizim tarafımızdan düzeltilemez, Lovable'a bildirildi.

---

## 📌 Kararlar

(önceki tablo + eklenenler:)
| **Landing page dil ilkesi** | Çiftçi/basit alıcı dilinde akıcı Türkçe; dürüstlük sinyalleri (Gösterim, örnek vb.) korunur ama kendinden emin/ürün-tanıtımı tonunda yazılır, özür diler gibi değil |
| **SEO/AI-SEO** | Title/meta/H2 hiyerarşisi + JSON-LD (Organization+FAQPage) + SSS bölümü — hem geleneksel arama hem AI arama modelleri için |
| **Animasyon tercihi** | Düz CSS `@keyframes` > SVG SMIL (kırılganlık riski nedeniyle) |

---

## 📋 Son Test Sonuçları

### Tedarik Zinciri Görsel Yeniden Tasarımı (2026-07-10) ✅ (2 iterasyon)
| Test | Sonuç |
|---|---|
| Metrik birleştirme (%14 vs %95 headline) | ✅ |
| SVG/SMIL particle denemesi | ❌ Kırıldı, kök neden teşhis edildi |
| CSS-only tek çubuk + marker yeniden tasarımı | ✅ Berkin tarafından canlı onaylandı ("Much better") |
| tsgo | ✅ (her iki denemede de) |

### Landing Page Kopya + SEO Denetimi (2026-07-10) ✅
| Test | Sonuç |
|---|---|
| Plan-mode onayı → tek build | ✅ Hiçbir madde atlanmadı (kod incelemesiyle doğrulandı) |
| Dil/ton/SEO/FAQ/JSON-LD (tüm madde) | ✅ |
| tsgo | ✅ |

### (Önceki tüm test sonuçları — RLS Güvenlik, Rekabet Hukuku, MCP Server, Landing v2, P16 serisi vb. — değişmedi, önceki sürümlerde)
