# Marka Kimliği Entegrasyonu — Hasat OS Milestone 3 (W2 wordmark + M1 monogram)

## Karar

Wordmark ailesi **W2**, monogram ailesi **M1** — MVP production candidate:
`W2A_wordmark.svg`, `M1A_monogram.svg`, `W2A_M1A_lockup.svg`. Karar Hasat
OS'te Milestone 3 — Brand Identity Freeze olarak kayıtlı.

Kaynak Drive klasörü: https://drive.google.com/drive/folders/11ITHk-rQ8IJ15fyd2cZeaXqRScsdCoeq

## Bu turda yapılan düzeltmeler (orkestratör oturumu, 2026-08-20/21)

Entegrasyon öncesi/sırasında iki gerçek, ölçekten bağımsız kusur bulunup
düzeltildi — path geometrisinin "yeniden tasarımı" değil, teknik/optik
düzeltme:

1. **M1A monogram küçük-ölçek okunabilirliği** — orijinal M1A 16-24px'te
   (favicon, küçük app-icon) aliasing yüzünden "Open Connection H" yerine
   Kiril "И"ye benziyordu. Düzeltme: stroke-width 18→22, çift-eğrili
   bağlantı → tek sade eğri. Aynı konsept/renk/viewBox korundu.
   → `M1A_monogram_v2_kucuk-olcek-duzeltmesi.svg` (Drive)
2. **Lockup ayırıcı çizgisi** — `W2A_M1A_lockup.svg`'de ayırıcı çizgi
   (x=625) wordmark'ın son harfi "T"nin ink'inin üzerinden geçiyordu
   (mobil entegrasyonu yapan Claude Code oturumu buldu, orkestratör hem
   matematiksel hem native-render ile bağımsız doğruladı). Düzeltme:
   çizgi x=625→660 (wordmark ile monogram arasındaki boşluğun ortası).
   → `W2A_M1A_lockup_v3_ayirici-cizgi-duzeltmesi.svg` (Drive)

Orijinal dosyalar Drive'da silinmedi. Bu iki düzeltme henüz Hasat OS'te
ayrıca "frozen" olarak işaretlenmedi — Berkin/ChatGPT onayı bekliyor.

## Mobil entegrasyon — hasat-mobile#33

- Kaynak dosyalar: W2A_wordmark.svg, M1A_monogram_v2_kucuk-olcek-duzeltmesi.svg, W2A_M1A_lockup_v2_kucuk-olcek-duzeltmesi.svg
- Değiştirilen dosyalar: assets/brand/*.svg (3 yeni), src/components/hasat/BrandLogo.tsx (yeni), app/login.tsx, app/_layout.tsx, app.json, assets/{icon,android-icon-*,favicon,splash-icon}.png (yeniden üretildi), assets/android-notification-icon.png (yeni), package.json (+react-native-svg ^15.15.5)
- Üretilen asset'ler: icon.png 1024×1024, android-icon-foreground.png 512×512, android-icon-monochrome.png 432×432, android-icon-background.png 512×512 (flat, eski Android Studio guide-line kalıntısı temizlendi), favicon.png 48×48, splash-icon.png 1024×1024, android-notification-icon.png 192×192
- Test sonuçları: tsc temiz, expo config temiz, expo export --platform ios (offline) 1860 modül hatasız, 16-64px okunabilirlik testi geçti, adaptive-icon/iOS icon mask önizlemeleri safe-zone içinde
- Bilinen sınırlamalar: lockup dosyası hiçbir üretim ekranında kullanılmadı (kusur nedeniyle, o zaman düzeltilmemişti); splash/adaptive-icon arkaplan rengi kararı ayrı, bu turda değiştirilmedi; gerçek cihaz testi yapılamadı
- PR: https://github.com/berkinsavciozen/hasat-mobile/pull/33

## Web entegrasyon — hasat-d2c-marketplace#44

- Kaynak dosyalar: W2A_wordmark.svg, M1A_monogram_v2_kucuk-olcek-duzeltmesi.svg, W2A_M1A_lockup_v2_kucuk-olcek-duzeltmesi.svg
- Değiştirilen dosyalar: src/assets/brand/* (6 SVG — ink+white varyantları), src/components/hasat/BrandLogo.tsx, src/routes/{__root,index,farmer,buyer,login,join,onboarding.farmer,privacy,terms}.tsx, public/{favicon.ico,favicon-16x16.png,favicon-32x32.png,apple-touch-icon.png,android-chrome-192x192.png,android-chrome-512x512.png,site.webmanifest}
- Üretilen asset'ler: favicon.ico (16/32/48 gömülü), favicon-16x16.png, favicon-32x32.png, apple-touch-icon.png (180×180), android-chrome-192x192.png, android-chrome-512x512.png — hepsi M1A-v2 monogramından
- Test sonuçları: tsc/eslint'te yeni hata yok (pre-existing durum korundu); build sandbox kısıtı nedeniyle çalıştırılamadı; Playwright ile gerçek bağlamda görsel doğrulama yapıldı
- Bilinen sınırlamalar: onboarding.buyer.tsx'te hiç logo yok (kapsam dışı bırakıldı); canlı build/dev server doğrulaması sandbox'ta yapılamadı; lockup hiçbir ekranda kullanılmadı (aynı kusur nedeniyle)
- PR: https://github.com/berkinsavciozen/hasat-d2c-marketplace/pull/44

## Doğrulama (orkestratör oturumu, kural #96)

İki PR da GitHub/Lovable üzerinden doğrudan okunarak bağımsız doğrulandı —
tüm iddialar (dosya değişiklikleri, kaldırılan placeholder'lar, üretilen
asset boyutları/renkleri, monogram/lockup path verisinin kaynakla
birebir eşleştiği) teyit edildi, hiçbir tutarsızlık bulunmadı.
