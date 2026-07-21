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
- [x] P18 serisi TAMAMEN TAMAMLANDI — 2026-07-20
- [x] P19 — Borsa Deneyimi: BACKEND + UI TAMAMEN CANLI — 2026-07-20/21
- [x] **İzmir hal genişletmesi: 3 → 27 ürün** (2026-07-21) — bkz. detay aşağıda

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
- [ ] Lovable kuyruk duraklatma sorunu (bkz. önceki not).

---

## 🏗️ Lovable/Supabase Build Sırası

> **P18 ve P19 (backend+UI) tamamen bitti. İzmir genişletmesi tamamlandı.** Sıradaki: **P19-C** devam ediyor — İstanbul entegrasyonu için Berkin'den Swagger çıktısı bekleniyor (bkz. aşağı, **açık blokaj**). Ardından Konya + global commodity API.

---

## 🎨 P18 — ARAYÜZ YENİLEME — **SERİ TAMAMEN TAMAMLANDI ✅**
(Değişmedi — bkz. önceki sürüm)

---

## 🟢 P19 — BORSA DENEYİMİ — **TAMAMEN TAMAMLANDI ✅**
(Değişmedi — bkz. önceki sürüm: backend, RPC'ler, İzmir pilotu, UI, routing — hepsi canlı doğrulandı)

---

## 🟠 P19-C — KAYNAK & ÜRÜN TİPİ GENİŞLETME *(devam ediyor — 2026-07-21)*

### ✅ Adım 1 — İzmir hal genişletmesi *(Tamamlandı — 2026-07-21, canlı doğrulandı)*
`crop_market_sources`'a 26 yeni satır eklendi (armut, biber, çilek, erik, greyfurt, havuç, ıspanak, kabak, karpuz, kavun, kayısı, kiraz, lahana, limon, marul, muz, nane, nar, patlıcan, portakal, salatalık, sarımsak, soğan, şeftali, üzüm, vişne). `sync-izmir-hal-prices` edge function'ının `CROP_PREFIXES` haritası genişletildi (v3). Canlı test (2026-07-17 verisiyle): **27 satır eklendi** (3 eski + 24 yeni — `nane`/`marul` beklenen şekilde atlandı, İzmir bülteninde bu ikisi KG değil ADET/BAĞ biriminde satılıyor, kod bunu doğru filtreliyor). Artık İzmir'den gelen toplam ürün sayısı **3 → 27**.

### 🔍 Adım 2 — Diğer kaynak araştırması *(Tamamlandı — 2026-07-21, top-3 belirlendi)*
- **`crop_config` tablosunda zaten 68 ürün var** — sadece taze sebze/meyve değil, buğday/mısır/pamuk/ayçiçeği/arpa/kanola/susam/nohut/mercimek/çavdar gibi bulk/ihracat ürünleri de dahil.
- **Top-3 öneri:**
  1. **İstanbul (İBB) — `halfiyatlaripublicdata.ibb.gov.tr`** — İzmir'den daha kapsamlı: birden fazla hal türü + tarihsel veri + Swagger dokümantasyonlu resmi web servis. **Canlı probe ile doğrulandı:** endpoint'ler gerçek ve JSON dönebiliyor (`Accept: */*` ile `/api/UrunFiyat/...` gerçek bir ASP.NET Web API 404 JSON hatası döndürdü — yani API canlı, sadece doğru action/route adı tahmin edilemedi). **🔴 AÇIK BLOKAJ: Berkin'den Swagger UI çıktısı (`https://halfiyatlaripublicdata.ibb.gov.tr/swagger/ui/index`, tarayıcıda açılıp yapıştırılacak) bekleniyor** — sayfa JS ile render olduğu için sunucu tarafından görülemiyor.
  2. **Konya Büyükşehir Belediyesi** — `acikveri.konya.bel.tr`, CKAN tabanlı "Meyve-Sebze Hal Fiyatları" veri seti + API (JSON/XML/CSV/TSV). Tarımsal açıdan da önemli (Konya, Hasat'ın tahıl ürünleriyle örtüşen bir bölge). Henüz canlı probe yapılmadı.
  3. **Global emtia API'si (buğday/mısır/pamuk/ayçiçeği için)** — bu 4 ürün hiçbir Türk halinde fiyatlanmıyor (bulk/ihracat malı, hal değil borsa/global piyasa ürünü), o yüzden gerçek "maksimum veri" değeri en yüksek buradan geliyor. **🔴 AÇIK BLOKAJ: API key/hesap kaydı gerekiyor** (`commodities-api.com` gibi bir sağlayıcıda, ücretsiz katman mevcut) — Berkin'in hesap açması gerekiyor, Claude bunu kendi başına yapamıyor.
- **Ankara** için "Şeffaf Ankara" açık veri platformu var ama hal fiyatları özelinde bir dataset bulunamadı (genel platform, spesifik doğrulama yapılmadı).

### Yapılacaklar (blokaj çözülünce devam)
1. **İstanbul:** Berkin'in Swagger çıktısı gelince → gerçek endpoint + response şeması netleşir → İzmir deseniyle aynı şekilde (idempotent edge function + `pg_cron` + `market_sources` kaydı) entegre edilir.
2. **Konya:** CKAN API'sinin canlı probe'u yapılacak (İzmir/İstanbul'a benzer bir edge function ile).
3. **Global commodity API:** Berkin hesap/key oluşturduğunda entegre edilecek — önce kaç çiftçinin buğday/mısır/pamuk/ayçiçeği gerçekten sattığı kontrol edilecek (boş bir "market" sayfası olmaması için).
4. Her yeni kaynak: idempotent edge function + `pg_cron` + güvenlik taraması + **her kaynak kendi ayrı kartında** kuralı (değişmez).

---

## 🟡 AĞUSTOS 2026 — Soft Launch
(Değişmedi)

## 🟣 P17 — GÜVEN ÇEKİRDEĞİ
(Değişmedi)

## 🔵 SAFRAN SEZONU / ⬜ SONRAKI FAZLAR
(Değişmedi)

---

## 📋 Lovable/Supabase Prompt Yazma Kuralları

(1-56 önceki sürümde — devam:)
57. **[Bu turda eklendi] Eski ASP.NET Web API'lerde (`.NET Framework` tabanlı belediye servisleri gibi) `Accept: application/json` veya `application/xml` başlığı 406 hatası verebilir — `Accept: */*` denenmeli.** Gerçek bir API 404'ü (JSON formatlı, "No HTTP resource was found" gibi) IIS'in statik 406/404 HTML hata sayfasından ayırt edilmeli; ilki API'nin canlı olduğunu, sadece route'un yanlış olduğunu gösterir.
58. **[Bu turda eklendi] Swagger UI sayfaları genellikle JS ile render olur, sunucu taraflı fetch/scrape ile içerik görülemez** — bu durumda kullanıcıdan tarayıcı çıktısını yapıştırması istenmeli (İzmir'de olduğu gibi), ya da spec dosyasının (`swagger.json`/`swagger/v1/swagger.json`) doğrudan çekilmesi denenmeli (her zaman aynı path'te olmayabilir).

---

## 📌 Kararlar

(önceki tablo + eklenenler:)
| **P19-C top-3 belirlendi (2026-07-21)** | İstanbul (İBB) > Konya > Global commodity API. İlk ikisi Türk hal ürünleri için, üçüncüsü Hasat'ın bulk/ihracat ürünleri (buğday/mısır/pamuk/ayçiçeği) için — hiçbir Türk halinde fiyatlanmayan tek kategori. |
| **İzmir genişletmesi tamamlandı (2026-07-21)** | 3 → 27 ürün, canlı doğrulandı. |
| **İki açık blokaj (2026-07-21)** | (1) İstanbul Swagger çıktısı Berkin'den bekleniyor. (2) Global commodity API için Berkin'in hesap/key açması gerekiyor. |

---

## 📋 Son Test Sonuçları

### İzmir Genişletmesi (2026-07-21) ✅
| Kontrol | Sonuç |
|---|---|
| `crop_market_sources`'a 26 satır eklendi | ✅ |
| Edge function v3, `CROP_PREFIXES` genişletildi | ✅ |
| Canlı test: 27 satır eklendi (3 eski+24 yeni) | ✅ |
| `nane`/`marul` doğru şekilde atlandı (birim uyuşmazlığı) | ✅ |

### İstanbul (İBB) Probe (2026-07-21) 🟡 Kısmi
| Kontrol | Sonuç |
|---|---|
| API canlı mı | ✅ (gerçek ASP.NET Web API 404 JSON hatası alındı) |
| Doğru `Accept` header | ✅ `*/*` çalışıyor |
| Doğru endpoint/route adı | ❌ Henüz bulunamadı — Berkin'den Swagger çıktısı bekleniyor |

### (Önceki tüm test sonuçları — değişmedi, önceki sürümlerde)
