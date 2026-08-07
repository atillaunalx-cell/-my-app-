# Emergent Prompt — Samsung Ürün Fiyat Karşılaştırma Mobil Uygulaması

Aşağıdaki metni Emergent'a tek parça olarak yapıştırabilirsin.

---

## PROMPT (kopyala-yapıştır)

Samsung ev elektroniği ürünleri için **native mobil uygulama** (iOS + Android) geliştirmemi istiyorum. Uygulamanın amacı: belirli Samsung model kodlarının akakce.com üzerindeki fiyat/satıcı bilgilerini kullanıcıya tek ekranda göstermek.

### 1. Genel Mantık

- Uygulama içinde kategori bazlı, önceden tanımlı bir **model kodu listesi** bulunacak (aşağıda veriyorum).
- Kullanıcı bir ürüne (model koduna) tıkladığında, uygulama **o anda** akakce.com'da bu model kodunu arar, şu bilgileri çeker ve gösterir:
  - Ürün görseli (akakce'deki ürün fotoğrafı)
  - En düşük fiyattan başlayarak **ilk 5 satıcı**: satıcı adı, fiyatı, satıcı linki, (varsa) stok durumu ve satıcı puanı
  - Son güncelleme tarihi/saati
- **Önemli:** Tüm ürün listesi toplu/otomatik olarak arka planda çekilmeyecek. Veri çekme işlemi **sadece kullanıcı o ürünün detay sayfasına girdiğinde** tetiklenecek (lazy fetch). Bu, akakce'nin bot/scraping engellerine yakalanma riskini azaltmak ve gereksiz trafik oluşturmamak için tercih edildi.
- Çekilen veri, tekrar tekrar aynı isteği atmamak için **cache'lenecek** (örn. 6 saat geçerli). Kullanıcı detay sayfasında "Yenile" butonuyla cache süresi dolmamış olsa da manuel yenileme yapabilecek.
- Akakce sayfa yapısı değişebileceği / bot koruması olabileceği için veri çekme katmanı **hata toleranslı** olmalı: çekim başarısız olursa kullanıcıya "şu an fiyat bilgisi alınamadı, akakce'de aç" gibi bir mesaj ve doğrudan akakce arama linkine yönlendirme butonu gösterilmeli.

### 2. Admin Paneli

- Uygulama içinde (veya ayrı bir basit web/admin ekranında) şifre korumalı bir **admin paneli** olacak.
- Admin panelinden:
  - Yeni ürün kodu eklenebilecek (kategori seçilerek)
  - Var olan ürün kodu silinebilecek
  - Ürün kodu düzenlenebilecek (kategori değiştirme, kod düzeltme)
  - Kategoriler de eklenip çıkarılabilecek
- Admin panelindeki değişiklikler anlık olarak mobil uygulamadaki listeye yansımalı (backend veritabanından okunmalı, kod içine gömülü statik liste olmamalı).

### 3. Ekranlar

1. **Ana Ekran / Kategori Listesi:** Kategoriler (Buzdolabı, Çamaşır Makinesi, vb.) kart/liste görünümünde.
2. **Kategori Detay:** Seçilen kategoriye ait model kodları liste halinde (arama/filtreleme kutusu ile).
3. **Ürün Detay:** Ürün görseli, model kodu, ilk 5 satıcı bilgisi (fiyat sıralı), "Akakce'de Aç" butonu, "Yenile" butonu, son güncelleme zamanı.
4. **Arama:** Tüm kategorilerdeki model kodlarında arama yapılabilen genel bir arama ekranı/kutusu.
5. **Admin Paneli:** Giriş (şifre) + ürün/kategori CRUD ekranları.

### 4. Tasarım Yönergeleri

- Arayüz **çok sade ve pratik** olmalı; gereksiz animasyon/karmaşık gezinme olmadan, 2-3 dokunuşla istenen ürüne ulaşılabilmeli.
- Renk paleti **Samsung marka renkleriyle uyumlu ama pastel tonlarda** olacak: Samsung mavisinin (#1428A0 civarı) **pastelleştirilmiş, yumuşatılmış** versiyonları kullanılacak (örn. açık gök mavisi, buz mavisi, çok açık lavanta-mavi tonları), koyu kontrast için lacivert sadece vurgu/metin renginde kullanılabilir. Arka planlar beyaz/açık gri, kartlar pastel mavi tonlarında olsun.
- Tipografi sade, okunaklı, büyük dokunma alanları (mobilde tek elle kullanım rahat olsun).
- Fiyat bilgisi görsel olarak öne çıksın (en düşük fiyat vurgulu gösterilsin, örn. "En uygun fiyat" etiketi).

### 5. Teknik Notlar

- Native mobil (iOS + Android) — React Native veya benzeri cross-platform bir teknoloji tercih edilebilir.
- Backend: ürün/kategori verisini ve cache'lenmiş fiyat sonuçlarını tutacak basit bir veritabanı (örn. PostgreSQL/MongoDB).
- Akakce'den veri çekme işlemi backend üzerinden yapılmalı (mobil taraftan direkt scraping yapılmamalı); bu, IP engellenmesi ve hız sınırlaması gibi riskleri backend tarafında yönetmeyi kolaylaştırır.
- Çekim başarısız olduğunda backend loglama yapmalı ki hangi model kodlarında sorun olduğu görülebilsin.

### 6. Ürün Listesi (Kategori → Model Kodları)

**BUZDOLABI**
RZ20DG3001WWTR, RI40H20FEFWTR, RT47CG6636WWTR, RT47CG6636S9TR, RB45DG600EWWTR, RB52DS33EWW/TR, RB45DG600ES9TR, RB52DS33ESA/TR, RB50DG601EWWTR, RB50DG601ES9TR, RT53DG7A14WWTR, RB50DG601EB1TR, RB58DS75EWW/TR, RT53DG7A14S9TR, RB58DS75ESA/TR, RT58K704RWW/TR, RT58K704RSL/TR, RT62K704RWW/TR, RT62K704RSL/TR, RS70F65QEFTR, RS70F65QETTR, RF65DG90BESLTR, RF71DG90BESLTR, RF71DB965E8CTR

**ÇAMAŞIR MAKİNESİ**
WW90FG3M05TWAH, WW90CGC04DAEAH, WW90CGC04DABAH, WW11DG5B25AEAH, WW11DG5B25ABAH, WW11DB7B34GEAH, WW11DB7B34GBAH, WW11DB8B95GHAH, WW11DB8B95GBAH, WD90TA046BE1AH, WD90HG5U34BEAH, WD11DG5B15BEAH, WD11DG5B15BBAH, WD11DB8B85GHAH, WD18DB8995BZAH

**KURUTMA MAKİNESİ**
DV90DG52A0AEAH, DV90DG52A0ABAH, DV10DG54A0AEAH, DV10DG54A0ABAH, DV10DB7440GEAH, DV10DB8440GHAH, DV10DB8440GBAH

**AIRDRESSER**
DF24CB9900CRAH

**BULAŞIK MAKİNESİ**
DW60DG540FWQTR, DW60DG540FSRTR, DW60DG550B00TR, DW60DG550FWQTR, DW60DG550FSRTR, DW60DG560FWQTR, DW60DG560FSRTR, DW60A8050FS/TR, DW70H73YCFRTR, DW70H73YCFVTR, DW60DB890FAPTR, DW80H73YBFFTR, DW80H73YBFZTR

**ELEKTRİKLİ SÜPÜRGE**
VC07R302MVR/TR, VC07T357MHB/TR, VR7MD97714G/TR, VS15A6031R4/TR, VS20B75ACR5/TR, VS20C8524TB/TR, VS70H25WLG/TR, VC07T357MHR/TR, VS70H28GLT/TR, VS80F28DFP/TR, VS90F40DAK/TR

**MİKRODALGA FIRIN**
MS20A3010AH/TR, MS23DG4504GTTR, MS23DG4504ATTR, MS23DG4504AGTR, MG23DG4524ATTR, MS23K3555ES/ND

**ANKASTRE FIRIN**
NV60K3110BS/TR, NV60K5140BB/TR, NV60K5140BW/TR, NV60K7140BB/TR, NV60M7140BW/TR, NV7B4420ZAK/TR, NV7B4520ZAS/TR, NV7B56458AE/TR, NV7B6665IAN/TR

**ANKASTRE OCAK**
NA64B3100AS/TR, NA64H3110AS/TR, NA64H3030AS/TR, NA64N7100AB/TR, NA64H3010AK/TR, NA64M7100AW/TR, CTR464EB01/XTR, CTR164NC01/XTR

**ANKASTRE DAVLUMBAZ**
NK24M3050PS/U1, NK24N7060VB/TR, NK24C5070DS/UR, NK24C5070US/UR, NK24M7060VW/TR

### 7. Ek Uyarı

Akakce.com'un kullanım şartlarına ve olası bot/scraping korumalarına dikkat edilmeli; çekim sıklığı ve isteği kullanıcı bazlı (talep üzerine) tutularak sunucuya aşırı yük bindirilmemeli.

---

## Notlar (senin için, prompta dahil değil)

- Cache süresini 6 saat önerdim; istersen bunu 1 saat / 24 saat gibi değiştirebilirsin, promptta tek satırlık bir değişiklik.
- Admin panel şifre koruması "basit" tuttum; eğer birden fazla admin kullanıcısı / rol bazlı yetki istiyorsan bunu ekleyebiliriz.
- Akakce'nin canlı HTML yapısını scrape etmek zaman zaman kırılabilir (site tasarımı değişirse); Emergent bu konuda hataya toleranslı bir yapı kuracak ama %100 kesintisiz çalışma garantisi olmayabilir, bunu bilerek ilerle.
