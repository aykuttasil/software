+++
title = "Android Architecture Components"
#url = "android-architecture-components"
desciption = "Android Architecture Components"
metaAlignment = "center"
categories = [
  "yazilim","android"
]
tags = [
  "software","android","kotlin","android-architecure","viewmodel","livedata","lifecycle"
]
keywords = [
  "yazilim","sofware","android","kotlin","android-architecure","viewmodel","livedata","lifecycle"
]
autoThumbnailImage = false
thumbnailImagePosition = "top"
thumbnailImage = ""
date = 2017-08-27
+++

# Android Architecture Components

Öncelikle şu linkleri verelim:

- https://developer.android.com/topic/libraries/architecture/index.html
- [Lifecycle](https://developer.android.com/topic/libraries/architecture/lifecycle.html)
- [LiveData](https://developer.android.com/topic/libraries/architecture/livedata.html)
- [ViewModel](https://developer.android.com/topic/libraries/architecture/viewmodel.html)


### LifeCycle


Daha önce Android uygulaması geliştirenler çok iyi bilirler ki bir activity veya fragment ın yaşam döngüsünü yönetmek sıkıntılı bir süreçtir. Activity'nin arka planda mı yoksa görünür vaziyette mi oluşuna göre düzenlenen akışlar çoğu zaman yoğun dikkat gerektiren süreçlerdir. Yukarıda ki [Lifecycle](https://developer.android.com/topic/libraries/architecture/lifecycle.html) linkine tıklarsanınz çok güzel bir örnek ile durumu açıklamışlar. 

Oluşturduğumuz nesneler eğer activity'nin yaşam döngüsüne bağlı ise bunu yönetmek için activity'mizin genelde **onCreate** ve **onStop** metodları içerisinde bu nesnelere ait fonksiyonları çağırıyorduk. Uygulama büyüdükçe **onCreate** ve **onStop** fonksiyonları şişiyor ve yönetilmesi karışık bir hal alıyordu. Kod kalabalıklığından ziyade gözden kaçması daha büyük sorunlara sebebiyet veriyordu. Hele bir de mevcut activity de asenkron bir süreç varsa ve asenkron işlem bitene kadar kullanıcı sabredemeyip başka bir activity e gitmiş ise ne olacak? Boom.. Bir çok çatlamanın sebebi de budur sayın seyirciler!

**Di** li geçmiş zaman kullandım çünkü artık bu sorunlar ortadan kalkıyor :) . Oluşturduğumuz sınıflara otomatik olarak activity veya fragment ın yaşam döngüsü fonksiyonlarını kontrol eden bir yapı ekleyebiliyoruz.

[Buraya](https://developer.android.com/topic/libraries/architecture/lifecycle.html) tıkladığınızda açılan örnekte location listener yapısı gösterilmiş. Normalde **Activity -> onStart** içerisinde konum dinlemeyi başlat, **Activity -> onStop** içerisinde durdur dememiz lazım. Lakin **onStart** içerisinde konum servisini başlat diyemeden (asenkron bir fonksiyon çalıştırıldığını farz ediyoruz) kullanıcı ekrandan çıkarsa yukarıda bahsettiğimiz olay olucak ve uygulama patlıcak. Ama bir nesnemiz olsa, bu nesnemiz otomatik olarak activity veya fragment ın yaşam döngüsüne bağlı çalışsa ve biz bu nesneyi istediğimiz fragment veya activity içerisinde kullanabilsek ve tüm bunları yaparken tekrar tekrar aynı yaşam döngüsü kontrollerini yazmasak nasıl olurdu?

Ne işe yaradığını sanıyorum anladık.



## ViewModel




