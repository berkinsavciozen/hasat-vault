Hasat Otomatik Tarif Üretim ve Yayınlama Pipeline'ı

Bu doküman kanonik ve canlı uygulama planıdır. 2026-08-17 tarihinde Hasat'ın canlı Supabase şeması, RLS politikaları, Edge Function desenleri ve gerçek hasat-webp.sh davranışıyla yapılan denetim plana işlenmiştir. Aşağıdaki kesinleşmiş kararlar önceki plandaki çelişen tüm önerilerin yerini alır.

1. Amaç ve başarı tanımı

Hasat'ın her hafta en az 10 yeni, kaliteli ve editoryal kontrolden geçmiş tarifi düşük manuel eforla üretebilmesini ve kontrollü biçimde yayınlamasını sağlamak.

Tarifler:

Türkiye pazarına ve Hasat'ın mevcut ürün/tarım taksonomisine uygun olmalı.

Bireysel alıcılar ile HoReCa kullanım senaryolarını dengeli kapsamalı.

Mevsimselliği canlı crop_config verisinden almalı.

Malzemeler, adımlar, süreler, zorluk, görseller ve gerekli yayın alanlarıyla birlikte üretilmeli.

Canlı recipes, recipe_ingredients ve recipe_steps yapısına güvenli ve tekrar çalıştırılabilir biçimde yazılmalı.

Gıda güvenliğiyle ilişkili sıcaklık, süre ve alerjen bilgileri için süresiz insan kontrolü gerektirmeli.

Metrik

İlk hedef

Haftalık planlanan tarif

10

Şema geçerlilik oranı

%100

Başarılı görsel varyantı

Tarif başına 2

Otomatik revizyon sınırı

En fazla 2 tur

İnsan onayı

Her tarif için zorunlu

Onaysız otomatik yayın

%0

2. Kesinleşmiş mimari kararlar

2.1 Çalışma zamanı

Zincirlenmiş, kısa ömürlü Supabase Edge Function çağrıları kullanılacak; ayrı veya sürekli çalışan bir Node.js servisi kurulmayacak.

Her invocation yalnızca bir aşama çalıştırır:

plan → write → qa → revise? → image → finalize → human approval → publish

recipe_generation_jobs satırı kalıcı devam noktasıdır. Her Edge Function:

Job'ı ve beklenen current_stage değerini okur.

İlgili aşamayı idempotent biçimde çalıştırır.

Ara sonucu ilgili tabloya yazar.

Job durumunu atomik olarak ilerletir veya failed yapar.

Gerekiyorsa sonraki Edge Function'ı tetikler.

Kısa ömürlü invocation'ı sonlandırır.

Sonraki aşamayı tetikleme mevcut dispatch_push / dispatch_sms desenini izlemelidir: pg_net / net.http_post, service-role kapsamlı mevcut anahtar deseni ve exception when others koruması. Tetikleme başarısızlığı tamamlanmış bir aşamanın sonucunu bozmamalıdır; job kaldığı aşamadan yeniden başlatılabilir olmalıdır.

2.2 OpenAI Agents SDK uygunluk kontrolü

Phase 0'ın ilk teknik spike'ı, OpenAI Agents SDK TypeScript paketinin Deno/Supabase Edge Runtime'da şu minimum yeteneklerle çalıştığını doğrulamalıdır:

Agent tanımı ve tek-agent run

Function tool çağrısı

Zod veya eşdeğer şema ile structured output

Timeout ve hata yakalama

Trace/run kimliğini uygulama kayıtlarına bağlama

Deno bundle/build uyumluluğu

Bu kontrol go/no-go kapısıdır. SDK uyumsuzsa yeni bir kalıcı runtime kurulmaz. Fallback:

Deno-native OpenAI API çağrısı

JSON Schema / Zod doğrulaması

Sınırlı retry

Function/tool çağrı döngüsünün yalnızca gereken küçük bölümü

Job tablosu üzerinden state ve resume

Uygulama seviyesinde stage/run logları

Pipeline kod tarafından orkestre edildiği için SDK'nın session ve handoff özellikleri kritik bağımlılık değildir.

2.3 Admin yetkilendirmesi

Yeni admin uç noktaları mevcut admin-kpi desenini aynen izlemelidir:

İstek x-admin-key header'ı taşır.

Değer ADMIN_DASHBOARD_KEY ile timing-safe karşılaştırılır.

Başarılı kontrolden sonra Edge Function service-role ile okur/yazar.

Lovable kullanıcı oturumu veya yeni bir is_admin RLS kurgusu oluşturulmaz.

Bu desen batch tetikleme, job retry/iptal, taslak onay/red/revizyon ve publish işlemlerinin tamamı için zorunludur. Canlı recipes tablosundaki normal kullanıcı INSERT politikası public/editoryal tarif yayınına izin vermediğinden normal kullanıcı oturumu kullanılmayacaktır.

2.4 Yayınlama sahipliği

Publish bir agent kararı veya agent tool'u olmayacak. İnsan onayı sonrası çalışan deterministik, idempotent bir Edge Function/RPC akışı olacaktır.

Agent'lar doğrudan canlı tarif yayınlayamaz/silemez, service-role kullanamaz veya serbest SQL çalıştıramaz.

3. Uçtan uca akış

flowchart TD
    A["Admin: haftalık batch"] --> B["Plan Edge Function"]
    B --> C["Writer Edge Function"]
    C --> D["QA Edge Function"]
    D -->|"Bloklayıcı sorun"| E["Revise Edge Function"]
    E --> D
    D -->|"Geçti"| F["Image Edge Function"]
    F --> G["Finalize Draft"]
    G --> H["Admin incelemesi"]
    H -->|"Revizyon"| E
    H -->|"Onay"| I["Deterministik Publish"]
    I --> J["Canlı Hasat tarifi"]

Pipeline state yalnızca yeni otomasyon tablolarında tutulur. Canlı recipes.status yalnızca draft | published kabul eder.

Önerilen job stage değerleri:

plan | write | qa | revise | image | finalize | awaiting_approval | publish

Önerilen job status değerleri:

queued | running | retryable | failed | awaiting_approval | approved |
rejected | completed | cancelled

4. Agent modeli

V1'de dört uzman rol vardır. Orkestratör bir LLM agent değil, job durumunu yöneten uygulama kodudur.

Bileşen

Agent mı?

Sorumluluk

Stage orchestrator

Hayır

Durumu okur, aşamayı çalıştırır, sonraki aşamayı tetikler

Portfolio Planner

Evet

Haftalık dengeli tarif brief'leri üretir

Recipe Writer

Evet

Bir brief'i tam tarif taslağına dönüştürür

Recipe QA Reviewer

Evet

Tutarlılık, uygulanabilirlik ve güvenlik incelemesi yapar

Image Director

Evet

Gemini promptu ve sabit görsel işleme spec'i üretir

Image processor

Hayır

14% chop, crop, WebP ve Storage yüklemesi yapar

Publisher

Hayır

Onaylı taslağı transaction/idempotency ile canlı tablolara yazar

Agent'lar birbirlerine serbestçe handoff yapmaz. Her agent dar kapsamlı input alır ve şemalı output üretir. Edge Function sonraki adımı belirler.

5. Agent ve aşama sözleşmeleri

5.1 Portfolio Planner

Amaç: Haftalık tarif portföyünü dengeler; aynı tip yemeklerin, ana ürünlerin veya mevcut tarif kopyalarının artmasını önler.

Zorunlu input

{
  "batch_id": "uuid",
  "requested_count": 10,
  "week": "2026-W34",
  "target_market": "TR",
  "audiences": ["bireysel", "horeca"],
  "editorial_constraints": {},
  "excluded_crops": [],
  "excluded_recipe_types": []
}

Tool/RPC

Görev

get_seasonal_crop_candidates

crop_config harvest window ve seasonality alanlarından adayları getirir

search_existing_recipes

Başlık, slug, ana crop ve benzerlik adaylarını getirir

get_recent_recipe_mix

Son tariflerin öğün, zorluk, crop ve kitle dağılımını verir

validate_recipe_plan

Brief setinin adet, crop ve tekrar kurallarını doğrular

Mevsimsellik kaynağı doğrudan crop_config; ingredient referansı crop_config.crop ile eşleşen text crop değeridir.

Structured output: RecipePlanBatch

{
  "weekly_theme": "Yaz ürünleriyle pratik tarifler",
  "briefs": [{
    "working_title": "Fırında Peynirli Kabak",
    "primary_crop": "kabak",
    "meal_type": "ana_yemek",
    "difficulty": "kolay",
    "audience": ["bireysel"],
    "servings": 4,
    "target_total_minutes": 40,
    "selection_reason": "Mevsimsel ve son tariflerle düşük benzerlikte",
    "required_traits": []
  }]
}

Planner output'u validate_recipe_plan geçmeden job yaratılmaz.

5.2 Recipe Writer

Amaç: Onaylanmış brief'i Türkçe, uygulanabilir ve canlı şemaya dönüştürülebilir tarife çevirir.

Input: RecipeBrief, ilgili crop_config kaydı, editoryal kurallar, izin verilen birimler, canlı şema sözleşmesi ve varsa önceki draft + structured QA feedback.

Tool/RPC

Görev

get_crop_context

İlgili crop_config kaydını kontrollü getirir

get_recipe_editorial_rules

Dil, ton, alan limitleri ve içerik politikası

normalize_recipe_units

Birimleri paylaşılan standarda dönüştürür

find_recipe_duplicates

Başlık/slug/içerik benzerliği adaylarını verir

Structured output: RecipeDraftPayload

{
  "title": "Fırında Peynirli Kabak",
  "slug": "firinda-peynirli-kabak",
  "summary": "Mevsim kabağıyla hazırlanan pratik bir ana yemek.",
  "difficulty": "kolay",
  "servings": 4,
  "prep_time_minutes": 15,
  "cook_time_minutes": 25,
  "ingredients": [{
    "name": "kabak",
    "quantity": 4,
    "unit": "adet",
    "preparation": "yıkanmış",
    "optional": false,
    "crop": "kabak"
  }],
  "steps": [{
    "order": 1,
    "instruction": "Fırını 190°C alt-üst ayarda ısıtın.",
    "duration_minutes": 5
  }],
  "tips": [],
  "storage_guidance": "",
  "dietary_claims": [],
  "safety_review_items": [{
    "type": "temperature",
    "value": "190°C",
    "human_review_required": true
  }]
}

Zorunlu kurallar:

Ingredient referansı crop_id değil text crop alanıdır.

difficulty yalnızca kolay | orta | zor olabilir.

allergen_labels canlı recipes şemasında yoktur. Migration kararına kadar persisted recipe payload'ına dahil edilmez.

Alerjen analizi geçici QA/safety review verisi olabilir; canlı tarifte saklandığı izlenimi verilmez.

Pipeline status değerleri output'a veya recipes.status alanına eklenmez.

Writer doğrudan DB'ye yazmaz; Edge Function doğrulanmış sonucu draft sürümü olarak saklar.

5.3 Recipe QA Reviewer

Input: Orijinal brief, güncel draft, editoryal kurallar, Postgres validation sonucu, benzer tarif adayları ve önceki QA/revision geçmişi.

RPC

Kontrol

validate_recipe_slug

Slug formatı ve tekilliği

validate_recipe_ingredient_coverage

Her malzemenin adımlarda kullanılması ve adım referanslarının varlığı

normalize_recipe_units

Birim standardizasyonu

validate_recipe_crop_values

ingredients.crop değerlerinin crop_config.crop ile uyumu

validate_recipe_structure

Zorunlu alanlar, sıralar, pozitif süre/miktar kuralları

Bu ortak validasyonlar Edge Function içine gömülmez; automation ve ileride extract-recipe tarafından kullanılmak üzere Postgres RPC olur.

Agent açıklık, uygulanabilirlik, malzeme mantığı, pişirme tekniği, Hasat ilgisi, özgünlük ve desteklenmeyen sağlık iddialarını değerlendirir.

Structured output: RecipeQAResult

{
  "decision": "revision_required",
  "overall_score": 82,
  "scores": {
    "clarity": 90,
    "feasibility": 80,
    "ingredient_consistency": 75,
    "originality": 70,
    "hasat_relevance": 85
  },
  "blocking_issues": [{
    "code": "UNUSED_INGREDIENT",
    "location": "ingredients[5]",
    "description": "Maydanoz hiçbir adımda kullanılmıyor.",
    "required_change": "Son adıma ekle veya malzemeden çıkar."
  }],
  "non_blocking_suggestions": [],
  "human_safety_review": {
    "temperature": true,
    "timing": true,
    "allergens": true,
    "notes": []
  },
  "approved_for_imaging": false
}

Temperature, timing ve allergen alanları skor yüksek olsa bile otomatik yayın muafiyeti oluşturmaz.

5.4 Revision

Revision ayrı bir serbest agent değildir; Recipe Writer önceki draft + structured QA blocking issues ile yeniden çalıştırılır. Output patch değil, yeni ve tam RecipeDraftPayload sürümüdür.

En fazla 2 otomatik revision turu.

Her turdan sonra RPC validation ve QA tekrar çalışır.

İki turdan sonra blocking issue kalırsa manuel revizyon gerekir.

Eski draft ve QA sonuçları silinmez.

5.5 Image Director

Üretici: Google Gemini image generation, “nano banana”nın entegrasyon anındaki en güncel seçilmiş sürümü. OpenAI image API kullanılmaz.

Input: QA'den geçmiş draft, ana crop, Hasat görsel kuralları, Gemini prompt kuralları, sabit source/crop parametreleri ve Storage/dosya kuralları.

Structured output: RecipeImageSpec

{
  "provider": "google_gemini",
  "source_width": 2048,
  "source_height": 2048,
  "prompt": "...",
  "negative_requirements": ["metin yok", "ek logo yok", "deforme araç yok"],
  "processing": {
    "chop_right_percent": 14,
    "chop_bottom_percent": 14,
    "crop_anchor": "center",
    "targets": ["16:9", "1:1"],
    "format": "webp",
    "quality": 82,
    "strip_metadata": true
  },
  "files": {
    "hero": "firinda-peynirli-kabak-16x9.webp",
    "square": "firinda-peynirli-kabak-1x1.webp"
  }
}

Agent sabit geometrinin yeniden keşfine çalışmaz.

6. Deterministik görsel pipeline

Mevcut hasat-webp.sh davranışı server-side image processing içinde birebir tekrar edilir:

Gemini'den 2048×2048 kare source üret.

Orijinali draft işleme alanında job'a bağla.

Sağdan %14 ve alttan %14 chop uygula.

Kalan görüntüyü merkezden 16:9'a crop et.

Aynı chopped source'u merkezden 1:1'e crop et.

Metadata'sız WebP quality 82 encode et.

Turkish→ASCII slug/filename normalizasyonu uygula.

Dış piksel bantlarında siyah/beyaz yoğunluğunu kontrol et.

Frame şüphesi varsa otomatik düzeltme yapma; human-review flag koy.

Doğrulanan asset'leri mevcut public crop-photos bucket'ına yükle.

Crop reference:  {crop-slug}.webp
Recipe hero:     {recipe-slug}-16x9.webp
Recipe square:   {recipe-slug}-1x1.webp

Normalization: ğ→g, ı→i, ş→s, ç→c, ö→o, ü→u, İ→i.

hasat-webp.sh içindeki 27 crop + 18 recipe × 2 = 63 manifest mevcut toplu asset setine aittir. Haftalık dinamik pipeline her job için beklenen iki recipe asset'ini recipe_assets üzerinden doğrular; sabit 63 haftalık job kuralı değildir.

Supabase Edge Runtime'da native sharp uyumluluğu Phase 0'da doğrulanmalıdır. Çalışmazsa aynı piksel sonucunu üreten Deno/WASM uyumlu library seçilir; yeni always-on servis kurulmaz.

7. Tool ve güven sınırları

Agent tool'ları serbest SQL veya genel Supabase client değildir. Her tool dar kapsamlı, typed ve allow-list edilmiş wrapper'dır.

Salt-okunur: get_seasonal_crop_candidates, get_crop_context, search_existing_recipes, get_recent_recipe_mix, get_recipe_editorial_rules.

Postgres validation RPC: validate_recipe_plan, validate_recipe_slug, validate_recipe_structure, validate_recipe_ingredient_coverage, validate_recipe_crop_values, normalize_recipe_units, find_recipe_duplicates.

Edge Function'ın deterministik yazıları: batch/job oluşturma, draft/QA/asset saklama, stage ilerletme ve job failure kaydı.

Agent'a verilmeyecek: publish_recipe, unpublish_recipe, replace_live_recipe, delete_recipe, execute_sql, service_role_client.

8. Önerilen migration taslağı

Bu bölüm öneridir; canlı naming, constraint ve index kontrolünden sonra gerçek SQL migration'a çevrilir.

recipe_generation_batches

id uuid PK, requested_count integer, week_key text, input_json jsonb, plan_json jsonb nullable, status text CHECK, created_by text, created_at, started_at, completed_at, error_summary jsonb.

recipe_generation_jobs

id uuid PK, batch_id uuid FK, recipe_id nullable (canlı PK türüyle aynı), brief_json jsonb, current_stage text CHECK, status text CHECK, revision_count integer default 0, attempt_count integer default 0, locked_at, lock_token uuid, next_attempt_at, last_error_code, last_error_json jsonb, agent_run_ref, created_at, updated_at, completed_at.

Stage + job bazlı idempotency key veya stage-run kaydı gerekir. Çift invocation ikinci draft, asset veya canlı recipe üretmemelidir.

recipe_drafts

id uuid PK, job_id uuid FK, version integer, recipe_json jsonb, schema_version text, validation_json jsonb, human_review_status text CHECK, human_review_notes, reviewed_at, created_at.

recipe_qa_results

id uuid PK, job_id uuid FK, draft_id uuid FK, decision text CHECK, overall_score numeric, scores_json jsonb, blocking_issues_json jsonb, suggestions_json jsonb, safety_review_json jsonb, agent_run_ref, created_at.

recipe_assets

id uuid PK, job_id uuid FK, draft_id uuid FK, asset_type text CHECK (source|hero_16x9|square_1x1), provider, provider_model, storage_bucket, storage_path, width, height, format, quality, processing_json jsonb, validation_status text CHECK, validation_json jsonb, prompt, created_at.

Yeni tablolar public/client yazımına açılmaz. Admin Edge Functions service-role ile yazar; admin endpoints x-admin-key doğrular.

9. Publish sözleşmesi

publish-recipe şu precondition'ları doğrular:

x-admin-key geçerli.

Job approved ve publish stage'inde.

Onaylanan draft version açıkça belli.

Son QA aynı draft version'a ait ve blocking issue yok.

Temperature, timing ve allergen insan checklist'i tamamlanmış.

Hero ve square asset doğrulanmış ve crop-photos içinde erişilebilir.

Slug publish anında yeniden unique.

Ingredient crop değerleri geçerli.

Aynı job daha önce publish edilmemiş.

Transaction veya transaction-equivalent RPC içinde:

recipes kaydı draft olarak oluştur/güncelle.

Ingredients yaz.

Steps yaz.

Asset path/URL mapping'ini uygula.

Son doğrulamaları çalıştır.

recipes.status = 'published' yap.

Job'a canlı recipe_id yaz ve completed yap.

Partial public recipe kalmamalı; retry ikinci recipe oluşturmamalıdır.

10. Admin deneyimi

Batch oluşturma

Hafta ve tarif adedi

Hedef kitle dağılımı

Dahil/harici crop'lar

Tarif tipi ve zorluk dengesi

Editoryal tema/not

Planı üret ve onayla

Draft inceleme

Stage/status, tarif preview, malzemeler/adımlar

16:9 ve 1:1 görseller

QA puanları, blocking issues ve RPC validation

Temperature/timing/allergen insan checklist'i

Frame-suspicion uyarısı ve revision geçmişi

Approve / reject / request revision / retry stage

ChatGPT-operated admin tarafı aynı Edge Function endpointlerine x-admin-key ile bağlanır; doğrudan service-role credential almaz.

11. Kalite ve insan kontrolü

Otomatik kapılar

JSON schema geçerli; difficulty Türkçe enum.

Slug doğru/unique; ingredient-step coverage geçerli.

Birimler normalize; crop değerleri crop_config ile uyumlu.

Adım sırası kesintisiz; süre/miktarlar pozitif ve makul.

Desteklenmeyen sağlık iddiası yok; duplicate eşiği kontrol edilmiş.

İki image variant mevcut; 14% chop/crop uygulanmış.

WebP q82, metadata strip ve normalize filename doğrulanmış.

Frame suspicion varsa warning kaydı var.

Süresiz insan kontrolü

Pişirme sıcaklıkları

Pişirme ve güvenli bekletme süreleri

Alerjen tespiti/etiketi

Bu kontrol QA score ile bypass edilemez. V1'de ayrıca her tarifin tamamı ve her AI görseli insan tarafından onaylanır.

12. Gözlemlenebilirlik ve retry

Her stage için: batch_id, job_id, stage, attempt, timestamps, duration_ms, provider/model, schema version, usage varsa, agent_run_ref/provider_request_id, result ve safe error payload kaydedilir.

Network/429/5xx: bounded exponential backoff + next_attempt_at.

Schema failure: sınırlı structured-output retry.

İçerik QA failure: revise stage; API retry değildir.

Deterministik validation failure: issue olarak Writer'a iletilir.

Image failure: sınırlı regeneration; frame warning insan review.

Publish failure: idempotent retry.

Maksimum deneme sonrası job failed ve manuel retry bekler.

Çift tetiklemeye karşı job lock/claim ve atomik compare-and-set gerekir.

13. OpenAI Agents SDK'nın katkısı

SDK state machine'in yerini almaz; uyumluysa kısa stage içindeki model loop'unu sadeleştirir.

SDK özelliği

Hasat'taki kullanım

Agent tanımı

Planner, Writer, QA ve Image Director için ayrı contract

Function tools

Dar RPC/read fonksiyonlarını typed çağırma

Structured outputs

Plan, draft, QA ve image spec şema garantisi

Guardrails

Input/output policy ve schema kontrolleri

Tracing

Model/tool çağrılarını job/stage ile ilişkilendirme

Usage

Tarif ve stage başına maliyet/latency

Kritik olmayanlar: uzun session memory, serbest handoff, SDK-managed long-running orchestration ve agent-controlled publishing. SDK uygun değilse minimum gerekli yetenek Deno-native uygulanır.

14. Uygulama fazları

Phase 0 — Teknik spike ve canlı sözleşme

Agents SDK'yı gerçek Supabase Edge/Deno ortamında test et.

Gemini API erişimini Edge Function'dan test et.

2048×2048 → 14% chop → 16:9 + 1:1 → WebP q82 pixel-output testi yap.

Canlı recipe/ingredient/step mapping'ini dokümante et.

Automation tabloları için gerçek migration SQL'i çıkar.

Postgres validation RPC imzalarını tasarla.

Existing chaining ve admin auth desenlerinden shared helper tasarla.

RecipeBrief, RecipePlanBatch, RecipeDraftPayload, RecipeQAResult, RecipeImageSpec şemalarını oluştur.

Açık kararları kapat.

Exit: SDK/fallback kararı, image runtime kanıtı, review-ready migration, tam live mapping ve chaining/auth POC.

Phase 1 — Tek tarif: Writer + QA

Migration/RPC'ler, Writer/QA stage functions, draft versioning, iki revision sınırı, lock/idempotency/retry ve basit admin draft görünümü.

Exit: Bir brief, elle DB düzeltmesi olmadan schema-valid ve QA-reviewed draft olur.

Phase 2 — Gemini görsel üretimi

Image Director, Gemini 2048×2048, 14% chop, crop, WebP q82, filename normalization, crop-photos upload, frame warning ve preview.

Exit: Onaylı draft iki doğru varyantla finalize olur; yerel script gerekmez.

Phase 3 — İnsan onayı ve publish

Admin approval endpoints, safety checklist, transactional/idempotent publish, live fetch verification ve recovery.

Exit: İnsan onaylı draft manuel Supabase işlemi olmadan canlıda görünür.

Phase 4 — Haftalık 10 tarif

Planner, batch/job fan-out, controlled concurrency/rate limits, progress ekranı, trigger ve 10 tariflik pilot.

Exit: Bir batch 10 review-ready tarif üretir; tek job hatası diğerlerini durdurmaz.

Phase 5 — Kalite, maliyet ve ölçek

Eval seti, approval/revision/latency/cost metrikleri, duplicate iyileştirme, prompt/schema döngüsü, bucket ayrıştırma değerlendirmesi ve yalnız gıda güvenliği dışındaki alanlar için kontrollü auto-approval araştırması.

Değişmez: Temperature, timing ve allergen insan onayında kalır.

15. Açık kararlar ve önerilen yön

Karar

Önerilen yön

Durum

hasat-webp.sh konumu

Runtime implementation'a yakın repo scripts/reference/; Edge kodu shared image module

Açık

AI recipe author type

Analitik için hasat_ai; constraint/UI etkisi migration'da incelensin

Açık

AI disclosure

Görselde mevcut “temsili görsel”; recipe-level kısa editoryal-AI açıklaması ayrıca kararlaştırılsın

Açık

Alerjen migration zamanı

Phase 1 ile birlikte; publish başlamadan kalıcı yer oluşsun

Açık

Exact automation schema

Bu taslak canlı naming/index/FK kurallarıyla SQL review'da netleştirilsin

Açık

16. V1 kapsam dışı

Serbest agent handoff'ları ve onaysız yayın

Ayrı memory/RAG ve multi-model routing

Karmaşık prompt-version platformu

Agent'a genel Supabase/service-role/SQL erişimi

Yeni always-on server

Watermark konumunu per-image yeniden tespit eden model

Frame warning'lerini sessizce otomatik düzeltme

17. Phase 0 başlangıç checklist'i

Agents SDK Deno spike branch'i açıldı

SDK go/no-go testi yazıldı

Gemini Edge Function çağrısı doğrulandı

Image processing pixel-output testi doğrulandı

Chaining ve admin auth referansları incelendi

Canlı recipe mapping tamamlandı

Automation migration ve validation RPC SQL draft'ları hazırlandı

Canonical structured-output schemas hazırlandı

Author type, disclosure ve allergen kararları kapandı

Tek recipe pilot brief seçildi

18. Referans implementasyonlar

dispatch_push / dispatch_sms: Postgres → Edge Function chaining ve hata izolasyonu.

admin-kpi: x-admin-key, timing-safe ADMIN_DASHBOARD_KEY ve service-role erişimi.

extract-recipe: Ayrı AI recipe import akışı; gelecekte ortak validation RPC'leri kullanabilir.

hasat-webp.sh: 14% chop, aspect crop, WebP q82, metadata strip, filename normalization, manifest ve frame warning referansı.
