+++
title = "Kotlin @JvmOverloads"
date = 2017-07-09
#url = "kotlin_jvmoverloads"
desciption = "Kotlin JvmOverloads Annotations"
metaAlignment = "center"
categories = [
  "yazilim","kotlin","android"
]
tags = [
  "software","kotlin","jvmoverloads","android","annotations"
]
keywords = [
  "yazilim","sofware","kotlin","jvmoverloads"
]
autoThumbnailImage = false
thumbnailImagePosition = "top"
thumbnailImage = ""
draft = false

+++

# Kotlin @JvmOverloads

Kotlin ile uygulama geliştirmeyi teşvik eden en büyük etkenlerden biri şüphesiz **Java** dili uyumlu yapısıdır. Her iki tarafdan da birbirlerine referanslar verilebilir.

Kotlin dilinin Java'dan ayıran özelliklerinden biri Java compiler ından daha zeki olmasıdır. Nitekim java dili yaşlanıyor :). Kotlin ile oluşturulan bazı yapıların Java tarafına uyumlu hale getirilmesi için bazen ufak düzenlemelere gerek duyulabiliyor.
Bu düzenlemelerden bir tanesi de `@JvmOverloads` annotation kullanımı.

```java 
fun ViewGroup.inflate(resId: Int, attachToRoot: Boolean = false): View {
    return LayoutInflater.from(context).inflate(resId, this, attachToRoot)
}
``` 

Yukarıda ki gibi **kotlin extension** yapısı kullanılarak **ViewGroup** nesnesine ek bir özellik kazandırdık.

Artık **LinearLayout** nesnemizden direk olarak `inflate` fonksiyonunu çağırabiliriz.

Ve Kotlin ile geliştilen başka bir sınıftan da bu **inflate** fonksiyonu çağırabilir. Ayrıca **default parametre ataması** sayesinde sadece `ViewGroupObjesi.inflate(resId)` ile fonksiyon kullanılabilir. Yani **attachToRoot** parametresi girmediğimiz için defaut olarak **false** olacaktır.

## Peki başka bir Java sınıfından inflate fonksiyonu çağırımı nasıl olacak ?

```java
View v = UtilsKt.inflate(parent, R.layout.view_item, false);
```

Yukarı daki gibi **inflate** fonksiyonu kullanılablir. Fakat;

```java
View v = UtilsKt.inflate(parent, R.layout.view_item)
```

bu şekilde bir kullanım sonucundan **Java** abimiz kızacak ve hata verecektir. **Default** paramtere ataması **Java** tarafından tanınmaz.

## @JvmOverloads

```java
@JvmOverloads
fun ViewGroup.inflate(resId: Int, attachToRoot: Boolean = false): View {
    return LayoutInflater.from(context).inflate(resId, this, attachToRoot)
}
```

Mevcut extension fonksiyonumuz başına `@JvmOverloads` ekleyerek default parametre atamamızın **Java** tarafından tanınmasını sağlayabilir.

Ve artık Java sınıfı içerisinde,

```java
View v = UtilsKt.inflate(parent, R.layout.view_item);
```

şeklinde kullanım sağlayabiliriz.


---

Java içerisinden **UtilsKt** sınıfını başka bir isimlendirme ile çağırmak için **UtilsKt** sınıfı içerisinde **package** tanımlamasından önce `@file:JvmName("AndroidUtils"` şeklinde bir ekleme yapabiliriz.

```java
AndroidUtils.logD("Debug");
AndroidUtils.logE("Error");
View v = AndroidUtils.inflate(parent, R.layout.view_item, false);
```

Yukarıda ki gibi **UtilsKt** sınıfına erişim sağlayabiliriz.



