---
title: Hasat — Master Roadmap & Build Log
updated: 2026-07-19
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
- [x] **Premium Sayfası Yeniden İnşası ("Elite" kaldırıldı) + Referral Ödül Mekanizması (canlı test) + AI Kredi/Limit Sistemi + UX + Çerez Kontrolü + 5 MCP Tool Canlı Testi** (2026-07-19) — bkz. detaylı bölüm aşağıda

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
- [x] ~~**Çerez onay banner'ı — kontrol edilmeli**~~ — **Tamamlandı (2026-07-19), gerek yok:** Kod taramasıyla doğrulandı — hiç çerez yok (Supabase oturumu `localStorage`'da), hiç 3. parti analytics/tracking yok. Dormant/kullanılmayan bir shadcn Sidebar cookie'si var ama hiçbir yerde render edilmiyor. Banner eklenmedi, `/privacy`'deki "çerez politikası" metni yalan değil (henüz sadece hiç uygulanmıyor).
- [ ] **[Benchmark re-audit] iyzico başvurusunda pazaryeri modeli + komisyon teklifi talebi** — hâlâ açık, insan aksiyonu.
- [x] ~~Gelir modeli kararlarının üründe doğrulanması (a) ve (b)~~ — Tamamlandı (önceki tur).
- [x] ~~**[Benchmark re-audit, (c)] "İlk 6 ay premium ücretsiz" feature-flag** — TEK KULLANICILIK persistence sorunu çözüldü~~ — bu artık daha geniş bir mekanizmanın (referral ödülü) parçası oldu, toplu feature-flag hâlâ ayrı bir görev olarak açık kalıyor (aşağıya taşındı).
- [ ] **[Benchmark re-audit] Toplu "ilk N kullanıcıya otomatik 6 ay premium" feature-flag mekanizması** — `activatePremium()` + referral sistemi artık var, ama otomatik/toplu tetikleme (örn. "ilk 500 kayıt") henüz tasarlanmadı.
- [x] ~~**[Benchmark re-audit] Referral suistimal koruması**~~ — **Tamamlandı (2026-07-19)**, bkz. detaylı bölüm aşağıda.
- [x] ~~**[Benchmark re-audit] AI kredi/limit mekanizması tasarımı**~~ — **Tamamlandı (2026-07-19)**, bkz. detaylı bölüm aşağıda.

### Düşük öncelikli cila
- [ ] `farmer.journal.new.tsx` "Önce bir parsel ekle" statik metni · `formatCrop()` heuristik · Landing page'de çok fazla rol-seçim butonu
- [ ] MCP yazma tool'larında aralıklı "No approval received" hatası — retry ile geçiyor
- [ ] **Lovable platform bug'ı (bilinen, düzeltilemez):** `plan_mode=false` sinyali agent'ın kod tool'larına bazen yansımıyor.
- [ ] Storage bucket public/private ayarını her zaman canlı doğrula
- [x] ~~`useHasat` (Zustand mock store) referansları için tüm codebase'de tarama~~ — Tamamlandı, temiz
- [ ] **[Audit'te bulundu] `crop_requests` tablosu için admin inceleme arayüzü** — Claude periyodik kontrol edecek (P17-E'ye kadar)
- [ ] `notifPrefs` sadece localStorage'da tutuluyor — cihazlar arası senkron değil, düşük öncelik
- [ ] **[Bu turda bulundu, düşük öncelik] `create_subscription` rol-temizliği eksik** — bir `farmer` rolündeki hesap teknik olarak "alıcı" gibi abonelik oluşturabiliyor (canlı MCP testinde bulundu). Güvenlik açığı değil (RLS her tarafın kendi verisini zaten koruyor), ama `harvest_subscriptions.buyer_id`'nin gerçekten `role='buyer'` bir profile ait olduğunu zorlayan bir CHECK/trigger eklenebilir.
- [ ] **[Bu turda bulundu, kozmetik] `FarmerAIChat.tsx` header'ı hâlâ "Premium • sınırsız sohbet" yazıyor** — artık 500/ay tavanı olduğu için %100 doğru değil (pratikte kimse ulaşmaz ama "sınırsız" ifadesi teknik olarak yanlış). "Premium • ayda 500 mesaj" gibi bir düzeltme düşünülebilir.

---

## 🏗️ Lovable Build Sırası — TAMAMLANDI ✅

> Sıradaki: **Ağustos E2E test kapsamının tamamlanması** (community reply UI, AI chat kalitesi, bildirimler, referral tam akış — artık ödül mekanizması da dahil) → sonrasında **P17 Güven Çekirdeği**.

---

### ✅ Premium Sayfası + Referral Ödül Mekanizması + AI Limit/UX + Çerez Kontrolü + MCP Tool Canlı Testi *(Tamamlandı — 2026-07-19)*

Bugünün son turu — TODO'daki 6 açık maddeden 5'ini kapattı, hepsi canlı doğrulandı:

**1. Premium sayfası tamamen yeniden inşa edildi (🔴 6. "sahte veri" bulgusu):** Ekran görüntüsüyle fark edildi — `farmer.premium.tsx` **kurgusal bir 3-katmanlı yapı** (Ücretsiz/Premium ₺299/**Elite ₺799**) gösteriyordu; `user_tier` enum'ında sadece `free`/`premium` var, **"Elite" veritabanında hiç yok**. Ayrıca "sadece Premium'da" diye pazarlanan özellikler (fiyat alarmı, gelişmiş analitik, hasat aboneliği, "3 ürün listeleme limiti") **zaten herkese açıktı** — sayfa var olmayan kısıtlamalar reklam ediyordu. **Düzeltme:** Sayfa tek gerçek farkı (AI sohbet kotası) gösterecek şekilde yeniden yazıldı, ₺149/ay'a düzeltildi, `billing.tsx`'teki "elite" plan dalı kaldırıldı. Alıcı tarafında eşdeğer bir sayfa yokmuş.

**2. Referral ödül mekanizması — sıfırdan inşa edildi + canlı test edildi:** `referred_by` kaydediliyordu ama hiçbir ödül vermiyordu. Yeni: `profiles.premium_until` kolonu + `referral_qualifications` tablosu + bir teklif `payment_status='paid'` olduğunda tetiklenen trigger. **Anti-abuse:** ödül sadece **gerçek bir ödemeyle** tetikleniyor (kayıt yetmiyor), her yönlendirilen kullanıcı **sadece bir kez** sayılıyor (UNIQUE constraint), her 3 nitelikli referansta `premium_until`'a 12 ay ekleniyor (üst üste büyüyebilir). **Claude tarafından uçtan uca canlı test edildi:** 3 sahte alıcı + Ahmet referrer olarak, 1. ve 2. ödemede hâlâ `free`, 3.'te `tier=premium, premium_until=+12 ay` — tam doğru. Mükerrer koruması da test edildi (aynı alıcının 2. siparişi yeni ödül üretmedi). Tüm test verisi temizlendi.

**3. AI kredi/limit mekanizması — Premium'a 500 mesaj/ay tavanı + UX redesign:** `can_send_ai_message()` RPC'sine premium için 500/ay tavanı eklendi (yer tutucu, gerçek COGS verisiyle kalibre edilecek). **UX tamamen yeniden tasarlandı** (Hasat'ın "güven" markasına uygun, dürüst/ön-uyarılı): 45/50 mesajda silinebilir yumuşak uyarı banner'ı; sert limitte **taslak mesaj artık kaybolmuyor** (kopyalanabilir kalıyor), "kota X tarihinde sıfırlanacak" bilgisi + gerçek fiyatla (₺149/ay) net Premium CTA'sı + acil durumlar için "insan desteği" WhatsApp linki (AI bypass'ı değil). Premium'un kendi (nadir) limitine ulaşması için de suçlayıcı olmayan, güvence verici bir panel eklendi.

**4. Çerez banner'ı kontrolü — gerek yok:** Kod taramasıyla doğrulandı, hiç çerez/analytics yok (yukarı bkz.).

**5. 5 yeni MCP tool'u (topluluk + abonelik) canlı test edildi:** `create_community_post` (normal + moderasyon-flag'li gönderi, **ilk kez gerçek kullanıcı JWT'siyle** doğrulandı — önceki test service-role ile yapılmıştı), `list_community_posts`, `create_subscription`, `list_my_subscriptions`, `cancel_subscription` — hepsi ✅. Küçük bir bulgu (rol-temizliği eksikliği) not edildi, düşük öncelik.

**Toplam maliyet:** Premium rebuild 1.9 + AI limit/UX 3.5 + çerez kontrolü 1 = **6.4 kredi** (referral mekanizması dahil değil, o ayrı bir mesajda geldi — plan-mode dahil ücretsiz sayıldı).

---

### ✅ (Önceki tüm tamamlanan işler — Sertifika/useHasat/Gating denetimi, Audit Takibi, Detaylı E2E, Alıcı Sayfaları, Mock Data, Fiyatlandırma, Tedarik Zinciri, Landing, RLS, Rekabet Hukuku, MCP Server, Benchmark Re-Audit, P16 serisi)

(Detaylar önceki sürümlerde — değişmedi)

---

## 🟡 AĞUSTOS 2026 — Soft Launch — ŞİMDİ BURADAYIZ

### E2E Test — devam ediyor
- [x] Farmer/Buyer onboarding · Landing · Draft-İlan · S1-S13 · Fiyatlandırma/Keşfet/Mesajlar/Raporlar/Abonelikler/Üretici Detay/Fotoğraflar · Sertifika/useHasat/Gating · **Premium sayfası, Referral ödülü, AI limit/UX, Çerez, 5 MCP tool** — hepsi ✅
- [ ] Kalan kapsam: community+reply UI, AI chat kalitesi (sohbet cevap kalitesi, journal entegrasyonu), bildirimler
- [x] ~~Referral tam akış~~ — **Tamamlandı** (ödül mekanizması artık gerçek ve test edilmiş)

### Teknik Final
- [ ] hasat.lovable.app → custom domain yayını · iyzico canlı ödeme testi
- [x] P16-C tam havale e2e testi — kapandı ✅

---

## 🟣 P17 — GÜVEN ÇEKİRDEĞİ *(Eylül 2026)*

(Değişmedi — bkz. önceki sürüm: P17-A/B/C/D/E/F/G)

---

## 🔵 SAFRAN SEZONU / ⬜ SONRAKI FAZLAR

(Değişmedi — bkz. önceki sürüm)

---

## 📋 Lovable Prompt Yazma Kuralları

(1-38 önceki sürümde — devam:)
39. **[Bu turda eklendi] Pazarlama/fiyatlandırma sayfaları da "sahte veri" denetiminin kapsamına girer** — sadece uygulama içi işlevsel sayfalar değil, statik görünen bir fiyat/plan sayfası da (bu turdaki "Elite" örneği gibi) tamamen kurgusal olabilir. Fiyatlandırma sayfalarını da düzenli olarak gerçek şema/gating mantığıyla karşılaştır.
40. **[Bu turda eklendi] Yeni bir ödül/mekanizma tasarlarken, anti-abuse'u mekanizmayla BİRLİKTE tasarla, sonradan eklemeye çalışma** — referral ödülü hiç var olmadığı için "önce ödülü kur sonra koru" değil, ikisi aynı promptta birlikte inşa edildi; bu, "önce çalışan bir şey yap sonra güvenliğini düşün" tuzağına düşmemek için iyi bir örnek oldu.
41. **[Bu turda eklendi] Gerçek kullanıcı JWT'siyle test etmek, service-role testinden farklı ve tamamlayıcıdır** — MCP tool'ları üzerinden gerçek bir kullanıcı olarak test etmek (bu turdaki topluluk moderasyonu testi gibi), sadece service-role ile yapılan testlerin kaçırabileceği auth.uid()-bağımlı davranışları da doğrular.

---

## 📌 Kararlar

(önceki tablo + eklenenler:)
| **Premium fiyatlandırma (2026-07-19)** | Tek katman, ₺149/ay, tek gerçek fark AI sohbet kotası (50 ücretsiz / 500 premium, ay sonunda sıfırlanır). "Elite" katmanı ve "3 ürün limiti" gibi kurgusal kısıtlamalar tamamen kaldırıldı. |
| **Referral ödülü (2026-07-19)** | 3 **nitelikli** (gerçek ödemesi tamamlanmış) referans = 12 ay premium, üst üste büyür. Sahte kayıt ödül üretemez, mükerrer sayım engellenir. |
| **Çerez politikası (2026-07-19)** | Şu an hiç çerez/analytics yok, banner gerekmiyor — ileride gerçek analytics eklenirse bu karar yeniden değerlendirilmeli. |

---

## 📋 Son Test Sonuçları

### Premium + Referral + AI Limit/UX + Çerez + MCP Tool Testi (2026-07-19) ✅
| Kontrol | Sonuç |
|---|---|
| Premium sayfası — "Elite" kaldırıldı, ₺149/ay tek katman | ✅ tsgo temiz |
| Referral: 1./2. nitelikli referans → hâlâ `free` | ✅ |
| Referral: 3. nitelikli referans → `tier=premium`, `premium_until=+12ay` | ✅ Canlı doğrulandı |
| Referral: mükerrer önleme (aynı alıcının 2. siparişi) | ✅ Yeni ödül üretmedi |
| AI limit: `can_send_ai_message` RPC (50 free / 500 premium) | ✅ Kod doğrulandı |
| AI UX: soft-warn banner + taslak korumalı hard-limit paneli | ✅ tsgo temiz |
| Çerez/analytics taraması | ✅ Hiç yok, banner gerekmiyor |
| MCP: `create_community_post` (gerçek JWT, moderasyon dahil) | ✅ |
| MCP: `list_community_posts`, `create_subscription`, `list_my_subscriptions`, `cancel_subscription` | ✅ Hepsi, test verisi temizlendi |

### (Önceki tüm test sonuçları — Sertifika/useHasat/Gating, Audit Takibi, Detaylı E2E, Alıcı Sayfaları, Mock Data, Fiyatlandırma, Tedarik Zinciri, Landing, RLS, Rekabet Hukuku, MCP Server, Benchmark Re-Audit, P16 serisi — değişmedi, önceki sürümlerde)
