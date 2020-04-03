+++
autoThumbnailImage = false
categories = ["yazilim", "kotlin"]
coverImage = "https://c8.staticflickr.com/8/7421/9339731831_9ba94f287c_k.jpg"
date = "2017-06-06T10:47:58+03:00"
desciption = ""
keywords = ["yazilim", "sofware", "kotlin", "apply"]
metaAlignment = "center"
tags = ["software", "kotlin", "apply", "extensin function"]
thumbnailImage = ""
thumbnailImagePosition = "top"
title = "Kotlin apply Kullanımı"
url = "kotlin-apply"

+++

```kotlin
fun <T> T.apply(f: T.() -> Unit): T { f(); return this }
```

`apply()` tüm tipler için belirlenmiş bir extension function dır. apply() fonksiyonu uygulanan nesnenin özelliklerine direk olarak apply kod bloğu içerisinden erişilebilir. 

```kotlin
var file = File(dir)
file.mkdirs()
```

veya Java ile yazacak olursak

```kotlin
File makeDir(String path) {
    File result = new File(path);
    result.mkdirs();
    return result;
}
```

Bu kodu aşağıdaki yapıya çevirebiliriz.

```kotlin
File(dir).apply { mkdirs() }
```
