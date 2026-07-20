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
- [x] Premium Sayfası Yeniden İnşası + Referral Ödül Mekanizması + AI Kredi/Limit Sistemi + Çerez Kontrolü + 5 MCP Tool Canlı Testi
- [x] **Bildirim Ayarları DB Bağlantısı + SMS Mimarisi Client'tan DB'ye Taşındı + AI Chat Kalite Denetimi** (2026-07-19) — bkz. detaylı bölüm aşağıda

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
- [x] ~~Çerez onay banner'ı~~ — Tamamlandı, gerek yok.
- [ ] **[Benchmark re-audit] iyzico başvurusunda pazaryeri modeli + komisyon teklifi talebi** — hâlâ açık, insan aksiyonu.
- [x] ~~Gelir modeli kararlarının üründe doğrulanması~~ — Tamamlandı.
- [ ] **[Benchmark re-audit] Toplu "ilk N kullanıcıya otomatik 6 ay premium" feature-flag mekanizması** — hâlâ açık.
- [x] ~~Referral suistimal koruması~~ / ~~AI kredi/limit mekanizması~~ — Tamamlandı.

### Düşük öncelikli cila
- [ ] `farmer.journal.new.tsx` "Önce bir parsel ekle" statik metni · `formatCrop()` heuristik · Landing page'de çok fazla rol-seçim butonu
- [ ] MCP yazma tool'larında aralıklı "No approval received" hatası — retry ile geçiyor
- [ ] **Lovable platform bug'ı (bilinen, düzeltilemez):** `plan_mode=false` sinyali agent'ın kod tool'larına bazen yansımıyor.
- [ ] Storage bucket public/private ayarını her zaman canlı doğrula
- [ ] **[Audit'te bulundu] `crop_requests` tablosu için admin inceleme arayüzü** — Claude periyodik kontrol edecek (P17-E'ye kadar)
- [ ] **[Bu turda bulundu, düşük öncelik] `create_subscription` rol-temizliği eksik** — `harvest_subscriptions.buyer_id`'nin `role='buyer'` bir profile ait olduğunu zorlayan bir CHECK eklenebilir.
- [ ] **[Bu turda bulundu, kozmetik] `FarmerAIChat.tsx` header'ı "Premium • sınırsız sohbet"** — artık 500/ay tavanı var, "ayda 500 mesaj" gibi düzeltilebilir.
- [ ] **[Bu turda bulundu] `useHasat.setNotifPref` + store'daki `notifPrefs` slice'ı artık dead code** — bildirim ayarları gerçek DB'ye taşındığı için bu local state hiç kullanılmıyor, temizlenebilir (Lovable'ın kendi notu).
- [ ] **[Bu turda bulundu, karar bekliyor] AI chat'te "hava sor" ipucu tamamen kurgusal** — hiç hava durumu kaynağı yok. Karar bekleniyor: (a) ipucunu kaldır, (b) Open-Meteo gibi ücretsiz bir kaynakla gerçek hava bağlamı ekle.

---

## 🏗️ Lovable Build Sırası — TAMAMLANDI ✅

> Sıradaki: aşağıdaki **"Sıradaki Faz Planı"** bölümüne bkz.

---

### ✅ Bildirim Ayarları + SMS Mimarisi + AI Chat Kalite Denetimi *(Tamamlandı — 2026-07-19)*

**1. Bildirim tercihleri ayarları — 7. "sahte" bulgu, düzeltildi:** `farmer.settings.notifs.tsx` yine `useHasat` local store'a yazıyordu, DB'ye hiç dokunmuyordu. `notif_prefs` tablosunda **sıfır satır** vardı (tüm SMS'ler sessizce opt-out). **Düzeltme:** Sayfa gerçek `notif_prefs` tablosuna bağlandı (`useNotifPrefs`/`useUpdateNotifPrefs`), 22 mevcut kullanıcı için varsayılan satır oluşturuldu (`new_offer_sms=true`), `handle_new_user` yeni kayıtlar için de aynısını yapacak şekilde güncellendi.

**2. 🔴 Bugünün en önemli mimari bulgusu — SMS gönderimi client-side'da yaşıyordu, DB'de hiç değildi:** Canlı E2E testinde (MCP ile gerçek bir teklif oluşturulup loglar kontrol edilerek) bulundu — `send-sms` fonksiyonu **sadece** tarayıcıdan `useCreateOffer`'ın `onSuccess`'inde çağrılıyordu. MCP, direkt SQL, veya gelecekteki her türlü entegrasyon bu yolu **atlıyor**, SMS asla gönderilmiyordu. Ayrıca kabul/karşı teklif/ödeme onayı olaylarında **hiç** SMS bağlantısı yoktu. **Düzeltme:** `pg_net` uzantısı açıldı, yeni `dispatch_sms()` fonksiyonu (notif_prefs kontrolü + Twilio çağrısı) `notify_offer_received()` trigger'ına bağlandı, client-side çağrı kaldırıldı. **Gerçek bir Twilio isteğiyle (201 yanıtı) canlı doğrulandı** — test verisi temizlendi. Kapsam bilinçli olarak sadece `new_offer` ile sınırlı tutuldu (diğer olaylar için `notif_prefs`'te henüz SMS kolonu yok) — genişletme aşağıya, sıradaki faz planına taşındı.

**3. AI Chat kalite denetimi — 2 bulgu:**
   - Alıcı için AI chat hiç yok (aktif bug değil, ama component ileride alıcıya taşınırsa `fetchContextBlock`'un hep boş döneceği ve sistem promptunun çiftçi diliyle konuşacağı doğrulandı — not edildi).
   - **🔴 8. "vaat edilen ama arkası boş" bulgusu:** "Hava sor" ipucu (`FarmerAIChat.tsx`) tamamen kurgusal, hiçbir hava durumu kaynağı yok — bir çiftçi sorarsa model tamamen halüsinasyon üretir. Karar bekliyor (yukarı bkz.).

**Maliyet:** Bildirim ayarları 3.3 + SMS mimarisi 2.8 + AI chat denetimi 2 = **8.1 kredi**.

---

### ✅ (Önceki tüm tamamlanan işler — Premium/Referral/AI Limit, Sertifika/useHasat/Gating, Audit Takibi, Detaylı E2E, Alıcı Sayfaları, Mock Data, Fiyatlandırma, Tedarik Zinciri, Landing, RLS, Rekabet Hukuku, MCP Server, Benchmark Re-Audit, P16 serisi)

(Detaylar önceki sürümlerde — değişmedi)

---

## 🧭 SIRADAKİ FAZ PLANI (2026-07-19 itibarıyla)

> Bugüne kadarki her tur, "bir önceki turun bulduğu son 1-2 maddeyi kapat" şeklinde ilerledi ve her seferinde yeni, daha derin bir "sahte veri/kırık akış" bulgusu ortaya çıkardı (bugüne kadar toplam **8 bağımsız örnek**: journal foto, Üretici Detay, Abonelik Oluştur, Premium sayfası, referral ödülü, bildirim ayarları, SMS mimarisi, hava sor). Bu noktada, tek tek "bir sonraki küçük şey" yerine, **kalan kapsamı 3 net fazda** gruplamak daha sağlıklı:

### Faz A — Ağustos E2E Kapsamını Resmi Olarak Kapatmak (GTM'den önce, kısa)
Kalan sadece 2 gerçek açık madde var:
1. **Community reply UI (alıcı tarafı)** — henüz hiç bakılmadı, muhtemelen benzer bir "gerçek mi sahte mi" kontrolü gerekiyor.
2. **"Hava sor" kararı** — (a) kaldır ya da (b) gerçek Open-Meteo entegrasyonu. Karar + varsa implementasyon.

Bunlar bitince **Ağustos E2E kapsamı resmi olarak tamamlanmış olur** — TODO'daki "Sıradaki: P17" notu gerçek anlamda doğru hale gelir.

### Faz B — Bugünün Bulduğu Takip İşleri (küçük, dağınık, ama gerçek)
Bugünkü 8 bulgunun bıraktığı **küçük ek işler** (hiçbiri acil değil, ama biriktirmemek için toplu halledilebilir):
- SMS kapsamını genişletme: `notif_prefs`'e `offer_accepted_sms`, `payment_confirmed_sms` gibi kolonlar + `notify_order_status()`/`notify_offer_accepted()` trigger'larına `dispatch_sms()` bağlama (bugünün deseniyle birebir aynı, tekrarı kolay)
- `create_subscription` rol-temizliği CHECK'i
- `useHasat` dead code temizliği (bildirim slice'ı)
- Kozmetik: "Premium • sınırsız" → "ayda 500 mesaj"
- Toplu premium feature-flag mekanizması (Benchmark'tan kalan)

### Faz C — P17 Güven Çekirdeği (Eylül, büyük)
TODO'daki P17-A/B/C/D/E/F/G serisi — bloke ödeme, teslim/ihtilaf akışı, gerçek review sistemi, fatura, RFQ, tekrar sipariş, KPI görünümü. Bu, **Faz A bitmeden başlamamalı** (roadmap'in kendi sıralaması da bunu söylüyor).

**Önerim:** Şimdi **Faz A**'ya devam edelim (2 madde, kısa) — bitince Faz B'yi tek bir toplu turda hallederiz, sonra P17'ye geçeriz. Onaylıyor musun?

---

## 🟡 AĞUSTOS 2026 — Soft Launch — ŞİMDİ BURADAYIZ

### E2E Test — devam ediyor
- [x] S1-S13 · Fiyatlandırma/Keşfet/Mesajlar/Raporlar/Abonelikler/Üretici Detay/Fotoğraflar · Sertifika/useHasat/Gating · Premium/Referral/AI Limit · **Bildirim ayarları + SMS mimarisi + AI chat denetimi** — hepsi ✅
- [ ] Kalan kapsam (Faz A): **community+reply UI**, **AI chat "hava sor" kararı**

### Teknik Final
- [ ] hasat.lovable.app → custom domain yayını · iyzico canlı ödeme testi
- [x] P16-C tam havale e2e testi — kapandı ✅

---

## 🟣 P17 — GÜVEN ÇEKİRDEĞİ *(Eylül 2026 — Faz C, bkz. yukarı)*

(Değişmedi — bkz. önceki sürüm: P17-A/B/C/D/E/F/G)

---

## 🔵 SAFRAN SEZONU / ⬜ SONRAKI FAZLAR

(Değişmedi — bkz. önceki sürüm)

---

## 📋 Lovable Prompt Yazma Kuralları

(1-41 önceki sürümde — devam:)
42. **[Bu turda eklendi] Bir bildirim/entegrasyon özelliğini test ederken, sadece "kod var mı" değil "hangi YOL üzerinden tetikleniyor" sorusu sorulmalı** — SMS kodu gerçekten vardı ve çalışıyordu, ama sadece TEK bir giriş noktasına (tarayıcı) bağlıydı. Bugünün en derin bulgusu buydu — "çalışıyor" ile "her yoldan çalışıyor" farklı şeyler.
43. **[Bu turda eklendi] Çok sayıda ardışık "bul-düzelt" turu sonrası, bir noktada durup kalan işi FAZLARA ayırmak gerekir** — sürekli "bir sonraki küçük şey"i kovalamak, büyük resmi (Ağustos kapanışı vs Eylül P17 başlangıcı) gözden kaçırabilir.

---

## 📌 Kararlar

(önceki tablo + eklenenler:)
| **SMS mimarisi (2026-07-19)** | Dispatch client-side'dan DB'ye (`pg_net` + `dispatch_sms()`) taşındı — kaynak (web/MCP/gelecek) fark etmeksizin tutarlı çalışır. Kapsam şimdilik sadece `new_offer`, genişletme Faz B'ye bırakıldı. |
| **Faz planı (2026-07-19)** | Kalan iş Faz A (Ağustos E2E kapanışı — community reply + hava sor kararı) → Faz B (bugünün küçük takip işleri, toplu) → Faz C (P17, Eylül) olarak gruplandı. |

---

## 📋 Son Test Sonuçları

### Bildirim Ayarları + SMS Mimarisi + AI Chat Denetimi (2026-07-19) ✅
| Kontrol | Sonuç |
|---|---|
| `notif_prefs` DB bağlantısı + 22 kullanıcı backfill | ✅ |
| SMS: MCP üzerinden oluşturulan teklif → önceden hiç SMS tetiklemiyordu | ❌→✅ Bulundu, `pg_net`/`dispatch_sms()` ile düzeltildi |
| SMS canlı doğrulama (gerçek Twilio 201 yanıtı) | ✅ Test verisi temizlendi |
| Alıcı AI chat taraması | ✅ Yok, aktif bug değil, not edildi |
| "Hava sor" kaynak taraması | ❌ Hiç kaynak yok, karar bekliyor |

### (Önceki tüm test sonuçları — Premium/Referral/AI Limit, Sertifika/useHasat/Gating, Audit Takibi, Detaylı E2E, Alıcı Sayfaları, Mock Data, Fiyatlandırma, Tedarik Zinciri, Landing, RLS, Rekabet Hukuku, MCP Server, Benchmark Re-Audit, P16 serisi — değişmedi, önceki sürümlerde)
