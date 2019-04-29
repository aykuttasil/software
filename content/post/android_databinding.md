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

Temiz kod yazmak için **MVP, MVVM** vs. kod tasarım mimarilerinden birini seçerek yazılan kodların ve etkileşimlerinin birbirinden ayrımını sağlamaktayız. Bu kod tasarım kalıpları ile beraber bize çok faydası olacak bir mimari daha bulunmakta: **DataBinding**

## DataBinding

Uygulamanın arayüzünü tasarlamak için kullandığımız **layout** dosyamızın içerisine 
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

Kodları vermeden önce **DataBinding** mekanizmasının nasıl çalıştığını kısaca özetleyecek olursak, ilk olarak ilgili layout kodlarımızı `<layout> </layout>` tagları arasına alıyoruz. Daha sonra **app/build.gradle** dosyamızda **databinding**'i aktifleştiriliyoruz. Ve projemizi **build** ettiğimizde **databinding** sistemini ilgili dosyaları otomatik olarak oluşturacaktır. Oluşan dosyaya erişmek için **Activity** veya **Fragment** içerisinden **DataBindingUtil** sınıfını kullanıyoruz. Dosya ismi LayoutName**Binding** şeklinde olacaktır. Ve sonrasında layout dosyamıza entegre etmek istediğimiz dataları, nesneleri vs. bu otomatik oluşan sınıf aracılığıyla gerçekleştiriyoruz.

---

Şimdi ne gibi düzenlemeler yapmamız gerekiyor kod üzerinden görelim.

**app/build.gradle** dosyamızı aşağıdaki gibi düzenleyelim.
<script src="https://gist.github.com/aykuttasil/4c92b3b4f770d1a64ae3e5de0eaba102.js"></script>

Activity kodlarımız aşağıdaki gibi olacak şekilde düzenlemeleri yapalım.
<script src="https://gist.github.com/aykuttasil/5a1c82abaa5e8638880c8a552e5fd1c4.js"></script>

> `binding.lifecycleOwner = this` satırını eklediğimizden emin olalım. Bu kodu **LiveData** kullanarak **DataBinding** yapmak istiyorsak eklemek zorundayız.

Yukarıda ki kodda bulunan **bind<>()** fonksiyonu kotlin extension fonksiyonudur.
<script src="https://gist.github.com/aykuttasil/6117974f91a0312b248d976e6479aa76.js"></script>

**LoginViewModel.kt** dosyamızı aşağıdaki gibi düzenleyelim.
<script src="https://gist.github.com/aykuttasil/063671e6353b61954daeeef52c12eed5.js"></script>

Kalıtım aldığımız **RxAwareViewModel.kt** sınıfının kodları aşağıdaki gibidir. Bu sınıfı **Rx** fonksiyonları çağırırken her defasında **CompositeDisposable** nesnesi oluşturmamak için kullanıyoruz.
<script src="https://gist.github.com/aykuttasil/ff97723de6f9182f00369f1eb64adb8b.js"></script>

**Login** ekranımızın **layout**'unu aşağıdaki gibi düzenleyelim.
<script src="https://gist.github.com/aykuttasil/064ab78950d9a62bedd2228a70b67903.js"></script>

**Layout** kodumuzu incelicek olursak

- `android:text="@={viewmodel.liveEmail}"` satırını çift yönlü binding yapmak için kullanıyoruz. Çift yönlü **binding** için **="@={}"** yapısını kullanıyoruz.
Yani, **EditText**'e yazılan değerler aynı anda **ViewModel** sınıfımızın içindeki **liveEmail** nesnemize atanmış olucak. 

**Not:** Burada dikkat edilmesi gereken bir nokta var. Çift yönlü binding yapmak istiyorsak, kullanacağımız **livedata** nesnemizin primitive tip olması gerekiyor. Bunu aşmanın çeşitli yöntemleri var ama bence en kolay ve okunabilir yöntem her çift yönlü binding yapılmak istenen alanın ayrı ayrı oluşturulması. 

Primitive tipten kastımız `val liveEmail = MutableLiveData<String>()` şeklinde yani **String** tipinde olması. Bu alanı MutableLiveData<**Int**>() şeklinde de oluşturabiliriz. Burada yapamayacağımız şey direk olarak bir **Personel**, **User** gibi nesne kullanamayacak olmamız.

`val liveUser = MutableLiveData<User>()` şeklinde nesnemizi oluşturduğumuzu ve **layout** kodumuzu da `android:text="@={viewmodel.liveUser.email}"` şeklinde düzelttiğimizi düşünelim. Burada çift yönlü binding çalışmayacaktır. Yani **User** nesnesinin **email** alanı otomatik olarak doldurulmayacaktır. Biraz fazla uzadı ama bu konuya dikkat etmemiz gerekiyor.

- `android:text="@{viewmodel.displayName}"` bu satırda tek yönlü binding yapıyoruz. Yani ViewModel sınıfımız içerisindeki **displayName** nesnesinin değerini bu **TextView** nesnemize text olarak gönderiyoruz. ViewModel içerisinde ki bu nesnenin değeri değiştiğinde **TextView** otomatik olarak güncellenecektir.

Burada tek yönlü binding'den kastımız data akışı **viewmodel -> layout** şeklinde olacak olmasıdır.

Çift yönlü binding de ise data akışı **viewmodel <-> layout** şeklinde olacaktır.

- `android:enabled="@{!TextUtils.isEmpty(viewmodel.liveEmail) &amp;&amp; !TextUtils.isEmpty(viewmodel.livePass)}"` satırında layout bileşenimizin, bizim durumumuz için **Button**, enable olup olmamasını bir koşula bağladık. Ve dinamik değişen değerler neticesinde eğer koşullar sağlanıyor ise **enable**, sağlanmıyor ise **disable** olacaktır. Burada **TextUtils** sınıfından bir fonksiyon kullandık. Bu şekilde harici bir sınıfa erişim sağlamak istiyorsak, bunu layout dosyamızda **import** ederek belirtmeliyiz.

`&amp;&amp;` kısmı aslında `&&` anlamına geliyor. Yani **Ve** koşulu.

- `android:onClick="@{()->viewmodel.login(edtEmail.getText().toString(),editParola.getText().toString())}"` bu satırda yaptığımız şey **viewmodel** sınıfımızdan bir fonksiyon çağırmak ve bu fonksiyona parametre olarak **edittext**'lere (email,parola) yazılan değerleri göndermek. **EditText** nesnesinin değerine ulaşmak için nesnenin **id** sini kullandığımıza dikkat edelim.

Login fonksiyonunu parametre göndermeden de çağırabilirdik. Çünkü zaten **edittext** bileşenlerine yazılan değerler otomatik olarak **viewmodel** sınıfındaki ilgili alanları doldurmuş oluyor. Ve biz bu nesnelere **(liveEmail,livePass)**, **login** fonksiyonunun içerisinden direk erişim sağlayarak ilgili işlemleri yapabiliriz.

---

**DataBinding** mimarisinin **ViewModel** ve **LiveData** ile birlikte kullanımı bu şekilde. Dediğim gibi farklı yaklaşımlar var ama modern ve sağlam olanı bu yaklaşım diyebiliriz :)

# Kaynaklar

- <https://developer.android.com/reference/android/databinding/package-summary>
- <https://developer.android.com/topic/libraries/data-binding>
- <https://github.com/googlesamples/android-databinding>
