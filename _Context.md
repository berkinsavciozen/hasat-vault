---
title: Hasat — AI Context
updated: 2026-06-30
tags: [hasat, ai-context]
---

# Hasat AI Context
> Yeni bir Claude oturumuna bu notu yapıştır — anında tam bağlam sağlar.

## Ne?
İki paralel iş kolu:
1. **Dijital:** Türkiye'nin organik/özel ürün çiftçileri için mobil tarım zekası + D2C pazar yeri. ₺99/mo çiftçi sub, %5 GMV komisyon, ₺299/mo alıcı premium.
2. **Çiftlik:** Gerçek hibrit indoor (50m², M24) + outdoor (5.5 dönüm safran + 1.0 dönüm lavanta, M28) — yaşayan lab + gelir kaynağı.

**Kurucu:** Dataroid'de Senior PM. M1–M22 yarı zamanlı, M23'ten itibaren tam zamanlı.

## Çiftlik (Safran + Lavanta) Doğrulanmış Sayıları (Nisan 2026)
> **Not (2026-07-25):** Bu tablo Berkin'in KENDİ FİZİKSEL ÇİFTLİĞİNE (indoor+outdoor safran+lavanta üretimi, "Çiftlik" iş kolu) aittir — Hasat DİJİTAL PLATFORMUNUN genel/ortalama sayıları değildir. Platform 70+ crop'u eşit şekilde destekler (bkz. `crop_config`); safran bunlardan sadece biridir, platformun "amiral gemisi" ürünü değildir. Bu tablo sık geçiyor çünkü Çiftlik iş kolunun ürünü safran+lavanta — platform stratejisiyle karıştırılmamalı.

| Parametre | Değer | Güven |
|---|---|---|
| Korm toptan | ₺300/kg (100 kg/dönüm) | Yüksek |
| Safran D2C fiyatı | ₺400/g üretici direkt | Yüksek |
| Safran markalı Y3+ | ₺600–800/g Hasat platform | Orta |
| Safran hal toptan | ₺80–200/g (hedef kanal değil) | Yüksek |
| Y1 outdoor yield | 350g/dönüm (5.5 dönüm = 1,925g) | Yüksek |
| Y2 outdoor yield | 700g/dönüm (5.5 dönüm = 3,850g) | Orta |
| Indoor Y1 yield (50m², 4-kat) | ~400g | Orta |
| TKDK onay süresi | ~7 ay (Ağustos başvurudan) | Yüksek |
| TKDK penceresi | Ağustos–Eylül (yıllık) | Yüksek |

## Zaman Çizelgesi
| Ay | Olay |
|---|---|
| M1 (Mayıs 2026) | Build başlangıcı (yarı zamanlı) |
| M2 (Haziran 2026) | MVP teknik olarak hazır (Lovable AI sayesinde) |
| M3 (Temmuz 2026) | GTM hazırlığı: ödeme, foto upload, public URL |
| M4 (Ağustos 2026) | 🚀 Soft launch — ilk gerçek işlem |
| M6 (Ekim 2026) | **Tüm crop'larda** %5 komisyon açılışı (safran hasat sezonuyla aynı aya denk geliyor, ama karar crop-bağımsız — o tarihte platformdaki her ürün için komisyon başlıyor) |
| M7 (Kasım 2026) | ₺99/mo sub açılışı |
| M16 | TKDK proje yazımı |
| M18 | TKDK başvurusu (Ağustos penceresi) |
| M23 | **QUIT JOB** — MRR ₺230K |
| M24 | Indoor çiftlik (50m²) |
| M28 | Outdoor safran ekimi (Ağustos) |
| M31 | Outdoor Y1 hasat: ~1,925g · ₺770K |

## Gelir Modeli
| Akış | Fiyat | Başlangıç |
|---|---|---|
| GMV komisyonu | %5 | Ekim 2026 |
| Farmer subscription | ₺99/mo | Kasım 2026 |
| Buyer premium | ₺299/mo | Ocak 2027 |
| Farm D2C | ₺400–600/g | M27 indoor hasat |

**İlk 3 ay (Ağustos–Ekim 2026): tamamen ücretsiz — GMV yarat, sonra para al.**

## Ürün Durumu (Haziran 2026)
**Live:** https://hasat.lovable.app  
**Builder:** Lovable AI · **Backend:** Supabase `efuqpiaavrzimvstpdpm`  
**Repo:** github.com/berkinsavciozen/hasat-d2c-marketplace

| Tamamlanan | Durum |
|---|---|
| Auth (Phone OTP, Twilio) | ✅ |
| Farmer akışı (tüm ekranlar) | ✅ |
| Buyer akışı (tüm ekranlar) | ✅ |
| Marketplace / Listings / Offers | ✅ |
| P15: Teklif state machine (ball_side, ping-pong) | ✅ |
| AI özellikleri (P9-P14) | ✅ |
| Bildirimler (in-app + Twilio SMS) | ✅ |
| Auth/profiles fix (UNIQUE, orphan cleanup) | ✅ |

| Yapılacak | Öncelik |
|---|---|
| P15 completion (A1-A4) | 🔴 |
| Ürün fotoğrafı upload | 🔴 |
| Public vitrin URL | 🔴 |
| IBAN ödeme köprüsü → iyzico | 🔴 |

## Tech Stack
| Katman | Araç |
|---|---|
| Frontend | React + TanStack Start |
| Builder | Lovable AI |
| Backend | Supabase (Auth, DB, Storage, Realtime, Edge Functions) |
| Auth | Phone OTP — Twilio WhatsApp/SMS |
| State | TanStack Query |
| SMS | Twilio Edge Function `send-sms` |
| Deploy | hasat.lovable.app |

### Repolar (2026-07-28 itibarıyla)

| Repo | İçerik | Kim yazıyor |
|---|---|---|
| `hasat-d2c-marketplace` | Web uygulaması (React 19 + TanStack Start, SSR) | Lovable (`main`, sync bot) + Claude Code (feature branch → PR) |
| `hasat-mobile` | Mobil uygulama (Expo + Expo Router + Nativewind) — **M5'te açılacak** | %100 Claude Code (Lovable RN üretemiyor) |
| `hasat-core` | Paylaşılan TS: tipler, saf mantık, sorgu hook'ları, zod, design token — **M1'de açılacak** | Claude Code; iki repoya git subtree ile iner |
| `hasat-vault` | İş notları, roadmap, dokümanlar (public, kod/sır yok) | Claude Code PR + Berkin merge |

### Mobil stack (M5+)
- **Expo + EAS Build** — şirket Mac'i olduğu için local Xcode/imzalama yönetilemiyor; EAS bulutta derliyor ve submit ediyor
- **Expo Router** (dosya tabanlı, TanStack Router'a benzer) · **Nativewind** (Tailwind sözdizimi)
- **TanStack Query** web ile ortak (React Native'de çalışıyor)
- Oturum: `expo-secure-store` (web'de `localStorage`) — adapter sınırı
- Push: `device_tokens` + APNs (iOS) / FCM (Android); `notif_channel` enum'unda `push` zaten mevcut

### Mimari ilke
İki client'ın da ihtiyaç duyduğu mantık **veritabanında** (RPC/view) yaşar; monorepo kurulmaz (Lovable sync'ini kırma riski). Detay: `Build/Shared-Architecture.md`.

## DB Kritik Notlar
- Journal tablosu = `harvest_entries` (journal_entries değil)
- Parsel konum = `location_label`
- Community yazar = `author_id`
- Phone format = `905XXXXXXXXX` (+ prefix'siz)
- Offers: `ball_side`, `current_price`, `current_quantity`, `payment_status` eklendi (P15)
- `offer_messages` tablosu eklendi (P15)
- → Detay: `Build/DB-Schema.md`

## Test Kullanıcıları
| Rol | Telefon | UUID |
|---|---|---|
| Farmer | 905001234567 | 0868e4fe-86d2-4c5d-8ba5-f15fd4fac146 |
| Buyer | 905009876543 | 032eb467-661d-4df4-adf5-3d277d9b6549 |
OTP test: `123456`

## Lovable Prompt Kuralları
1. Her prompt'a schema doğrulaması ekle (Lovable yanlış kolon adı tahmin eder)
2. `ALTER TABLE ... ADD COLUMN IF NOT EXISTS` — migration yok
3. RLS politikaları: her yeni insert için policy gerekebilir
4. UUID'leri hardcode et — phone lookup ambiguous column hatası verir

## Kararlar (Haziran 2026)
- Şirket: Şahıs (tescil ediliyor)
- Ödeme: iyzico (2 hafta onay bekliyor, bu sürede IBAN köprüsü)
- İlk 3 ay: ücretsiz
- Sosyal medya: Berkin yapıyor + Claude haftalık içerik desteği
- Çiftçi ağı: MVP hazır olunca ağ geliştirme çalışmaları paralel

## Dosya Haritası
```
Berkin/Hasat/
├── _Context.md        ← bu dosya
├── TODO.md            ← master todo + roadmap (el ile güncelle)
├── Build/
│   ├── Prompt-Queue.md   ← Lovable prompt geçmişi + sıradakiler
│   ├── App-Audit.md      ← bug + improvement takibi
│   └── DB-Schema.md      ← tablo + enum referansı
├── Research/
│   ├── Saffron.md
│   ├── Lavender.md
│   ├── Market.md
│   ├── Tech-Indoor-Farming.md
│   ├── Incentives-TKDK.md
│   └── Competitive.md
├── Finance/
│   └── Model.md          ← konsolide finansal model
└── _Archive/             ← eski PARA yapısı
```
