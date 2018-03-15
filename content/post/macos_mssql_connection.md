+++
coverImage = "https://c8.staticflickr.com/8/7421/9339731831_9ba94f287c_k.jpg"
desciption = ""
metaAlignment = "center"
autoThumbnailImage = false
date = "2017-01-11T15:50:10+03:00"
categories = [
  "yazilim",
  "sql",
]
thumbnailImage = ""
tags = [
  "software","mssql","dbeaver","macos mssql connection"
]
keywords = [
  "yazilim",
  "sofware","mssql","dbeaver","macos mssql connection"
]
thumbnailImagePosition = "top"
title = "Mac OS ile MSSQL Bağlantısı Kurmak"
#url = "macos-mssql-connection"

+++


**MSSQL** oldukça gelişmiş ve birçok firma – kişi tarafından kullanılan bir veritabanın sistemidir. Oldukça büyük miktarda verinizi, doğru şekilde tasarlayarak tatmin edici derecede hızlı bir şekilde kontrol edebilirsiniz.

Senaryomuz şu şekilde;

Mac OS yüklü bilgisayarımızdan MSSQL yüklü sunucumuza bağlantı kurmak istiyoruz.

**Not :** Aşağıda bahsedecek olduğum uygulamayı  Windows yüklü bilgisayarınızda da SQL Management Studio alternatifi olarak kullanabilirsiniz.

 

## Dbeaver

http://dbeaver.jkiss.org/download/


Tüm işletim sistemleri için sürüm mevcuttur. Kendi işletim sisteminize göre olanı indirip kullanmaya başlayabilirsiniz.

**Database Connection**

<a href="http://imgur.com/RQqA0wI"><img src="http://i.imgur.com/RQqA0wI.png" title="source: imgur.com" /></a>

**Connections** a sağ tıklayarak Create New Connection diyoruz.

<a href="http://imgur.com/wb55lJW"><img src="http://i.imgur.com/wb55lJW.png" title="source: imgur.com" /></a>

Burada SSH yapılandırması yapabiliriz.

<a href="http://imgur.com/R0Vtyt9"><img src="http://i.imgur.com/R0Vtyt9.png" title="source: imgur.com" /></a>

**Connection Type** için seçenekler mevcut.Edit diyerek arasındaki farkları görebiliriz. Biz Test seçeneğini seçiyoruz.

<a href="http://imgur.com/6ft683a"><img src="http://i.imgur.com/6ft683a.png" title="source: imgur.com" /></a>

Aşağıdaki gibi gerekli yerleri dolduruyoruz.

<a href="http://imgur.com/BoSw3vL"><img src="http://i.imgur.com/BoSw3vL.png" title="source: imgur.com" /></a>

Hangi veritabanına bağlanmak istiyorsak onu seçiyoruz. Eğer ilk defa seçmiş iseniz bazı dosyalar indirmek isteyecektir.

Biz **MS SQL Server** ve **Microsoft Driver** seçeneğini seçiyoruz.

**Test Connection** butonuna basarak bağlantı durumunu kontrol edebiliriz.

<a href="http://imgur.com/zM89SAI"><img src="http://i.imgur.com/zM89SAI.png" title="source: imgur.com" /></a>

 

 

 



