+++
date = "2017-06-06T04:33:06+03:00"
title = "Kotlin Let Kullanımı"
description = "`let()` temel olarak, belirli bir kapsam için bir **değişken/kod bloğu** oluşturmamızı sağlayan bir kapsamlayıcı işlevdir. Yani `let()` bir sarmalıyıcı fonksiyondur."
url = "kotlin-let"

categories = [
  "yazilim","kotlin"
]
keywords = [
  "yazilim",
  "sofware","kotlin","let","android"
]
tags = [
  "software","kotlin","let","android"
]

+++

## Kotlin

```kotlin
fun <T, R> T.let(f: (T) -> R): R = f(this)
```

`let()` temel olarak, belirli bir kapsam için bir **değişken/kod bloğu** oluşturmamızı sağlayan bir kapsamlayıcı işlevdir. Yani `let()` bir sarmalıyıcı fonksiyondur.

Örneğin:

```kotlin
private var mPhotoUrl: String? = null

fun uploadClicked() {
    if (mPhotoUrl != null) {
        uploadPhoto(mPhotoUrl!!)
    }
}
```

Yukarıda ki `if (mPhotoUrl != null)` satırı ile null kontrolü yapılır ve eğer `null` değilse `uploadPhoto(mPhotoUrl!!)` kod bloğu çalıştırılır. Bu kodu `let()` ile çok daha kolay ve anlaşılır hale getirebiliriz.

```kotlin
    private var mPhotoUrl: String? = null

    fun uploadClicked() {
        mPhotoUrl?.let { uploadPhoto(it) }
    }
```
---

**Not:** `let()` ile sarmalanan bir kod bloğu içerisinde sarmalayıcıya `it` ile ulaşılabilir.

```kotlin
File("a.txt").let {
    // it kullanılarak file nesnesine erişilebilir.
}
```
