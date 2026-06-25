---
name: content-brief-writer
description: >
  İçerik yazarları ve AI araçları için detaylı brief oluşturur.
  İki modu vardır: Blog Brief ve Kategori Brief.
  Blog briefleri H1, keyword, alt başlık, inlink, title, description ve yazar notları içerir.
  Kategori briefleri CTR odaklı olup H1, keyword, alt başlık, inlink, title, description ve yazar notları içerir.
  Araştırmayı Ahrefs API ve SERP top 10 analizi + AI Overview incelemesi ile yapar.
  25 kategoriye kadar toplu brieflendirme destekler.
  Use when user says "content brief yaz", "brief oluştur", "blog brief",
  "kategori brief", "içerik briefi", "brief hazırla", "brieflendir",
  "kategori içerik", "blog içerik planı", "toplu brief".
user-invokable: true
argument-hint: "[blog|kategori] [konu/url listesi veya xlsx dosya yolu]"
license: MIT
metadata:
  author: inbound
  version: "1.0.0"
  category: seo-content
---

# Content Brief Writer

İçerik yazarlarına ve AI içerik araçlarına verilmek üzere detaylı, araştırma destekli briefler oluşturur. İki ana mod destekler: **Blog Brief** ve **Kategori Brief**.

## Kullanım

```
/content-brief-writer blog [başlık listesi veya xlsx dosya yolu]
/content-brief-writer kategori [başlık+url listesi veya xlsx dosya yolu]
```

---

## ADIM 0: Girdi Alma — BAŞLIK KULLANICIDAN GELİR

**KRİTİK KURAL: Başlıkları (H1) kullanıcı sağlar. Skill başlık üretmez, önermez veya değiştirmez.**

Kullanıcı aşağıdaki yollardan biriyle başlık sağlayabilir:
1. **Doğrudan mesajda**: "Blog brief hazırla: Ekran Görüntüsü Nasıl Alınır?"
2. **Liste halinde**: Birden fazla başlık alt alta
3. **xlsx dosyası ile**: Başlıklar ve URL'ler dosyadan okunur
4. **URL + başlık çifti olarak**: Her URL'nin yanında H1 başlığı

Kullanıcı başlık vermezse, brief oluşturma. Başlık iste.

---

## ADIM 1: Mod Belirleme

Kullanıcının girdisine göre modu belirle:

### Blog Brief Modu
- Kullanıcı blog başlıklarını sağladığında
- Informational intent ağırlıklı içerikler için
- Tek tek veya toplu blog briefi üretir
- **H1 kullanıcıdan gelir, aynen kullanılır**

### Kategori Brief Modu
- Kullanıcı kategori URL'leri + H1 başlıklarını sağladığında
- CTR odaklı (Click-Through Rate optimization)
- E-ticaret kategori sayfaları için
- 25 kategoriye kadar toplu brieflendirme destekler
- **H1 kullanıcıdan gelir, aynen kullanılır**

---

## ADIM 2: Araştırma (Her İki Mod İçin Zorunlu)

### 2a. Ahrefs Araştırması

Ahrefs MCP araçları ZORUNLU olarak kullanılacak:

1. **`keywords-explorer-overview`**: Hedef keyword'ün arama hacmi, difficulty, CPC verileri
2. **`keywords-explorer-related-terms`**: İlişkili keyword fırsatları
3. **`keywords-explorer-search-suggestions`**: Arama önerileri (long-tail fırsatlar)
4. **`serp-overview`**: SERP'teki ilk 10 sonucu analiz et
5. **`site-explorer-organic-keywords`**: (Mevcut sayfa varsa) Mevcut keyword sıralamaları
6. **`site-explorer-top-pages`**: Rakip sayfaların performansı

> **ÖNEMLİ**: Ahrefs `doc` tool'unu her tool'u ilk kez kullanmadan önce çağır.
> Parasal değerler USD sent cinsindendir, 100'e bölerek USD'ye çevir.
> `render_with` metadata'sı döndürüldüğünde belirtilen render tool'unu çağır.

### 2b. SERP Top 10 Analizi

Her hedef keyword için SERP'teki ilk 10 sonucu incele:

1. **WebSearch** ile hedef keyword'ü ara (Türkçe, Google TR)
2. İlk 10 organik sonucun URL'lerini topla
3. Her sonuç için **WebFetch** ile sayfayı çek:
   - H1, H2, H3 yapısını çıkar
   - Tahmini kelime sayısını belirle
   - Title ve meta description'ı oku
   - İç link yapısını incele
   - Schema markup varlığını kontrol et
4. Ortak ve eksik konuları belirle
5. Rakip içeriklerin güçlü ve zayıf yönlerini not et

### 2c. AI Overview İncelemesi

1. SERP sonuçlarında AI Overview varlığını kontrol et
2. AI Overview varsa:
   - Hangi kaynakları cite ettiğini belirle
   - Hangi soruları yanıtladığını not et
   - AI Overview'da eksik kalan konuları fırsat olarak işaretle
3. AI Overview yoksa:
   - Featured Snippet varlığını kontrol et
   - People Also Ask (PAA) sorularını topla
4. Brief'e "AI Overview Fırsatı" bölümü ekle

---

## ADIM 3: Brief Oluşturma

### Blog Brief Formatı

Her blog brief'i aşağıdaki yapıda olmalı:

```
┌─────────────────────────────────────────────────────────────────┐
│ BLOG BRIEF                                                       │
├─────────────────────────────────────────────────────────────────┤
│ URL          : [varsa mevcut URL, yoksa önerilen slug]           │
│ H1 (Başlık)  : [KULLANICININ VERDİĞİ BAŞLIK — değiştirme]      │
│ Keyword      : [ana keyword + destekleyici keywordler]           │
│ Arama Hacmi  : [aylık arama hacmi - Ahrefs'ten]                 │
│ KD           : [Keyword Difficulty - Ahrefs'ten]                │
├─────────────────────────────────────────────────────────────────┤
│ Alt Başlıklar (Hx):                                              │
│   H2 - [Alt başlık 1]                                            │
│   H2 - [Alt başlık 2]                                            │
│     H3 - [Alt alt başlık]                                        │
│   H2 - [Alt başlık 3]                                            │
│   ...                                                            │
├─────────────────────────────────────────────────────────────────┤
│ Link Verilecek Kelimeler (Internal Links):                       │
│   "[anchor text]" → [hedef URL]                                  │
│   "[anchor text]" → [hedef URL]                                  │
│   ...                                                            │
├─────────────────────────────────────────────────────────────────┤
│ Title Tag    : [50-60 karakter, keyword önde]                    │
│ Description  : [130-155 karakter, CTA ile biten]                │
├─────────────────────────────────────────────────────────────────┤
│ Yazara Notlar:                                                   │
│   - Hedef kelime sayısı: [X-Y kelime]                           │
│   - Rakip içerik analizi özeti                                   │
│   - Mutlaka değinilmesi gereken konular                          │
│   - CTA önerisi                                                  │
│   - Özel talimatlar (crosslink, tool mention vb.)               │
│   - AI Overview fırsatı notu                                     │
└─────────────────────────────────────────────────────────────────┘
```

### Kategori Brief Formatı (CTR Odaklı)

Her kategori brief'i aşağıdaki yapıda olmalı:

```
┌─────────────────────────────────────────────────────────────────┐
│ KATEGORİ BRIEF (CTR Odaklı)                                     │
├─────────────────────────────────────────────────────────────────┤
│ URL          : [kategori sayfası URL'si]                         │
│ H1           : [KULLANICININ VERDİĞİ BAŞLIK — değiştirme]       │
│ Ürün Sayısı  : [kategorideki ürün sayısı - varsa]               │
│ Arama Hacmi  : [aylık arama hacmi - Ahrefs'ten]                 │
│ KD           : [Keyword Difficulty - Ahrefs'ten]                │
├─────────────────────────────────────────────────────────────────┤
│ Keywordler   :                                                   │
│   Ana: [ana keyword]                                             │
│   Destekleyici: [kw1, kw2, kw3, ...]                            │
├─────────────────────────────────────────────────────────────────┤
│ Alt Başlıklar (Hx):                                              │
│   H2 - [Soru formatında, CTR artırıcı]                          │
│   H2 - [Soru formatında, CTR artırıcı]                          │
│   H2 - [Soru formatında, CTR artırıcı]                          │
├─────────────────────────────────────────────────────────────────┤
│ Link Verilecek Kelimeler (Internal Links):                       │
│   "[anchor text]" → [hedef URL]                                  │
│   "[anchor text]" → [hedef URL]                                  │
│   "[anchor text]" → [hedef URL]                                  │
│   ...                                                            │
├─────────────────────────────────────────────────────────────────┤
│ Title Tag    : [50-60 karakter]                                  │
│   Mevcut     : [mevcut title - eğer varsa]                      │
│   Önerilen   : [yeni title önerisi]                              │
│   Güncelle?  : [Evet/Hayır + sebep]                              │
│                                                                  │
│ Description  : [130-155 karakter]                                │
│   Mevcut     : [mevcut description - eğer varsa]                │
│   Önerilen   : [yeni description önerisi]                        │
│   Güncelle?  : [Evet/Hayır + sebep]                              │
├─────────────────────────────────────────────────────────────────┤
│ Yazara Notlar:                                                   │
│   - Hedef kelime sayısı: min 500 kelime                         │
│   - İç linkleri içeriğin farklı yerlerine dağıt                  │
│   - Her iç linki sadece 1 kere kullan                           │
│   - CTR artırma stratejisi                                       │
│   - Rakip sayfaların ortak özellikleri                           │
│   - AI Overview fırsatı                                          │
└─────────────────────────────────────────────────────────────────┘
```

---

## ADIM 4: Çıktı Formatı

### Tek Brief Çıktısı
- Yukarıdaki formata göre markdown olarak çıktı ver
- Her brief arasında net bir ayırıcı kullan

### Toplu Brief Çıktısı (xlsx)
- 25 kategoriye kadar toplu brieflendirme yapılabilir
- Toplu çıktı istendiğinde Excel (xlsx) formatında çıktı üret
- Sütunlar brief moduna göre ayarlanır

#### Blog Brief xlsx Sütunları:
| URL | H1 (kullanıcıdan) | Keyword | Alt Başlık - Hx | Link Verilecek Kelimeler | Title | Desc | Notlar |

#### Kategori Brief xlsx Sütunları:
| URL | H1 (kullanıcıdan) | Ürün Sayısı | Arama Hacmi | Keyword | Alt Başlık | Link Verilecek Kelimeler | Mevcut Title | Önerilen Title | Güncelle? | Mevcut Desc | Önerilen Desc | Güncelle? | Notlar |

---

## Kritik Kurallar

### CTR Odaklı Yaklaşım (Kategori Briefleri İçin)
- Alt başlıklar SORU formatında olmalı ("... Nelere Dikkat Edilmeli?", "... Nasıl Seçilir?")
- Title'da kullanıcıyı tıklamaya teşvik eden ifadeler kullan
- Description'da somut bilgi + CTA olmalı
- Ürün kategorisine uygun, alıcı niyetini yakalayan keywordler seç

### İç Link Kuralları
- Her brief'te en az 3, en fazla 6 iç link öner
- İç linkler sitedeki gerçek sayfaları hedeflemeli
- Anchor text doğal olmalı, keyword stuffing yapılmamalı
- Her iç link içerikte sadece 1 kere kullanılmalı
- İç linkler içeriğin farklı bölümlerine dağıtılmalı (yan yana değil)

### İç Link Keşfi — Erişim Sorunları ve Çözümleri

Bazı siteler JS rendering, bot koruması (Cloudflare, Akamai, PerimeterX vb.) veya rate limiting nedeniyle doğrudan taranamayabilir. Bu durumda aşağıdaki fallback zincirini SIRAyla uygula — ilk çalışan yöntemle devam et:

#### Seviye 1: Doğrudan Tarama (WebFetch)
- Hedef sayfayı ve ilişkili sayfaları WebFetch ile çek
- Çalışırsa: HTML'den iç linkleri, anchor text'leri ve URL yapısını çıkar
- **Başarısız olma belirtileri**: 403/503 hatası, boş body, Cloudflare challenge sayfası, CAPTCHA, "Please enable JavaScript" mesajı

#### Seviye 2: Sitemap Taraması
WebFetch başarısız olursa:
1. `domain.com/sitemap.xml` adresini çek
2. Sitemap yoksa `domain.com/sitemap_index.xml` dene
3. O da yoksa `domain.com/robots.txt` dosyasından `Sitemap:` satırını oku
4. Sitemap'ten tüm URL'leri çıkar
5. URL yapısından kategori/alt-kategori ilişkilerini belirle (örn: `/erkek/boxer/` → `/erkek/` üst kategorisi)
6. İlişkili URL'leri iç link önerisi olarak kullan

#### Seviye 3: Ahrefs Site Explorer
Sitemap de erişilemezse:
1. **`site-explorer-linked-anchors-internal`**: Sitenin kendi iç link yapısını ve anchor text'lerini çek
2. **`site-explorer-pages-by-internal-links`**: En çok iç link alan sayfaları bul
3. **`site-explorer-top-pages`**: Trafiğe göre en önemli sayfaları listele
4. **`site-explorer-crawled-pages`**: Ahrefs'in crawl ettiği sayfa listesinden URL yapısını çıkar
5. Bu verilerden ilişkili sayfaları ve doğal anchor text önerilerini türet

#### Seviye 4: Google site: Araması
Ahrefs verisi de yetersizse:
1. **WebSearch** ile `site:domain.com keyword` sorgusu yap
2. Sonuçlardan ilişkili sayfa URL'lerini ve Google'ın gösterdiği title/description'ları topla
3. Farklı keyword varyasyonlarıyla birden fazla `site:` sorgusu yap
4. Bulunan URL'leri iç link önerisi olarak kullan

#### Seviye 5: Chrome MCP ile Tarayıcı Üzerinden Tarama
Tüm otomatik yöntemler başarısız olursa ve Chrome MCP bağlıysa:
1. **`navigate`** ile hedef sayfayı aç (JS render edilir, bot koruması geçilir)
2. **`read_page`** ile sayfa yapısını oku (tüm linkler dahil)
3. **`find`** ile "navigation", "menu", "sidebar", "footer links" gibi bölümleri tara
4. **`get_page_text`** ile sayfa içeriğini çek
5. Bulunan iç linkleri ve anchor text'leri kaydet

#### Seviye 6: Kullanıcıya Sor
Hiçbir yöntem çalışmazsa:
1. Durumu açıkça raporla: hangi yöntemler denendi, neden başarısız oldu
2. Kullanıcıdan sitemap URL'si, sayfa listesi veya xlsx ile iç link hedefleri iste
3. Kullanıcı veri sağladığında onunla devam et

#### Raporlama
Her brief'te hangi yöntemle iç link keşfi yapıldığını Yazara Notlar bölümünde kısaca belirt:
- "İç linkler sitemap.xml'den türetildi" 
- "İç linkler Ahrefs crawl verisinden alındı"
- "İç linkler site: araması ile bulundu"
- "İç linkler tarayıcı üzerinden tarandı"

### Title & Description Kuralları
- Title: 50-60 karakter (asla 50'nin altında, 60'ın üstünde olmasın)
- Description: 130-155 karakter
- Kategori brieflerinde mevcut title/desc varsa karşılaştır ve sadece gerekli ise güncelleme öner
- Güncelleme önerirken sebebini açıkla

### Keyword Araştırma Kuralları
- Ana keyword + en az 5 destekleyici keyword belirle
- Ahrefs'ten arama hacmi ve difficulty verilerini çek
- Long-tail keyword fırsatlarını belirle
- Türkçe keyword varyasyonlarını (yazım hataları dahil) dahil et

### Yazar Notları Kuralları
- Hedef kelime sayısı aralığı ver (rakip analizine dayalı)
- Mutlaka değinilmesi gereken spesifik konuları listele
- CTA (Call to Action) önerisi ekle
- Crosslink edilmesi gereken ilişkili yazıları belirt
- Teknik terimler varsa sade dille açıklanması gerektiğini not et

### Dil Kuralları
- Tüm brief çıktıları Türkçe olmalı
- Keyword'ler Türkçe arama hacmine göre belirlenmeli
- Ahrefs sorgularında country: "tr" kullan
- SERP araştırmasında Google TR sonuçlarını analiz et

---

## Toplu İşlem Akışı (25 Kategoriye Kadar)

1. Kullanıcıdan başlık listesi (+ URL'ler) veya xlsx dosyası al
2. **H1 başlıkları kullanıcının verdiği şekilde kullan — değiştirme**
3. Her başlık/URL çifti için sırayla:
   a. (Kategori modu) Sayfayı fetch et (title, description, ürün sayısı)
   b. H1'den ana keyword'ü çıkar
   c. Ahrefs araştırması yap
   d. SERP top 10 analizi yap
   e. AI Overview kontrol et
   f. Brief oluştur (H1 aynen kalır)
4. Tüm briefleri tek bir xlsx dosyasında topla
5. Özet rapor sun (toplam keyword hacmi, ortalama KD, güncellenmesi gereken title/desc sayısı)

---

## Hata Yönetimi

| Senaryo | Aksiyon |
|---------|---------|
| URL erişilemez | Hatayı raporla, içerik tahmin etme, kullanıcıdan doğrulama iste |
| Ahrefs data yok | Keyword'ü not et, manual araştırma öner |
| SERP sonucu az | Daha geniş keyword ile tekrar ara |
| xlsx dosya okunamaz | Formatı kontrol et, desteklenen format bilgisi ver |
| 25'ten fazla URL | İlk 25'i işle, kalanları ikinci batch olarak öner |

---

## Örnek Çalışma Akışı

### Blog Brief Örneği
```
Kullanıcı: /content-brief-writer blog
  Başlıklar:
  - Ekran Görüntüsü Nasıl Alınır? (iPhone / Android / PC Rehberi)
  - Ekran Kaydı Nasıl Yapılır? Telefon ve Bilgisayar Rehberi

Her başlık için:
1. H1 = kullanıcının verdiği başlık (değiştirme!)
2. H1'den ana keyword'ü çıkar
3. Ahrefs → keywords-explorer-overview (country: tr)
4. Ahrefs → serp-overview: SERP ilk 10
5. WebSearch → Google TR'de ara
6. WebFetch → Top 5 rakip sayfayı analiz et
7. AI Overview kontrol
8. Brief oluştur (H1 aynen kalır)
```

### Kategori Brief Örneği
```
Kullanıcı: /content-brief-writer kategori
  - https://www.spx.com.tr/erkek/boxer/ → H1: Erkek Boxer
  - https://www.spx.com.tr/kadin/mayo-bikini/ → H1: Mayo & Bikini

Her URL+H1 çifti için:
1. H1 = kullanıcının verdiği başlık (değiştirme!)
2. WebFetch → Mevcut sayfayı çek (title, desc, ürün sayısı)
3. H1'den ana keyword'ü çıkar
4. Ahrefs → keywords-explorer-overview (country: tr)
5. Ahrefs → serp-overview: SERP ilk 10
6. WebSearch → Google TR'de ara
7. WebFetch → Top 5 rakip sayfayı analiz et
8. AI Overview kontrol
9. Mevcut title/desc ile önerileni karşılaştır
10. Brief oluştur (H1 aynen kalır)
```
