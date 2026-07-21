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
- [x] P18 serisi TAMAMEN TAMAMLANDI (A/B/H/I/G/F/E/C/D) — 2026-07-20
- [x] **P19 — Borsa Deneyimi: BACKEND + UI TAMAMEN CANLI** (2026-07-20/21) — bkz. detay aşağıda

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
- [ ] `farmer.settings.tsx` accordion iç-içe kart görünümü (kozmetik).
- [ ] Lovable proje mesaj kuyruğu duraklatılmış (queue paused) olabilir — kontrol edilmeli, "queued but paused" hatasında tekrar gönderme, editörden açılmalı.
- [ ] **[Bu turda not edildi] `price_points` tablosu hâlâ ölü kod adayı** — aktif kullanımda değil, temizlik önerisi geçerliliğini koruyor.

---

## 🏗️ Lovable/Supabase Build Sırası

> **P18 ve P19 (backend+UI) tamamen bitti.** Sıradaki: **P19-C** (Kaynak & Ürün Tipi Genişletme — bu turda tanımlandı, aşağı bkz.). Ardından **Faz C / P17** (Eylül).

---

## 🎨 P18 — ARAYÜZ YENİLEME — **SERİ TAMAMEN TAMAMLANDI ✅** (2026-07-20)

Tüm fazlar (A→B→H→I→G→F→E→C→D) canlı doğrulandı. Detaylı doğrulama notları önceki sürümlerde — özet: tema/ortak component'ler, sohbet-önce ana sayfa akışı, retention katmanı, onboarding turu, ayarlar accordion'u, Fiyatlar/Analitik borsa-tarzı görünüm, vitrin paylaşım kartı, sipariş WhatsApp entegrasyonu, alıcı raporları kart tasarımı — hepsi `src/` içinde, backend'e dokunmadan tamamlandı, hepsi dosya okuması/diff ile canlı doğrulandı.

---

## 🟢 P19 — BORSA DENEYİMİ — **TAMAMEN TAMAMLANDI ✅** (2026-07-20/21)

### Backend *(Tamamlandı — 2026-07-20, canlı Supabase sorgularıyla doğrulandı)*
- **Şema:** `market_sources` (code, display_name, region), `crop_market_sources` (crop, source_code — many-to-many), `price_history.market_source_code` (opsiyonel), `source` CHECK constraint'i `'external'` değerini de kabul edecek şekilde genişletildi.
- **RPC'ler geriye dönük uyumlu genişletildi:** `get_price_history_summary`/`get_price_history_series` artık `market_sources`/`market_series` alanlarını da dönüyor — eski `hasat_data`/`official_data`/`hasat_series`/`official_series` alanları AYNEN duruyor, P18-F bozulmadı. Her kaynak kendi ayrı segmentinde, hiçbiri birleştirilmiyor (rekabet hukuku ayrımı genişletilerek korundu).
- **Pilot kaynak — İzmir Büyükşehir Belediyesi açık API'si** (`https://openapi.izmir.bel.tr/api/ibb/halfiyatlari/sebzemeyve/{yyyy-MM-dd}`, gerçek 2026-07-17 verisiyle canlı doğrulandı, tam ~90 ürünlük gerçek yanıt Berkin tarafından yapıştırılıp incelendi). Şu an entegre: domates/elma/patates.
- **`sync-izmir-hal-prices` edge function'ı:** deploy edildi (`verify_jwt=false`, mevcut `whatsapp-ai-webhook` deseniyle aynı), **idempotent** (aynı gün+crop+kaynak için önce silip yeniden yazıyor — tekrar tetiklemede mükerrer yok), `pg_net` ile canlı 200 OK test edildi (domates ₺41.79, elma ₺71.50, patates ₺25.75/kg, gerçek İzmir verisinden hesaplanmış).
- **`pg_cron` açıldı**, `sync-izmir-hal-prices-daily` job'ı AKTİF (jobid=1), her gün 06:00 UTC (09:00 TR) otomatik çalışıyor.
- Güvenlik taraması temiz, yeni risk kategorisi yok (`get_advisors` ile 2 kez doğrulandı).

### UI *(Tamamlandı — 2026-07-21, canlı doğrulandı)*
- `PricesPageBody.tsx`: 3 katmanlı akıllı önceliklendirme — "Ürünlerin"/"İlgilendiğin Ürünler" (çiftçi: `useFarmerListings`, alıcı: `useBuyerOrders`) → "İzleme Listesi" (`usePriceAlerts`) → "Tüm Piyasa" (arama çubuğu + varsayılan kapalı accordion, arama açıkken otomatik açılır).
- Her ürün kartı tıklanınca yeni bir detay sayfasına gidiyor: `/farmer/prices/$crop` ve `/buyer/prices/$crop` (yeni `CropDetailBody.tsx` + `PriceChart.tsx`). Zaman aralığı sekmeleri: Son 3 Ay (13 hafta) / Son 6 Ay (26 hafta) / Son 1 Yıl (52 hafta) — haftalık bucket'a uygun, gün/saat bazlı sahte kısa aralık yok.
- **Her kaynak kendi ayrı kartında:** "Hasat Topluluk Fiyatı", "Resmi Hal Fiyatı — {kaynak}" (varsa), ve her `market_sources` girdisi (şu an "İzmir Toptancı Hali") kendi başlığıyla ayrı ayrı — hiçbiri tek bir grafikte üst üste bindirilmiyor. Veri/seri yoksa o bölüm hiç render edilmiyor, icat edilmiş nokta yok.
- **Routing notu:** `farmer.prices.tsx`/`buyer.prices.tsx` → `.index.tsx` olarak yeniden adlandırıldı (TanStack Router konvansiyonu, `$crop` sibling route'u eklemek için gerekliydi). `routeTree.gen.ts` üzerinden doğrulandı: `to: "/farmer/prices"` (trailing slash'siz) hâlâ doğru çalışıyor, mevcut sidebar linkleri kırılmadı.
- `tsgo` temiz.

**Not (plan mode tutarlılığı):** Bu seride Lovable, `plan_mode=true` gönderilmesine rağmen 2 kez direkt build etti (bilinen platform tutarsızlığı, TODO kural #50/54'te not edilmişti). Her seferinde metne güvenilmeden gerçek diff/routeTree/Supabase sorgusuyla doğrulama yapıldı.

---

## 🟠 P19-C — KAYNAK & ÜRÜN TİPİ GENİŞLETME *(yeni — 2026-07-21, henüz başlanmadı)*

> Berkin'in isteği: farklı hallerin/borsaların ve global API'lerin entegrasyonu + bu kaynakların ürün kataloglarını değerlendirerek Hasat'ın kendi ürün tipi listesini genişletme.

### 🔍 Bu turda yapılan analiz (somut bulgular)
- **`crop_config` tablosunda zaten 68 ürün var** (memory notlarında sanılandan çok daha geniş) — sadece domates/elma/lavanta/safran/patates değil; armut, biber, buğday, çilek, erik, greyfurt, havuç, ıspanak, kabak, karpuz, kavun, kayısı, kiraz, lahana, limon, marul, mısır, muz, nane, nar, patlıcan, pamuk, portakal, salatalık, sarımsak, soğan, şeftali, üzüm, vişne, ayçiçeği, arpa, kanola, susam, nohut, mercimek, çavdar, fındık, zeytin, zeytinyağı ve daha fazlası dahil.
- **İzmir hal verisiyle çapraz kontrol (gerçek 2026-07-17 yanıtı üzerinden) — 26 EK ürün zaten örtüşüyor**, hiçbir yeni entegrasyon gerekmeden: armut, biber, çilek, erik, greyfurt, havuç, ıspanak, kabak, karpuz, kavun, kayısı, kiraz, lahana, limon, marul, muz, nane, nar, patlıcan, portakal, salatalık, sarımsak, soğan, şeftali, üzüm, vişne. **Bunlar için sadece `crop_market_sources`'a satır eklemek + `sync-izmir-hal-prices` edge function'ındaki `CROP_PREFIXES` eşleme listesini genişletmek yeterli** — düşük efor, yüksek etki.
- **Küresel emtia API'leriyle gerçek örtüşme sınırlı:** `crop_config`'teki buğday/mısır/pamuk/ayçiçeği (wheat/corn/cotton/sunflower) küresel borsalarda (CBOT/ICE tipi) gerçekten işlem gören ürünler — bunlar için `commodities-api.com` gibi (önceki turda araştırılan) ücretsiz katmanlı global API'ler gerçek bir seçenek. Diğer bulk ürünler (arpa/kanola/susam/nohut/mercimek/çavdar) için küresel API kapsamı daha zayıf/tutarsız, öncelik düşük.

### Yapılacaklar (kapsam netleşince plan_mode ile gönderilecek)
1. **İzmir eşleşen 26 ürünü aktifleştir** — `crop_market_sources`'a satır ekle (migration), edge function'ın `CROP_PREFIXES` map'ini genişlet, İzmir bülteninde her ürünün gerçek çeşit-adı önekini doğrula (domates/elma'da yapılan "çeşitlerin ortalaması" mantığı tekrarlanacak).
2. **Diğer büyükşehir belediyeleri açık API araştırması** — Ankara, İstanbul, Konya, Bursa, Antalya için İzmir'dekine benzer bir `openapi.*.bel.tr` veya açık veri portalı var mı, tek tek doğrulanacak (İzmir'de olduğu gibi web_fetch ile değil, gerekirse edge function probe'uyla — web_fetch aracı sadece önceden görülen URL'leri çekebiliyor).
3. **TOBB Ticaret Borsaları ağı (İzmir/Ankara/Konya Ticaret Borsaları)** — bunlar hal'den farklı, gerçek emtia borsaları (fındık/zeytin/buğday gibi ürünlerde daha güçlü kaynak) — API'leri var mı yoksa sadece PDF/HTML bülten mi, araştırılacak.
4. **Küresel commodity API pilot (buğday/mısır/pamuk/ayçiçeği için)** — `commodities-api.com` gibi bir sağlayıcının ücretsiz katmanı gerçek anahtar ile test edilecek, `market_sources`'a `code='global_commodity'` gibi bir kayıt + `crop_market_sources` eşlemesi eklenecek. Not: bu ürünler Hasat'ın çiftçi tabanında şu an aktif üretilmiyor olabilir — önce kaç çiftçinin bu ürünleri gerçekten sattığı kontrol edilmeli, aksi halde boş bir "market" sayfası olur.
5. Her yeni kaynak için: idempotent edge function + `pg_cron` job (İzmir deseniyle aynı), güvenlik taraması, ve **her kaynak kendi ayrı kartında** kalması (asla birleştirilmez) kuralı korunacak.

---

## 🟡 AĞUSTOS 2026 — Soft Launch
(Değişmedi)

## 🟣 P17 — GÜVEN ÇEKİRDEĞİ
(Değişmedi — P19-C ile paralel değerlendirilebilir)

## 🔵 SAFRAN SEZONU / ⬜ SONRAKI FAZLAR
(Değişmedi)

---

## 📋 Lovable Prompt Yazma Kuralları

(1-54 önceki sürümde — devam:)
55. **[Bu turda eklendi] Yeni bir dış veri kaynağı/API entegrasyonu önerilirken, önce Hasat'ın KENDİ mevcut ürün kataloğu (`crop_config`) ile kaynağın ürün kataloğu çapraz kontrol edilmeli** — "hangi ürünleri destekleyelim" sorusunun cevabı çoğunlukla zaten şemada var, sadece bağlanmamış.
56. **[Bu turda eklendi] `web_fetch` aracı sadece önceden arama/fetch sonucunda görünen URL'leri çekebiliyor — tarih/parametre doldurarak oluşturulan URL'ler reddediliyor.** Böyle bir API'yi canlı test etmek için ya kullanıcıdan tarayıcı çıktısı istenmeli, ya da gerçek bir Supabase Edge Function/`pg_net` ile sunucu tarafında sınanmalı.

---

## 📌 Kararlar

(önceki tablo + eklenenler:)
| **P18/P19 tamamen bitti (2026-07-21)** | Her ikisi de canlı doğrulandı, ayrıntılar yukarıda. |
| **Faz D veri yoğunluğu kararı** | Seçenek (b) — domates için hedefli demo verisi eklendi (değişmedi). |
| **P19 çoklu-borsa kararı** | "Farklı borsalar" = ayrı dış kaynaklar, yeni backend modeli (değişmedi, uygulandı). |
| **P19-C açıldı (2026-07-21)** | Diğer hal/borsa + global API entegrasyonu ve ürün tipi genişletme analizi — somut bulgular yukarıda, henüz build başlamadı. |

---

## 📋 Son Test Sonuçları

### P19 Backend + UI (2026-07-20/21) ✅ — tamamı canlı doğrulandı
| Kontrol | Sonuç |
|---|---|
| Migration'lar sadece yeni tablo/kolon/RPC, mevcut şema/RLS değişmedi | ✅ |
| `sync-izmir-hal-prices` idempotent, gerçek 200 OK | ✅ |
| `pg_cron` job aktif, günlük 09:00 TR | ✅ |
| RPC'ler geriye dönük uyumlu (`hasat_data`/`official_data` bozulmadı) | ✅ |
| `PricesPageBody` 3 katmanlı öncelik + `/prices/$crop` detay sayfası | ✅ |
| Routing (`routeTree.gen.ts` üzerinden `to: "/farmer/prices"` doğrulaması) | ✅ |
| Her kaynak ayrı kartta, birleştirme yok | ✅ |
| Güvenlik taraması (2 kez) | ✅ Temiz |

### (Önceki tüm test sonuçları — değişmedi, önceki sürümlerde)
