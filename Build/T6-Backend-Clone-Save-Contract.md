---
title: T6 Backend — Klonlama/Kaydetme Kontratı (tasarım, dispatch edilmedi)
created: 2026-09-10
status: Salt-okunur tasarım turu — Berkin'in seçimiyle üretildi. Henüz Claude Code'a dispatch edilmedi, henüz Faz'a eklenmedi. T6 UI implementasyonu bu kontrat kabul edilmeden ChatGPT/Codex'e gönderilmeyecek (Berkin'in KONSOLİDE talimatı).
tags:
  - hasat
  - t6
  - backend
  - contract
  - design
---

# T6 Backend — Klonlama/Kaydetme Kontratı

## 0. Bağlam ve önceki bulgu

`claude/T3-Insan-Onay-Gate-ve-KONSOLIDE-Durum-2026-09-09.md` §5'te hem web hem
mobil repoda T6'nın klonlama/kaydetme akışının genuinely hiç implemente
edilmediği doğrulanmıştı (canlı: `cloned_from_recipe_id` her satırda null,
`source_type`'ta hiç "clone"/"ai_customize" değeri yok). Bu doküman o
bulgunun üzerine, Berkin'in KONSOLİDE mesajının Section 4'ünde istenen
kontratı tasarlıyor: yetki/sahiplik modeli, idempotency key, rollback,
nutrition-invalidation event. Kod yazılmadı, hiçbir şey dispatch edilmedi —
bu salt-okunur bir tasarım turu.

Ürün/UX tarafı zaten `Lansman-Plani-v2.md` §T6'da ve §9'da (Codex'in FAB
kararı: tek FAB → menü) kesinleşmişti — bu doküman yalnızca backend
kontratını ekliyor, ürün kararlarını değiştirmiyor.

## 1. Bu turda canlıda doğrulanan, kontratı doğrudan etkileyen 3 yeni bulgu

Tasarımı yazmadan önce ilgili şema/RLS/grant durumu bu turda canlı sorgulandı
(kural #96 — eski plan metnine güvenmek yerine güncel gerçeği doğrulamak):

**Bulgu 1** — `source_type` CHECK constraint'i `'ai_customize'`'i kabul
etmiyor. Canlı constraint: `recipes_source_type_check = CHECK (source_type =
ANY (ARRAY['manual','text','photo','url']))`. Planın orijinal akış
diyagramı klonlanan tarife `source_type='ai_customize'` yazılmasını
öngörüyordu ama bu değer bugün DB'de yasak. Backend kontratının zorunlu bir
parçası: bu constraint'e `'ai_customize'` eklenmeli (migration, aşağıda
§3'te).

**Bulgu 2** — `authenticated`'ın INSERT'te (yalnızca UPDATE'te değil) hâlâ
nutrition/alerjen kolonlarına tam erişimi var. T4-A2 (§19-20, Lansman Planı
v2) tablo-geneli `UPDATE` grant'ini revoke edip yalnızca 24 kolonu geri
açmıştı — ama INSERT ayrı bir privilege, ona hiç dokunulmamış. Bu turda
`information_schema.column_privileges` ile doğrulandı: `authenticated`,
`allergens_reviewed`, `allergens_reviewed_at`, `allergens_reviewed_by`,
`allergen_labels`, `calories`, `nutrition_source` dahil tüm kilitli
kolonlara INSERT yetkisine sahip. Yani bugün, teorik olarak bir kullanıcı
kendi private tarifini INSERT ederken `allergens_reviewed=true` ya da
`nutrition_source='computed'` gibi sahte değerleri doğrudan yazabilir.

Ciddiyet değerlendirmesi (Product Readiness kuralı — otomatik Faz 0 değil):
RLS INSERT policy'si `owner_id=auth.uid() AND visibility='private'` ile
sınırlı — yani bu yalnızca kullanıcının kendi private tarifini etkiler,
T3'ün gerçek güven sınırı olan public korpusa hiçbir şekilde sızmıyor (bir
private tarif, ayrı bir sunucu-taraflı publish akışından geçmeden asla
public olamıyor — `visibility` kolonu için de aynı WITH CHECK zaten var).
Düşük önem: kendi kendine yalan söyleme, kimseye zarar yok. Faz 0'a otomatik
eklenmiyor. Ama T6'nın kontratı bunu varsayılan grant'a güvenerek değil,
açıkça kontrol ederek tasarlanmalı (aşağıda §4) — ve ayrı, düşük öncelikli
bir backlog kalemi olarak not düşülüyor: T4-A2'nin INSERT'e de
genişletilmesi (tablo-geneli INSERT revoke + aynı 24-kolonluk allow-list,
T3-A/T3-A2'nin de aynı mantıkla INSERT'e genişletilmesi).

**Bulgu 3** — `recipe_ingredients` ve `recipe_steps`'in INSERT RLS
policy'leri zaten F7 deseniyle birebir uyumlu. İkisi de `EXISTS (SELECT 1
FROM recipes r WHERE r.id = ...recipe_id AND r.owner_id = auth.uid())` —
yani T6'nın klon akışı hiçbir yeni RLS policy gerektirmeden, F7'nin
`saveDraft()` akışıyla birebir aynı sırayla (önce `recipes` satırı, sonra
`recipe_ingredients`/`recipe_steps`) çalışabilir. `can_send_ai_message` /
`increment_ai_usage` RPC'leri de canlıda gerçek ve `SECURITY DEFINER` —
planın iddiası doğrulandı, kota için yeniden kullanılabilir.

## 2. Klonlama/kaydetme akışı — iki net faz

**Faz A — Öneri üretimi (yazma YOK).**

```
İstemci → customize-recipe edge function
  ├─ can_send_ai_message(auth.uid()) kontrolü (kota)
  ├─ kaynak tarifi oku: id, visibility='public', author_type != 'kullanici'
  │    (author_type='kullanici' ise 403 — başka bir kullanıcının private
  │     tarifini "klonlama" bahanesiyle okumaya izin verme)
  ├─ malzeme + adım listesini oku
  ├─ LLM'e yapılandırılmış düzenleme iste (prompt: kullanıcının serbest metni)
  ├─ F2'nin mevcut deterministik doğrulayıcılarından geçir
  │    (validate_recipe_crop_values / validate_recipe_units /
  │     validate_recipe_ingredient_coverage — implementasyon turunda bu
  │     üçünün güncel imzası/erişilebilirliği teyit edilmeli, bu tasarım
  │     turunda yeniden doğrulanmadı)
  ├─ increment_ai_usage(auth.uid()) — YALNIZCA LLM başarıyla döndükten sonra
  └─ changedFields + önerilen malzeme/adım listesini İSTEMCİYE döndür
       (hiçbir DB yazma işlemi yok — henüz hiçbir yeni satır yok)
```

**Faz B — Kaydetme (F7'nin `saveDraft()` yolunu birebir kullanır).**

```
Kullanıcı önizlemeyi (changedFields highlight'lı) onaylayıp "Kaydet"e basar
  → aynı authenticated/RLS yazma yolu, TEK bir mantıksal işlem olarak:
     1. INSERT recipes (
          owner_id=auth.uid(), visibility='private', author_type='kullanici',
          source_type='ai_customize' (§3'teki migration sonrası geçerli),
          cloned_from_recipe_id=<kaynak id>,
          -- allergen_labels / allergens_reviewed* / nutrition_* : HİÇBİRİ
          -- istemci tarafından set edilmiyor, kolon listesinde bile yok →
          -- DB default'ları geçerli olur (allergens_reviewed=false,
          -- nutrition_source=null) — §1 Bulgu 2'deki INSERT açığına rağmen
          -- güvenli, çünkü müşteri kodu bu kolonlara hiç dokunmuyor
        )
     2. INSERT recipe_ingredients (yeni recipe_id'ye, önerilen malzeme listesi)
     3. INSERT recipe_steps (yeni recipe_id'ye, önerilen adım listesi)
  → T4-A5'in mevcut `recipe_ingredients` INSERT trigger'ı (FOR EACH STATEMENT)
    otomatik olarak fn_recalc_recipe_nutrition_ids(yeni recipe_id) çağırır
    → nutrition_source/calories/vb. hesaplama motoru tarafından, EK KOD
      YAZILMADAN dolduruluyor (bkz. §5)
```

## 3. Gerekli şema değişikliği (implementasyon turunun kapsamı)

Tek migration: `recipes_source_type_check`'e `'ai_customize'` eklenmesi
(mevcut 4 değere ek, hiçbiri kaldırılmadan — `ALTER TABLE ... DROP
CONSTRAINT ... ADD CONSTRAINT ...` ya da eşdeğeri). Başka hiçbir şema
değişikliği zorunlu değil — `cloned_from_recipe_id` zaten var,
`recipe_ingredients`/`recipe_steps` RLS'i zaten yeterli.

## 4. Yetki/sahiplik modeli

- Kaynak tarif erişimi: yalnızca `visibility='public' AND author_type IN
  ('hasat','ciftci','sef')` (yani `!= 'kullanici'`) — plan metnindeki UI
  kısıtının (`author_type != 'kullanici'`) backend'de de edge function
  içinde açıkça tekrarlanması gerekiyor, yalnızca UI'da gizlemek yetmez (bir
  kullanıcı API'yi doğrudan çağırıp başka bir kullanıcının private tarifini
  "kaynak" olarak veremesin).
- Yeni klon: `owner_id=auth.uid()` (RLS zaten zorluyor), `visibility='private'`
  (RLS zaten zorluyor, değiştirilemez), asla yayınlanamaz — bu zaten mevcut
  "kullanıcı kendi private tarifini public yapamaz" WITH CHECK'iyle yapısal
  olarak garanti.
- Alerjen/nutrition kolonları: istemci INSERT'te bu kolonlara HİÇ değinmez
  (§2 Faz B, madde 1'deki not) — §1 Bulgu 2'nin gerçek riskini sıfırlayan
  asıl savunma budur, grant'a güvenmek değil.
- Rozet: "AI ile özelleştirildi" — yeni bir kolon gerekmiyor,
  `source_type='ai_customize' AND cloned_from_recipe_id IS NOT NULL`'dan
  türetiliyor (istemci tarafında hesaplanabilir, ekstra sorgu gerekmez).

## 5. Nutrition-invalidation event — yeni kod gerekmiyor, mevcut altyapı yeterli

Berkin'in KONSOLİDE talebi: "T6 recipe kaydedildiğinde `nutrition_input_hash`
yenilenmeli, önceki sonuç bayat sayılmalı, idempotent yeniden hesaplama
kuyruklanmalı, başarısızlık/retry davranışı tanımlı olmalı, bayat nutrition
asla doğrulanmış gibi gösterilmemeli."

Bu gereksinimlerin tamamı T4-A5'in (PR #119, production'da doğrulanmış —
bkz. `F0-24-T4-A4-Guncelleme-2026-09-09.md` §11) mevcut trigger altyapısı
tarafından otomatik karşılanıyor, T6'ya özel yeni bir invalidation
mekanizması yazmaya gerek yok:

- Klon yeni bir `recipe_id` ile oluşuyor — "önceki sonuç" diye bir şey yok,
  `nutrition_source` baştan `null` başlıyor (bayat veri gösterme riski
  yapısal olarak yok, UI zaten null'u "hesaplanmadı" olarak gösteriyor).
- `recipe_ingredients` INSERT'i, T4-A5'in `FOR EACH STATEMENT` trigger'ını
  tetikler → `fn_recalc_recipe_nutrition_ids([yeni recipe_id])` çağrılır →
  `calculate_recipe_nutrition` otomatik çalışır, `nutrition_input_hash`
  gerçek malzeme listesinden baştan hesaplanır (kopyalanmaz).
- İdempotency zaten `nutrition_input_hash` üzerinden garanti (aynı girdi
  ikinci kez tetiklenirse no-op — T4-A4'ün doğrulanmış davranışı).
- Hata/retry: trigger'ın `fn_recalc_recipe_nutrition_ids` fonksiyonu her
  `recipe_id` için `exception when others` ile sarmalı (T4-A5, §11.2) —
  hesaplama hatası klonun kaydedilmesini asla bloklamaz, yalnızca o tarif
  `nutrition_source=null/partial` kalır, sonraki bir düzenlemede (ya da
  manuel bir yeniden-tetikleme) otomatik telafi eder.

Tek koşul: klon-kaydetme kodu `recipe_ingredients`'a normal bir SQL INSERT
ile yazmalı (F7'nin zaten yaptığı gibi) — toplu/bypass bir yazma yolu (ör.
`COPY`, RLS'i atlayan bir service-role bulk-insert) kullanılırsa trigger
yine de tetiklenir (`FOR EACH STATEMENT` trigger'lar INSERT komutunun
kendisine bağlı, satır sayısından bağımsız) ama örtük bir kural ihlali
riski taşımaz, yalnızca not olarak düşülüyor.

## 6. Idempotency key

F2/T4-A4/A5'in aksine bu bir arka-plan pipeline değil, kullanıcı tetikli tek
seferlik bir aksiyon — ama çift-tıklama/ağ retry'ı hâlâ iki kez klon
oluşturabilir. Tasarım:

- İstemci, "AI ile özelleştir" akışını başlatırken bir UUID idempotency key
  üretir (Faz A'nın başında), bunu Faz B'nin "Kaydet" çağrısına kadar taşır.
- `customize-recipe` edge function'ı bu key'i (henüz yazmadan) bir
  `ai_customize_requests(idempotency_key uuid primary key, user_id uuid,
  source_recipe_id uuid, status text, created_recipe_id uuid nullable,
  created_at)` tablosuna kaydeder (Faz A başında `status='pending'`).
- Faz B'nin kaydetme adımı, aynı `idempotency_key` ile daha önce
  `status='completed'` bir kayıt varsa yeni bir klon oluşturmaz,
  `created_recipe_id`'yi olduğu gibi döner (retry-safe).
- Bu tablo aynı zamanda rollback/denetlenebilirlik için doğal bir audit log
  işlevi görür (§7).

## 7. Rollback / transactionality

- Faz A (öneri) hiçbir kalıcı yazma yapmıyor — rollback konusu yok.
- Faz B'nin 3 INSERT'i (recipes → recipe_ingredients → recipe_steps) tek
  bir DB transaction'ında olmalı (edge function içinde, ya da tek bir
  `rpc_create_ai_customized_recipe(...)` fonksiyonu — implementasyon
  turunda karar verilecek, ikisi de RLS'i koruyabilir). Herhangi bir adım
  başarısızsa tamamı geri alınır, yarım bir tarif (malzemesiz/adımsız) asla
  kalıcı olmaz.
- `ai_customize_requests` kaydı (§6) transaction başarısızsa
  `status='failed'` olarak işaretlenir — kullanıcı yeniden dener, aynı
  idempotency key'le (yeni bir `pending` girişimi tetikler, `failed` bir
  önceki denemeyi bloklamaz).
- Nutrition hesaplama hatası (§5) klonun kendisini asla rollback ettirmez —
  bu, T4-A5'in "recalc hatası çağıranın yazmasını asla bozmaz" ilkesinin
  T6'ya doğrudan mirası.

## 8. Bu turda dispatch edilmeyen, sonraki (ayrı) bir turun işi

- Bu kontratın gerçek kodu (migration + edge function + varsa
  `ai_customize_requests` tablosu) — Claude Code'a ayrı, bounded bir
  dispatch olarak (bu turda değil, Berkin'in onayından sonra).
- T4-A2'nin INSERT'e genişletilmesi (§1 Bulgu 2'nin kalıcı kapatılması) —
  düşük öncelikli, ayrı bir backlog kalemi, T6 ile birlikte de gidebilir
  ayrı da.
- T6 UI implementasyonu — backend PR merge + bağımsız doğrulama (kural
  #96) tamamlanmadan ChatGPT/Codex'e "T6 UI READY" checkpoint'i
  gönderilmeyecek (Berkin'in açık talimatı).
- T6'nın Faz durumu değişmedi — hâlâ Faz 2 (T3+T4 bittikten sonra), bu
  tasarım turu otomatik olarak Faz 0'a girmiyor, Berkin aksini söylemedikçe.
