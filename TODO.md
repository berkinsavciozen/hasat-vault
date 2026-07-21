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
- [x] İzmir hal genişletmesi: 3 → 27 ürün, Q3 backfill (17/21 gün, ~450 nokta), `unit` alanı RPC'lerde
- [x] **UI birim gösterimi Lovable'da publish edildi** (2026-07-21) — canlı doğrulandı

### P16 — TÜM SERİ TAMAMLANDI ✅
(Detaylar önceki sürümlerde)

---

## 🔎 BENCHMARK RE-AUDIT (Temmuz 2026)
(Değişmedi)

---

## 🔴 ŞİMDİ — Temmuz 2026
(Değişmedi — bkz. önceki sürüm)

### Düşük öncelikli cila
(Değişmedi — bkz. önceki sürüm)
- [ ] `price_points` tablosu hâlâ ölü kod adayı.
- [ ] Lovable kuyruk duraklatma sorunu — tekrarlayan, editörden manuel açılması gerekiyor.

---

## 🏗️ Lovable/Supabase Build Sırası

> **P18/P19 bitti. İzmir genişletme + Q3 backfill + `unit` alanı — hepsi backend'de VE UI'da canlı, publish edildi.** Sıradaki: İstanbul entegrasyonu — **Berkin'den `swagger/docs/v1` ham JSON çıktısı hâlâ bekleniyor** (URL sunucu tarafında TLS hatası veriyor, Berkin'in kendi tarayıcısında da açılmıyor — bu, sunucunun kendi bozukluğu, araç sorunu değil). **Konya elendi** (aşağı bkz. — statik/eski arşiv, kullanılamaz).

---

## 🎨 P18 — ARAYÜZ YENİLEME — **SERİ TAMAMEN TAMAMLANDI ✅**
(Değişmedi — bkz. önceki sürüm)

---

## 🟢 P19 — BORSA DENEYİMİ — **TAMAMEN TAMAMLANDI ✅**
(Değişmedi — bkz. önceki sürüm)

---

## 🟠 P19-C — KAYNAK & ÜRÜN TİPİ GENİŞLETME *(devam ediyor)*

### ✅ Adım 1/1b/1c — İzmir genişletme + Q3 backfill + `unit` alanı *(Tamamen tamamlandı — 2026-07-21)*
Hepsi canlı doğrulandı (bkz. önceki sürüm). **Ek olarak bu turda:** UI birim gösterimi prompt'u işlendi ve **publish edildi** — `queries.ts`'e `unit` alanı eklendi, yeni `priceWithUnit()` yardımcı fonksiyonu (`src/lib/hasat/format.ts`) `formatTRY(n)/unit` formatını üretiyor (unit yoksa "kg" fallback), `PriceChart.tsx`'e `unit?` prop'u eklendi ve "Son"/"Aralık" gösterimlerinde kullanılıyor. Dosya okumasıyla doğrulandı.

### 🔍 Adım 2 — Diğer kaynak araştırması

**İstanbul (İBB) — hâlâ açık blokajda, kapsamlı şekilde denendi:**
- Berkin'in ekran görüntüsünden gerçek spec URL'i netleşti: `https://halfiyatlaripublicdata.ibb.gov.tr/swagger/docs/v1`.
- Sunucu taraflı (edge function) bu URL'e ulaşmaya çalışıldığında tekrarlayan TLS bağlantı hatası alındı.
- **Berkin'in kendi tarayıcısında da bu URL "çalışmıyor"** — bu, iki bağımsız kanaldan (sunucu taraflı + normal tarayıcı) aynı sonucu doğruluyor: **sorun benim aracımda değil, İBB'nin sunucusunda gerçek bir bozukluk/yanlış yapılandırma var.**
- Kör route-adı taraması yapıldı (`/api/Urun`, `/api/UrunFiyat/Listele`, `/api/UrunFiyat/GetAll` vb. — 10+ deneme): tüm temel controller'lar (`/api/HalFiyat`, `/api/Urun`, `/api/UrunFiyat`, `/api/Hal`, `/api/Kategori`, `/api/Birim`, `/api/UrunKategori`, `/api/HalTuru`, `/api/BirimTuru`) sadece statik "Hal Fiyatları Public Data" açılış sayfasını dönüyor; tahmin edilen action adları (`Listele`/`GetAll`/`Get`/`GunlukFiyatlar`/`TarihiFiyatlar`) gerçek ama yanlış-route 404 JSON'u dönüyor (API canlı, doğru action adı bilinmiyor).
- **Sonuç: kör tahminle devam etmek verimsiz — duraklatıldı.** Gerçek ilerleme için ya (a) İBB'nin "Veri Talebi" formu üzerinden resmi başvuru, ya da (b) spec dosyasının kendisinin düzelmesi gerekiyor. Bu ikisi de Claude'un kontrolünde değil.

**Konya — ELENDİ (bu turda test edildi, kullanılamaz):**
- `hal_fiyatlari.csv` doğrudan indirildi ve içeriği incelendi: **2004-03-02'den başlayan, "2 yıl önce" son güncellenmiş, durağan/statik bir tarihsel arşiv.** İzmir'in canlı günlük REST API'sinin aksine gerçek zamanlı veya yakın-güncel veri sağlamıyor. Fiyat sayfası için kullanılamaz, top-3'ten çıkarıldı.

**Ankara:** Hâlâ hal-fiyatları özelinde bir dataset bulunamadı (genel "Şeffaf Ankara" platformu var, spesifik doğrulama yapılmadı).

**Global commodity API (buğday/mısır/pamuk/ayçiçeği için):** Değişmedi — Berkin'in hesap/key oluşturması gerekiyor.

### Güncellenmiş öncelik sırası
1. **Global commodity API** — şu an en gerçekçi ilerleme yolu (sadece Berkin'in bir hesap açması gerekiyor, teknik blokaj yok).
2. **İstanbul** — Berkin'in Veri Talebi formu üzerinden resmi başvurusuna kadar beklemede.
3. Konya/Ankara için başka bir aday aranabilir (örn. Bursa, Antalya) — henüz araştırılmadı.

---

## 🟡 AĞUSTOS 2026 — Soft Launch
(Değişmedi)

## 🟣 P17 — GÜVEN ÇEKİRDEĞİ
(Değişmedi)

## 🔵 SAFRAN SEZONU / ⬜ SONRAKI FAZLAR
(Değişmedi)

---

## 📋 Lovable/Supabase Prompt Yazma Kuralları

(1-61 önceki sürümde — devam:)
62. **[Bu turda eklendi] Bir açık veri portalındaki CSV/kaynak kaydının "aktivite geçmişi" bölümü kontrol edilmeli** — "X yıl önce güncellendi" notu, o kaynağın canlı/güncel mi yoksa donmuş bir arşiv mi olduğunu gösterir; indirmeden önce bu sinyali okumak zaman kazandırır (Konya'da atlanmış, veri indirilip incelendikten sonra fark edilmiş).
63. **[Bu turda eklendi] Bir sunucu-taraflı TLS/bağlantı hatası kullanıcının kendi tarayıcısında da tekrarlanıyorsa, bu ikisi birbirini doğrular** — sorun Claude'un aracında değil, hedef sunucuda gerçek bir bozukluktur; bu noktada kör deneme yapmayı bırakıp (Veri Talebi formu gibi) resmi bir kanala yönlendirmek daha doğru.

---

## 📌 Kararlar

(önceki tablo + eklenenler:)
| **UI birim gösterimi publish edildi (2026-07-21)** | Backend + frontend tam canlı, dosya okumasıyla doğrulandı. |
| **Konya elendi (2026-07-21)** | CSV içeriği incelendi, 2004'ten kalma durağan arşiv, canlı veri değil. |
| **İstanbul duraklatıldı (2026-07-21)** | Sunucu tarafında gerçek bir TLS/yapılandırma bozukluğu var (Berkin'in tarayıcısı da doğruladı) — kör route taraması bırakıldı, resmi Veri Talebi formuna kadar beklemede. |
| **Güncellenmiş öncelik: global commodity API en gerçekçi yol** | Teknik blokaj yok, sadece Berkin'in hesap açması gerekiyor. |

---

## 📋 Son Test Sonuçları

### UI Birim Gösterimi (2026-07-21) ✅
| Kontrol | Sonuç |
|---|---|
| `queries.ts`'e `unit` alanı eklendi | ✅ |
| `priceWithUnit()` yardımcı fonksiyonu, kg fallback'li | ✅ |
| `PriceChart.tsx`'e `unit?` prop'u, "Son"/"Aralık"da kullanılıyor | ✅ |
| Publish edildi | ✅ (Berkin onayladı) |

### İstanbul Probe — Kapsamlı Deneme (2026-07-21) 🔴 Duraklatıldı
| Kontrol | Sonuç |
|---|---|
| API canlı mı | ✅ (gerçek 404 JSON alınabiliyor) |
| Spec dosyası (`swagger/docs/v1`) erişilebilir mi | ❌ Hem sunucu tarafında hem Berkin'in tarayıcısında hata |
| 10+ tahmin edilen route adı | ❌ Hiçbiri eşleşmedi |
| Karar | Kör tahmine devam edilmiyor, resmi kanal bekleniyor |

### Konya Probe (2026-07-21) ❌ Elendi
| Kontrol | Sonuç |
|---|---|
| CSV indirilebiliyor mu | ✅ |
| Veri güncel mi | ❌ 2004'ten kalma, durağan arşiv |
| Kullanılabilir mi | ❌ Top-3'ten çıkarıldı |

### (Önceki tüm test sonuçları — değişmedi, önceki sürümlerde)
