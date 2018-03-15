+++
thumbnailImage = ""
date = "2017-01-11T17:14:22+03:00"
title = "Javascript Gariplikleri"
#url = "javascript-gariplikleri"
keywords = [
  "yazilim",
  "sofware","javascript"
]
desciption = ""
categories = [
  "yazilim",
  "javascript",
]
autoThumbnailImage = false
thumbnailImagePosition = "top"
metaAlignment = "center"
tags = [
  "software","javascript"
]
coverImage = "https://c8.staticflickr.com/8/7421/9339731831_9ba94f287c_k.jpg"

+++

Saatlerdir uğraştığım ve javascriptle temel düzeyden biraz daha ileri düzeyde uğraşan kişilerin başına gelebilecek bir saçmalığı belirtmek istiyorum.

Her dilde var olan Replace fonksiyonu Javascript de saçma sapan bir şekilde yapılandırılmış. Bu kadar basit bir şeyi bu kadar komplike bir vaziyete sokmak gerçekten harika.

Örneğin elinizde

`var degisken = ‘İSTANBUL’ ;`

şeklinde bir değişkeniniz var. Ve siz ihtiyacınız doğrultusunda bu kelimeyi veritabanından çektiğiniz ‘istanbul’ kelimesi ile karşılaştırmak istiyorsunuz.

```javascript
 $('#Iller > option').each(function(index) {
                var il = 'istanbul'; // veritabanından çektiğimizi düşünelim
               
                var txt = $(this).text() // -> 'İSTANBUL'
                if (txt === il) {
                   // Diğer işlemler
                }           
            });
 ```

Yukarıda

`if (txt === il)`
ile eşitlik kontrol ediliyor.

Ve **true** değeri dönmüyor. Çünkü biri küçük diğeri büyük harfle yazılmış. Gayet normal.

Peki tamam.

Büyük yazılan **‘İSTANBUL’** kelimesini javascript in çok değerli toLowerCase() fonksiyonu ile küçültüyoruz.

`var txt = txt.toLowerCase(); // -> 'istanbul'`

Evet ikisi de ‘istanbul’ oldu artık. Peki şimdi yukarıda belirttiğimiz eşitlik sağlanıcak mı ?


**HAYIR !**

Çünkü **‘İSTANBUL’** değişkenini Javascript abimiz küçültürken, büyük ‘İ’ harfiyle sevişiyor. Ve eşitlik bir türlü sağlanamıyor.

Birde çok değerli Javascript abimizin ( dünya ahiret bacım bu saatten sonra) replace fonksiyonu efsane çalışıyor.

Replace olmasını istediğimiz değerin sadece ilkini değiştiriyor. Diğerleri pek umrunda değil. Bu nedenle /İ/g fln saçma salak bişiler yapmak gerekiyor.

Aşağıda ki fonksiyonu çözüm için kullanabilirsiniz.

Bu dile şuan – günümüzde (31/03/2016) çok da güvenmeyin abiler ablalar !. Zaten kısa bir araştırma sonrasında Javascript tutarsızlıklarıyla ilgili bir sürü dökümana ulaşabilirsiniz.
**Not:** Esneklik beraberinde hata getirir.

 
Çözüm (Gerçekten bu yazıyı yazarken sinirliyim !)

```javascript
            function clearText(text) {
                return text.replace(/İ/g, "i").replace(/Ş/g, "s").replace(/Ü/g, "u").replace(/Ö/g, "o").replace(/Ç/g, "c")
                    .toLowerCase()
                    .replace(/ü/g, "u").replace(/ı/g, "i").replace(/ç/g, "c")
                    .replace(/ğ/g, "g").replace(/ş/g, "s").replace(/ö/g, "o");
             
            }
```            
 

 



