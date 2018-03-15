+++
desciption = ""
thumbnailImagePosition = "top"
title = "Android Runtime Permission"
categories = [
  "yazilim",
  "java","android"
]
tags = [
  "software","java","android","permission","runtime permission"
]
thumbnailImage = ""
date = "2017-01-11T17:24:52+03:00"
keywords = [
  "yazilim",
  "sofware","java","android","permission","runtime permission"
]
autoThumbnailImage = false
metaAlignment = "center"
coverImage = "https://c8.staticflickr.com/8/7421/9339731831_9ba94f287c_k.jpg"

+++

Yavaş yavaş mevcut android cihazlarının API level düzeyi doğal olarak yükselmekte ve bizlerinde tabi ki buna ayak uydurması gerekmekte.

Bunlardan biri de Android M – Marshmallow (23) ile  gelen Runtime Permissions olayı.

Kullanıcı açısından bakıldığında oldukça yararlı birşey gibi duruyor ama tabi ki biz kullanıcılar runtime sırasında çıkan permission dialog daki yazıyı ne kadar okuruz ve buna göre onay veririz meçhul. Aslına bakarsanız çok da okunacağını düşünmüyorum ama olsun yine de güzel. İlerleyen zamanlarda illa ki bu alışkanlığı edinecek insanlar olacaktır 😉

Fazla uzatmadan birkaç link ve ipucu vericem.

Android Runtime Permissions olayının nasıl yapıldığını görmek için

- Link : https://github.com/googlesamples/android-RuntimePermissions burada süper bir örnek var.

- Link : http://android-developers.blogspot.com.tr/2015/08/building-better-apps-with-runtime.html burada kısa bir özet var.

 

Takıldığınız yeri yorumlarda belirtebilirsiniz.

**Not :** Örneği incelediğiniz de şu satırı yazarken dikkat edin.

`ActivityCompat.requestPermissions(this, new String[]{android.Manifest.permission.CAMERA},123456);`

burada sadece

`Manifest.permission.CAMERA`

yerine

`android.Manifest.permission.CAMERA`

kullanacaksınız. Yoksa izin listesini bulamaz.

