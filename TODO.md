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
- [x] **Hasat MCP Server** — `hasat.lovable.app/mcp`, OAuth 2.1 (Supabase issuer), 16 tool (çiftçi+alıcı, okuma+yazma, hassas işlemlerde `confirm:true` zorunlu)
- [x] **Rekabet Hukuku Risk Denetimi + Düzeltmeleri** (bkz. detaylı bölüm aşağıda)
- [x] **Kapsamlı RLS Güvenlik Denetimi + 4 Kritik/Orta Düzeltme** (bkz. detaylı bölüm aşağıda)
- [x] **E2E-QA Dokümanı** oluşturuldu (`Build/E2E-QA`) — Hasat MCP + Supabase MCP ile otomasyon mimarisi

### P16 — TÜM SERİ TAMAMLANDI ✅
(P16-A/B/C/D/E/F/G/H/H-Ext/I/J + Bug Fix Batch 3/4 + Crop/Şehir Konsolidasyonu + Draft-İlan Pipeline'ı + Bireysel Alıcı Desteği + Landing v2 — hepsi tamamlandı, detaylar önceki sürümlerde)

### Bug Fixes — Tamamlanan (özet, tam liste önceki sürümlerde)
- [x] Onboarding/parsel/draft-ilan/crop-şehir tutarsızlıkları (2026-07-08)
- [x] `handle_new_user` ON CONFLICT, buyer_type denormalizasyonu
- [x] Landing page v2 görsel bug'ları (2026-07-09, bkz. aşağıda)
- [x] **4 kritik/orta güvenlik açığı** (2026-07-10, bkz. aşağıda)

### Araştırma & Planlama
- [x] Landing page tasarım auditleri v1/v2
- [x] **Rekabet Kurumu risk denetimi** — algoritmik fiyatlama, e-pazaryeri sektör raporu (2021) referansları
- [x] **MCP server'dan nasıl faydalanılacağı** planlandı — E2E otomasyon + ürün farklılaştırıcı olarak

---

## 🔴 ŞİMDİ — Temmuz 2026

### Yasal & Altyapı
- [ ] Şahıs şirketi kur · iyzico başvurusu · Alan adı araştır · Twilio Verify service
- [ ] **WhatsApp Business başvurusu → hemen başla**

### Düşük öncelikli cila
- [ ] `farmer.journal.new.tsx` "Önce bir parsel ekle" statik metni
- [ ] `formatCrop()` heuristik (crop_config'ten canlı okumuyor)
- [ ] Landing page'de çok fazla rol-seçim butonu (karışma riski, bir testte yaşandı)
- [ ] Landing v2 "ilk pas" sadeleştirmeleri (harita pin sayısı, tedarik zinciri animasyon zenginliği)
- [ ] MCP yazma tool'larında aralıklı **"No approval received"** hatası — retry ile geçiyor, kök neden izleniyor
- [ ] `profiles`/`certifications` güvenlik düzeltmeleri için canlı exploit testi henüz yapılmadı (kod seviyesinde doğrulandı, düşük öncelik)

---

## 🏗️ Lovable Build Sırası — TAMAMLANDI ✅

> P16 serisi + mobil audit + bireysel alıcı + landing v2 + MCP server + rekabet hukuku düzeltmeleri + güvenlik denetimi bitti. Sıradaki: **Ağustos E2E test kapsamının tamamlanması** (buyer-tarafı özellikler: sertifika, community reply, AI chat, bildirimler).

---

### ✅ Kapsamlı RLS Güvenlik Denetimi + Düzeltmeleri *(Tamamlandı — 2026-07-10)*

Kullanıcı talebiyle "tüm QA bitene kadar" sistematik bir güvenlik taraması yapıldı: tüm 24 `public` şema tablosunun RLS policy'leri tek tek incelendi (`with_check IS NULL` deseni arandı — "kim yazabilir" var ama "ne yazabilir" kısıtı yok).

**Bulgular ve düzeltmeler:**
1. 🔴 **KRİTİK — `offers` tablosu:** Alıcı/çiftçi, `payment_status`/fiyat/miktar/durum alanlarını resmi akışı (respond_to_offer/mark_transfer_sent/confirm_payment_received) atlayıp direkt API isteğiyle **hiçbir kısıtlama olmadan** değiştirebiliyordu. En ciddi somut risk: bir alıcı hiç ödeme yapmadan `payment_status='paid'` yazabilirdi. **Düzeltme:** `enforce_offer_transitions_trg` — tam state machine'i (ödeme geçişleri, fiyat/miktar sadece karşı-teklif sırasında ve sırası olan tarafça, durum geçiş whitelist'i) DB seviyesinde kodladı.
2. 🔴 **KRİTİK — `community_posts`:** Fiyat-koordinasyon moderasyon filtresi (`looksLikePriceCoordination`) sadece client-side JS'te çalışıyordu, direkt API isteğiyle tamamen bypass edilebiliyordu. **Düzeltme:** `enforce_community_moderation_trg` — aynı mantık artık DB'de, client'ın gönderdiği değeri her zaman geçersiz kılıp kendi hesaplıyor.
3. 🟠 **ORTA-YÜKSEK — `profiles`:** Kullanıcı kendi `role`/`tier`/`premium`/`referred_by`/`buyer_type` alanlarını sınırsız self-update edebiliyordu (rol yükseltme, ücretsiz premium riski). **Düzeltme:** `enforce_profile_self_update_restrictions_trg`.
4. 🟡 **ORTA — `certifications`:** Çiftçi kendi sertifikasını `verified_at` ile self-doğrulayabiliyordu. **Düzeltme:** `enforce_cert_verification_trg` + UI'da "Doğrulama bekleniyor" fallback metni.
5. ✅ **Güvenli doğrulanan tablolar:** `orders` (UPDATE policy'si yok), `offer_messages`, `order_timeline`, `listing_harvest_entries`, `crop_config`, `price_points`, `listings.status/quantity`'nin statik kalması (tasarım gereği, dinamik stok hesaplamasıyla tutarlı).

**Canlı doğrulama (en değerli kısım):** Supabase MCP admin oturumunda `auth.uid()` her zaman `NULL` döndüğü için (trigger'lar bu durumda enforcement'ı atlıyor, tasarım gereği), gerçek exploit testleri **`set_config('request.jwt.claims',...)` ile gerçek bir buyer JWT'si impersonate edilerek**, sonu kasıtlı `RAISE EXCEPTION` ile rollback edilen bir transaction içinde yapıldı:
- `unpaid→paid` atlama denemesi → **bloklandı** ("Gecersiz odeme durumu gecisi")
- Sırası olmayanın fiyat değiştirme denemesi → **bloklandı**
- Pending teklifte fiyat değiştirme denemesi → **bloklandı**
- Topluluk moderasyon bypass denemesi (direkt INSERT) → tekrar denendi, artık **doğru yakalanıyor**

**Regresyon testi:** Yeni trigger'lar aktifken S2 (uçtan uca teklif/pazarlık/ödeme) senaryosu Hasat MCP üzerinden gerçek OAuth oturumlarıyla (Ahmet↔Zeynep) sıfırdan tekrar koşuldu — **sıfır regresyon**, meşru akış tamamen sorunsuz.

**Sonuç: Gün sonunda bilinen kritik/yüksek öncelikli bug kalmadı.**

---

### ✅ Rekabet Hukuku Risk Denetimi + Düzeltmeleri *(Tamamlandı — 2026-07-09)*

Kullanıcı talebiyle Rekabet Kurumu (4054 sayılı Kanun) açısından risk denetimi yapıldı — Kurum'un 2026 algoritmik fiyatlama/dijital platform denetim önceliği ve 2021 E-Pazaryeri Sektör Raporu (öz-tercihlendirme, algoritmik fiyatlama, MFN, veri kilitleme) referans alındı. **Not: Bu bir hukuki görüş değildir, gerçek bir avukatla teyit edilmeli.**

**Bulunan riskler ve uygulanan azaltmalar:**
1. **`price_feed` RLS tamamen açıktı** (herkes ham, tekil, farmer-ID'li kayıtları görebiliyordu) → `get_price_feed_summary()` RPC'si eklendi: sadece agregat (30 gün, min. 5 farklı çiftçi eşiği), ham okuma kapatıldı
2. **AI fiyat uyarısı doğrudan sayı öneriyordu (rakip fiyat sinyali riski)** → band-based (YÜKSEK/UYGUN/DÜŞÜK) sisteme çevrildi, hiçbir zaman spesifik sayı önermiyor
3. **WhatsApp AI + in-app AI chat** → sistem promptlarına aynı band-based çerçeve + "asla spesifik fiyat önerme" talimatı eklendi
4. **ToS'a** fiyat/arz/müşteri koordinasyonu yasağı maddesi eklendi
5. **Topluluk moderasyonu** — ilk versiyon (client-side keyword flag) eklendi, sonradan 2026-07-10'da DB trigger'ına taşınarak güçlendirildi (bkz. yukarı)
6. **Sıralama şeffaflığı** — landing/storefront/discover'a kısa açıklama eklendi
7. **Veri dışa aktarma** — farmer.settings'e "Verilerimi İndir" JSON export butonu eklendi (KVKK/taşınabilirlik savunması)

**Karşılaşılan ve düzeltilen bug:** `get_price_feed_summary()` ilk halinde "ambiguous column" hatası veriyordu (RETURNS TABLE çıktı adı, tablo kolonuyla çakışıyordu) — düzeltildi, canlı doğrulandı.

---

### ✅ Hasat MCP Server Kurulumu + Genişletmesi *(Tamamlandı — 2026-07-09/10)*

Uygulama, `hasat.lovable.app/mcp` adresinde OAuth 2.1 korumalı (Supabase Auth issuer, Dynamic Client Registration) bir MCP server olarak yayınlandı. Herhangi bir MCP istemcisi (Claude dahil), gerçek bir kullanıcı hesabıyla OAuth yapıp, RLS'in doğal sınırladığı işlemleri yapabiliyor.

**İlk 4 tool** (okuma + 1 yazma) → **plan-mode audit sonrası 16 tool'a genişletildi** (çiftçi: parsel/ilan/teklif yönetimi; alıcı: keşfet/teklif/ödeme — ilk kez eklendi). Her hassas tool (`respond_to_offer`, `respond_to_counter`, `confirm_payment_received`, `mark_transfer_sent`) Zod seviyesinde `confirm: z.literal(true)` zorunluluğu taşıyor.

**Güvenlik tasarımı:** Hiçbir tool service-role kullanmıyor, hepsi kullanıcının kendi bearer token'ıyla çalışıyor (RLS tek güvence). Kullanıcının UI'da yapabileceğinin dışına çıkamıyor.

**Kullanım alanı:** Hem gerçek ürün özelliği (WhatsApp dışında herhangi bir AI asistanı Hasat hesabına bağlanabilir) hem de Claude'un E2E test otomasyonu için birincil araç — bkz. `Build/E2E-QA`.

**Bilinen kısıt:** Bir connector bağlantısı = bir kullanıcı kimliği; çiftçi↔alıcı geçişi disconnect/reconnect gerektiriyor (sabit-OTP seed hesaplarla ~30 saniye).

---

### ✅ Landing Page v2 — Görsel Düzeltmeler *(Tamamlandı — 2026-07-09)*

Canlı gözden geçirme sonrası (Berkin'in ekran görüntüleri) bulunan 8 sorun düzeltildi:
1. Hero mini "Tarla Günlüğü" kartı sıkışıklığı → padding/spacing artırıldı
2. Tedarik Zinciri bölümü (bozuk "→değer" glyph'leri) → value-bar karşılaştırma (toggle: Geleneksel/Hasat ile) ile yeniden inşa edildi
3. İzlenebilirlik telefon mockup'ı → aynı premium spacing
4. Pazar yeri kartları: "Örnek ilan" pill'i kaldırıldı, çiftçi isimleri maskelendi (örn. "A. Y.")
5. **Ürün-fotoğraf uyumsuzluğu** (zeytinyağı→orman, lavanta→okyanus, fındık→soğan fotoğrafı) → doğru Unsplash görselleriyle değiştirildi
6. Türkiye haritası (tanınmaz blob) → gerçek basitleştirilmiş Türkiye anahatı
7. **Bug:** Sticky Nav'ın `backdrop-blur`'ı, scroll'da bölümler arası "sızma"/boş şerit görünümüne sebep oluyordu → Nav opak yapıldı, her section'a explicit background verildi
8. **Bug:** `RoleSwitcher` (dev widget'ı) public landing'de sızıyordu → `/` route'unda gizlendi

---

### Sosyal Medya / Çiftçi Ağı (değişmedi, bkz. önceki sürümler)

---

## 🟡 AĞUSTOS 2026 — Soft Launch — ŞİMDİ BURADAYIZ

### E2E Test — devam ediyor
- [x] Farmer/Buyer onboarding E2E · Landing page E2E · Draft-İlan pipeline E2E
- [x] **S1 (Parsel→Draft→Günlük→Yayınla)** — Hasat MCP ile tam koşuldu ✅
- [x] **S2 (Teklif→Pazarlık→Ödeme)** — Hasat MCP ile 2 kez tam koşuldu (güvenlik düzeltmesi öncesi + regresyon testi) ✅
- [x] **S3 (Fiyat özeti/rekabet hukuku)** ✅ · **S5 (Topluluk moderasyonu)** ✅ (bug bulundu+düzeltildi) · **S7 (RLS güvenlik denetimi)** ✅
- [ ] Kalan kapsam: sertifika (foto upload), community+reply UI, AI chat kalitesi, bildirimler, referral tam akış — MCP tool'u olmayan alanlar, hibrit/manuel test gerekiyor

### Teknik Final
- [ ] hasat.lovable.app → custom domain yayını · iyzico canlı ödeme testi
- [x] P16-C tam havale e2e testi — **S2 ile kapandı** ✅

### İlk Kullanıcılar
(değişmedi, bkz. önceki sürümler)

---

## 🔵 SAFRAN SEZONU / ⬜ SONRAKI FAZLAR

(değişmedi, bkz. önceki sürümler — Landing v2 zenginleştirme roadmap'te kalıyor)

---

## 📋 Lovable Prompt Yazma Kuralları

(1-18 önceki sürümde — devam:)
19. **RLS `with_check IS NULL` sistematik olarak taranmalı** — özellikle iki tarafın (buyer+farmer gibi) aynı satırı güncelleyebildiği tablolarda, "kim yazabilir" kısıtı "ne yazabilir" kısıtı garantisi vermez. Business-kritik alan geçişleri (ödeme durumu, rol, doğrulama) için ayrı bir `BEFORE UPDATE` trigger gerekir.
20. **Güvenlik düzeltmesi sonrası canlı exploit testi için:** Supabase MCP admin oturumu `auth.uid()=NULL` döndürür, trigger'lar genelde bunu atlar (service-role bypass tasarımı) — gerçek testin `set_config('request.jwt.claims',...)` ile kullanıcı JWT'si impersonate edilip, sonu kasıtlı `RAISE EXCEPTION`'la rollback edilen bir transaction içinde yapılması gerekir.
21. **Rekabet hukuku hassasiyeti olan özelliklerde** (fiyat şeffaflığı, AI önerileri, topluluk forumları) bilgilendirici dil ile tavsiye edici dil arasındaki çizgi net tutulmalı — spesifik sayı önermek yerine bant/aralık göstermek tercih edilmeli.

---

## 📌 Kararlar

(önceki tablo + eklenenler:)
| İlan durumu, Draft otomasyonu, Hukuki sayfalar, Alıcı tipi, Güven Skoru isimleri | *(değişmedi)* |
| **MCP server** | `hasat.lovable.app/mcp`, OAuth 2.1, 16 tool, hem ürün özelliği hem E2E test aracı olarak kullanılıyor |
| **Rekabet hukuku duruşu** | Fiyat verisi her zaman agregat+geciktirilmiş+min-N-eşikli; AI hiçbir zaman spesifik fiyat önermez, sadece bant gösterir |
| **Güvenlik ilkesi** | RLS tek güvence yeterli değil — iki-taraflı paylaşımlı tablolarda (offers gibi) her zaman ek bir state-machine trigger'ı olmalı |
| **QA otomasyon mimarisi** | Hasat MCP (gerçek kullanıcı aksiyonu) + Supabase MCP (sadece doğrulama/temizlik) ayrımı — `Build/E2E-QA` dokümanında detaylı |

---

## 📋 Son Test Sonuçları

### RLS Güvenlik Denetimi (2026-07-10) ✅
| Test | Sonuç |
|---|---|
| 24 tablo sistematik RLS taraması | ✅ 2 kritik + 2 orta bulgu |
| `offers` — 3 exploit denemesi (JWT impersonasyonu) | ✅ Hepsi bloklandı |
| `community_posts` — bypass denemesi | ✅ Artık yakalanıyor |
| S2 regresyon (yeni trigger'lar aktifken) | ✅ Sıfır regresyon |
| tsgo | ✅ |

### Rekabet Hukuku Düzeltmeleri (2026-07-09) ✅
| Test | Sonuç |
|---|---|
| `get_price_feed_summary` RPC | ❌→✅ (ambiguous column bug düzeltildi) |
| Band-based AI nudge | ✅ |
| ToS + moderasyon + sıralama şeffaflığı + veri export | ✅ |

### Hasat MCP Server (2026-07-09/10) ✅
| Test | Sonuç |
|---|---|
| 4 ilk tool (okuma+create_harvest_entry) | ✅ Gerçek OAuth ile test edildi |
| 12 ek tool (16 toplam) | ✅ tsgo temiz, tüm hassas tool'larda confirm zorunlu |
| S1, S2 canlı MCP testi | ✅ Tam geçti |

### Landing Page v2 Görsel Düzeltmeleri (2026-07-09) ✅
| Test | Sonuç |
|---|---|
| 8 madde (spacing, tedarik zinciri, foto uyumu, harita, Nav bug, RoleSwitcher leak) | ✅ Hepsi düzeltildi |

### (Önceki tüm test sonuçları — P16 serisi, Bireysel Alıcı, Mobil Audit, Landing v1, Bug Fix Batch 3/4 — değişmedi, önceki sürümlerde)
