+++
categories = [
  "yazilim","kotlin"
]
tags = [
   "software","kotlin","lambda"
]
keywords = [
  "yazilim",
  "sofware","kotlin","lambda","functional programming"
]
#url = "kotlin_lambda"
metaAlignment = "center"
autoThumbnailImage = false
thumbnailImage = ""
title = "Kotlin Lambda"
desciption = "Kotlin Lambda"
coverImage = "https://c8.staticflickr.com/8/7421/9339731831_9ba94f287c_k.jpg"
thumbnailImagePosition = "top"
date = "2017-06-18T02:24:37+03:00"

+++


# Kotlin Lambda Kullanımı

Kotlin dili ile geliştirme yaparken fonksiyonel programlama nimetlerinden faydalanmamızı sağlayan **lambda** birçok konuda bize yardımcı olacaktır. Doğru kullanımını öğrendiğimiz ölçüde nimetlerinin farkına varabiliriz.

Belli başlı lambda kullanımları için aşağıdaki örneği inceleyebilirsiniz.

```
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_kotlin_lambda)

        // Normal Kullanım
        ButtonPress.setOnClickListener(object : View.OnClickListener {
            override fun onClick(view: View?) {
                toast("Press Me Click")
            }

        })

        // Yukarıda ki yapıyı lambda kullanarak bu şekle çevirebiliriz
        ButtonPress.setOnClickListener({ v -> toast("Press Me") })

        // Eğer son parametre lambda fonksiyonu ise bu fonksiyonu parantez '()' dışına çıkarabiliriz.
        // Birden fazla parametre var ise sadece en son lambda parametresi parantez dışına çıkarılabilir.
        ButtonPress.setOnClickListener() {
            v ->
            toast("Press Me")
        }

        // Fonksiyon tek bir parametre alıyorsa ve bu lambda parametresi ise parantezler silinebilir
        ButtonPress.setOnClickListener {
            v ->
            toast("Press Me")
        }

        // Lambda fonksiyonunun tek bir parametresi var ise (v) ve kullanılmayacaksa 'v ->' silinebilir
        ButtonPress.setOnClickListener {
            toast("Press Me")
        }

        // Lambda fonksiyonunun tek bir parametresi var ise ve eğer bu parametreye ihtiyaç duyulur ise
        // v-> yerine 'it' özel kelimesi kullanılabilir.
        ButtonPress.setOnClickListener {
            toast(it.javaClass.name)
        }

    }
```     


