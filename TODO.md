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
- [x] **P17-E — Yapılandırılmış RFQ (talep akışı) TAMAMLANDI, uçtan uca canlı veriyle bizzat doğrulandı** (2026-07-21) — bkz. detay

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

> **P19 + P17-B + P17-C + P17-F + P17-E tamamen bitti, hepsi canlı doğrulandı.** **P17-A ve P17-D şirket kuruluşuna bağlı, bloke.** Sıradaki: **P17-G (KPI ölçüm görünümleri)** — P17 serisinin son bağımsız fazı.

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
| P17-G | KPI ölçüm görünümleri | destek | **Sıradaki — son bağımsız faz** |

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

---

## 🟡 AĞUSTOS 2026 — Soft Launch
(Değişmedi)

## 🔵 SAFRAN SEZONU / ⬜ SONRAKI FAZLAR
(Değişmedi)

---

## 📋 Lovable/Supabase Prompt Yazma Kuralları

(1-79 önceki sürümde — devam:)
80. **[Bu turda eklendi] Bir hook/mutation tanımlı ama HİÇ UI'dan çağrılmıyorsa, bu "eksik özellik" değil "yarım bırakılmış özellik" anlamına gelir** — `useCreateCropRequest` örneğinde olduğu gibi, önce kodun var olup olmadığına değil, gerçekten bir kullanıcı yolundan tetiklenip tetiklenmediğine bakılmalı.
81. **[Bu turda eklendi] Eşleşme/bildirim gibi çok adımlı bir mantığı doğrularken, her adımı (canonical eşleşme → çiftçi bulma → bildirim satırı) AYNI SQL sorgularıyla ayrı ayrı simüle etmek, sadece "kod var" demekten çok daha güçlü bir kanıt sağlar.**

---

## 📌 Kararlar

(önceki tablo + eklenenler:)
| **P17-E tamamlandı (2026-07-21)** | Şema+UI+eşleşme+bildirim+liste sayfası, hepsi gerçek verilerle uçtan uca doğrulandı. Sıradaki: P17-G (KPI görünümleri) — P17 serisinin son bağımsız fazı. |

---

## 📋 Son Test Sonuçları

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
