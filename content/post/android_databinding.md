---
title: "Android DataBinding"
date: 2019-02-25T00:00:27+03:00
lastmod: 2019-02-25T00:00:27+03:00
draft: false
keywords: ["android","databinding","twoway-binding"]
description: "Android DataBinding"
tags: ["mobile","android","databinding"]
categories: ["mobile","android"]
author: "Aykut Asil"
comment: true
toc: true
autoCollapseToc: false
postMetaInFooter: true
hiddenFromHomePage: false
---

# Android DataBinding

Android dünyasında temiz kod(**Clean Code**) günümüzde çokça konuşulan konular arasında yer almakta ve neden temiz kod yazmalıyız ile alakalı bir çok makale yazılmaktadır.

Temiz kod yazmak için **MVP, MVVM** vs. kod tasarım mimarilerinden birini seçerek yazılan kodların ve etkileşimlerinin birbirinden ayrımını sağlamaktayız. Bu kod tasarım kalıpları ile beraber bize çok faydası olacak bir mimari daha bulunmakta:**DataBinding**

## DataBinding

Uygulamamızın arayüzünü oluşturmak için kullandığımız **layout** xml dosyalarına gerekli **data**yı doğrudan entegre ederek, arayüz değişikliği ile ilgili tüm akışların harici müdahaleye gerek kalmadan otomatik olarak gerçekleşmesini sağlayabiliriz. Bununla birlikte **null** kontrolü gibi kontrollerin de otomatik yapılmasını sağlayarak uygulamamızın milyon dolarlık hataya yakalanmasını engelleyebiliriz ;) ve bunun gibi bir çok faydası bulunmakta elbette.

### Nasıl Kullanılır?

**DataBinding** mimarisinin nasıl kullanıldığını örnek bir **Login** sayfası üzerinde görelim.

> **Not:** Birkaç farklı kullanma şekli olsa da burada en modern ve en kolay yaklaşımı ele alıcaz.

![Login Screen](/image/android_databinding_sample_ss.png "SweetLoc Login Screen")

Yukarıda ki ekran görüntüsüne benzer bir ekranımız var.

Yapmak istediklerimiz:

- **Email** ve **Parola** alanına bir şey yazılmadan **Giriş Yap** ve **Kayıt Ol** butonuna basılamasın.
- **Email** alanına giriş yapıldığında, yazılan metnin aynısı parola alanının hemen altında gözüksün.
- **Giriş Yap** veya **Kayıt Ol** butonlarına basıldığında doğrudan **ViewModel**imiz içerisinde ki ilgili fonksiyonları çağıralım. Bunu yaparken, parametre olarak girilen bilgileri verelim.

Kodları vermeden önce **DataBinding** mekanizmasının nasıl çalıştığını kısaca özetleyecek olursak, ilk olarak ilgili layoutumu dosyamızı `<layout> </layout>` tagları arasına alıyoruz.

**app/build.gradle** dosyamızı aşağıdaki gibi düzenleyelim.
<script src="https://gist.github.com/aykuttasil/4c92b3b4f770d1a64ae3e5de0eaba102.js"></script>

Activity kodlarımız aşağıdaki gibi olacak şekilde düzenlemeleri yapalım.
<script src="https://gist.github.com/aykuttasil/5a1c82abaa5e8638880c8a552e5fd1c4.js"></script>

> `binding.lifecycleOwner = this` satırını eklediğimizden olalım. Bu kodu **LiveData** kullanarak **DataBinding** yapmak istiyorsak eklemek zorundayız.

Yukarıda ki kodda bulunan **bind<>()** fonksiyonu kotlin extension fonksiyonudur.
<script src="https://gist.github.com/aykuttasil/6117974f91a0312b248d976e6479aa76.js"></script>

**LoginViewModel.kt** dosyamızı aşağıdaki gibi düzenleyelim.
<script src="https://gist.github.com/aykuttasil/063671e6353b61954daeeef52c12eed5.js"></script>

Kalıtım aldığımız **RxAwareViewModel.kt** sınıfının kodları aşağıdaki gibidir. Bu sınıfı **Rx** fonksiyonları çağırırken her defasında **CompositeDisposable** nesnesi oluşturmamak için kullanıyoruz.
<script src="https://gist.github.com/aykuttasil/ff97723de6f9182f00369f1eb64adb8b.js"></script>

**Login** ekranımızın **layout**'unu aşağıdaki gibi düzenleyelim.
<script src="https://gist.github.com/aykuttasil/064ab78950d9a62bedd2228a70b67903.js"></script>



# Kaynaklar

- <https://developer.android.com/reference/android/databinding/package-summary>
- <https://developer.android.com/topic/libraries/data-binding>
- <https://github.com/googlesamples/android-databinding>
