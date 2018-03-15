+++
title = "Kotlin Sealed Class"
#url = "kotlin_sealed_class"
desciption = "Kotlin Sealed Class"
metaAlignment = "center"
categories = [
  "yazilim","kotlin","android"
]
tags = [
  "software","kotlin","android","sealed","enum"
]
keywords = [
  "yazilim","sofware","kotlin","sealed-class","android"
]
autoThumbnailImage = false
thumbnailImagePosition = "top"
thumbnailImage = ""
date = 2017-07-01

+++

# Kotlin Sealed Class

**Enum** yapısına benzer bir yapıya sahiptir. Ve benzer görevler için kullanılır. Aradaki fark enum lar sabit değer ifade etmek için kullanılırken **sealed** yapısı normal sınıf gibi fakat enum mantığıyla kullanılır. Yani belli bir duruma ait fonksiyonları bir arada tutmak ve **when()** gibi fonksiyonlar ile birlikte kullanımını sağlamak için kullanılır.

```java 
    // Sealed class enum yapısına benzer
    // Bir durum için belli başlı akışları bir arada tutmamızı sağlar ve bu akışların yönetimini kolaylaştırır.
    // abstract fonksiyon tanımlanamabilir
    sealed class Intention {
        object None : Intention() {
            override fun go() {

            }

            fun xyc() {

            }
        }

        object Refresh : Intention() {
            override fun go() {

            }
        }

        data class Error(val reason: String) : Intention() {
            override fun go() {

            }
        }

        data class LoadContent(val content: List<String>) : Intention() {
            override fun go() {

            }
        }

        abstract fun go()
    }
```

```java
 override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_kotlin_sealed_class)

        val intentation: Intention = Intention.LoadContent(emptyList())

        when (intentation) {
            Intention.None -> {
                Intention.None.apply {
                    this.xyc()
                    this.go()
                }
                println("none")
            }
            Intention.Refresh -> {
                println("refresh")
            }
        }
    }
```


şeklinde kullanılabilir.
