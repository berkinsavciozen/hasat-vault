---
title: Hasat — MCP Server Referansı
updated: 2026-07-09
tags:
  - hasat
  - mcp
  - test
---

# Hasat MCP Server

Hasat, `https://hasat.lovable.app/mcp` adresinde **OAuth 2.1 korumalı, gerçek kullanıcı kimliğiyle (Supabase Auth) çalışan** bir MCP server yayınlıyor. Herhangi bir MCP uyumlu istemci (Claude, ileride başka asistanlar), OAuth ile giriş yapan gerçek bir çiftçi/alıcı hesabı adına, RLS'in doğal olarak sınırladığı işlemleri yapabilir.

**Temel prensip:** Her tool, o an giriş yapmış kullanıcının gerçek yetkisiyle çalışır — bir tool, kullanıcının UI'da yapabileceğinden fazlasını asla yapamaz (RLS ana güvence, service-role kullanımı yok).

---

## Bağlantı

- **URL:** `https://hasat.lovable.app/mcp`
- **Auth:** OAuth 2.1, issuer = `https://efuqpiaavrzimvstpdpm.supabase.co/auth/v1`, Dynamic Client Registration açık
- **Claude'da ekleme:** Settings → Connectors → Add custom connector → URL'i yapıştır → "Hasat" login ekranından (telefon+SMS OTP) bir test hesabıyla giriş yap
- **Tek bağlantı = tek kullanıcı.** Aynı anda hem çiftçi hem alıcı olarak bağlı olamazsın — iki taraflı bir senaryo test etmek için ya iki ayrı connector bağlantısı (farklı isimlerle, aynı URL) dene, ya da bir tarafı MCP'den bir tarafı gerçek UI'dan test et.
- **Yayın gecikmesi notu:** Lovable'da kod değişikliği yapıp **Publish** demeden, `hasat.lovable.app` üzerindeki canlı MCP server güncellenmez (editör preview'ı değil, gerçek yayın gerekli). Ayrıca Claude connector'ı tool listesini bağlantı anında önbelleğe alıyor — sunucu tarafında yeni tool eklenince connector'ı **disconnect/reconnect** etmek gerekebilir.

---

## Tool Listesi (v0.2.0 — 16 tool)

### Çiftçi tarafı
| Tool | Tip | Açıklama |
|---|---|---|
| `list_my_listings` | Okuma | `status` filtreli (active/draft/sold/any) |
| `list_my_parcels` | Okuma | |
| `list_recent_harvests` | Okuma | `crop` filtreli |
| `create_harvest_entry` | Yazma | parsel + ürün + miktar + tarih; otomatik ilana bağlanır |
| `create_parcel` | Yazma | isim, şehir, dönüm, ürünler — otomatik draft ilan(lar) tetikler |
| `publish_listing` | Yazma | draft → active, miktar/fiyat girilir |
| `list_offers_on_my_listings` | Okuma | tüm statüler |
| `respond_to_offer` | Yazma ⚠️ **HASSAS** | accept/decline/counter — `confirm:true` zorunlu, accept stok kilitler + sipariş oluşturur, geri alınamaz |
| `confirm_payment_received` | Yazma ⚠️ **HASSAS** | havale onayı, siparişi tamamlar — `confirm:true` zorunlu |

### Alıcı tarafı
| Tool | Tip | Açıklama |
|---|---|---|
| `browse_marketplace` | Okuma | `crop`/`city` filtreli, sadece aktif ilanlar (public) |
| `create_offer` | Yazma | bir ilana teklif verir |
| `respond_to_counter` | Yazma ⚠️ **HASSAS** | accept/decline — `confirm:true` zorunlu |
| `list_my_offers` | Okuma | tüm statüler |
| `mark_transfer_sent` | Yazma ⚠️ **HASSAS** | "havaleyi gönderdim" — `confirm:true` zorunlu, simüle ödeme (iyzico henüz bağlı değil) |
| `list_my_orders` | Okuma | |

### Paylaşılan
| Tool | Tip | Açıklama |
|---|---|---|
| `list_pending_offers` | Okuma | Çiftçi ya da alıcı, yanıt bekleyen teklifler |

---

## Güvenlik Tasarımı

- **RLS tek güvence** — hiçbir tool handler'ı `supabaseAdmin`/service-role kullanmıyor, hepsi `supabaseForUser(ctx)` (kullanıcının kendi bearer token'ı) ile çalışıyor
- **4 hassas tool** (`respond_to_offer`, `respond_to_counter`, `confirm_payment_received`, `mark_transfer_sent`) → Zod şemasında `confirm: z.literal(true)` — eksik/false olursa çağrı reddediliyor
- **Defense-in-depth:** RLS'e ek olarak her handler'da açık `farmer_id`/`buyer_id` eşleşme kontrolü var (örn. `respond_to_offer`, sadece `farmer_id = auth.uid()` olan teklifleri güncelleyebiliyor)
- **Buyer'a asla draft/sold/expired ilan sızmıyor** — `browse_marketplace` sadece `status='active'` okuyor

---

## Test Hesapları (bkz. TODO.md — Kararlar tablosu)
- Çiftçi: `05421241011`
- Alıcı: `05398545294`

## Canlı Doğrulanan Testler (2026-07-09)
- `list_my_listings`, `list_recent_harvests`, `list_pending_offers`, `create_harvest_entry` (ilk 4 tool) — gerçek OAuth oturumuyla çağrıldı, DB'de doğrulandı (`create_harvest_entry` sonrası otomatik `listing_harvest_entries` bağlantısı da teyit edildi)
- `browse_marketplace` — gerçek OAuth oturumuyla çağrıldı, 5 aktif ilan doğru döndü
- Genişletilmiş 12 tool — `tsgo` temiz, `extract_mcp_manifest` ile 16 tool + hassas tool'larda `confirm` zorunluluğu doğrulandı (Lovable tarafında)

## Bilinen Kısıtlar
- Tek connector bağlantısı = tek kullanıcı kimliği. Farmer↔buyer iki taraflı senaryo için ayrı bağlantı ya da UI+MCP kombinasyonu gerekiyor.
- Yeni tool eklendiğinde canlı yayın (Publish) + connector reconnect gerekiyor, otomatik yansımıyor.
