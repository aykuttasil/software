+++
title = "Kotlin Let Kullanımı"
desciption = "Kotlin let kullanımı "
keywords = [
  "yazilim",
  "sofware","kotlin","let","android"
]
thumbnailImagePosition = "top"
#url = "kotlin_let"
autoThumbnailImage = false
date = "2017-06-06T04:33:06+03:00"
tags = [
  "software","kotlin","let","android"
]
thumbnailImage = ""
metaAlignment = "center"
categories = [
  "yazilim","kotlin"
]
+++

# Kotlin

```
fun <T, R> T.let(f: (T) -> R): R = f(this)
```

`let()` temel olarak, belirli bir kapsam için bir **değişken/kod bloğu** oluşturmamızı sağlayan bir kapsamlayıcı işlevdir. Yani `let()` bir sarmalıyıcı fonksiyondur.

Örneğin: 

```
private var mPhotoUrl: String? = null

fun uploadClicked() {
    if (mPhotoUrl != null) {
        uploadPhoto(mPhotoUrl!!)
    }
}
```
Yukarıda ki `if (mPhotoUrl != null)` satırı ile null kontrolü yapılır ve eğer `null` değilse `uploadPhoto(mPhotoUrl!!)` kod bloğu çalıştırılır. Bu kodu `let()` ile çok daha kolay ve anlaşılır hale getirebiliriz.

```
    private var mPhotoUrl: String? = null

    fun uploadClicked() {
        mPhotoUrl?.let { uploadPhoto(it) }
    }
``` 

---

**Not:** `let()` ile sarmalanan bir kod bloğu içerisinde sarmalayıcıya `it` ile ulaşılabilir.

```
File("a.txt").let {
    // it kullanılarak file nesnesine erişilebilir.
}
```


