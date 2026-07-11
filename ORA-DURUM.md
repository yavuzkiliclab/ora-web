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

## Ora INTL - Oturum Durumu 2 (2026-07-11)

### Bu oturumda TAMAMLANAN (hepsi canlıda, staging)

**FAQ ayrı sayfası + ana sayfa trim** (commit'ler faq + trim)
- Yeni docs/faq.html (kök seviye, layout:base, legal-doc CSS, <details class="reveal"> accordion, 9 soru; orijinal 10'dan 2+4 birleştirildi)
- Ana sayfa FAQ 10'dan 4 öne çıkan soruya indirildi (days/language/package/start), diğer 6 sed satır-bazlı silindi (yedek /tmp'ye)
- "All questions ->" faq.html'e relative_url ile bağlandı
- Korunan işaretler doğrulandı: V2-BACKFILL 4, wa.me 2, ST-2800 1

**Title/SEO dinamikleştirme** (commit c76b470)
- head.html <title> + og:title + twitter:title dinamik: {{ page.title | default: "ana sayfa basligi" }}
- DIKKAT tirnak: <title> cift tirnak (dis tirnak yok), og/twitter content='...' TEK tirnak (content cift tirnakla sarili, cakisma onlendi)
- faq + privacy + terms + disclaimer front-matter'ina title eklendi
- Ana sayfa DEGISMEDI (title'i yok, default'a dusuyor), canlida dogrulandi

**8 disiplin tedavi sayfasi** (commit f7fe65c + implant ilk hali 8f30f62)
- docs/treatments/ altinda 8 sayfa: implants (genisletilmis), aesthetic-dentistry, crowns-and-bridges, oral-surgery, orthodontics, endodontics, periodontology, jaw-joint-and-pediatric
- HEPSI DRAFT (amber legal-flag + "awaiting physician review" + [Physician review TO CONFIRM]), Ingilizce, legal-doc CSS
- Her sayfa: giris + kime uygun + surec (process-steps numarali daire, head.html'de counter(step) CSS) + ne beklemeli + Recovery/aftercare + Common questions (<details> accordion 4 soru) + Related treatments (kardes sayfalara relative_url ic link) + neden Ora + CTA
- jaw/pediatric farkli yapi (TMJ + pediatric ayri bolum, process-steps YOK)
- Guvenli tibbi dil: spesifik oran/garanti YOK, "kisiye/hekime gore degisir", genel bakim (ilac/doz yok); endo+oral surgery en hassas
- Ana sayfa: 8 #work karti (01-08) + 2 #guide karti (implant, Veneers->aesthetic) relative_url ile bagli
- Ic link agi 404 vermiyor (8 birlikte push edildi), 8 sayfa 200 + kendi title, accordion/process-steps/DRAFT canli dogrulandi

### OGRENILEN teknik notlar (bu oturum)
- Uzun icerik CC->Claude aktarim katmaninda BOZUK gorunebilir (implant guncellemede yasandi), ama CC disk-ustu grep gercegi soyler. Cozum: scratch dosyaya yaz -> grep kanitla -> gercek konuma cp -> re-dogrula.
- Yapisal denge (details=/details=summary, section=1/1, ol=/ol=li) kaba kelime-regex'ten daha guvenilir kirik-HTML gostergesi. Kaba regex false positive uretir.
- 8 sayfayi BIRLIKTE push et (ic linkler ancak hedef sayfalar var olunca 404 vermez).
- Legacy deploy tekrar hatirlatildi: gh run / Actions ALAKASIZ, deploy dogrulama dogrudan canlidan (curl 200 poll + incognito).

### KALAN is
- Klinik/Dr. Mahmut 8 disiplin sayfasi TOPLU FIZIKSEL ONAY bekliyor. Onay sonrasi DRAFT bandi + [Physician review TO CONFIRM] kaldirma ayri tur.
- Klinik-spesifik teyit gereken iddialar: "in-house laboratory" (crowns), "3D imaging" (oral surgery/implant)

### ACIK BLOKORLER (degismedi, gercek yayin oncesi)
- Legal sayfalardaki [TO CONFIRM] + avukat onayi
- WhatsApp/telefon numarasi (wa.me placeholder)
- oradental.com domaini (Aytasdent Ltd. adina), baglaninca _config.yml + standalone og:url'ler duzeltilir
- Ana sayfa [TO CONFIRM]: Team "[Dr. Ad Soyad]", certs "[Ruhsat/Isletme Belgesi]" vb.
- github.io STAGING: numara + domain gelene kadar paylasilmiyor
