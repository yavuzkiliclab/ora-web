## Ora INTL - Oturum Durumu (2026-07-10 gece)

### Bu oturumda TAMAMLANAN (hepsi canlıda, staging)

**FAZ 3.0 - Jekyll tek-kaynak iskelet** (commit b171e69)
- _config.yml, _includes/head.html + header.html + footer.html, _layouts/base.html
- index.html front-matter layout:base'e çevrildi
- Manuel merge-diff: içerik farkı sıfır. Canlı: head=1/body=1, pixel-diff 0.

**FAZ 3.1 - relative_url yol stratejisi** (commit'ler 2813d25, 2ea1cd5, 3c9d1aa)
- _config.yml'e baseurl: "/ora-web" eklendi (ADIM1, izole test, ana sayfa pixel-diff 0)
- head.html (7 asset url) + header.html (9 link/nav) relative_url filtresine çevrildi (ADIM2)
  Syntax: {{ '/assets/img/x.png' | relative_url }} ve nav {{ '/' | relative_url }}#hedef
- Alt-klasör kanıtlandı (ADIM3): legal/privacy.html'de logo src /ora-web/assets/... (legal/assets DEĞİL), nav /ora-web/#work, kırık-img yok
- KANIT: hem kök hem alt-klasör /ora-web/... üretiyor, klasör derinliğinden bağımsız

**FAZ 4 - Legal sayfalar** (commit'ler 670dc76, 449123c, 224db5a)
- docs/legal/privacy.html (h2=10), terms.html (h2=9), disclaimer.html (h2=7)
- Üçü de ÖZGÜN (kopya değil), İngilizce, DRAFT işaretli, veri sorumlusu Aytaşdent Ltd.
- legal-doc/legal-flag/legal-updated CSS head.html'de (--porcelain #FBFCFD, --marine-soft #4A6274)
- Footer'da üç link relative_url ile bağlı, iki derinlikte de /ora-web/legal/*.html çözüyor
- Canlı doğrulama: üç sayfa 200, doğru başlıklar, amber DRAFT bandı #FFF5E6, kırık-img yok, konsol temiz

### ÖĞRENILEN teknik notlar
- overwrite/create aracı mevcut dosyayı TEMİZLEMEDEN üstüne ekliyor (bozuk HTML). ÇÖZÜM: rm + cat > dosya << 'EOF' heredoc ile sıfırdan yaz. Her legal sayfa böyle yazıldı.
- Jekyll _ ile başlayan dosyaları sessizce derlemez (404). Sayfa adları _ ile başlamamalı.
- Pages deploy LEGACY branch-deploy (docs/), Actions workflow YOK. Deploy durumu Actions/pages-builds API'de güvenilmez; doğrulama doğrudan canlıdan (curl 200 + incognito).
- Lokal Jekyll build yok (Ruby 2.6.10 vs ffi çakışması). Doğrulama push sonrası canlıda.

### KALAN iş
- 8 disiplin sayfası + FAQ. FAQ girdisiz gidebilir. Disiplin sayfaları tıbbi içerik: Dr. Mahmut hekim onayı + Bosphorus'tan kopyalamama gerekiyor.

### AÇIK BLOKÖRLER (gerçek yayın öncesi)
- Legal sayfalardaki [TO CONFIRM] alanları (tarih, gizlilik e-postası, saklama süreleri, iptal/uyuşmazlık mahkemeleri) + avukat onayı
- WhatsApp/telefon numarası (wa.me placeholder)
- oradental.com domaini (Aytaşdent Ltd. adına tescil), bağlanınca _config.yml baseurl/url güncellenir
- github.io STAGING: numara + domain gelene kadar paylaşılmıyor
