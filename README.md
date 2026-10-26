# T.C. Millî Eğitim Bakanlığı - Özel Eğitim Uygulama Okulu Ders Kitapları Portalı

Bu doküman, projeyi inceleyen geliştiriciler, içerik yöneticileri ve **Yapay Zekâ (AI / LLM) kodlama asistanları** için sistemin mimarisini, dosya yapısını, kurallarını ve geliştirme süreçlerini eksiksiz olarak açıklamak üzere hazırlanmıştır.

---

## 📌 1. Proje Özeti ve Kurumsal Kimlik

- **Ana Kurum Adı:** T.C. Millî Eğitim Bakanlığı (MEB)
- **Bağlı Genel Müdürlük:** Özel Eğitim ve Rehberlik Hizmetleri Genel Müdürlüğü (ORGM)
- **Proje Amacı:** Özel Eğitim Uygulama Okullarında (1, 2 ve 3. kademeler) okutulan 11 farklı derse ait 69 adet dijital ders kitabının ve öğretim materyallerinin web üzerinden incelenmesi, video önizlemeleriyle keşfedilmesi ve yüksek çözünürlüklü PDF formatında ücretsiz indirilmesi.
- **Yayın Türü:** Statik HTML5 / Vanilla CSS3 / Vanilla JavaScript (Sunucu bağımsız, taşınabilir).

---

## ⚠️ 2. GELİŞTİRİCİLER VE YAPAY ZEKÂLAR İÇİN ALTIN KURAL (Single Source of Truth)

> [!IMPORTANT]
> **HTML DOSYALARINI (`index.html` ve alt sayfaları) ASLA ELLE DÜZENLEMEYİN!**  
> Bu projedeki tüm HTML sayfaları (`index.html` ve 11 kategori sayfası), `sitemap.xml` ve `robots.txt` dosyaları [build_site.py](file:///c:/Users/Administrator/Desktop/kodlar/orgm_site3/build_site.py) Python derleyicisi tarafından üretilmektedir.
>
> Yapılacak tüm HTML, CSS, JavaScript, SEO veya içerik değişiklikleri **[build_site.py](file:///c:/Users/Administrator/Desktop/kodlar/orgm_site3/build_site.py)** dosyasında yapılmalı, ardından terminalden:
> ```bash
> python build_site.py
> ```
> komutu çalıştırılarak tüm site senkronize olarak yeniden üretilmelidir.

---

## 📂 3. Dizin ve Dosya Yapısı

```plaintext
orgm_site3/
│
├── build_site.py              # [MERKEZİ DERLEYİCİ] Tüm siteyi, stilleri ve şemaları üreten ana script
├── ders_kitaplari.md          # 69 kitaba ait ad, sayfa sayısı ve MB boyutu üstverilerini içeren kaynak
├── kitaplar.txt               # Ham kitap listesi dökümü
│
├── index.html                 # Ana sayfa (11 dikey kategori kartı galerisi)
├── turkce.html                # Türkçe dersi kitapları (12 adet)
├── matematik.html             # Matematik dersi kitapları (11 adet)
├── hayat-bilgisi.html         # Hayat Bilgisi ve Günlük Yaşam Becerileri kitapları (7 adet)
├── din-kulturu.html           # Din Kültürü ve Ahlak Bilgisi kitapları (7 adet)
├── iletisim-becerileri.html   # İletişim Becerileri kitapları (7 adet)
├── saglikli-yasam.html        # Sağlıklı Yaşam ve Güvenlik Becerileri kitapları (6 adet)
├── bagimsiz-yasam.html        # Bağımsız Yaşam Becerileri kitapları (5 adet)
├── beden-egitimi.html         # Beden Eğitimi, Oyun ve Spor kitapları (5 adet)
├── gorsel-sanatlar.html       # Görsel Sanatlar kitapları (4 adet)
├── sosyal-beceriler.html      # Sosyal Beceriler kitapları (3 adet)
├── muzik.html                 # Müzik kitapları (2 adet)
│
├── sitemap.xml                # Arama motorları için öncelik ve frekans tanımlı site haritası
├── robots.txt                 # Bot tarama izinleri ve sitemap referansı
│
├── assets/                    # Kurumsal varlıklar, logolar ve yerel yazı tipleri
│   ├── logo.webp & logo.png   # T.C. Millî Eğitim Bakanlığı ORGM logosu
│   ├── logo2.webp             # Türkiye Yüzyılı logosu
│   ├── logo3.webp             # Cumhuriyetin 100. Yılı logosu
│   └── fonts/                 # Yerel barındırılan 'Inter' Latin/Latin-ext woff2 dosyaları
│
├── ders_kitaplari/            # 69 adet orijinal MEB Özel Eğitim PDF ders kitabı dosyaları
├── kapak_gorselleri/          # Her kitabın optimize edilmiş 590×842 (~7:10 dikey) oranlı .webp kapak görselleri
└── kategori_videolar/         # 11 kategoriye ait .mp4 sessiz animasyonlar ve *_last_frame.webp afişleri
```

---

## 🌐 4. Alt Klasör Dağıtım Mimarisi (Relative Path Standardı)

Bu site, herhangi bir ana kurum web sitesinin içerisine **klasör olarak atılıp çalıştırılmak** üzere tasarlanmıştır (Örn: `https://orgm.meb.gov.tr/kitaplar/`):

- **Sıfır Kök Dizin (`/`) Bağımlılığı:** Hiçbir link, stil dosyası, script veya görsel `/` ile başlamaz. Tüm adresler doğrudan göreceli (`index.html`, `turkce.html`) veya `./` (`./assets/...`, `./ders_kitaplari/...`) olarak bağlanmıştır.
- **Dizin Derinliği:** Proje ana sitenin hangi alt klasöründe olursa olsun, üst siteye ait sayfalara müdahale etmez; sayfalar arası gezinme ve PDF açma işlemleri her zaman kendi klasör sınırları içerisinde kalır.
- **Geri Dön Butonu:** Kategori sayfalarındaki geri dön bağlantısı `index.html#kategoriler` hedefine gider, böylece ana sitenin değil, projenin kendi ana sayfasına döner.

### 4.1 Bağımsız Dağıtım Denetimi (Audit) — `2026-10-23`

Sitenin "klasör olarak atılıp bağımsız çalışma" amacına uygunluğu, tüm sayfalarda (`index.html` + 11 kategori sayfası) otomatik tarama ile denetlenmiştir. Sonuçlar aşağıdadır.

#### ✅ Doğrulanmış Denetimler (Sorun Bulunamadı)

| Denetim | Sonuç | Detay |
|:---|:---:|:---|
| Kök dizine (`/`) bağımlı `href` / `src` | ✅ Temiz | Hiçbir HTML sayfasında veya `build_site.py` şablonlarında `/` ile başlayan mutlak yol bulunmaz. |
| İç sayfa bağlantıları | ✅ Temiz | 12 adet dahili `.html` bağlantısının tümü mevcut dosyalara işaret eder (kırık link / 404 yok). |
| Yerel varlık referansları | ✅ Temiz | Kapak görselleri, videolar, PDF'ler ve assets referansları: **166 benzersiz yol / 0 kayıp dosya**. |
| PDF indirme | ✅ Temiz | Tüm kitap linkleri `./ders_kitaplari/*.pdf` şeklinde görecelidir; klasör hangi alan adına atılırsa atılsın çalışır. |
| CSS `url()` tanımları | ✅ Temiz | Font tanımları dahil tüm CSS kaynakları `./assets/...` şeklinde görecelidir. |
| Schema.org JSON-LD | ✅ Çalışır | Yapılandırılmış veri `./`, `index.html`, `turkce.html` gibi görecel adresler kullanır; Google görecel değerleri kabul eder (absolute tercih edilir; aşağıdaki 4.2 notuna bakınız). |

#### 📋 Dağıtım Öncesi Kontrol Listesi

Klasörü ana sitenin herhangi bir dizinine (`/kitaplar/`, `/ozel-egitim/kitaplar/` vb.) atarken:

1. `build_site.py` çalıştırılarak en güncel HTML'lerin üretildiğinden emin olun; elle düzenlenmiş eski HTML'lerle klasörün taşınmamasına dikkat edin.
2. Klasörün tamamının (`ders_kitaplari/`, `kapak_gorselleri/`, `kategori_videolar/`, `assets/` dahil) birlikte kopyalandığını doğrulayın.
3. Ek `.htaccess` / `web.config` yapılandırması gerekmez; statik dosya sunumu yeterlidir.
4. Klasör ve alt klasörler için sunucuda **okuma** erişimi tanımlı olmalıdır; yazma izni gerekmez.

### 4.2 Bilinen Kısıtlar ve Dış Bağımlılıklar

Dağıtım mimarisi tamamen bağımsız olsa da, sitenin kalıcı yayın adresi (domain) derleme zamanında bilinmediği için `sitemap.xml` ve yapılandırılmış veriler tam URL üretemez. Aşağıdaki maddeler sitenin **çalışmasını engellemez**; yalnızca arama motoru optimizasyonunu (SEO) etkiler:

1. **`sitemap.xml` görecel `<loc>` kullanır (`index.html`):**
   Sitemaps.org spesifikasyonu `<loc>` değerinin **tam nitelikli (fully-qualified)** URL olmasını ister. Görecel URL'ler arama motorları tarafından sitemap'ın konumuna göre çözümlenebilir, ancak Google'ın bunu garantilediği söylenemez. **Çözüm:** Site kalıcı adresine taşındıktan sonra `sitemap.xml` içindeki `<loc>` değerlerini tam URL'ye çevirin (örn. `https://orgm.meb.gov.tr/kitaplar/index.html`).
2. **`robots.txt` içinde `Sitemap: sitemap.xml` göreceldir:** Çoğu arama motoru robotsu bu değeri `robots.txt` konumuna göre çözümler; ancak kesinlik için tam URL önerilir. Çözüm yukarıdakiyle aynıdır.
3. **`canonical`, `og:url` ve JSON-LD `@id` değerleri göreceldir:** Kalıcı adres bilinmediği için değerler görecel üretilir. Google görecel canonical'ları işleyebilir; ancak tam URL'ler tavsiye edilir. **Çözüm:** Kalıcı adres biliniyorsa `build_site.py` şablonlarına `BASE_URL` sabiti eklenip tüm şemaların absolute üretilmesi sağlanmalıdır.
4. **Font Awesome 6 ve Google Fonts CDN yüklemesi:** İkonlar (`all.min.css`) CDN'den indirilir; internet erişimi olmayan ortamda ikonlar görüntülenmeyebilir. Inter yazı fontu hem yerel (`assets/fonts/*.woff2`) hem de uzak Google Fonts stylesheet'inden tanımlıdır; çevrimdışı senaryoda yerel `woff2` devreye girer, internet varken font iki kaynaktan da istenebilir (hafif fazla trafik; görünüm değişmez). Tam çevrimdışı çalışma istenirse Font Awesome CSS ve Google Fonts stylesheet'i de `assets/` içine yerelleştirilmelidir.

---

## 🎨 5. Tasarım Sistemi ve Kullanıcı Deneyimi (UI/UX)

### Renk Paleti (MEB Kurumsal Kimlik)
- **Primary (Ana Renk):** `#DC2626` (MEB Kırmızı)
- **Primary Gradient:** `linear-gradient(135deg, #DC2626 0%, #991B1B 100%)`
- **Primary Light / Border:** `#FEF2F2` / `#FEE2E2`
- **Metin Renkleri:** `#0F172A` (Koyu Lacivert/Siyah), `#475569` (Muted Gri)
- **Arka Plan:** `#F8FAFC` (Slate Soft Body), `#FFFFFF` (Kartlar ve Header)

### Ana Sayfa Hero Bölümü ve İnteraktif Vitrin (`index.html`)
- **Hero Rozeti:** Ana başlığın altında simgesiz, bağımsız ve merkezlenmiş `DERS KİTAPLARI` rozeti (`.hero-badge`) yer alır.
- **Ana Başlık ve Açıklama:** `<h1>Özel Eğitim Uygulama Okulu</h1>` başlığı altında Türkiye Yüzyılı Maarif Modeli vurgulu kapsamlı açıklama paragrafı (`.hero-subtitle`) bulunur.
- **11 Kitap İnteraktif Vitrini (`.hero-books-stage`):**
  - **A'dan Z'ye Kaskad Dizilim (Masaüstü):** 11 dersin ilk kitapları **Türkçe alfabetik A→Z sırasına göre** (Bağımsız Yaşam Becerileri ➔ Beden Eğitimi, Oyun ve Spor ➔ Din Kültürü ve Ahlak Bilgisi ➔ ... ➔ Türkçe) **soldan sağa** kaskad şekilde dizilir; her kitap sırasındaki bir sonraki kitabın altında kalacak şekilde birbirinin üzerine biner (`z-index` soldan sağa artar). Fareyle üzerine gelindiğinde ilgili kitap yukarı kalkarak büyür ve **tam kategori adını** gösteren rozetiyle belirir (rozet en fazla 2 satıra sarar, kitap üstünde ortalanır); tıklandığında kategoriye yönlendirir. Aktif kitaba komşu kitaplar hafifçe açılarak vurguyu destekler.
  - **Tablet ve Mobilde (Dokunmatik Kitap Karuseli — Sonsuz Döngü):** Ekran ortasında **tam boyutta öndeki 1 kitap**, hemen sağ ve solunda **yarısı görünen 2 komşu kitap**, diğerleri ekran dışında gizli şekilde başlar. Kitaplar **parmakla sağa/sola süpürülerek** geçilir (yerel `scroll-snap`; her süpürmede kitap kapağına yaslanır). Şerit; asıl 11 kitabın solunda ve sağında birebir kopyalarla **sonsuz döngüye** alınmıştır: süpürme durakladığında kullanıcı hissettirilmeden ortadaki serinin eşdeğer konumuna sıçranır. Böylece daire gibi dönen bir vitrinde sağa veya sola **sınırsız** şekilde süpürülebilir. Öndeki olmayan bir kitaba dokunulduğunda o kitap süzülerek merkeze gelir; **öndeki kitaba** dokunulduğunda ilgili kategori sayfası açılır.
  - **Kitap Sesi Efekti (Süpürme / Hover):** Kitap vitrininde süpürme sırasında her kitap geçişinde (tablet/mobil) ve masaüstünde kitaplar üzerinde gezerken her hover'da, WebAudio ile **kod tarafından üretilen** ince ve hafif bir "klik" sesi çalar (tepe ~720-840 Hz'den kısa sürede alçalan tek sinüs dalgası; ilâve ses dosyası veya lisans gerekmez). Saf sinüs dalga modeli tüm tarayıcı ve cihazlarda en tutarlı çalan yapıdadır. Ses seviyesi çok düşüktür (vurum zirvesi ~0.09) ve ardışık tetiklemeler 90ms eşiğiyle kısıtlanarak sürekli ses oluşması önlenir. Tarayıcı otomatik ses oynatma politikası gereği ses, kullanıcının ilk etkileşiminden (dokunma/klik/klavye sonrası) sonra aktif hale gelir.

### Ana Sayfa Kategori Kartları ve Video Sistemi (`index.html`)
- **Ekran Genişliğine Göre Duyarlı Kart Izgarası (`.categories-grid`):**
  - **Masaüstü (>1024px):** `repeat(auto-fill, minmax(280px, 1fr))`, 24px boşluk — daha dar masaüstü (~1025–1239px) **3 kolon ferah kart**, en geniş ekranlar (**≥1240px**) **4 kolon** tam dolu ızgara (kart ~280px).
  - **936–1024px:** `repeat(auto-fit, minmax(280px, 360px))` — **3 kolon**, ortalanmış.
  - **481–935px:** **2 kolon** (kart 260–360px, ortalanmış).
  - **≤480px (mobil):** **1 kolon**, kart 340px ile ortalanır.
- **Çift Rozet Sistemi:**
  - **Sol Üst:** Kategori adı ve ilgili ders ikonu (`.category-name-badge`).
  - **Sağ Üst:** O kategorideki toplam kitap sayısı (`.category-count-badge`, örn: "5 Kitap").
  - **Hover / Etkileşim Görünürlüğü:** Kart üzerine gelindiğinde sol rozet MEB kırmızısı zemin üzerine net beyaz metinle parlar; sağ rozet beyaz zemin üzerinde kırmızı kenarlık ve kırmızı metinle kontrast kazanır. Arka plandaki video hareketinden etkilenmeyen katı, keskin ve okunabilir bir tipografi sunar.
- **Dikey Medya Alanı (Kapak Oranı 590 / 842):**
  - Kategori kartı görsel/video alanı, kapak posterlerinin gerçek en-boy oranı olan **590 / 842** oranındadır (`aspect-ratio: 590 / 842`; posterler 590×842 px).
  - Kapak görseli ve video alan içine **tam sığacak şekilde** (`object-fit: contain`) ortalanır; oran farklı dikey videolar (örn. 9:16) da alana sığarak merkezde sunulur.
  - Kartlar **hover'da yerinde kalır** (yukarı kalkma/transform yok); geri bildirim yalnızca kenarlık ve gölge vurgusu ile verilir.
  - Tablet ve mobilde (`<=1024px`) kart kenarlıkları kapsayıcının **maksimum genişliğini** (`width: 100%`) kaplar, saf beyaz arka planda (`#FFFFFF`) görsel/video alanı merkezlenerek sunulur.
- **Lazy Loading ve Otomatik 1 Kez Video Oynatma:**
  - Tüm videolar başlangıçta `preload="none"` ve `data-src` ile yapılandırılmıştır; ağ ve bellek tasarrufu sağlanır. Kart ekrana 300px yaklaştığında video kaynağı arka planda yüklenir.
  - **Masaüstünde:** Fare ile herhangi bir kategori kartı görselinin üzerine gelindiğinde video başlar ve fare ayrılsa bile **kesintisiz 1 kez sonuna kadar** oynayıp biter, ardından kapak görseline döner. Aynı görsel üzerine tekrar gelindiğinde video yine aynı şekilde baştan sona 1 kez oynar.
  - **Tablet ve Mobilde:** Sayfa aşağı veya yukarı kaydırıldıkça kart görselleri ekranın odak alanına (kartinın %45'inden fazlası görünür ve üst/alt %5'lik kenar boşluğunun içinde) girdiğinde video otomatik olarak **1 kez baştan sona** oynatılır ve biter. **Ekranda kaç tane kart görünüyorsa (tablet'te yan yana 2 kart vb.) TÜM videoları eşzamanlı oynar**; her kart kendi videosunu birbirinden bağımsız 1 kez oynatıp kapak posterine döner. Sayfa kaydırılıp kart ekrandan çıktığında sıfırlanır; kullanıcı sayfayı tekrar kaydırıp aynı görsel üzerine geldiğinde video yeniden 1 kez baştan sona otomatik oynatılır.
- **Alt Buton:** Kartın alt kısmında belirgin **"Kitapları Görüntüle →"** aksiyon butonu yer alır.

### Kategori Sayfaları ve Kitap Kartları
- **Mobil 2'li Kitap Izgarası:** Kategori detay sayfalarında kitaplar listelenirken mobilde tek tek değil, yan yana **en az 2 kitap** (`grid-template-columns: repeat(2, 1fr)`) olacak şekilde kompakt ve verimli listelenir.
- **3D Kitap Hareketi:** Kapak görseli üzerine gelindiğinde 3 boyutlu hafif derinlik kazanarak öne eğilir (`rotateX(2deg) rotateY(-2deg) scale(1.05)`).
- **Sheen Flare (Işık Parıltısı):** Kapak yüzeyinde soldan sağa doğru akan hafif beyaz ışık yansıması animasyonu devreye girer.
- **Sol Cilt Derinliği:** Her kapağın sol kenarında gerçek kitap sırtı hissi veren gölge gradyanı mevcuttur.
- Kartın altında sayfa sayısı, MB boyutu ve büyük **"PDF İndir"** butonu yer alır.

### Hero Bölümü 3'lü İstatistik Kartları
- **Tüm ekran boyutlarında** (Masaüstü, Tablet, Mobil 360px) her zaman **yan yana 3'lü** kalacak şekilde `grid-template-columns: repeat(3, 1fr)` olarak kurgulanmıştır.
- Mobilde kaba görünmemesi için dikey mini widget yapısına geçer (üstte 28-34px mini ikon, altta sayı ve etiket).
- Asla 2 satıra bölünmez veya taşma yapmaz.

### "Geri Dön" Butonu Görünürlük Kuralı
- **Geniş Ekranlar (>992px):** Hero bölümünün **sol üst köşesinde** (`top: 0; left: 24px;`) ortadaki rozetle simetrik durur.
- **Mobil ve Tablet (<=992px):** `display: none !important;` ile tamamen gizlenir. (Mobil cihazların kendi fiziksel/dokunmatik geri dön butonları kullanıldığı için ekran temiz tutulur).

---

## 🔍 6. SEO, Semantik Yapı ve Arama Motoru İndekslenmesi

Tüm sayfalar Google, Yandex ve Bing botlarının en hızlı şekilde dizine eklemesi için optimize edilmiştir:

### Title (Başlık) Standardı
- **`index.html`:**  
  `Özel Eğitim Uygulama Okulu Ders Kitapları | MEB Özel Eğitim ve Rehberlik Hizmetleri Genel Müdürlüğü`
- **Alt Sayfalar:**  
  `{Ders Adı} Ders Kitabı | MEB Özel Eğitim ve Rehberlik Hizmetleri Genel Müdürlüğü`

### Meta Etiketleri
- Her sayfa için özgün, anahtar kelime zengin `description` (140-160 karakter).
- `author`: `T.C. Milli Eğitim Bakanlığı Özel Eğitim ve Rehberlik Hizmetleri Genel Müdürlüğü`
- `publisher`: `T.C. Milli Eğitim Bakanlığı`
- `creator`: `Özel Eğitim ve Rehberlik Hizmetleri Genel Müdürlüğü`
- `robots`: `index, follow, max-snippet:-1, max-image-preview:large, max-video-preview:-1`
- `canonical`: Klasör içi göreceli bağlantı.

### Open Graph & Twitter Cards
- `og:locale`: `tr_TR`
- `og:site_name`: `T.C. Milli Eğitim Bakanlığı Özel Eğitim ve Rehberlik Hizmetleri Genel Müdürlüğü`
- `twitter:card`: `summary_large_image`
- `twitter:site` & `twitter:creator`: `@MebOrgm`

### Schema.org JSON-LD (Yapılandırılmış Veri)
- **Ana Sayfa:** `GovernmentOrganization` (MEB & ORGM), `WebSite` ve `CollectionPage`.
- **Kategori Sayfaları:** `BreadcrumbList` (Ana Sayfa ➔ Ders Adı) ve o derse ait tüm kitapların listelendiği `ItemList` + `Book` (`name`, `numberOfPages`, `url`, `image`, `learningResourceType`).

### Başlık Hiyerarşisi (H1 - H4)
- Her sayfada **sadece 1 adet `<h1>`** yer alır.
- Bölüm başlıkları için gizli/görünür `<h2>`, kart başlıkları için `<h3>`, footer için `<h4>` sıralaması asla bozulmaz.

---

## 📱 7. Mobil Uyumluluk ve Erişilebilirlik (Responsive & WCAG)

- **Sıfır Yatay Taşma:** `scrollWidth === clientWidth` (320px - 4K ekran testleri tamamlanmıştır).
- **Ölçeklenebilir Tipografi (`rem`):** Yazı boyutları kök fonta (`16px`) bağlı `rem` birimleriyle tanımlanmıştır; sistem yazı boyutunu büyüten kullanıcılarda arayüz uyum sağlar.
- **Dokunma Hedefleri:** Tüm butonlar ve bağlantılar dokunmatik ekranlar için minimum `44px` tıklama yüksekliğine sahiptir.
- **Mobil Menü:** Dışarıya tıklandığında otomatik kapanan pürüzsüz çekmece menü.
- **Sayfa Başına Dön (Back to Top):** Dairesel SVG kaydırma ilerleme halkasıyla zenginleştirilmiş FAB butonu; mobilde telif metnini örtmemesi için footer alt boşluğu `72px` olarak korunmuştur.

---

## 📚 8. Ders Kategorileri ve Kitap Dağılımı

| Sıra | Kategori Adı | Slug / HTML Dosyası | Kitap Sayısı | Video Dosyası |
|:---:|:---|:---|:---:|:---|
| 1 | Türkçe | `turkce.html` | 12 | `Turkce.mp4` |
| 2 | Matematik | `matematik.html` | 11 | `Matematik.mp4` |
| 3 | Hayat Bilgisi ve Günlük Yaşam Becerileri | `hayat-bilgisi.html` | 7 | `Hayat_Bilgisi_ve_Gunluk_Yasam_Becerileri.mp4` |
| 4 | Din Kültürü ve Ahlak Bilgisi | `din-kulturu.html` | 7 | `Din_Kulturu_ve_Ahlak_Bilgisi.mp4` |
| 5 | İletişim Becerileri | `iletisim-becerileri.html` | 7 | `Iletisim_Becerileri.mp4` |
| 6 | Sağlıklı Yaşam ve Güvenlik Becerileri | `saglikli-yasam.html` | 6 | `Saglikli_Yasam_ve_Guvenlik_Becerileri.mp4` |
| 7 | Bağımsız Yaşam Becerileri | `bagimsiz-yasam.html` | 5 | `Bagimsiz_Yasam_Becerileri.mp4` |
| 8 | Beden Eğitimi, Oyun ve Spor | `beden-egitimi.html` | 5 | `Beden_Egitimi_Oyun_ve_Spor.mp4` |
| 9 | Görsel Sanatlar | `gorsel-sanatlar.html` | 4 | `Gorsel_Sanatlar.mp4` |
| 10 | Sosyal Beceriler | `sosyal-beceriler.html` | 3 | `Sosyal_Beceriler.mp4` |
| 11 | Müzik | `muzik.html` | 2 | `Muzik.mp4` |
| **TOPLAM** | **11 Ders Kategorisi** | | **69 Kitap** | **11 Video** |

---

## 🚀 9. Kurulum ve Yerel Çalıştırma

Projeyi yerel ortamda çalıştırmak veya derlemek için Python 3 yeterlidir:

```bash
# 1. Sayfaları derlemek / güncellemek için:
python build_site.py

# 2. Yerel geliştirme sunucusu başlatmak için:
python -m http.server 8080

# 3. Tarayıcıda açmak için:
# http://localhost:8080/index.html
```
