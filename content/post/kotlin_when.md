+++
coverImage = "https://c8.staticflickr.com/8/7421/9339731831_9ba94f287c_k.jpg"
autoThumbnailImage = false
desciption = "Kotlin when kullanımı"
metaAlignment = "center"
#url = "kotlin_when"
categories = [
  "yazilim","kotlin"
]
keywords = [
  "yazilim",
  "sofware","kotlin","android","when"
]
date = "2017-06-06T04:42:56+03:00"
title = "Kotlin when kullanımı"
tags = [
  "software","kotlin","android","when"
]
thumbnailImagePosition = "top"
thumbnailImage = ""

+++

Kotlin'de `when` operatörü oldukça işimizi kolaylaştıran operatörlerden biridir.

`if- else if` yapısı yerine kullanılabileceği gibi bazı yardımcı operatörler ile birçok marifet kazanabilir.

```
    val i = 10
    when {
        i < 7 -> println("first block")
        fooString.startsWith("hello") -> println("second block")
        else -> println("else block")
    }
```    

Yukarıda ki örnek if-else if-else yapısının aynısıdır.

---

``` 
    when (i) {
        0, 21 -> println("0 or 21")
        in 1..20 -> println("in the range 1 to 20")
        else -> println("none of the above")
    }
``` 

Yukarıda ki gibi aralıklar tanımlanabilir ve bu aralıklara uyum kontrolü yapılarak ilgili işlemlerin yapılması sağlanılabilir.

---
```

    var result = when (i) {
        0, 21 -> "0 or 21"
        in 1..20 -> "in the range 1 to 20"
        else -> "none of the above"
    }
    println(result)
``` 
Yukarıda ki gibi `when` sonucu bir değişkene atanarak istenilen yerde kullanılabilir.