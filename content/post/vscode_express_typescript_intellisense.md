+++
autoThumbnailImage = false
date = "2017-01-11T18:14:05+03:00"
keywords = [
  "yazilim",
  "sofware","vscode","typescript","express","nodejs","intellisense"
]
title = "VS Code & Express & TypeScript & IntelliSense"
#url = "vscode-intellisense"
desciption = ""
metaAlignment = "center"
categories = [
  "yazilim",
  "vscode"
]
tags = [
  "software","vscode","typescript","express","nodejs","intellisense"
]
coverImage = "https://c8.staticflickr.com/8/7421/9339731831_9ba94f287c_k.jpg"
thumbnailImage = ""
thumbnailImagePosition = "top"

+++
# VS Code & Express & TypeScript & IntelliSense

[VS Code](https://code.visualstudio.com/),  [Atom](https://atom.io/) ile aynı çekirdeği paylaşan, [Electron](http://electron.atom.io/) kullanılarak microsoft tarafından özelleştirilmiş ve birçok dil ile uygulama geliştirmeniz için çeşitli eklentileri bulunan bir editördür.

Günümüzün lider dili malum Javascript ve belki de bunun böyle olmasının en temel sebeplerinden biri [NodeJS](https://nodejs.org/en/).

[NodeJS](https://nodejs.org/en/) ile çok hızlı bir şekilde ve tamamen javascript  kullanarak web siteleri, uygulamaları, api service leri vb. geliştirebiliriz.

Bu yazımızda **VS Code** un nimetlerinden yararlanarak hızlı ve kolay bir şekilde nasıl NodeJS uygulaması yazabiliriz, bunu görücez.

Ve bu uygulamamızı geliştirirlem NodeJS in en temel kütüphanelerinden biri olan Express i kullanıcaz.

 
**CLI** kullanarak express uygulaması oluşturabilmemiz için express-generator module ünün global olarak kaydediyoruz.

Ayrıntılı bilgi [burada](http://expressjs.com/en/starter/generator.html).

`npm install express-generator -g`

Daha sonra projemizi oluşturacağımız path e giderek;

`express SampleNodeApp`

yazarak uygulamamızın temel yapısını kolayca oluşturuyoruz. Daha sonra istediğimiz şekilde özelleştirme yapabiliriz tabiki.

Daha sonra oluşturduğumuz projeyi VS Code ile açıyoruz.

- VS Code un nimetlerinden faydalanabilmek için **jsconfig.js** dosyasını eklememiz gerekmekte. VS Code penceresinin sağ alt tarafında bulunan ampül e tıklarsanız kısa yoldan bu dosyayı oluşturabilirsiniz.
- Daha sonra VS Code un **Debug** sekmesine girerek sol üstte bulunan ayarlar simgesine tıklıyoruz ve Node seçeneğini seçerek launch.js dosyasını oluşturulmasını sağlıyoruz.
- Terminali açarak ilk olarak aşağıdaki satırı çalıştırarak typings modülünü yüklüyoruz.

`npm install -g typings`

- Daha sonra birçok dil için bulunan ve kod yazarken prompt çıkartarak bize öneride bulunan DefinitelyTyped ları ekliyoruz.Biz NodeJs uygulaması geliştirdiğimiz için node için olanı ve express module ünü kullanarak geliştirme yaptığımız içinde express için olanı yüklüyoruz. DefinetlyTyped ı buradan görebilirsiniz.

`typings install dt~node --global`
`typings install dt~express dt~serve-static dt~express-serve-static-core --global`
 

Ve artık hızlı bir şekilde NodeJS uygulaması geliştirmeye hazırız.

- Uygulamızın ilk hali aşağıdaki gibidir. Yani express SampleNodeApp yazdığımızda aşağıdaki dosyaları otomatik olarak oluşur.

<a href="http://imgur.com/AvhZ2zo"><img src="http://i.imgur.com/AvhZ2zo.png" title="source: imgur.com" /></a>

- **__dirname** in üstüne geldiğimizde any yazısını görürüz. Yani herhangi bir öneri gözükmemektedir.

<a href="http://imgur.com/aIa7lo5"><img src="http://i.imgur.com/aIa7lo5.png" title="source: imgur.com" /></a>

- Sol tarafta en altta bulunan simgeye tıklayarak açtığımız Debug sayfasında **Environment (Node.js)** i seçiyoruz ve default yapılandırmanın oluşmasını sağlıyoruz.

<a href="http://imgur.com/XDb5nZS"><img src="http://i.imgur.com/XDb5nZS.png" title="source: imgur.com" /></a>

- Sağ altta bulunan ampül simgesine tıklayarak (yukarıda ki resimlerden görebilirsiniz) jsconfig.js dosyasını oluşturuyoruz.

<a href="http://imgur.com/SObhMhv"><img src="http://i.imgur.com/SObhMhv.png" title="source: imgur.com" /></a>

<a href="http://imgur.com/eYvQGgC"><img src="http://i.imgur.com/eYvQGgC.png" title="source: imgur.com" /></a>

VS Code tarafında yapmamız gerekenler bunlar. Şimdi IntelliSense için gerekli modülleri yüklememiz gerekiyor.



<a href="http://imgur.com/YHze3y4"><img src="http://i.imgur.com/YHze3y4.png" title="source: imgur.com" /></a>

<a href="http://imgur.com/a1gOXCW"><img src="http://i.imgur.com/a1gOXCW.png" title="source: imgur.com" /></a>

<a href="http://imgur.com/ACedntg"><img src="http://i.imgur.com/ACedntg.png" title="source: imgur.com" /></a>

<a href="http://imgur.com/kC5no16"><img src="http://i.imgur.com/kC5no16.png" title="source: imgur.com" /></a>

<a href="http://imgur.com/KEw70NZ"><img src="http://i.imgur.com/KEw70NZ.png" title="source: imgur.com" /></a>

Gerekli modülleri yükledik. Biz NodeJS uygulaması geliştirdiğimiz için node ve express için olan modülleri kurdur. Diğer dillerden geliştirme yaparken o dile ait eklentileri kurmalısınız.

 

Ve sonuç olarak artık yazılan kodun üstüne gelerek açıklamayı ve nasıl kullanılması gerektiğine dair ipuçlarını görebiliriz.

<a href="http://imgur.com/00w0gP9"><img src="http://i.imgur.com/00w0gP9.png" title="source: imgur.com" /></a>

 
