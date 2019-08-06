+++
title = "Local IIS Site Kurulumu"
thumbnailImage = ""
coverImage = "https://c8.staticflickr.com/8/7421/9339731831_9ba94f287c_k.jpg"
metaAlignment = "center"
thumbnailImagePosition = "top"
date = "2017-01-10T23:20:12+03:00"
categories = [
  "genel",
  "yazilim",
  "windows",
  "iis"
]
tags = [
  "software",
  "iis",
  "site",
  "localwebserver",
  "hosts"
]
desciption = ""
keywords = [
  "yazilim",
  "sofware",
  "iis",
  "site",
  "localwebserver",
  "hosts"
]
autoThumbnailImage = false

+++

Uygulamanızı geliştirme sırasında Local IIS e sitenizi tanımlamanız gerekebilir. Sanki uzak sunucuda sitenizi custom domain ile host eder gibi (yani site kodlarınızı uzaktaki hostunuzda çalıştırır gibi) çalıştırabilirsiniz. İstediğiniz domain adresini belirtebilir, işlemlerinizi bu domaini kullanarak gerçekleştirebilirsiniz. Eğer bu yazıyı okuyor iseniz muhtemelen bunu yapmaya gerek duymuşsunuzdur.

İlk olarak windows yüklü bilgisayarımızın başlat menüsüne tıklayarak  **“Windows özellikleriniz Aç veya Kapat”** yazıp arama yapıyoruz.

<a href="http://imgur.com/d5obWDA"><img src="http://i.imgur.com/d5obWDA.png" title="source: imgur.com" /></a>

Yukarıda ki resimde görmüş olduğunuz gibi gerekli kutucukları işaretleyip kurulumu sağıyoruz.

### Visual Studio .Net Uygulamamızı IIS e Tanıtma

Visual Studio da uygulamızın ayarlar kısmına girerek yukarıdaki gibi yapılandırıyoruz.

**Servers'ın** alt kısmında görmüş olduğunuz **Local IIS'i** seçmeyi unutmayalım. Bu seçenek Local'imize **IIS** kurduktan sonra geldi.

Şimdi sıra **IIS** yapılandırmasında.

Bunun için yine başlat tuşuna basarak iis yazalım ve **Internet Information Services (IIS) Yöneticisi**'ne girelim.

<a href="http://imgur.com/e2lPp4E"><img src="http://i.imgur.com/e2lPp4E.png" title="source: imgur.com" /></a>

Siteler e sağ tıklayarak Web Sitesi Ekle diyelim.

<a href="http://imgur.com/UuDqEXb"><img src="http://i.imgur.com/UuDqEXb.png" title="source: imgur.com" /></a>

Aşağıdaki gibi gerekli yerleri dolduralım.

**Site adı :** Herhangi bir isim girebiliriz.

**Fiziksel Yol :**  uygulamamızın kodlarının bulunduğu klasörü göstermeliyiz.

**Not** : Web.Config dosyasının bulunduğu klasörün adresi gösterilmelidir.

**Ana bilgisayar adı :** Visual Studio da girdiğimiz adresin aynısını girmeliyiz.

<a href="http://imgur.com/SQwIO0c"><img src="http://i.imgur.com/SQwIO0c.png" title="source: imgur.com" /></a>

### Host dosyası yapılandırması

**IIS'e** kaydetmiş olduğumuz domain adresini Windows a söylemeliyiz.

Bunun için **Windows/System32/drivers/etc** klasörünün içinde bulunan Host dosyasını  açarak adresimizi eklemeliyiz.

**Not:** Host dosyasını masaüstünüze kopyalayın. Gerekli düzenlemeleri yaptıktan sonra dosyayı kopyalayıp tekrar **etc** klasörünün içine yapıştırın.

<a href="http://imgur.com/ABzsBY9"><img src="http://i.imgur.com/ABzsBY9.png" title="source: imgur.com" /></a>

Aşağıdaki resimdeki gibi adresimizi ekleyip kaydediyoruz.

<a href="http://imgur.com/BGUmzda"><img src="http://i.imgur.com/BGUmzda.png" title="source: imgur.com" /></a>

Artık Visual Studio muzda uygulamızı derleyebilir ve belirlediğimiz domainden erişim sağlayabiliriz.
