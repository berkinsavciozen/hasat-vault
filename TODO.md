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
- [x] P17-E — Yapılandırılmış RFQ (talep akışı) TAMAMLANDI, uçtan uca canlı veriyle bizzat doğrulandı (2026-07-21)
- [x] **P17-G — KPI ölçüm view'ları (20 view: North Star + çekirdek + çiftçi/alıcı/platform genişletmesi) TAMAMEN TAMAMLANDI**
- [x] **Admin KPI Dashboard (`/admin/kpi`, gizli route + anahtar korumalı) TAMAMLANDI — P17 serisinin son bağımsız fazı kapandı**

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

> **P19 + P17-B + P17-C + P17-F + P17-E + P17-G (backend + admin dashboard) tamamen bitti, hepsi canlı doğrulandı.** **P17-A ve P17-D şirket kuruluşuna bağlı, bloke.** P17 serisinden bağımsız yapılabilecek her şey bitti — sıradaki büyük karar: yeni bir P-serisi mi (örn. lojistik/kargo takibi, SMS/WhatsApp bildirim genişletmesi — BENCHMARK Gap #8/#10), yoksa doğrudan Ağustos soft-launch hazırlığına mı geçilecek.

---

## 🎨 P18 — ARAYÜZ YENİLEME — **SERİ TAMAMEN TAMAMLANDI ✅**
(Değişmedi)

## 🟢 P19 — BORSA DENEYİMİ — **TAMAMEN TAMAMLANDI ✅**
(Değişmedi)

## 🟠 P19-C — KAYNAK & ÜRÜN TİPİ GENİŞLETME — **KAPANDI**
(Değişmedi — bkz. önceki sürüm)

---

## 🟣 P17 — GÜVEN ÇEKİRDEĞİ — **SERİ TAMAMEN TAMAMLANDI ✅** *(2026-07-21)*

### Kapsam ve sıralama
| Kod | Konu | Şiddet | Durum |
|---|---|---|---|
| P17-B | Teslim/kargo/iptal/ihtilaf akışı | P0 | ✅ **TAMAMLANDI** |
| P17-C | Karşılıklı değerlendirme (rating/review) | P0 | ✅ **TAMAMLANDI** |
| P17-F | Tekrar sipariş + şube adresleri + abonelik bağlantısı | P0 (HoReCa) | ✅ **TAMAMLANDI** |
| P17-E | Yapılandırılmış RFQ (talep akışı) | P1 | ✅ **TAMAMLANDI** |
| P17-G | KPI ölçüm görünümleri + admin dashboard | destek | ✅ **TAMAMLANDI** (backend + UI) |
| P17-A | Gerçek bloke ödeme (escrow) | P0 | 🔴 Bloke — şirket kuruluşu + iyzico başvurusu |
| P17-D | Fatura/e-müstahsil | P0 (B2B) | 🔴 Bloke — vergi mükellefiyeti gerektiriyor |

Yalnızca P17-A ve P17-D kaldı, ikisi de şirket kuruluşuna bağlı — bağımsız olarak ilerlenebilecek başka madde yok.

### ✅ P17-B / P17-C / P17-F / Abonelik-Sipariş Bağlantısı — TAMAMLANDI
(Değişmedi — bkz. önceki sürüm)

### ✅ P17-E — Yapılandırılmış RFQ (Talep Akışı) — TAMAMLANDI *(2026-07-21, kapsamlı canlı doğrulama)*

**Bulgu:** `crop_requests` tablosu vardı ama sadece serbest metin + status — miktar/bölge/tarih/fiyat alanı yoktu. `useCreateCropRequest` hook'u tanımlıydı ama hiçbir UI'da hiç çağrılmıyordu.

**Uygulanan çözüm:**
- **Şema** ✅ — `crop_requests`'e `quantity`, `unit`, `region`, `target_date_start`, `target_date_end`, `target_price` kolonları eklendi.
- **Talep oluşturma UI'sı** ✅ — `buyer.discover.tsx`'in boş arama sonucu ekranına "Bu ürünü talep et" butonu + modal.
- **Eşleşme + bildirim** ✅ — `useCreateCropRequest`, `crop_config` üzerinden canonical eşleşme + `listings`/`parcels.crops` üzerinden çiftçi bulma + `notifications` insert.
- **"Taleplerim" sayfası** ✅ — `buyer.requests.tsx`, hesap sayfasına link.

**Doğrulama:** canonical eşleşme, çiftçi bulma, bildirim satırı hepsi gerçek SQL ile ayrı ayrı simüle edilip doğrulandı, test verisi temizlendi.

### ✅ P17-G — KPI Ölçüm Görünümleri + Admin Dashboard — TAMAMEN TAMAMLANDI *(2026-07-21)*

**Kapsam:** BENCHMARK.md §5 KPI çerçevesinin tamamı — North Star + P0 çekirdek (P17-G-1) + çiftçi/alıcı/platform genişletmesi (P17-G-2) + bunları gösteren admin dashboard (P17-G-UI). Tamamı Claude tarafından doğrudan Supabase MCP ile (backend) ve Lovable ile (dashboard UI) yapıldı.

**Kurulan 20 view (`public` şema, hepsi sadece `service_role`'e açık):**

*Çekirdek / North Star (P17-G-1):*
- `v_kpi_order_base` — temel view: her sipariş için tutar (`amount`), ürün, taraflar, `reached_delivery`, `is_realized_sale`, `has_dispute`
- `v_kpi_north_star` — aylık toplam/ihtilafsız GMV + pay % (hedef ≥%95)
- `v_kpi_dispute_rate` — aylık ihtilaf oranı (hedef ≤%2)
- `v_kpi_full_acceptance_rate` — aylık tam kabul oranı (hedef ≥%95)
- `v_kpi_buyer_repeat_rate` — segment bazlı tekrar alıcı oranı
- `v_kpi_review_avg` — kullanıcı/role/genel bazlı ortalama puan

*Çiftçi tarafı (P17-G-2):*
- `v_kpi_farmer_activation`, `v_kpi_listing_offer_rate`, `v_kpi_farmer_sellthrough`, `v_kpi_farmer_gmv`, `v_kpi_farmer_retention` (M1/M3 kohort), `v_kpi_farmer_verified_pct`

*Alıcı tarafı (P17-G-2):*
- `v_kpi_buyer_activation`, `v_kpi_buyer_aov_segment`, `v_kpi_horeca_order_frequency`, `v_kpi_buyer_gmv_retention` (M0→M1 kohort)

*Platform (P17-G-2):*
- `v_kpi_buyer_seller_ratio` (il bazlı), `v_kpi_supply_density` (il×ürün ≥3 satıcı), `v_kpi_offer_conversion`, `v_kpi_price_vs_market` (sadece İzmir pilot ürünleri: domates/elma/patates)

**Önemli tasarım notu:** `orders.status`'ta `completed` değeri henüz kullanılmıyor (veri: 110 delivered/9 preparing/1 shipped) — view'lar `status IN ('delivered','completed')` mantığıyla yazıldı, gerçek "completed" akışı devreye girince otomatik kapsanacak.

**🔴 Güvenlik bulgusu + düzeltme (P17-G-1'de bulundu, tüm 20 view'a uygulandı):** Yeni view'lar varsayılan olarak `anon`/`authenticated`'a SELECT (+ anlamsız INSERT/UPDATE/DELETE) yetkisiyle açılıyor ve RLS'i bypass ediyor (definer=postgres). Her batch sonrası `REVOKE ALL ... FROM anon, authenticated` + sadece `service_role`'e `GRANT SELECT` uygulandı ve `information_schema.role_table_grants` ile doğrulandı — **20 view'ın hiçbirinde sızıntı yok.**

**Bulunup düzeltilen 2 gerçek SQL/mantık bug'ı (kendi doğrulama sürecimde):**
1. `v_kpi_buyer_aov_segment`'te GROUPING SETS'in genel toplam satırı ile gerçek "segment=NULL" satırı aynı `'bilinmiyor'` etiketiyle çakışıyordu → `GROUPING()` fonksiyonuyla ayrıştırıldı, doğrulandı (6+13+42+10+39+0=110=genel toplam).
2. `v_kpi_farmer_retention`'da yanlışlıkla `GROUP BY`'a `farmer_id` eklenmişti, bu da kohort-ay bazında agregasyonu bozuyordu (her satır 1 çiftçiye düşüyordu) → düzeltildi, 4 kohortun toplamı (1+2+5+2=10) diğer view'lardaki aktif çiftçi sayısıyla tutarlı çıktı.

**Admin Dashboard (`/admin/kpi`, P17-G-UI):**
- Lovable'a gönderildi (`plan_mode=true` ile, ama bilinen bug nedeniyle yine direkt build etti — her adımda kod/deploy/curl kanıtıyla bağımsız doğrulandı, agent'ın metnine güvenilmedi).
- Mimari: `admin-kpi` Supabase Edge Function (service-role client, `x-admin-key` header + `ADMIN_DASHBOARD_KEY` secret ile timing-safe karşılaştırma, `verify_jwt=false`) + `/admin/kpi` route (sidebar/navigasyona **eklenmedi**, sadece URL ile erişilir) + 4 sekme (Genel/Çiftçi/Alıcı/Platform), tüm 20 view'ı kapsıyor.
- Anahtar sadece React state'te tutuluyor, localStorage/sessionStorage **kullanılmıyor** (bilinçli tercih).
- **Bulunup düzeltilen 2 gerçek bug (ilk turda):**
  1. Edge function `v_kpi_order_base`'den var olmayan `gmv` kolonunu okumaya çalışıyordu (gerçek ad `amount`) — hata `safe()` wrapper'da sessizce yutulup "Toplam GMV" kartı 0 gösterecekti. Kod okunarak yakalandı, düzeltildi, düzeltme sonrası `totals.total_gmv=12.887,372` kendi elimde topladığım north_star aylık toplamlarıyla (133.232+...+182.217) birebir eşleşti.
  2. **Kullanıcı bildirdi (2026-07-21): "Şifre girince hiçbir şey olmuyor."** Kök neden: Supabase-js her `functions.invoke` çağrısına otomatik `x-client-info` header'ı ekliyor, edge function'ın CORS `Access-Control-Allow-Headers` listesinde bu yoktu → tarayıcı preflight'ta isteği engelliyordu, istek edge function'a hiç ulaşmıyordu. Ayrıca hata yakalama kodu sadece `401` durumunu kontrol ediyordu, başka hiçbir hata için toast göstermiyordu (bu yüzden "hiçbir şey olmuyor" hissi). Düzeltme: CORS'a `x-client-info` eklendi + hata yakalama genişletildi (401/403/5xx/genel, hepsi toast gösteriyor) + Playwright ile gerçek tarayıcı testiyle (ekran görüntüleriyle) doğrulandı: yanlış anahtar → 401 + "Hatalı anahtar" toast'ı; doğru anahtar → 200 + dashboard render.

**Erişim (Berkin için):**
- URL: `{preview-veya-production-domain}/admin/kpi`
- Anahtar: Supabase secret `ADMIN_DASHBOARD_KEY` (değer bende ve Berkin'de kayıtlı, şifre yöneticisinde saklanmalı, kimseyle paylaşılmamalı)

---

## 🟡 AĞUSTOS 2026 — Soft Launch
(Değişmedi)

## 🔵 SAFRAN SEZONU / ⬜ SONRAKI FAZLAR
(Değişmedi)

---

## 📋 Lovable/Supabase Prompt Yazma Kuralları

(1-79 önceki sürümde — devam:)
80. **Bir hook/mutation tanımlı ama HİÇ UI'dan çağrılmıyorsa, bu "eksik özellik" değil "yarım bırakılmış özellik" anlamına gelir** — önce kodun var olup olmadığına değil, gerçekten bir kullanıcı yolundan tetiklenip tetiklenmediğine bakılmalı.
81. **Eşleşme/bildirim gibi çok adımlı bir mantığı doğrularken, her adımı AYNI SQL sorgularıyla ayrı ayrı simüle etmek, sadece "kod var" demekten çok daha güçlü bir kanıt sağlar.**
82. **Yeni bir Supabase view/tablo oluşturulduğunda, varsayılan grant'lar (`anon`/`authenticated`'a otomatik SELECT) kontrol edilmeden bırakılmamalı** — özellikle RLS'i bypass eden view'larda (definer=postgres), bu tek başına bir veri sızıntısıdır. Her yeni view/tablo sonrası `information_schema.role_table_grants` ile kontrol + gereksiz grant'ları `REVOKE` etmek standart adım.
83. **`apply_migration` tek bir transaction olarak çalışıyor** — son statement hata verirse önceki statement'lar (view'lar dahil) da BAŞARILI olsa bile rollback olur. Hatadan sonra `pg_views`/`information_schema` ile gerçekte ne kaldığı kontrol edilmeli.
84. **[Bu turda eklendi] GROUPING SETS kullanırken, "genel toplam" satırının etiketi ile gerçek NULL/bilinmeyen kategori satırının etiketi çakışmamalı** — `COALESCE(kolon, 'bilinmiyor')` her iki durumda da aynı sonucu verir; `GROUPING()` fonksiyonu ile ayrıştırılmalı, aksi halde iki farklı anlam aynı satırda birleşir (sessiz veri bozulması).
85. **[Bu turda eklendi] Agregasyon view'larında `GROUP BY`'a yanlışlıkla bir "detay" kolonu (örn. `farmer_id`) eklenirse, view sözdizimsel olarak çalışır ama anlamsal olarak yanlış agregasyon üretir** — her satır beklenen kohort/ay yerine tek bir varlığa düşer. Bu tür hatalar hata vermez, sadece SONUÇLARI çapraz kontrol ederek (başka bir view'daki toplamla karşılaştırarak) yakalanabilir.
86. **[Bu turda eklendi] Lovable/edge function tarafında bir Supabase view'ından `.select("kolon_adı")` yazarken kolon adı birebir view tanımıyla eşleşmeli** — yanlış kolon adı Postgrest hatası üretir ama bu hata `try/catch` ile sessizce yutulursa (örn. "hata olursa null dön" deseni), sonuç UI'da "0" veya "veri yok" olarak görünür, hiçbir uyarı vermez. Yeni bir view entegre edilirken Lovable'a TAM kolon adları listesi verilmeli, kontrol edilmeden geçilmemeli.
87. **[Bu turda eklendi] Bir edge function'a tarayıcıdan `supabase.functions.invoke()` ile istek atılırken, supabase-js kütüphanesinin otomatik eklediği header'lar (örn. `x-client-info`) edge function'ın CORS `Access-Control-Allow-Headers` listesinde YOKSA, istek preflight aşamasında sessizce engellenir — hiçbir network log'u, hiçbir hata mesajı görünmez, kullanıcıya "hiçbir şey olmuyor" gibi gelir. Yeni bir edge function + frontend entegrasyonunda CORS header listesi supabase-js'in gerçekte gönderdiği header'larla eşleştirilmeli, sadece "makul görünen" bir liste yazılmamalı.
88. **[Bu turda eklendi] Hata yakalama kodunda sadece BEKLENEN hata durumu (örn. 401) için kullanıcıya mesaj gösterilirse, beklenmeyen her hata (CORS, network, 500, farklı bir status kodu) sessizce yutulur ve kullanıcı deneyimi "hiçbir şey olmuyor" gibi görünür. Hata yakalama her zaman "tanımadığım hata" durumu için de genel bir mesaj göstermeli.**

---

## 📌 Kararlar

(önceki tablo + eklenenler:)
| **P17-E tamamlandı (2026-07-21)** | Şema+UI+eşleşme+bildirim+liste sayfası, hepsi gerçek verilerle uçtan uca doğrulandı. |
| **P17-G tamamen tamamlandı (2026-07-21)** | 20 KPI view'ı (North Star + çiftçi + alıcı + platform) + admin dashboard (`/admin/kpi`) canlı, gerçek veriyle doğrulandı. Süreçte bulunan 4 gerçek bug (2 SQL mantık hatası, 1 kolon adı hatası, 1 CORS/hata-yakalama hatası) hepsi düzeltildi ve bağımsız doğrulandı. **P17 serisinden bağımsız yapılabilecek her şey bitti** — kalan P17-A/P17-D şirket kuruluşuna bağlı. |

---

## 📋 Son Test Sonuçları

### P17-G Tam Doğrulama (2026-07-21) ✅
| Kontrol | Sonuç |
|---|---|
| 20 view'ın tamamı `pg_views`'ta mevcut, hepsi service_role-only | ✅ |
| North Star / dispute-rate / full-acceptance tutarlılığı (disputes=0 satır) | ✅ %0/%100/%100, birbiriyle tutarlı |
| Çapraz doğrulama: aktif çiftçi/alıcı sayıları 4+ farklı view'da birebir eşleşiyor (10 alıcı, 10-11 çiftçi) | ✅ |
| `v_kpi_buyer_aov_segment` GROUPING bug'ı | ✅ Bulundu, düzeltildi, doğrulandı |
| `v_kpi_farmer_retention` GROUP BY bug'ı | ✅ Bulundu, düzeltildi, doğrulandı |
| Admin dashboard edge function `gmv`→`amount` kolon bug'ı | ✅ Bulundu, düzeltildi, north_star toplamıyla eşleştirilerek doğrulandı |
| Admin dashboard CORS + hata-yakalama bug'ı ("şifre girince hiçbir şey olmuyor") | ✅ Bulundu (Playwright ile), düzeltildi, ekran görüntüsüyle doğrulandı |
| anon/authenticated grant sızıntısı (tüm 20 view) | ✅ Sıfır yetki, sadece service_role SELECT |

### P17-E Tam Doğrulama (2026-07-21) ✅
(Değişmedi — bkz. önceki sürüm)

### (Önceki tüm test sonuçları — değişmedi, önceki sürümlerde)
