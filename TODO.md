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
- [x] İzmir hal genişletmesi: 3 → 27 ürün
- [x] **Fiyat RPC'lerine `unit` alanı eklendi** (2026-07-21) — bkz. detay
- [x] **İzmir Q3 2026 geriye dönük veri doldurma** (2026-07-21) — bkz. detay

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
- [ ] Lovable kuyruk duraklatma sorunu — bu turda 2 kez daha yaşandı, editörden manuel açılması gerekiyor, otomatik çözülmüyor.

---

## 🏗️ Lovable/Supabase Build Sırası

> **P18/P19 bitti. İzmir genişletildi + Q3 backfill yapıldı + `unit` alanı RPC'lere eklendi.** Sıradaki: UI birim gösterimi prompt'u Lovable kuyruğunda bekliyor (kuyruk duraklatılmış, editörden açılması gerekiyor). Ardından **İstanbul entegrasyonu** — Berkin'den Swagger `docs/v1` ham JSON çıktısı bekleniyor (açık blokaj, aşağı bkz.).

---

## 🎨 P18 — ARAYÜZ YENİLEME — **SERİ TAMAMEN TAMAMLANDI ✅**
(Değişmedi — bkz. önceki sürüm)

---

## 🟢 P19 — BORSA DENEYİMİ — **TAMAMEN TAMAMLANDI ✅**
(Değişmedi — bkz. önceki sürüm)

---

## 🟠 P19-C — KAYNAK & ÜRÜN TİPİ GENİŞLETME *(devam ediyor)*

### ✅ Adım 1 — İzmir hal genişletmesi *(Tamamlandı — bkz. önceki sürüm)*

### ✅ Adım 1b — Q3 2026 Geriye Dönük Veri Doldurma *(Tamamlandı — 2026-07-21, canlı doğrulandı)*
İzmir API'sinden 2026-07-01 → 2026-07-21 arası 21 gün için geriye dönük veri çekildi (`pg_net` ile 21 paralel istek). **17/21 gün başarılı** (gün başına 26-27 satır, toplam ~450 gerçek fiyat noktası). 4 gün boş yanıt döndü — **beklenen, hata değil:** 5/12/19 Temmuz Pazar günleri (hal kapalı, bülten yok), 21 Temmuz bugün (bülten henüz yayınlanmamış). `sync-izmir-hal-prices` v4'e güncellendi: boş/parse edilemeyen yanıtları artık zarif şekilde `{inserted:0, note:"..."}` olarak dönüyor, `SyntaxError` fırlatmıyor. Domates için doğrulama: artık **4 gerçek haftalık İzmir noktası** var (29 Haz - 20 Tem), gerçek bir trend çizgisi oluşturuyor.

### ✅ Adım 1c — Fiyat Birimleri (`unit`) RPC'lere Eklendi *(Tamamlandı — 2026-07-21, canlı doğrulandı)*
`crop_config.default_unit` kolonu (zaten mevcuttu — örn. domates/elma "kg", safran "g") `get_price_history_summary`/`get_price_history_series` RPC'lerine geriye dönük uyumlu şekilde `unit` alanı olarak eklendi. Canlı test: domates → `"unit":"kg"`, safran → `"unit":"g"` doğru geldi. **UI tarafı Lovable kuyruğunda bekliyor** (`PriceSummaryCard`/`CropDetailBody`/`PriceChart`'a birim gösterimi ekleme prompt'u gönderildi, kuyruk duraklatıldığı için henüz işlenmedi).

### 🔍 Adım 2 — Diğer kaynak araştırması *(top-3 belirlendi, İstanbul'da açık blokaj)*
(Değişmedi — bkz. önceki sürüm: İstanbul/Konya/global commodity API top-3, Ankara'da spesifik hal verisi bulunamadı)

**Bu turda ek bulgu — İstanbul probe'u ilerletildi:** Berkin'in paylaştığı ekran görüntüsünden gerçek spec URL'i (`https://halfiyatlaripublicdata.ibb.gov.tr/swagger/docs/v1`) netleşti. Sunucu taraflı (edge function) bu URL'e ulaşmayı denedim ama bu spesifik host+path kombinasyonunda tekrarlayan bir TLS bağlantı hatası aldım (`peer closed connection without sending TLS close_notify` — Deno/rustls'in bu eski IIS sunucusuyla uyumsuzluğu, küçük `/api/` yanıtlarında sorun yok ama bu büyük spec dosyasında var). **🔴 AÇIK BLOKAJ (güncellendi): Berkin'den artık Swagger UI değil, doğrudan `https://halfiyatlaripublicdata.ibb.gov.tr/swagger/docs/v1` ham JSON çıktısı bekleniyor** — tarayıcıda açıp yapıştırması gerekiyor.

### Yapılacaklar (blokaj çözülünce devam)
1. **İstanbul:** Swagger JSON gelince gerçek endpoint/parametre şeması netleşir → İzmir deseniyle entegre edilir.
2. **Konya:** CKAN API'sinin canlı probe'u henüz yapılmadı.
3. **Global commodity API:** Berkin hesap/key oluşturduğunda entegre edilecek.
4. Her yeni kaynak: idempotent edge function + `pg_cron` + güvenlik taraması + ayrı kart kuralı (değişmez).

---

## 🟡 AĞUSTOS 2026 — Soft Launch
(Değişmedi)

## 🟣 P17 — GÜVEN ÇEKİRDEĞİ
(Değişmedi)

## 🔵 SAFRAN SEZONU / ⬜ SONRAKI FAZLAR
(Değişmedi)

---

## 📋 Lovable/Supabase Prompt Yazma Kuralları

(1-58 önceki sürümde — devam:)
59. **[Bu turda eklendi] `generate_series` ile tarih döngüsü kurarken `d::date::text` kullanılmalı, `d::text` kullanılırsa timestamp'in saat kısmı da URL'e karışır ve `net.http_post` "Malformed input to a URL function" hatası verir.**
60. **[Bu turda eklendi] Dış bir API'den geriye dönük (backfill) veri çekerken, "boş yanıt" senaryosu (tatil/hafta sonu kapanışı, henüz yayınlanmamış bugünün verisi) baştan beklenmeli** — edge function bunu `SyntaxError` fırlatmak yerine zarif bir `{inserted:0, note:"..."}` olarak ele almalı; aksi halde gerçek "kapalı gün" durumları sahte hata gibi görünür.
61. **[Bu turda eklendi] Bazı eski IIS/.NET sunucularında büyük yanıtlı endpoint'lere (örn. Swagger spec JSON) Deno/rustls üzerinden TLS bağlantı hatası alınabiliyor, aynı sunucunun küçük yanıtlı endpoint'leri çalışırken bile** — bu durumda kullanıcıdan tarayıcı çıktısı istemek tek pratik çözüm.

---

## 📌 Kararlar

(önceki tablo + eklenenler:)
| **Q3 2026 backfill tamamlandı (2026-07-21)** | 17/21 gün, ~450 gerçek fiyat noktası, 4 boş gün beklenen (3 Pazar + bugün). |
| **`unit` alanı eklendi (2026-07-21)** | Backend tamam, UI tarafı Lovable kuyruğunda bekliyor. |
| **İstanbul blokajı güncellendi (2026-07-21)** | Artık Swagger UI değil, ham `swagger/docs/v1` JSON çıktısı isteniyor (TLS sorunu nedeniyle sunucu tarafında alınamıyor). |

---

## 📋 Son Test Sonuçları

### Q3 Backfill + Unit (2026-07-21) ✅
| Kontrol | Sonuç |
|---|---|
| 21 günlük backfill, 17 başarılı | ✅ |
| 4 boş gün doğru şekilde açıklanabilir (Pazar×3 + bugün) | ✅ |
| Edge function v4, zarif boş-yanıt işleme | ✅ |
| Domates için 4 gerçek haftalık İzmir noktası | ✅ |
| RPC'lerde `unit` alanı (domates=kg, safran=g) | ✅ |

### (Önceki tüm test sonuçları — değişmedi, önceki sürümlerde)
