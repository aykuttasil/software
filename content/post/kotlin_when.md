+++
date = "2017-06-06T04:42:56+03:00"
title = "Kotlin when kullanımı"
description = "Kotlin'de `when` operatörü işimizi oldukça kolaylaştıran operatörlerden biridir. `if- else if` yapısı yerine kullanılabileceği gibi bazı yardımcı operatörler ile birçok marifet kazanabilir."
url = "kotlin-when"

categories = [
  "yazilim","kotlin"
]
keywords = [
  "yazilim",
  "sofware","kotlin","android","when"
]
tags = [
  "software","kotlin","android","when"
]
+++

Kotlin'de `when` operatörü işimizi oldukça kolaylaştıran operatörlerden biridir.

`if- else if` yapısı yerine kullanılabileceği gibi bazı yardımcı operatörler ile birçok marifet kazanabilir.

```kotlin
    val i = 10
    when {
        i < 7 -> println("first block")
        fooString.startsWith("hello") -> println("second block")
        else -> println("else block")
    }
```

Yukarıda ki örnek if-else if-else yapısının aynısıdır.

---

```kotlin
    when (i) {
        0, 21 -> println("0 or 21")
        in 1..20 -> println("in the range 1 to 20")
        else -> println("none of the above")
    }
```

Yukarıda ki gibi aralıklar tanımlanabilir ve bu aralıklara uyum kontrolü yapılarak ilgili işlemlerin yapılması sağlanılabilir.

---

```kotlin
    var result = when (i) {
        0, 21 -> "0 or 21"
        in 1..20 -> "in the range 1 to 20"
        else -> "none of the above"
    }
    println(result)
```

Yukarıda ki gibi `when` sonucu bir değişkene atanarak istenilen yerde kullanılabilir.