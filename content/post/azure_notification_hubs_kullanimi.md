+++
desciption = ""
thumbnailImage = ""
autoThumbnailImage = false
thumbnailImagePosition = "top"
metaAlignment = "center"
coverImage = "https://c8.staticflickr.com/8/7421/9339731831_9ba94f287c_k.jpg"
keywords = [
  "yazilim",
  "sofware",
  "azure",
  "notification",
  "notification hubs"
]
date = "2017-01-11T00:21:36+03:00"
categories = [
  "yazilim",
  "azure"
]
tags = [
  "software",
  "azure",
  "notification hubs"
]
title = "Azure Mobile Services ve Notification Hubs Kullanımı"
#url = "azure-notification-hubs-kullanimi"

+++

Azure Mobile Services çok hızlı ve kolay bir şekilde uygulama geliştirmeniz için önceden veya uygulama yazımı sırasında yapılması gereken işlemleri minimize eden bir servistir.

Kullanımı oldukça kolay olmakla beraber ilk bir kaç adımı gerçekleştirmek bazen can sıkıcı olabilmekte.

Bunun için ufak bir örnekle bu ilk birkaç adımı beraber atıcaz.


https://azure.microsoft.com/tr-tr/ adresinden portal a girdiniz.Ve mobile services tab ından new diyerek yeni bir mobile services oluşturdunuz.

 

<a href="http://imgur.com/AzIDXWf"><img src="http://i.imgur.com/AzIDXWf.jpg" title="source: imgur.com" /></a>

 

Oluşturduğunuz Mobile Service tıkladınız ve çıkan ekranda Push tabına girdiniz.Bu taba tıkladıktan sonra notification gönderebilieceğiniz platformları göreceksiniz.

Biz bu örneğimizde Android Platformu için push gönderimini anlatıcaz ve bunun için GCM API kısmına https://console.developers.google.com/ linkinden oluşturduğumuz yeni proje nin API KEY ini yazıyoruz.Bu API KEY i bulmak için yeni proje oluşturduğunuzda Credentials sekmesinden > Add Credentials > API Key ve Server Key tıklıyoruz ve buradaki API KEY i kullanıyoruz.

**NOT :** **APIS** sekmesinden **Cloud Messaging for Android** API sini aktif etmeyi unutmayın.

Azure tarafında yapmanız gerekenler bunlar.

Client tarafında ki kodlar için ise her platforma özel kodlar bulunmaktadır.

Android Client ı için örnek kodlara https://github.com/aykuttasil/AzureMobileService linkinden erişebilirsiniz.

 
---

 

Azure tarafında bir çok farklı işlem gerçekleştirebilirsiniz.

Örneğin herhangi bir client da (android , ios , http vs.) mobile service altyapısını kullanarak veritabanına veri eklediniz.Ve her veri girişi yapıldığda uygulamanın yüklü olduğu tüm cihazlara bildirim göndermek istiyorsunuz.

Bu ve buna benzer işlemlerinizi Azure Server tarafında halledebilirsiniz.

Aşağıdaki resimde her Item tablosuna veri eklendiğin de bildirim gönderen kod bloğunu görebilirsiniz.

<a href="http://imgur.com/VSKzp0b"><img src="http://i.imgur.com/VSKzp0b.jpg" title="source: imgur.com" /></a>




