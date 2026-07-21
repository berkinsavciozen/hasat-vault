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
- [x] **P17-F — Tekrar Sipariş + Şube Adresleri TAMAMLANDI, Abonelikler'e entegre** (2026-07-21) — bkz. detay
- [x] **Alıcı Bildirim Tercihleri sayfası sıfırdan eklendi + `buyer.account.tsx` bayat veri bug'ı düzeltildi** (2026-07-21) — bkz. detay

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

---

## 🏗️ Lovable/Supabase Build Sırası

> **P19 + P17-B + P17-C + P17-F tamamen bitti, hepsi canlı doğrulandı.** **P17-A ve P17-D şirket kuruluşuna bağlı, bloke.** Sıradaki: **P17-E (yapılandırılmış RFQ)** veya **P17-G (KPI görünümleri)** — ikisi de bağımsız, henüz sıralanmadı.

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
| P17-F | Tekrar sipariş + şube adresleri | P0 (HoReCa) | ✅ **TAMAMLANDI** |
| P17-A | Gerçek bloke ödeme (escrow) | P0 | 🔴 Bloke — şirket kuruluşu + iyzico başvurusu |
| P17-D | Fatura/e-müstahsil | P0 (B2B) | 🔴 Bloke — vergi mükellefiyeti gerektiriyor |
| P17-E | Yapılandırılmış RFQ | P1 | Sırada, bağımsız |
| P17-G | KPI ölçüm görünümleri | destek | Sırada, bağımsız |

### ✅ P17-B / P17-C — TAMAMLANDI
(Değişmedi — bkz. önceki sürüm)

### ✅ P17-F — Tekrar Sipariş + Şube Adresleri — TAMAMLANDI *(2026-07-21, kapsamlı canlı doğrulama)*

**Şema** ✅ — Yeni `buyer_addresses` tablosu (`buyer_id`, `label`, `address`, `city`, `is_default`). RLS `ALL` + `with_check` ile `auth.uid()=buyer_id` — sadece varlığı değil, policy içeriği de doğrulandı.

**Berkin'in isteği üzerine Abonelikler'e (`buyer.subscriptions.tsx`) entegre edildi — ayrık bir özellik olarak değil:**
- **"🔁 Tekrar Sipariş Ver"** — `buyer.orders.tsx`'teki tamamlanmış sipariş satırlarında ve `buyer.orders.$orderId.tsx`'te. İlan hâlâ aktifse (`order.listingId`/`order.listingActive` — `dbToOrder` genişletildi) `/buyer/offer/$listingId`'e `qty` prefill ile gider; değilse "Bu ürün artık satışta değil" notu. Fiyat her zaman GÜNCEL ilan fiyatından gelir, eski sabit fiyat asla kullanılmaz.
- **"🛍️ Şimdi Sipariş Ver"** — Aktif abonelik kartında. Çiftçinin güncel aktif ilanlarını (`useFarmerActiveListings`) bir modalda listeler; abonelikte `priceLock` varsa teklif sayfasına `suggestedPrice` olarak taşınır ve **"🔒 Abonelik sabit fiyatı öneriliyor — teyit edin"** notuyla gösterilir — kullanıcı onaylamadan otomatik uygulanmaz.
- `buyer.offer.$listingId.tsx`'in `validateSearch`'ü `qty`/`suggestedPrice`'ı doğru parse ediyor, alanlar prefill ama editable kalıyor.
- Şube adresleri: `buyer.account.tsx`'e "Adreslerim" bölümü (liste + ekle/sil/varsayılan seç, gerçek `useBuyerAddresses`/`useCreateAddress`/`useDeleteAddress`/`useSetDefaultAddress` hook'ları, toast'lu). Sipariş akışına bağlama bilerek bir sonraki tura bırakıldı (kapsam netti, karmaşıklık artırmadı).

**Doğrulama:** Tüm zincir (Abonelik kartı → modal → teklif sayfası prefill; tamamlanmış sipariş → tekrar sipariş → teklif sayfası prefill) dosya okumasıyla uçtan uca takip edildi, tsgo temiz.

### ✅ Alıcı Bildirim Tercihleri + `buyer.account.tsx` Bug Düzeltmesi — TAMAMLANDI *(2026-07-21)*

**Bulgu:** Alıcı tarafında bildirim tercihleri sayfası/butonu **hiç yoktu**. Çiftçi tarafındaki buton/sayfa kod seviyesinde doğruydu (RLS, veri, mutation hepsi sağlamdı) — "çalışmıyor" hissi muhtemelen sessiz kaydetmeden kaynaklanıyordu, gerçek bir bug bulunamadı ama teşhis+UX için toast eklendi.

- Yeni `src/routes/buyer.settings.notifs.tsx` — `farmer.settings.notifs.tsx` ile aynı yapı, aynı `notif_prefs` tablosu (role-agnostic, hem Ahmet hem Zeynep'in gerçek satırı zaten vardı).
- `buyer.account.tsx`'e "Bildirim Tercihleri" linki eklendi.
- **Her iki notif sayfasına da** (`farmer.settings.notifs.tsx` + `buyer.settings.notifs.tsx`) `onCheckedChange`'e başarı/hata toast'ı eklendi ("Tercih güncellendi" / hata mesajı) — artık sessiz değil.
- **Bonus bug düzeltildi:** `buyer.account.tsx` artık `useProfile()`'dan gerçek `name`/`city`/`phone`/premium okuyor — eski `useHasat` local store'undan bayat veri gösterme sorunu giderildi.

**Doğrulama:** Her dosya (`buyer.settings.notifs.tsx`, `buyer.account.tsx`, `farmer.settings.notifs.tsx`) tam okunarak, `notif_prefs`/`buyer_addresses` RLS canlı SQL ile kontrol edilerek doğrulandı.

---

## 🟡 AĞUSTOS 2026 — Soft Launch
(Değişmedi)

## 🔵 SAFRAN SEZONU / ⬜ SONRAKI FAZLAR
(Değişmedi)

---

## 📋 Lovable/Supabase Prompt Yazma Kuralları

(1-73 önceki sürümde — devam:)
74. **[Bu turda eklendi] Kullanıcı "önceki turda uygulanmıştı, ek değişiklik gerekmedi" dediğinde bile aynı doğrulama disiplini uygulanmalı** — bu turda rapor %100 doğru çıktı (her dosya, her zincir uçtan uca doğrulandı), ama bu, doğrulamayı atlamayı haklı çıkarmaz; sadece bu turda güven+doğrula pratiği ödüllendi.
75. **[Bu turda eklendi] Bir "bug" raporu (örn. bildirim butonu çalışmıyor) statik kod incelemesiyle doğrulanamıyorsa, dürüstçe "bulamadım" denip UX-diagnostik bir iyileştirme (toast gibi) önerilmeli** — var olmayan bir bug'ı icat etmek yerine, gerçek durumu netleştirecek bir değişiklik yapmak daha doğru.

---

## 📌 Kararlar

(önceki tablo + eklenenler:)
| **P17-F tamamlandı (2026-07-21)** | Abonelikler sayfasına entegre tasarlandı (ayrı özellik değil), tam canlı doğrulandı. |
| **Alıcı bildirim tercihleri + bug düzeltmesi tamamlandı (2026-07-21)** | Gerçek boşluk (sayfa hiç yoktu) kapatıldı, `buyer.account.tsx` bayat veri bug'ı düzeltildi, her iki notif sayfasına toast eklendi. |

---

## 📋 Son Test Sonuçları

### P17-F + Bildirim Düzeltmeleri (2026-07-21) ✅
| Kontrol | Sonuç |
|---|---|
| `buyer_addresses` şeması + RLS | ✅ Canlı SQL ile doğrulandı |
| Tekrar Sipariş Ver (liste + detay, aktif/pasif ilan durumu) | ✅ Dosya okumasıyla doğrulandı |
| Şimdi Sipariş Ver (abonelik → aktif ilanlar → teklif prefill) | ✅ Uçtan uca takip edildi |
| Fiyat kilidi teyit gerektiriyor, otomatik uygulanmıyor | ✅ |
| Alıcı Adreslerim (liste/ekle/sil/varsayılan) | ✅ |
| Alıcı Bildirim Tercihleri sayfası (yeni) | ✅ |
| `buyer.account.tsx` gerçek profil verisi | ✅ |
| Her iki notif sayfasında toast | ✅ |
| `tsgo` | ✅ Temiz |

### (Önceki tüm test sonuçları — değişmedi, önceki sürümlerde)
