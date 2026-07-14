# Hasat — Rekabet Benchmark & Use-Case Denetimi

**Tarih:** 2026-07-14 · **Yöntem:** Web araştırması (TR rakipler + global best practice) + Hasat canlı DB şema envanteri (Supabase, 28 tablo). Hasat tarafı tahminle değil, şema kanıtıyla dolduruldu. Kayırma yok: UI'da var görünüp uçtan uca akışı kapatmayan özellikler "yok" sayıldı.

---

## 1. Rakip Evreni

### Katman A — Doğrudan dijital rakipler (TR)
| Rakip | Model | Öne çıkan güçlü yönü |
|---|---|---|
| **DİTAP** (devlet) | Talep bazlı eşleşme + sözleşmeli tarım | ÇKS/MERSİS doğrulamalı kimlik; hasat öncesi alım sözleşmesi (teslim tarihi + ödeme vadesi); e-Devlet girişi; ücretsiz |
| **Harman App** | İlan pazaryeri + veri | 17 halden 200+ ürün için günlük min/max fiyat; hava durumu + tarımsal uyarı; hasat takvimi; içerik/rehber SEO makinesi |
| **Tarım Pazar** | İlan + teklif | Sabit fiyat vs pazarlıklı ilan ayrımı; teklif karşılaştırma ekranı; hal fiyatı entegre |
| **Tarım Cepte** | İlan + pazarlık | Basit mobil akış, 4.8 App Store puanı; geniş kategori (yem, gübre, ekipman dahil) |
| **ÇiftçidenEve** (2015'ten beri) | D2C butik/organik | Sertifika + şeffaflık + satış puanına göre üretici sıralaması ("Yeşil Puan"); coğrafi işaret/organik filtreleri; etki raporu; topluluk (tarif, takip) |
| **ToptanTR** | Genel B2B toptan | 81 il / 922 ilçeye 24 saatte sevk iddiası; "Excel'le sepet doldur" (!); cari/limitli sipariş |

### Katman B — Use-case bazlı dikey rakipler (HoReCa tedarik)
| Rakip | Öne çıkan pratik |
|---|---|
| **Haljet** (6.000+ işletme, İstanbul) | Sabah teslimatı; hal fiyatına yakın **stabil** fiyat (dalgalanma koruması); işletmeye özel fiyat listesi; sipariş+fatura+ödeme takibi tek ekran; 7 gün destek |
| **TazeKasam** | Önceki günün siparişini tek tıkla tekrar; onaylı müşteriye cari limitle kredi kartsız sipariş; çoklu şube/teslimat adresi yönetimi; "firesiz kullanım" standardı |
| Geleneksel hal tedarikçileri (AS Fresh, Aycenk vb.) | Soğuk zincir + frigorifik filo; şef/satınalma ile birebir ilişki; mevsimsel planlama desteği |

### Katman C — Gayri resmi ama asıl rakipler
| Alternatif | Neden kullanılıyor | Bilinen acı |
|---|---|---|
| **Hal komisyoncusu** | Garantili satış kanalı + avans/finansman rolü | ~%9 komisyon + rüsum; fiyat şeffaflığı yok; birkaç "baron"un fiyat belirlediği ürünler (ör. Malatya kayısı: üreticiden 2-3 TL → market 20-30 TL) |
| **Telefon + WhatsApp grupları** | Sıfır öğrenme maliyeti, mevcut ilişki ağı | Kayıt yok, ihtilafta kanıt yok; kapora dolandırıcılığı yaygın; grup dışına erişim yok |
| **Facebook / sahibinden** | Erişim geniş | Sahte ilan/sahte "güvenli ödeme" linki dolandırıcılığı Şikayetvar'da yüzlerce vaka; tarıma özel değil |
| **Excel / defter** | Alışkanlık | Sipariş-tahsilat-fire takibi kopuk; kurumsal alıcının satınalma süreciyle entegre değil |

### Katman D — Global best practice (TR'de karşılığı olmayan çözümler) ⭐
| Oyuncu | TR'de olmayan inovasyon |
|---|---|
| **Ninjacart** (Hindistan) | **24 saat içinde çiftçi ödemesi garantisi**; teminatsız kredi + trade finance; köy toplama merkezleri (fiziksel+dijital hibrit); 12 saatte 1.400 ton sevk kapasitesi; ödeme riski azaltma + kalite standardı + lojistiği tek üründe birleştirme |
| **ProducePay** (ABD/LatAm) | **Hasat ÖNCESİ finansman** — beklenen ürün satışını teminat sayan avans; quick-pay factoring (alıcı vadesini beklemeden çiftçiye erken ödeme); gerçek zamanlı tedarik zinciri görünürlüğü |
| **Frubana** (Kolombiya) | Restoran odaklı dar segment + rota yoğunluğu ekonomisi; aracıyı tamamen atlayan next-day model |
| **AI kalite derecelendirme** (Hindistan ihracat pratiği) | Fotoğraftan boyut/renk/nem/kusur bazlı otomatik grading — kalite ihtilafını sübjektiflikten çıkarıyor |
| **Blockchain/QR izlenebilirlik** (ihracat) | Parti bazlı QR ile menşe+sertifika+parti geçmişi; ihracatçı alıcının güven/uyum ihtiyacını çözüyor |
| **Escrow / kilometre taşı ödemesi** (agri-B2B genel patern) | Para platformda bloke → teslim+kalite onayı → serbest bırakma; güven sorununun yapısal çözümü |

---

## 2. Use-Case Matrisi

Şiddet skalası: **P0** = kullanıcı bu yüzden Hasat'ı terk eder / hiç gelmez · **P1** = rakip belirgin üstün · **P2** = fark yaratma fırsatı.

### UC-1 · Fiyat keşfi ("Ürünüm bugün ne eder?")
- **Bugünkü çözüm:** Komisyoncuyu aramak; Harman App/İzteder gibi hal fiyat listeleri; WhatsApp grubunda sormak.
- **Pain:** Hal fiyatı min-max aralığı geniş ve manipüle edilebilir; çiftçi kendi kalitesinin nereye düştüğünü bilmiyor.
- **Rakip best practice:** Harman App — 17 hal, 200+ ürün, günlük güncelleme; Tarım Pazar — ilan ekranında güncel fiyat.
- **Hasat durumu:** ✅ Farklı ve daha savunulabilir: `price_history` gerçek tamamlanmış siparişlerden besleniyor (trigger'lı) + `price_alerts` + bant bazlı AI fiyat önerisi (rekabet hukuku uyumlu). ❌ Ama soğuk başlangıç sorunu: işlem hacmi azken veri seyrek; Harman'ın hal verisi her gün dolu.
- **Gap: P1.** Öneri: gerçek işlem fiyatını "Hasat gerçekleşen fiyatı" olarak markalayıp hal min-max'ı **referans bant** olarak yanına koymak (kendi verisi seyrekken bile ekran boş kalmaz, doluluk arttıkça fark görünür).

### UC-2 · Alıcı bulma / ürün bulma (eşleşme)
- **Bugünkü çözüm:** Komisyoncu ağı; DİTAP'ta alıcı talebi → çiftçi teklifi; Harman/Tarım Pazar'da ilan.
- **Pain:** İlan pazaryerlerinde ilan verip beklemek; ciddi alıcı ile meraklıyı ayırt edememek.
- **Rakip best practice:** DİTAP'ın **talep bazlı** modeli (alıcı talep açar, üretici teklif verir) — arzın değil talebin yönlendirdiği akış.
- **Hasat durumu:** ✅ `listings` + `offers` (arz yönlü) + `crop_requests` (talep sinyali) var. ❌ `crop_requests` serbest metin + status'tan ibaret; miktar/bölge/tarih/fiyat alanı yok, yapılandırılmış RFQ değil. Alıcı→çiftçi talep akışı zayıf.
- **Gap: P1.** Yapılandırılmış RFQ (ürün, kalibre, miktar, teslim ili, tarih aralığı) + eşleşen çiftçilere bildirim.

### UC-3 · Pazarlık / teklif
- **Bugünkü çözüm:** Telefon; Tarım Pazar'da teklif mekanizması.
- **Hasat durumu:** ✅ Güçlü: `offers` içinde `negotiation_history`, `ball_side`, `counter_offer`, `offer_messages` — turlu pazarlık modellenmiş. Rakiplerin çoğunda pazarlık uygulama dışına (telefona) taşıyor.
- **Gap: P2 (avantaj).** Bunu pazarlamada anlatmak gerek: "pazarlık kayıt altında" = ihtilafta kanıt.

### UC-4 · Sözleşme / ön anlaşma (hasat öncesi satış)
- **Bugünkü çözüm:** Sözlü söz + kapora; DİTAP sözleşmeli tarım modülü.
- **Pain:** "Ürün tarlada kalır mı" korkusu; alıcı için sezon fiyat riski.
- **Rakip best practice:** DİTAP — teslim tarihi + ödeme şekli + vade içeren karşılıklı sözleşme; ProducePay — hasat öncesi finansmanla birleştirme.
- **Hasat durumu:** ✅ `harvest_subscriptions` (buyer↔farmer, `next_harvest_date`, `volume_commitment`, `price_lock`, `locked_price`) — sözleşmeli tarımın hafif versiyonu şemada VAR. Bu TR startup rakiplerinin hiçbirinde yok, sadece DİTAP'ta var.
- **Gap: P2 (gizli silah).** Ürünleştirilip görünür kılınmalı; landing v2'de anlatılmaya aday.

### UC-5 · Ödeme güvenliği
- **Bugünkü çözüm:** Elden nakit, havale, vadeli çek; güven = kişisel tanışıklık.
- **Pain:** Kapora dolandırıcılığı (Facebook/sahibinden vakaları); çiftçi tarafında "malı verdim, para gelmedi"; komisyoncunun geç ödemesi.
- **Global best practice:** Escrow/milestone ödeme; **Ninjacart'ın 24 saatte çiftçi ödemesi** — güvenin altyapıya gömülmesi.
- **Hasat durumu:** ❌ En kritik boşluk. `offers.payment_status` bir alan olarak var ama ödeme kaydı tablosu, PSP entegrasyonu, escrow, iade akışı yok. Trust-infrastructure tezi olan bir platformda paranın kendisi platform dışında akıyorsa güven vaadi eksik kalır.
- **Gap: P0.** MVP: iyzico/PayTR pazaryeri modeli ile "para Hasat'ta bloke → teslim onayı → çiftçiye aktarım" + net iade politikası. (Not: tam escrow lisans/ödeme kuruluşu gerektirir — pazaryeri alt-üye işyeri modeli ile başlanabilir.)

### UC-6 · Lojistik / teslimat
- **Bugünkü çözüm:** Alıcının aracı, nakliyeci ilişkileri, komisyoncunun organizasyonu.
- **Rakip best practice:** Haljet/TazeKasam — sabah teslimat + soğuk zincir; Ninjacart — kendi filo + rota optimizasyonu; Frubana — rota yoğunluğu.
- **Hasat durumu:** ❌ Şemada lojistiğe dair tek alan `offers.delivery` + `delivery_date`. Taşıma partneri, araç tipi (frigorifik), kargo takibi, teslim tutanağı yok.
- **Gap: P0 (HoReCa segmenti için) / P1 (diğerleri).** Kendi filosu olmayan model için gerçekçi yol: nakliyeci marketplace'i değil, sipariş akışına "taşımayı kim üstleniyor + teslim fotoğrafı/tutanağı" adımını eklemek; ileride 3PL anlaşması.

### UC-7 · Tekrar sipariş & rutin tedarik (HoReCa günlük ritmi)
- **Bugünkü çözüm:** Her sabah WhatsApp'tan aynı listeyi yazmak / tedarikçiyi aramak.
- **Rakip best practice:** TazeKasam — dünkü siparişi tek tıkla tekrarla; Haljet — işletmeye özel ürün-fiyat listesi; ToptanTR — Excel'le sepet doldurma.
- **Hasat durumu:** ❌ Sipariş şablonu, favori liste, tekrar sipariş, şube adresi kavramı yok (`buyer_profiles` tek adressiz kayıt).
- **Gap: P0 (restoran/otel hedefleniyorsa).** "Cuma listemi tekrarla" tek başına HoReCa retention'ının bel kemiği.

### UC-8 · Kalite güvencesi & ihtilaf
- **Bugünkü çözüm:** Malı görmeden almama; ihtilafta ilişkiyi kesme.
- **Pain:** "Fotoğraftaki mal gelen mal değil"; kalibre/fire anlaşmazlığı — HoReCa'nın 1 numaralı tedarikçi değiştirme sebebi (kalite tutarlılığı).
- **Global best practice:** AI foto-grading (boyut/renk/kusur); teslimde fotoğraflı kabul; kalite standardı sözleşmeye gömme (Ninjacart).
- **Hasat durumu:** ⚠️ Yarım: `harvest_entries.quality` + `photo_urls` + `listings.quality` var (menşe fotoğrafı güçlü), ama teslim anı kabul/red akışı, ihtilaf (dispute) tablosu, kısmi iade yok.
- **Gap: P0.** Teslim onayı + fotoğraflı kabul + 24 saatlik itiraz penceresi; UC-5'teki bloke ödemeyle birleşince güven döngüsü kapanır.

### UC-9 · İtibar & güven sinyalleri
- **Rakip best practice:** ÇiftçidenEve — sertifika+şeffaflık+satış geçmişinden bileşik "Yeşil Puan" ile üretici sıralaması; ihracatta QR-menşe.
- **Hasat durumu:** ⚠️ Hammadde mükemmel: `parcels` → `harvest_entries` → `listing_harvest_entries` → `listings` zinciri (parsele kadar izlenebilirlik, TR'de benzersiz), `certifications` (verified_at/expires_at), `public_farmer_profiles`. ❌ Ama **rating/review tablosu hiç yok** ve izlenebilirlik zinciri alıcıya tek ekranda sunulmuyor (public_parcel_cards var, sipariş sonrası "bu ürünün hikayesi" görünümü yok).
- **Gap: P0 (tez bazlı).** Trust platformunda karşılıklı değerlendirme olmaması tezle çelişiyor. + "Parselden tabağa" QR/link görünümü = ihracatçı ve premium HoReCa için farklılaştırıcı.

### UC-10 · Finansman, vade & kayıt
- **Bugünkü çözüm:** Komisyoncu avansı (çiftçi); vadeli çek/cari hesap (alıcı); muhasebe defterde.
- **Rakip best practice:** TazeKasam — onaylı müşteriye cari limit; ProducePay — hasat öncesi avans + factoring; Ninjacart — teminatsız kredi; Haljet — fatura takibi tek ekran.
- **Hasat durumu:** ❌ Vade/cari yok, fatura/e-müstahsil makbuzu yok (`harvest_entries.costs` sadece maliyet günlüğü). Kurumsal alıcı faturasız çalışamaz — bu tek başına HoReCa onboarding engeli.
- **Gap: P0 (fatura) / P1 (vade) / P2 (finansman).** Sıra: önce e-belge entegrasyonu (e-fatura/e-müstahsil), sonra onaylı alıcıya vade, en son (partner banka/fintech ile) hasat öncesi avans.

---

## 3. Rakiplerden Alınacak Somut Pattern'ler (kısa liste)

1. **TazeKasam:** "Son siparişi tekrarla" + çoklu şube adresi → HoReCa retention temeli.
2. **Haljet:** İşletmeye özel fiyat listesi + "dalgalanmaya karşı stabil fiyat" mesajı → satış argümanı olarak fiyat *istikrarı*, fiyat *ucuzluğu* değil.
3. **DİTAP:** ÇKS ile çiftçi kimlik doğrulama fikri → Hasat'ta "ÇKS doğrulanmış üretici" rozeti (kamu verisiyle güven sinyali, entegrasyon araştırılmalı).
4. **Harman App:** İçerik/SEO makinesi (hasat takvimi, rehberler, hal fiyat sayfaları) → organik trafik kanalı; Hasat'ın price_history'si benzersiz içerik hammaddesi.
5. **ÇiftçidenEve:** Bileşik güven puanı (sertifika + şeffaflık + satış geçmişi) → Hasat'ın verisiyle daha güçlü versiyonu yapılabilir.
6. **Ninjacart:** "24 saatte ödeme" gibi tek cümlelik, ölçülebilir güven vaadi → landing v2 mesajlaşması.
7. **ProducePay:** Hasat öncesi finansmanı `harvest_subscriptions` ile birleştirme (uzun vade, partner gerektirir).
8. **Tarımsal Pazaryeri (TAGEM projesi):** SMS ile ilan sorgulama — internete uzak çiftçi segmenti; Hasat'ın Twilio altyapısı buna hazır (SMS/WhatsApp ile ilan + teklif bildirimi zaten doğal uzantı).
9. **İhracat pratiği:** Parti bazlı QR menşe kartı → `public_parcel_cards` üstüne ince bir katman.
10. **AI foto-grading:** Uzun vadeli; önce insan akışı (fotoğraflı kabul) sonra otomasyon.

---

## 4. Önceliklendirilmiş Gap Listesi → Backlog Önerisi

| # | Gap | Şiddet | Önerilen kapsam | Aday |
|---|---|---|---|---|
| 1 | Ödeme güvencesi (bloke ödeme/pazaryeri PSP) | P0 | iyzico/PayTR alt-üye işyeri modeli; payment kayıt tablosu; teslim onayına bağlı serbest bırakma | P17 |
| 2 | Teslim kabul + ihtilaf akışı | P0 | Teslim fotoğrafı, kabul/kısmi kabul/red, 24s itiraz penceresi, disputes tablosu | P17 (1 ile birlikte) |
| 3 | Karşılıklı değerlendirme (rating/review) | P0 | Sipariş sonrası iki yönlü puan+yorum; farmer profile'da agregat | P16 içine sıkışabilir |
| 4 | Fatura / e-müstahsil makbuzu | P0 (B2B) | Önce PDF sipariş dekontu, sonra e-belge entegratörü | P17 |
| 5 | Tekrar sipariş + şube adresleri | P0 (HoReCa) | reorder endpoint + saved lists + buyer_addresses tablosu | P17 |
| 6 | Yapılandırılmış RFQ (talep akışı) | P1 | crop_requests'i miktar/il/tarih/hedef fiyatla genişlet + eşleşme bildirimi | P17 |
| 7 | Hal fiyatı referans bandı | P1 | Günlük hal verisi ingestion (kaynak: hal müdürlükleri/İzteder benzeri) + price_history yanında gösterim | P17 |
| 8 | Lojistik adımı (taşıma sorumlusu + takip) | P1 | Sipariş akışına taşıma bilgisi + durum; 3PL sonrası | P18 |
| 9 | "Parselden tabağa" QR görünümü | P2 | public_parcel_cards + sipariş bağlama; landing v2'de vitrin | P16 landing ile senkron |
| 10 | SMS/WhatsApp bildirim genişletmesi | P2 | Twilio üstünden teklif/sipariş bildirimleri (zaten kısmen var mı → doğrulanmalı) | P18 |
| 11 | Onaylı alıcıya vade/cari | P1→P2 | Risk gerektirir; önce manuel onay süreciyle pilot | P19+ |
| 12 | Hasat öncesi finansman | P2 | Partner fintech/banka; harvest_subscriptions verisi teminat sinyali | Uzun vade |

**Hasat'ın savunulabilir farkları (korunacak):** parsel-düzeyi izlenebilirlik zinciri · gerçek işlem bazlı price_history · kayıtlı turlu pazarlık (negotiation_history) · harvest_subscriptions (fiyat kilitli ön anlaşma) · rekabet hukuku uyumlu bant bazlı fiyat önerisi.

---

## 5. KPI Çerçevesi (fizibilite öncesi hazırlık)

**North Star Metric önerisi:** *Sorunsuz Tamamlanan Sipariş GMV'si* — teslim edilmiş + ihtilafsız + ödemesi kapanmış sipariş hacmi. Ham GMV değil; çünkü Hasat'ın tezi güven. Ham GMV büyürken ihtilaflı sipariş oranı da büyüyorsa tez çöküyor demektir. (Alt gösterge: toplam GMV içinde sorunsuz payı ≥ %95 hedef.)

### 5.1 Çiftçi tarafı KPI'ları
| Aşama | KPI | Tanım | Erken dönem hedef bandı | Veri kaynağı (bugün) |
|---|---|---|---|---|
| Aktivasyon | Kayıt → ilk ilan süresi | Medyan saat | ≤ 48 saat | `profiles` + `listings.created_at` ✅ |
| Aktivasyon | 7 günde ilan veren % | Kayıt kohortunda | ≥ %40 | ✅ |
| Likidite | İlanın teklif alma oranı | ≥1 teklif alan ilan % (14 gün) | ≥ %50 | `listings` + `offers` ✅ |
| Likidite | İlk teklife kadar süre | Medyan | ≤ 72 saat | ✅ |
| Likidite | Sell-through | 30 günde satışa dönen ilan % | ≥ %30 | `listings.status` + `orders` ✅ |
| Gelir | Aktif çiftçi başına aylık GMV | Sipariş tutarı / aktif satıcı | iç baz oluşturulacak | `orders`+`offers` ✅ |
| Fiyat | Hal referansına göre net fiyat farkı | Hasat gerçekleşen fiyatı vs hal ortalaması (komisyon düşülmüş karşılaştırma) | ≥ +%5 çiftçi lehine | `price_history` ✅ + hal verisi ❌ (Gap #7) |
| Ödeme | Sipariş → para hesapta | Medyan gün | ≤ 3 gün → hedef 24 saat | ❌ ödeme altyapısı yok (Gap #1) |
| Retention | M1/M3 satıcı retention | Sipariş alan satıcının sonraki ay tekrar ilan/satış | M1 ≥ %60 | ✅ |
| Güven | Doğrulanmış üretici % | Sertifika/ÇKS rozetli aktif satıcı | ≥ %70 | `certifications` ✅ |

### 5.2 Alıcı tarafı KPI'ları
| Aşama | KPI | Tanım | Erken dönem hedef bandı | Veri kaynağı (bugün) |
|---|---|---|---|---|
| Aktivasyon | Kayıt → ilk sipariş | Medyan gün | ≤ 7 gün (HoReCa ≤ 3) | ✅ |
| Likidite | Talep karşılanma (fill rate) | RFQ/aramanın siparişle sonuçlanma % | ≥ %40 | `crop_requests` yapılandırılınca (Gap #6) |
| Ekonomi | AOV (segment bazlı) | HoReCa / market / ihracatçı / bireysel ayrı | segment bazı oluşturulacak | ✅ |
| Frekans | Haftalık sipariş sıklığı (HoReCa) | Aktif HoReCa alıcısı başına | ≥ 2/hafta (olgunlukta) | ✅ (`buyer_profiles.company_type`) |
| Retention | Tekrar alıcı oranı | >1 sipariş veren alıcı % | %30–60 bandı; rutin tedarikte çeyreklik ≥ %50 | ✅ |
| Retention | M1 GMV retention (kohort) | Kohortun 1. ay harcamasının korunumu | ≥ %60 (HoReCa) | ✅ hesaplanabilir |
| Kalite | İhtilaf oranı | İhtilaflı sipariş / toplam | ≤ %2 | ❌ disputes yok (Gap #2) |
| Kalite | Tam kabul oranı | Redsiz/kısmi-iadesiz teslim % | ≥ %95 | ❌ teslim kabul akışı yok (Gap #2) |
| Kalite | Zamanında teslim % | Söz verilen tarihte teslim | ≥ %90 | ⚠️ `order_timeline` kısmen |
| Memnuniyet | NPS / puan ortalaması | Sipariş sonrası | NPS ≥ 40 | ❌ rating yok (Gap #3) |

### 5.3 Platform / pazar sağlığı
| KPI | Tanım | Referans | Not |
|---|---|---|---|
| GMV (aylık) + sorunsuz payı | — | — | North Star bileşeni |
| Efektif take rate | Net gelir / GMV | Uzman B2B pazaryerlerinde %1–5; olgun tüketici pazaryerlerinde tek-çift hane | Psikolojik tavan: komisyoncunun ~%9'u. Hasat %9'un belirgin altında + "kayıtlı, güvenli, hızlı ödeme" değer paketiyle konumlanmalı |
| Alıcı:Satıcı oranı | Aktif alıcı / aktif satıcı | Segmente göre; dengesizlik likidite katili | İl+ürün bazında izlenmeli (yerel yoğunluk > toplam sayı) |
| Tedarik yoğunluğu | İl×ürün hücresinde ≥3 aktif satıcı olan hücre % | Frubana/Ninjacart dersi: yoğunluk ekonomisi | Coğrafi odaklanma kararlarını bu yönetmeli |
| Zaman-içi-eşleşme | Teklif → sipariş dönüşüm % ve süresi | — | `offers.status` ✅ |

### 5.4 Rakip referans noktaları (kamuya açık)
| Oyuncu | Metrik | Değer | Hasat için kullanım |
|---|---|---|---|
| Ninjacart | Çiftçi ödeme süresi | 24 saat | Ödeme KPI'sının kuzey yıldızı |
| Ninjacart | Ölçek | 100K+ çiftçi; 12 saatte ~1.400 ton sevk | Uzun vade ölçek referansı, hedef değil |
| Haljet | Aktif işletme (İstanbul) | 6.000+ | TR HoReCa'da tek şehirde ulaşılabilir tavan kanıtı |
| Hal komisyoncusu | Komisyon | ~%9 (+rüsum) | Take rate tavan çıpası |
| Marketplace genel (Greylock) | Tekrar alıcı | %30–60 sağlıklı bant | Alıcı retention hedef bandı |
| B2B rutin tedarik | Çeyreklik tekrar alıcı | > %50 | HoReCa segment hedefi |
| Uzman B2B pazaryeri | Take rate | %1–5 | Fiyatlama fizibilite girdisi |
| ÇiftçidenEve / DİTAP | Hacim/kullanıcı | Kamuya açık güvenilir veri bulunamadı | Boşluk — fizibilitede varsayımla ilerlenecek |

**Fizibiliteye köprü (şimdilik sadece not):** Fizibilite aşamasında forecast'in üç girdisi bu bölümden gelecek — (1) take rate senaryoları (%2 / %3.5 / %5), (2) segment bazlı AOV × sipariş frekansı (Hasat mock verisi + Haljet/hal gözlemleriyle kalibre), (3) retention eğrileri (yukarıdaki bantlar). KPI'ların 5'i bugün ölçülebilir durumda değil ve tamamı Gap listesi #1-#3 ve #6'ya bağlı — yani **P17 çekirdeği aynı zamanda ölçüm altyapısıdır.**

---

## 6. Kaynaklar
- harmanapps.com · tarim-pazar.com · ciftcideneve.com · toptantr.com · haljet.com · tazekasam.com
- DİTAP: dijitaltarimpazari.gov.tr, İl Tarım Müdürlüğü duyuruları (ÇKS/MERSİS kayıt şartı, sözleşmeli tarım modülü)
- Komisyoncu ekonomisi: tarimdunyasi.net (hal yasası değişiklikleri), ekşi sözlük hal komisyonu tartışmaları (%9 komisyon, kayısı fiyat zinciri örneği)
- Dolandırıcılık vakaları: sikayetvar.com (Facebook/sahibinden sahte güvenli ödeme linkleri)
- Global: ninjacart.com, FAO STI Portal (Ninjacart 24s ödeme), producepay.com/PerishableNews (quick-pay factoring, hasat öncesi finansman), AgFunder (Frubana modeli), ISF Advisors (marketplace lending analizi), tradologie.com (AI grading, blockchain QR ihracat)

> Not: Bu doküman hasat-vault'a `BENCHMARK.md` olarak eklenecek (GitHub MCP read-only → manuel yapıştırma).
- KPI benchmarkları: a16z (GMV retention kohort çerçevesi), Greylock/S. Rothman aktarımı (tekrar alıcı %30–60, dittofi.com), kissmetrics.io (B2B take rate %1–5), gurustartups.com (take rate benchmark raporu), financialmodelslab.com (rutin B2B tedarikte çeyreklik >%50 tekrar alıcı), sharetribe.com (likidite/repeat metrik tanımları)
