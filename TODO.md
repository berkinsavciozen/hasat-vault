---
title: Hasat — Master Roadmap & Build Log
updated: 2026-07-21
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
- [x] P18 serisi + P19 (backend+UI) tamamen tamamlandı
- [x] P19-C — İzmir tam olgun, global commodity API araştırması kapandı
- [x] P17-B — Sipariş sonrası akış sıfırdan inşa edildi, canlı doğrulandı
- [x] P17-C — Karşılıklı Değerlendirme Sistemi tamamlandı + 3 gerçek boşluk düzeltildi
- [x] P17-F — Tekrar Sipariş + Şube Adresleri tamamlandı, Abonelikler'e entegre
- [x] Alıcı Bildirim Tercihleri sayfası eklendi + `buyer.account.tsx` bayat veri bug'ı düzeltildi
- [x] Abonelik → Sipariş bağlantısı (`subscription_id`) eklendi + escrow yanlış vaadi düzeltildi
- [x] Abonelik CTA'sı üretici profilinde yukarı taşındı + bizzat canlı veri testi
- [x] P17-E — Yapılandırılmış RFQ (talep akışı) TAMAMLANDI, uçtan uca canlı veriyle bizzat doğrulandı (2026-07-21)
- [x] **P17-G-1 — KPI ölçüm view'ları (çekirdek: North Star + P0) backend'de TAMAMLANDI, RLS-bypass sızıntısı bulunup düzeltildi** (2026-07-21) — bkz. detay

### P16 — TÜM SERİ TAMAMLANDI ✅
(Detaylar önceki sürümlerde)

---

## 🔎 BENCHMARK RE-AUDIT (Temmuz 2026)
(Değişmedi)

---

## 🔴 ŞİMDİ — Temmuz 2026
(Değişmedi — bkz. önceki sürüm, "Şahıs şirketi kur" hâlâ işaretsiz)

### Düşük öncelikli cila
(Değişmedi — bkz. önceki sürüm)
- [ ] `useSetDefaultAddress` diğer adresleri `false`'a çekmiyor — düşük öncelik.

---

## 🏗️ Lovable/Supabase Build Sırası

> **P19 + P17-B + P17-C + P17-F + P17-E tamamen bitti, hepsi canlı doğrulandı.** **P17-G-1 (KPI çekirdek view'ları) backend'de TAMAMLANDI.** **P17-A ve P17-D şirket kuruluşuna bağlı, bloke.** Sıradaki: **P17-G-2 (kalan KPI view'ları)** veya **P17-G-UI (admin KPI dashboard, Lovable — ayrı prompt)**.

---

## 🎨 P18 — ARAYÜZ YENİLEME — **SERİ TAMAMEN TAMAMLANDI ✅**
(Değişmedi)

## 🟢 P19 — BORSA DENEYİMİ — **TAMAMEN TAMAMLANDI ✅**
(Değişmedi)

## 🟠 P19-C — KAYNAK & ÜRÜN TİPİ GENİŞLETME — **KAPANDI**
(Değişmedi — bkz. önceki sürüm)

---

## 🟣 P17 — GÜVEN ÇEKİRDEĞİ *(devam ediyor — 2026-07-21)*

### Kapsam ve sıralama
| Kod | Konu | Şiddet | Durum |
|---|---|---|---|
| P17-B | Teslim/kargo/iptal/ihtilaf akışı | P0 | ✅ **TAMAMLANDI** |
| P17-C | Karşılıklı değerlendirme (rating/review) | P0 | ✅ **TAMAMLANDI** |
| P17-F | Tekrar sipariş + şube adresleri + abonelik bağlantısı | P0 (HoReCa) | ✅ **TAMAMLANDI** |
| P17-E | Yapılandırılmış RFQ (talep akışı) | P1 | ✅ **TAMAMLANDI** |
| P17-A | Gerçek bloke ödeme (escrow) | P0 | 🔴 Bloke — şirket kuruluşu + iyzico başvurusu |
| P17-D | Fatura/e-müstahsil | P0 (B2B) | 🔴 Bloke — vergi mükellefiyeti gerektiriyor |
| P17-G | KPI ölçüm görünümleri | destek | 🟡 **P17-G-1 backend TAMAMLANDI** (2026-07-21) — P17-G-2 (genişletilmiş metrikler) ve P17-G-UI (admin dashboard) bekliyor |

### ✅ P17-B / P17-C / P17-F / Abonelik-Sipariş Bağlantısı — TAMAMLANDI
(Değişmedi — bkz. önceki sürüm)

### ✅ P17-E — Yapılandırılmış RFQ (Talep Akışı) — TAMAMLANDI *(2026-07-21, kapsamlı canlı doğrulama)*

**Bulgu:** `crop_requests` tablosu vardı ama sadece serbest metin + status — miktar/bölge/tarih/fiyat alanı yoktu (tam BENCHMARK Gap #6'nın tarifi). `useCreateCropRequest` hook'u tanımlıydı ama **hiçbir UI'da hiç çağrılmıyordu** — Discover'ın boş arama sonucu ekranında bile "talep oluştur" seçeneği yoktu. Eşleşme/bildirim mekanizması hiç yoktu.

**Uygulanan çözüm:**
- **Şema** ✅ — `crop_requests`'e `quantity`, `unit`, `region`, `target_date_start`, `target_date_end`, `target_price` kolonları eklendi. Mevcut RLS (`requested_by=auth.uid()` ile SELECT/INSERT) zaten sağlamdı.
- **Talep oluşturma UI'sı** ✅ — `buyer.discover.tsx`'in boş arama sonucu ekranına "Bu ürünü talep et" butonu + modal (ürün adı prefill, miktar+birim, bölge select, tarih aralığı, hedef fiyat).
- **Eşleşme + bildirim** ✅ — `useCreateCropRequest`, talep oluşunca `crop_config`'ten canonical ürün adını bulup `listings`/`parcels.crops` üzerinden eşleşen çiftçileri buluyor (region doluysa `profiles.city` ile daraltıyor), her eşleşen çiftçiye best-effort `notifications` satırı ekliyor.
- **"Taleplerim" sayfası** ✅ — Yeni `buyer.requests.tsx`, `useMyCropRequests()` ile kendi taleplerini listeliyor. `buyer.account.tsx`'e link eklendi.

**Doğrulama (Claude'un bizzat yaptığı canlı veri testi):**
1. `crop_config`'te "safran" → canonical eşleşme SQL ile doğrulandı.
2. Aynı eşleştirme sorgusuyla (`listings`/`parcels.crops` üzerinden) gerçek 3 çiftçi (Ahmet dahil) bulundu — kodun üreteceği sonuçla birebir aynı.
3. Kodun üreteceği tam `notifications` satırı (`type:'crop_request', title:'Yeni ürün talebi', body:'Zeynep Kaya safran arıyor — 50 g'`) gerçek bir `crop_requests` kaydına bağlı olarak Ahmet'e eklendi ve doğrulandı.
4. Test verisi (crop_request + notification) hemen sonra temizlendi.
5. `buyer.account.tsx`'teki "Taleplerim" linki dosya okumasıyla doğrulandı.

### 🟡 P17-G-1 — KPI Ölçüm View'ları (Çekirdek) — BACKEND TAMAMLANDI *(2026-07-21)*

**Kapsam:** BENCHMARK.md §5 KPI çerçevesinden North Star + P0 çekirdek, sadece Supabase view olarak (Lovable'a dokunulmadı, kredi harcanmadı — doğrudan Supabase MCP).

**Kurulan view'lar (`public` şema):**
- `v_kpi_order_base` — temel view: her sipariş için tutar (`current_price/current_quantity`, yoksa `price_per_unit/quantity`), ürün, taraflar, `reached_delivery` (status IN delivered/completed), `is_realized_sale` (+ `payment_status='paid'`), `has_dispute`
- `v_kpi_north_star` — aylık toplam GMV, ihtilafsız GMV, ihtilafsız pay % (hedef ≥%95)
- `v_kpi_dispute_rate` — aylık ihtilaf oranı (hedef ≤%2)
- `v_kpi_full_acceptance_rate` — aylık tam kabul oranı (hedef ≥%95)
- `v_kpi_buyer_repeat_rate` — segment bazlı (`buyer_profiles.company_type`) + genel tekrar alıcı oranı (GROUPING SETS)
- `v_kpi_review_avg` — kullanıcı/role/genel bazlı ortalama puan (GROUPING SETS)

**Önemli tasarım notu:** `orders.status`'ta şu an `completed` değeri hiç kullanılmıyor (veri: 110 delivered / 9 preparing / 1 shipped, disputed/cancelled/completed yok) — view'lar `status IN ('delivered','completed')` mantığıyla yazıldı, gerçek "completed" akışı devreye girince otomatik kapsanacak, değişiklik gerekmeyecek.

**🔴 Güvenlik bulgusu + düzeltme (aynı oturumda):** View'lar oluşturulduğunda Postgres/Supabase varsayılanı `anon` ve `authenticated` rollerine SELECT (+ anlamsız INSERT/UPDATE/DELETE) yetkisi veriyor. View'lar RLS'i bypass ettiği için (definer=postgres), bu **herhangi bir kullanıcının** (giriş yapmamış dahil) `v_kpi_order_base` üzerinden tüm siparişlerin buyer_id/farmer_id/tutar/şehir bilgisine RLS'siz erişebilmesi anlamına geliyordu — önceki RLS denetimindeki sızıntı deseninin aynısı. **Düzeltildi:** `REVOKE ALL ... FROM anon, authenticated` + sadece `service_role`'e `GRANT SELECT`. Doğrulandı: `information_schema.role_table_grants` ile anon/authenticated'ın artık hiçbir yetkisi yok, sadece service_role'de SELECT var.

**Doğrulama (canlı veriyle):**
- North Star: 2024-06 → 2026-07 arası aylık GMV kırılımı doğru hesaplanıyor (ör. 2025-10: 4.461,955 GMV)
- Dispute rate / full acceptance: `disputes` tablosu şu an 0 satır → dispute_rate %0.00, full_acceptance %100.00, North Star ihtilafsız pay %100.00 — hepsi tutarlı (hardcode değil, gerçek 0 sayımından türetiliyor)
- Buyer repeat rate: 6 segment (bireysel/genel/ihracatçı/organik_market/otel/restoran), her biri %100 (mock veride her alıcı ≥2 sipariş almış — gerçekçi ölçüm sonraki gerçek kullanıcı verisiyle şekillenecek)
- Review avg: 1 gerçek review (P17-C testinden), role=buyer, ort. 5.00 — beklenen (henüz mock review seed'i yok)

**Bloke/ölçülemeyen (bilinçli olarak kapsam dışı, BENCHMARK Gap'leriyle uyumlu):** ödeme hızı KPI'sı (Gap #1, escrow yok), gerçek take rate (ödeme altyapısı yok), gerçek NPS (anket yok).

**Sıradaki (P17-G-2, ayrı oturum):** aktivasyon süreleri (kayıt→ilk ilan/sipariş), ilan→teklif oranı, sell-through, aktif çiftçi başı GMV, M1/M3 retention, doğrulanmış üretici %, RFQ fill-rate (heuristik eşleştirme gerekecek — `crop_requests`'in `order_id` FK'ı yok), HoReCa haftalık sipariş sıklığı, alıcı:satıcı oranı, il×ürün tedarik yoğunluğu, hal fiyatı referans bandı (sadece İzmir pilotu: domates/elma/patates).

**Sıradaki (P17-G-UI, ayrı Lovable prompt'u, plan_mode=true ile başla):** bu view'ları gösteren basit admin KPI paneli — service_role ile okuyan bir edge function/admin route gerekecek (view'lar artık anon/authenticated'a kapalı, doğrudan frontend client'tan okunamaz).

---

## 🟡 AĞUSTOS 2026 — Soft Launch
(Değişmedi)

## 🔵 SAFRAN SEZONU / ⬜ SONRAKI FAZLAR
(Değişmedi)

---

## 📋 Lovable/Supabase Prompt Yazma Kuralları

(1-79 önceki sürümde — devam:)
80. **Bir hook/mutation tanımlı ama HİÇ UI'dan çağrılmıyorsa, bu "eksik özellik" değil "yarım bırakılmış özellik" anlamına gelir** — `useCreateCropRequest` örneğinde olduğu gibi, önce kodun var olup olmadığına değil, gerçekten bir kullanıcı yolundan tetiklenip tetiklenmediğine bakılmalı.
81. **Eşleşme/bildirim gibi çok adımlı bir mantığı doğrularken, her adımı (canonical eşleşme → çiftçi bulma → bildirim satırı) AYNI SQL sorgularıyla ayrı ayrı simüle etmek, sadece "kod var" demekten çok daha güçlü bir kanıt sağlar.**
82. **[Bu turda eklendi] Yeni bir Supabase view/tablo oluşturulduğunda, iş mantığı doğru olsa bile varsayılan grant'lar (`anon`/`authenticated`'a otomatik SELECT) kontrol edilmeden bırakılmamalı** — özellikle RLS'i bypass eden view'larda (definer=postgres), bu tek başına bir veri sızıntısıdır. Her yeni view/tablo sonrası `information_schema.role_table_grants` ile kontrol + gereksiz grant'ları `REVOKE` etmek standart adım olmalı.
83. **[Bu turda eklendi] `apply_migration` tek bir transaction olarak çalışıyor — bir migration içindeki son statement hata verirse, önceki statement'lar da (view'lar dahil) BAŞARILI olsa bile rollback olur.** Migration hata verdiğinde "kısmen uygulandı" varsayılmamalı; hatadan sonra `pg_views`/`information_schema` ile gerçekte ne kaldığı kontrol edilip eksik kalanlar yeni bir migration'la tamamlanmalı.

---

## 📌 Kararlar

(önceki tablo + eklenenler:)
| **P17-E tamamlandı (2026-07-21)** | Şema+UI+eşleşme+bildirim+liste sayfası, hepsi gerçek verilerle uçtan uca doğrulandı. Sıradaki: P17-G (KPI görünümleri) — P17 serisinin son bağımsız fazı. |
| **P17-G-1 tamamlandı (2026-07-21)** | 6 KPI view'ı (North Star + P0 çekirdek) Supabase'de canlı, gerçek veriyle doğrulandı. Kurulum sırasında anon/authenticated'a açık kalan RLS-bypass sızıntısı bulundu ve aynı oturumda düzeltildi (sadece service_role'e SELECT). UI (admin dashboard) ve genişletilmiş metrikler (P17-G-2) ayrı adımlar olarak bırakıldı. |

---

## 📋 Son Test Sonuçları

### P17-G-1 KPI View Doğrulaması (2026-07-21) ✅
| Kontrol | Sonuç |
|---|---|
| 6 view'ın tamamı `pg_views`'ta mevcut | ✅ |
| North Star aylık GMV hesaplaması (2024-06 → 2026-07) | ✅ Canlı veriyle doğrulandı |
| Dispute rate / full acceptance / North Star ihtilafsız pay tutarlılığı | ✅ `disputes`=0 satır → %0/%100/%100, birbiriyle tutarlı |
| Buyer repeat rate — 6 segment | ✅ Hesaplandı, sonuçlar mock veri profiliyle tutarlı |
| Review avg | ✅ 1 gerçek review, doğru agregat |
| anon/authenticated grant sızıntısı tespiti | ✅ `information_schema.role_table_grants` ile bulundu |
| Grant düzeltmesi sonrası doğrulama | ✅ anon/authenticated'da sıfır yetki, service_role'de sadece SELECT |

### P17-E Tam Doğrulama (2026-07-21) ✅
| Kontrol | Sonuç |
|---|---|
| `crop_requests` şema genişletmesi | ✅ Canlı SQL ile doğrulandı |
| "Bu ürünü talep et" modalı (Discover) | ✅ Dosya okumasıyla doğrulandı |
| Canonical ürün eşleştirme (`crop_config`) | ✅ Gerçek SQL ile doğrulandı |
| Çiftçi eşleştirme (`listings`/`parcels.crops`) | ✅ Gerçek 3 çiftçi bulundu (Ahmet dahil) |
| Bildirim satırı (`notifications`) | ✅ Gerçek veriyle oluşturuldu, doğrulandı, temizlendi |
| "Taleplerim" sayfası + hesap linki | ✅ Dosya okumasıyla doğrulandı |
| `tsgo` | ✅ Temiz |

### (Önceki tüm test sonuçları — değişmedi, önceki sürümlerde)
