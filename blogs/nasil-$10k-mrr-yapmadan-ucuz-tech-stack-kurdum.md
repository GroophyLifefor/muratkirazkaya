# Nasıl $10K MRR yapmadan ucuz tech stack kurdum?

> Sırrı paramın olmamasında!

Geçenlerde Serdar Doğruyol abimin LinkedIn'de Steve Hanov'un "How I run multiple \$10K MRR companies on a \$20/month tech stack" blog yazısını paylaştığını gördüm ve ben de benzer ama farklı şeyler yaptığımı söyledim, bunun üzerine belki makale yazarım dediğimin sonuçlarıyla yüzleşiyorum.

Öncelikle tech stack'im ucuz mu sorusunun cevabı değişkendir, hatta ürüne göre de değişir. Ama bu belirsizliklerin hiçbirinin önemi yok çünkü inanılmaz ucuz yapamazsam bunu karşılayamam.

## Düz server kullan ama self hosted deployment platformu ile birlikte

Self-hosted deployment artık ciddi bir alternatif olmaktan çıktı, doğrudan tercih edilmesi gereken bir seçenek haline geldi.

2026’dayız ve hâlâ her projeyi platform bağımlı şekilde deploy etmek zorunda değiliz. Vercel gibi çözümler hızlı başlangıç için iyi olabilir ama ölçek ve maliyet tarafında uzun vadede sorgulanması gerekiyor.

Aynı kaynaklarla (2 vCPU, 4 GB RAM, 80 GB disk) farklı sağlayıcıların aylık maliyetlerine bakalım:

| Provider        | Aylık Ücret | Depolama  | Bant genişliği |
| --------------- | ----------- | --------- | -------------- |
| Hetzner         | $5.6        | 80 GB SSD | 20 TB          |
| DigitalOcean    | $24         | 80 GB SSD | 4 TB           |
| Linode          | $24         | 80 GB SSD | 4 TB           |
| AWS (Lightsail) | $24         | 80 GB SSD | 4 TB           |
| Azure           | ~$25 ila $50 | disk ayrı | trafik ayrı    |
| Google Cloud    | ~$55 ila $65 | disk ayrı | trafik ayrı    |

Özellikle Hetzner tarafında fiyat/performans oranı oldukça agresif. Aylık yaklaşık 250 TL’ye bu seviyede bir kaynak elde edilebiliyor.

Yani evet, bazı managed platformlar geliştirici deneyimi açısından hâlâ avantajlı. Ama maliyet ve kontrol tarafını öncelikliyorsanız, self-hosted çözümleri görmezden gelinemez.

Kendi altyapınızı kurmak artık eskisi kadar zor değil. Üstelik kontrol tamamen sizde çünkü artık self-hosted deployment platformları var.

### Coolify

Self-hosted tarafına geçerken en büyük soru genelde şu oluyor: “Her şeyi sıfırdan mı kuracağım?”, “O kadar teknik değilim” falan filan. Ben de sorarım, paran mı var?

Cevap veremezsen tam burada Coolify devreye giriyor.

Coolify, kendi sunucun üzerinde çalışan ve sana Vercel benzeri bir deneyim sunan açık kaynak bir deployment platformu. Yani kendi VPS’ini kullanıyorsun ama kullanım deneyimi hâlâ modern ve pratik kalıyor.

Neler sağlıyor:

* Git tabanlı deployment
* Otomatik build ve deploy süreçleri
* Docker veya Docker Compose tabanlı servis yönetimi
* Veritabanı ve servisleri tek panelden yönetme
* SSL ve domain işlemlerini kolaylaştırma

Kısacası, düz bir VPS alıp her şeyi manuel kurmak zorunda değilsin. Coolify gibi araçlarla hem maliyeti düşük tutup hem de geliştirici deneyiminden çok fazla ödün vermeden ilerleyebilirsin.

Ayrıca Hetzner direkt Coolify sunucusu kurmana yardımcı oluyor, diyeceksiniz nasıl oluyor? bildiğin bir sunucu oluştururken işletim sistemi yerine direkt Coolify'ı seçersen sana en uygun seçimleri yapıp içine Coolify kurup teslim ediyor, sana da \<ip\>:8000 adresine gidip keyfine bakman kalıyor. 

Arayüzü: \
![coolify_screenshot]

### Dokploy

Dokploy, Coolify’e benzer bir yaklaşım sunuyor ama daha sade bir tarafta konumlanıyor.

Daha az karmaşa, daha hızlı kurulum ve direkt işini görmek isteyenler için mantıklı bir alternatif. Benim hiç deneme fırsatım olmadı.

### İşleri karmaşıklaştıran her şeyden uzak durun

Bunu da bir şarkıyı değiştirerek söyleyeceğim. İngilizce ve hakaret içermektedir.

```
I had a cloud project named George
He was cool but his billing made me really bored
So I gave him to my mom in finance and she loves him very much
Because predictable budgets are better than cloud services

Cloud Bills, shut the f*ck up
Kubernetes, shut the f*ck up
Serverless, shut the f*ck up
Everyone needs to shut the f*ck up
Redis, shut the f*ck up
Microservice, shut the f*ck up
GraphQL, shut the f*ck up
Everyone needs to shut the f*ck up
Multi region, shut the f*ck up
Java, shut the f*ck up
Read replicas, shut the f*ck up
Everyone needs to shut the f*ck up

~> Sophie Powers - STFU
```

Çok güzel cover edememiş olabilirim beni yargılamayın ama konunun özü önemli, gerçekten ihtiyacımız yok yok yok yok yok.

Günlük 10bin organik istek alan bir projem var, büyük bir spor salonunun mobil uygulaması için yazılmış bir backend.

Hetzner'den en ucuz VPS, içinde bir adet Docker Compose, içinde 2 servis, birisi Deno (JavaScript ve TypeScript çalıştırıcı) uygulaması ve MongoDB, ayrıca MongoDB için ayrılmış volume.

- Tek makinede çalıştığı için gecikme az olur ve servisler birbirine hızlı bağlanır
- Docker Compose sayesinde kurulum ve taşıma kolay olur, aynı sistemi başka VPS’e birebir alabilirsin
- MongoDB volume kullanınca veri kalıcı olur, container silinse bile veri kaybolmaz
- Ayrıca serverless veya managed DB gibi ek katmanlar olmadığı için gereksiz kompleksite ve ek ücret oluşmaz

Her zaman sade olun, sadelik her şeydir.

Benim param yok diyordum, ki evet yok, spor salonunun da mı yoktu? Onların parası var, sadece enayi değiller.

## AI kullanımı

Geçenlerde bir arkadaşım Claude'un kaç X modelini kullanıyorsun diye sordu, nasıl yani X dedim. 1X Pro \$120, 5X \$600 ve 20X \$1200 olarak Pro Max Ultra Nova gibi gidiyor. 

Benim KYK kredim aylık 4000 TL, bahsettiği şeyin en ucuzu 5400 TL :'(

- Claude ve ChatGPT'de Free kullanıyorum
- Gemini için öğrencilere 1 yıllık pro vermişti
- Bir de bir süredir Cursor Pro kullanıyorum, ücreti aylık 20$ ama sebebi biraz farklı

### Cursor

Öncelikle baştan söyleyeyim, üyeliğimi iptal etmeyi düşünüyorum çünkü pahalı. Yapay zeka dediğimiz şey pahalı bi' kere. Kullanıyorum çünkü API kullanımın dışında Composer'ı ayrı tutması ve Composer 2'nin performansının yanında fiyatının uygun kalması hoşuma gitti ama hem geçen ay limitlerim çok hızlı doldu hem de bu ay direkt akmasından dolayı artık düşüncelerim değişti. Cursor artık mantıklı gelmiyor. Alternatif arayışım da yok, VSCode'a geri dönüp elimle dayanarak kod yazmayı düşünüyorum. Zaten temel architecture'ı hep ben kuruyordum ve neyi neden yaptığımı biliyorum.

### Doğru AI kullanımı

#### Yanlışlar
- Terminal (1-3070) Fix it ~> Context Filling yapmayın
- Fix it ~> Neyi düzeltecek, bunu anlamak için kendini gereksiz zorlayacak
- Çalışmıyor ~> Evet çalışmıyor, başka soru?
- Bunu optimize et ~> Neyini optimize edeyim paşam?
- Neden hata veriyor ~> Ben sana soruyorum soruya soruyla cevap verme
- Bunu düzelt ve geliştir ~> Oldu paşam yanında ayran ama dönere aynı anda bitecek

#### Token Nedir?

Token dediğimiz şey ne harf, ne hece, ne de her zaman tam kelime. Daha çok metnin anlamlı parçalanmış küçük blokları gibi düşünebilirsin.

Şöyle örnekleyebilirsin:

- “merhaba” bazen tek token olur
- “merha” + “ba” diye iki parçaya da bölünebilir
- “unbelievable” gibi kelimeler “un”, “believ”, “able” diye parçalanabilir
- noktalama işaretleri bile ayrı token olabilir

Yani sistem kelimeyi değil, dilde tekrar eden en küçük anlamlı parçaları öğrenerek bölüyor.

VE Her Bir Token PARADIR.

#### Context Filling

##### Fazla veri vermek

Sıklıkla yapılan şeylerden biri elindeki her şeyi vermek ve yapay zekanın gerekli olanı almasını istemek lakin bu çok fazla input token olarak size geri yansımaktadır. Eğer bir hata varsa, tüm Terminali vermenize gerek yok, alakalı kısmı bulmaya çalışın ve ilgili kısmı verin. 

Şimdi canlı bir test yaptım ve loglarım arasından tüm terminali verseydim 3600 Token'a tekabül ediyordu lakin sadece gerekli kısmı aradım, buldum ve 280 Token ile sorunu anladı. Bu sayede input ücretinde %92 ucuzlaştırmış oldum.

Size token az gelmiş olabilir, unutmayın damlaya damlaya göl olur. 3600 olsun dersiniz, 36000 olsun dersiniz, 360000 olsun dersiniz, 3600000 olsun dersiniz en sonunda işin içinden çıkamazsınız.

##### Sohbeti uzatmak

Sizin her mesajınız bir user mesajdır, her cevap bir assistant mesajdır ama biliyorsunuzdur ki bu yapay zekalar aynı sohbet içinde eskiyi unutmuyor. 

- Kullanıcı sorusu sorarsınız (10K Token input)
- Yapay Zeka cevabı alırsınız (10K Token output)

ama sonra tekrar soru sorduğunuzda

- Kullanıcı sorusunu sorar (20K(önceki sohbet) + 10K Token input)
- - Yanında eski kullanıcı soruları
- - Yanında eski yapay zeka cevapları
- Yapay Zeka cevabını alırsın (10K token output)

önceki sohbet input'a dahil oldu.

tekrar soru sorulursa

- Kullanıcı sorusunu sorar (40K(önceki sohbet) + 10K Token input)
- - Yanında eski kullanıcı soruları
- - Yanında eski yapay zeka cevapları
- Yapay Zeka cevabını alırsın (10K token output)

| Tur | Prompt (Input)            | Output | Toplam Input | Birikimli Context |
| --- | ------------------------- | ------ | ------------ | ----------------- |
| 1   | 10K                       | 10K    | 10K          | 20K               |
| 2   | 30K (20K eski + 10K yeni) | 10K    | 30K          | 50K               |
| 3   | 50K (40K eski + 10K yeni) | 10K    | 50K          | 70K               |
| 4   | 70K (60K eski + 10K yeni) | 10K    | 70K          | 90K               |
| 5   | 90K (80K eski + 10K yeni) | 10K    | 90K          | 110K              |

Senin sorularının uzunluğu aynı bile kalsa, geçmiş sohbetle fiyat giderek katlanıyor. Çözümler ise birkaç başlıkta ayrılıyor.

#### Nasıl Çözeriz?

##### Yeni sohbete geçmek

Sohbet uzadıkça sadece son mesajın değil, tüm geçmiş konuşma da modele gönderilir. Bu da her yeni mesajda daha fazla token kullanılmasına neden olur.

Yani eski mesajlar silinmediği sürece her konuşma, önceki tüm konuşmaları da “taşır”. Bu da hem maliyeti artırır hem de gereksiz token tüketimine yol açar.

Bu yüzden zaman zaman yeni bir sohbet açmak, token kullanımını düşük tutmak için önemlidir.


##### Mesajını düzenle

Mesaj düzenlemeyi daha sık kullanmak, geçmişe yönelik düzenleme yapmak

Şöyle düşün

Her yeni mesaj attığında sistem şunu yapıyor
“Tamam, şimdiye kadar konuşulan HER şeyi tekrar okuyayım”

Sen sadece 1 cümle yazsan bile
arkada belki 50 mesaj yeniden işleniyor

Bu da şu demek
küçük bir ekleme için bile koca bir geçmişi tekrar ödüyorsun

Ama mesajı düzenlediğinde
aynı konuşmayı uzatmak yerine direkt metni güncellemiş oluyorsun
yani yeni bir yük bindirmiyorsun

Gündelik dille

Yeni mesaj atmak
her seferinde baştan roman okutmak gibi

Mesaj düzenlemek
sadece bir paragrafı düzeltmek gibi

O yüzden

küçük ekleme mi yapacaksın
bir şeyi yanlış mı yazdın
detay mı ekleyeceksin

yeni mesaj atmak yerine düzenle
hem daha ucuz hem daha temiz hem de model daha iyi anlıyor

##### Root Cause problemi

Sürekli “fix it” demek aslında yüzeyle uğraşmak demek

Modeli bir tamirci gibi çağırıyorsun ama neyin bozuk olduğunu söylemiyorsun
O da tahmin ederek bir şeyleri düzeltmeye çalışıyor
çoğu zaman geçici çözümler üretiyor

Ama sorunun kökünü anlatırsan işler değişir

Ne yapıyordun
Ne bekliyordun
Ne oldu
Hata nerede başlıyor

bunları verdiğinde model artık tahmin etmiyor, gerçekten problemi çözüyor

Gündelik dille

fix it demek
arabayı ustaya bırakıp “çalışmıyor” demek gibi

kök nedeni anlatmak
“araç çalışıyor ama 3. viteste tekliyor” demek gibi

İkincisinde çözüm hem daha hızlı gelir hem de kalıcı olur

##### Farklı modellere şans vermek

Tek bir modele takılı kalmak her zaman en iyi sonuçları vermez. Farklı modellerin güçlü olduğu alanlar farklıdır. Bazıları kodda daha iyidir, bazıları yazı üretiminde, bazıları ise daha uygun maliyetlidir.

Mesela Anthropic tarafından geliştirilen Claude genelde güçlüdür ama maliyet tarafı daha yüksek olabilir. Aynı işi daha uygun fiyata yapan başka modeller de bulunabilir.

Bu yüzden yaklaşım şu olmalı

Her iş için en pahalı modeli kullanmak yerine
işe uygun modeli seçmek

Basit bir özet, kısa içerik ya da deneme için daha ucuz model
daha kritik ve karmaşık işler için daha güçlü model

Gündelik dille

Her yere taksiyle gitmek yerine
bazen yürümek, bazen toplu taşıma kullanmak gibi

Hem maliyeti düşürür hem de kaynakları daha verimli kullanmanı sağlar

### AI kullanacaksan, ön-ödemeli olsun

Ucu açık ödemeler elbet bir gün kontrolden çıkmaya mahkumdur. Kullandığın kadar öde modeli en sakıncalı en uzak durmanız gereken ödeme modelidir çünkü bir anlık açıkla patlayabilirsiniz. Kullanmayı bilene mantıklı, ben teknik biriyim, ben doğru yönetiyorum falan filan hikaye seni kurabiye yapıp yerler.

Gider OpenAI'dan ChatGPT, Anthropic'ten Claude, Google'dan Gemini direkt kullanırsanız bir gün faturanızdaki sıfırlarla gözünüzün büyümesi olasıdır.

#### OpenRouter

Ben OpenRouter kullanıyorum, içine örneğin 5 dolar atıyorum. İstediğim modeli kullanıyorum, bu ay kullanmazsan diğer ay paran gidecek durumu da yok. Bakiye yüklediğim kadar kullanıyorum. 

İstediğim zaman model değiştirebiliyor olmak, retry ekleyebilmek, fallback model ekleyebilmek işleri de daha kolaylaştırıyor tabii ki.

### AI IDEs

Birçok AI IDE'ye şans verdim ama genel olarak hepsi hayal kırıklığı. Düz VSCode dışı en iyisi Cursor'du ama o da gerçekten iyi değil. 

#### VSCode + Copilot 

Aslında baya yeterli ve güzeldi ta ki bazı modelleri öğrencilere kapatana kadar, GitHub'dan gelen Pro üyeliğim var ama güzel modelleri kullanmama izin vermiyor. Bu olaydan sonra OpenRouter'ı bağladım Copilot'a ama aynı performansı alamadım.

Sonra Cursor'a geçtim

#### Cursor

Cursor temel felsefesinde kötü değil, architecture'ında kötü, resmen vibe coding yapılmış bir IDE olduğunu hissettiriyor. VSCode açılma süresi 1 saniye ise, Cursor açılma süresi 25 saniye. Uygulamayı kapatırken ayrı yavaş, kod yazarken ayrı problemli, kod önerileri rezalet, tamamen içi boş show uygulaması. Peki neden bu kadar kullanılıyor? Kimsenin bunlarla derti yok ki uygulamaya giren birtakım insanlar Claude modelini bağlayıp aylık \$600 harcama derdinde.

#### Windsurf

Denedim ve hâlâ deniyorum ama yorum yapma konusunda yeterince kullanıcı deneyimim olmadı. Projelerimde şans vereceğim ama üyelik konusu tartışmalı.

#### AntiGravity

Gereksiz.

## Database

“Use SQLite for everything” \
“Use PostgreSQL for everything” \
:D komik değil mi?

Komik olan şey şu, niye bu kadar takıyorsunuz, yeni bir projede bu kadar önemli değil. Projen milyon dolarlara yüksek ihtimalle ulaşmayacak. Çook çok fazla proje geliştirdim ve en çok istek alan proje aylık 280 bin istek alıyor. 

Ben bir projeye başlarken genelde MongoDB tercih ediyorum, çünkü migrationlar işimi yavaşlatıyorum. Kendim için kurduğum bir pattern var, MongoDB'de işlerimi yaparken unexpected hataları önlüyor ve gayet memnunum. Peki her şey için MongoDB mi kullanayım? :D

Yelix Cloud -> Fazla analitik vardı ClickHouseDB kullandık \
Ordu.dev -> PostgreSQL \
Spor salonu -> MongoDB

Her proje için aynı database'i kullanmayın. Hangisi hem projeye daha uygun hem size daha rahatsa onu kullanın ve kafanızda büyütmeyin. Çok para kazandırırsa refactor etmeyi düşünürsünüz.

Ayrıca Cloud'da DB mümkünse kullanmayın, aynı VPS'in içinde bir tane kurabilirsiniz ve darboğaz olana kadar gidersiniz, yedeklerinizi almayı unutmayın :)

## Sonuç olarak

Eğer yukarıda dediklerimi yapmasaydım aylık ödemem sanırım 400 ile 2000 dolar arası olurdu ki benim için inanılmaz fazla ama dediklerimi yaptığım için Cursor üyeliği dahil 30 dolar gibi bir giderim oluyor.

Ben Murat Kirazkaya, umarım makalemden keyif almışsınızdır. 19 Yaşındayım ve 8 yıldır yazılımla uğraşıyorum, bir şirkette çalışmıyorum genelde freelance işler yapıyorum ve OpenSource ekosistemine katkıda bulunuyorum. [Gitista](https://gitista.com/turkey/) istatistiklerine göre OpenSource'a katkı sıralamasında TR'de 68. Global'de ise ilk 12 bin'e girmişim. Keyifli günler dilerim.