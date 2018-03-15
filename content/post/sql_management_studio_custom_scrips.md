+++
tags = [
  "software","sql","mssql","custom script"
]
thumbnailImagePosition = "top"
metaAlignment = "center"
title = "SQL Management Studio Custom Generate Scripts"
date = "2017-01-11T15:39:47+03:00"
keywords = [
  "yazilim",
  "sofware",
  "sql",
  "mssqi",
  "sql management studio"
]
autoThumbnailImage = false
coverImage = "https://c8.staticflickr.com/8/7421/9339731831_9ba94f287c_k.jpg"
desciption = ""
categories = [
  "yazilim",
  "sql",
]
thumbnailImage = ""

+++


SQL Management Studio kullanarak MSSQL e erişim yapmanız size birçok fayda sağlayacaktır. Tabi dezavantajları da yok değil !.

Senaryomuz şu şekilde;

Test veritabanında çalışarak kendinizi production veritabanında izole ettiniz ve güvenlik konusunda bir adım öne geçtiniz.

Test veritabanı ile işlemleriniz bittikten sonra artık sıra geldi oluşturmuş olduğumuz tüm yapıları taşımaya !

Bu kısım biraz sıkıcı olsa da SQL Management Studio nun buna getirmiş olduğu kolaylıklar var.

 
Oluşturmuş olduğumuz tabloların sadece şemasını yani içiindeki veriler olmaksızın taşımak istersek, ( ki bu durum default olarak yapılandıırlmış durumdur ) işimiz oldukça kolay.

Test veritabanında  ki tabloya sağ tıklayarak aşağıdaki gibi seçim yapmak.

- **File** seçeneği oluşturulan script in bir dosyaya kaydedilmesini
- **Clipboard** seçeneği hafızaya alınmasını ( NotDefteri > CTRL + V diyerek yapıştırabiliriz )
sağlar.


<a href="http://imgur.com/phM7Y8A"><img src="http://i.imgur.com/phM7Y8A.png" title="source: imgur.com" /></a>

 
Tüm oluşturmuş olduğumuz yapıyı bu yöntem ile script haline getirebiliriz.

Lakin şöyle bir sıkıntımız olduğunu varsayalım.

Tablomuzu içindeki verilerle beraber taşımak istiyoruz. Yani script in verileride kapsamasını istiyoruz.

 

Bunun için ilk olarak tablomuzun bulunduğu veritabanına sağ tıklayarak aşağıdaki adımları gerçekleştiriyoruz.

<a href="http://imgur.com/TUEWNjo"><img src="http://i.imgur.com/TUEWNjo.png" title="source: imgur.com" /></a>


<a href="http://imgur.com/3pGyapU"><img src="http://i.imgur.com/3pGyapU.png" title="source: imgur.com" /></a>

<a href="http://imgur.com/6xCVWnQ"><img src="http://i.imgur.com/6xCVWnQ.png" title="source: imgur.com" /></a>
 

Aşağıdaki resimde görmüş olduğunuz Data only seçeneği sadece tabloda kayıtlı olan verilerin scriptini çıkarır.

Schema and data seçeneği hem schema nın hem de verilerin scriptini çıkararı

Schema only sadece tablonun schema sının scriptini çıkarır.

 <a href="http://imgur.com/dB2gpdH"><img src="http://i.imgur.com/dB2gpdH.png" title="source: imgur.com" /></a>

Son ekranda scriptimizin başarılı şekilde oluştuğunu görüyoruz.

<a href="http://imgur.com/7Nx3w3O"><img src="http://i.imgur.com/7Nx3w3O.png" title="source: imgur.com" /></a

 

Oluşturmuş olduğumuz script i hangi veritabanında kullanmak istiyorsak oraya giderek query penceresine yapıştırmamız ve çalıştırmamız yeterli olacaktır.

