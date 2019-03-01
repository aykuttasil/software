---
title: "Android Test"
date: 2018-09-18T11:51:07+03:00
lastmod: 2019-03-01T11:51:07+03:00
draft: false
keywords: ["android","test","junit","roboloectric","mockito","mock","espresso"]
description: "Android Test"
tags: ["android","test","junit","roboloectric","mockito","mock","espresso"]
categories: ["android","test"]
author: "Aykut Asil"
comment: true
toc: true
autoCollapseToc: false
postMetaInFooter: true
hiddenFromHomePage: false
---

> **Not:** Bu yazıyı hazırlarken ben de öğrenme sürecinde olucam. Araştırdığım ve öğrendiğim tüm teknik bilgileri ve yöntemleri, best-practice leri gelişi güzel(karmakarışık değil) bir şekilde yazıcam. Sonrasında zaman bulduğum vakit bir düzenleme yapıcam.

# Unit Test ve Instrumentation Test

## Unit Test

**Unit Test:** Android framework ünden bağımsız olan sınıfları/metodları test etmek için kullanılır. **Robolectric** ve **JUnit** popüler unit test araçlarıdır.

>If you run local unit tests, a special version of the android.jar (also known as the Android mockable jar) is created by the tooling. This modified JAR file is provided to the unit test so that all fields, methods and classes are available. Any call to the Android mockable JAR results, by default, in an exception, but you can configure Android to return default values. See Activating default return values for mocked methods in android.jar for details.

## Instrumentation Test (on-device)

Android framework'ü ile gerçek anlamda etkileşime girmesi gereken sınıfların testi için **Instrumentation Test** yapılması gerekmektedir. **Espresso**, **UIAutomator**, **Robotium** popüler instrumentation test araçlarıdır.

- **Unit Test** sınıfları aşağıdaki resimde görülen **test** package'ı altına eklenir.
- **Instrumentation Test** sınıfları resimde görülen **androidTest** package'ı altına eklenir.
- **app/src/test/java** - for any unit test which can run on the JVM
- **app/src/androidTest/java** -> for any test which should run on an Android device

<img src="/image/unit_vs_instrumential.png" height="400px" />

> Android bağımlılıklarını mock edemiyorsak Instrumentation Test yazılır. Eğer mock edebileceğimiz bir yapıya sahip ise unit test yazılır. Bu sayede hızlı bir şekilde testler koşturulabilir.

- Instrumentation test yapılırken sınıf **@AndroidJUnitRunner** annotations'ı ile etiketlenmelidir.
- Instrumentation testleri JVM yerine gerçek bir cihazda veya emülatör de koşturulur.

AndroidJunitRunner provides access to the instrumentation API, via the InstrumentationRegistery.**

- **InstrumentationRegistry.getInstrumentation()**, returns the Instrumentation currently running.
- **InstrumentationRegistry.getContext()**, returns the Context of this Instrumentation's package.
- **InstrumentationRegistry.getTargetContext()**, returns the application Context of the target application.
- **InstrumentationRegistry.getArguments()**, returns a copy of arguments Bundle that was passed to this Instrumentation. 

This is useful when you want to access the command line arguments passed to the instrumentation for your test.
It also gives access to the life cycle via the ActivityLifecycleMonitorRegistry.

---

# Espresso

Uygulamanın arayüzü ile ilgli test yazımı için kullanılır.

---

# Roboloctric

Robolectric, kodunuzu yerel JVM'de gerçek(sahte olmayan) Android JAR'lere karşı çalıştırır. Bu, düşük seviyeli sistem bileşenlerini (UI gibi) simüle etmek için baytkod manipülasyonu kullanılarak yapılabilir.

Sonuç, bir emülatöre ya da cihaza dağıtma yükü olmadan daha gerçekçi bir test tertibatıdır. Gerçek Android framework ünün çalışan bir sürümünü kullanmak, test yürütmeyi yavaşlatır ve saf unit testleri ile karşılaştırıldığında bir derece kırılganlık ekler.

Roboloctric ile neredeyse her android componentinin **shadow** hali üretileblir. Ve bu üretilen shadow nesneleri normal componentlerin sahip olmadığı bazı fonksiyonlara sahiptir.

**Mockito, Espresso** gibi diğer test bileşenleri ile birlikte çalıştırılabilir.

```java
Activity activity = Robolectric.buildActivity(MyAwesomeActivity.class).create().get();
```

şeklinde test edeceğimiz **Activity** nin oluşturulmasını sağlıyoruz.

Aşağıdaki gibi activity nin lifecycle'ına uygun testler yazılabilir. Burada **onResume** sonrası çalışan kod bloğu için test yazımını görebiliriz.

<script src="https://gist.github.com/aykuttasil/3bd879a909ba1fe1567e0480833db476.js"></script>

>**visible()**

Gerçek bir Android uygulamasında, bir Etkinliğin görünüm hiyerarşisi, **onCreate()** çağrıldıktan sonraya kadar window'a eklenmez. Bu gerçekleşene kadar Activity'nin görünümleri görünür olarak bildirilmez. Bu, onları tıklayamayacağınız anlamına gelir. Ne zaman görünür olacağı varsayımları yapmak yerine, Robolectric testleri yazarken gücü geliştiricinin ellerine verir.

Ne zaman ihtiyacımız olacak? Activity içindeki görünümlerle etkileşime girdiğinizde.

**Robolectric.clickOn()** gibi yöntemler, işlev görmesi için view'in görünür olması ve düzgün şekilde eklenmesini gerektirir. Bunun için **create()** öğesinden sonra **visible()** öğesini çağırmalıyız.

## Robolectric 4

gradle.properties

```bash
android.enableUnitTestBinaryResources=true
```

build.gradle

<script src="https://gist.github.com/aykuttasil/a2e40a8dcf6bdc7d0297d8660da56b4b.js"></script>

test.kt (test package'ı altında)

<script src="https://gist.github.com/aykuttasil/e79486683816af6b2358a5d77bae9a7a.js"></script>

Yukarıda **RobolectricRunner** yerine **AndroidJunit4** kullanılmıştır. Ve **Espreesso**'nun tüm yetenekleri kullanılabilir. Normalde **Espresso** testi yazılması için **androidTest** package'ı altında tanımlama yapmak ve gerçek bir cihazda ya da emülatör de çalıştırmamız gerekecekti.

---

# Mockito

<script src="https://gist.github.com/aykuttasil/73b847750cedb0d63f818706d7165655.js"></script>

- Yukarıda ki bağımlılıklar eklenir.
- **@RunWith(MockitoJUnitRunner.class)** ile sınıf etiketlenir. Bu sayade mock objelerinin(@Mock) otomatik olarak inject edilmesi sağlanır.
- **@Mock** etiketi ile objeler etiketlenir.

> @Mock ve @Spy arasındaki fark

- https://www.javainuse.com/java/mockSpy
- Test sürecinde bir objenin tüm alanlarını/metodlarını mocklamamız gerekiyor ise **@Mock** kullanılır.
- Eğer sınıf içerisinde sadece belli metodları mocklamamız gerekiyor(Partial Mocking) ve diğer alanların gerçek değerleri ile işlem yapılacak ise **@Spy** ile etiketlenir.
- Partial mocking can also be achieved using mock thenCallRealMethod()

**Kotlin** ile geliştirilen uygulamaların test yazımı sırasında **mock**lanmak istenen sınıflar, alanlar vs. kotlin dilinin yapısı gereği çeşitli hataların üretilmesine neden olur. Bu uyuşmazlığın sebeplerinden biri kotlin ile oluşturulan sınıfların, alanların default olarak **final** olması ve **Mockito**'nun **final** sınıf veya alanları mocklayamamasıdır. Çözüm olarak sınıfların, fonksiyonların vs. başına **open** eklenmesi veya mockitoyu eklerken aşağıdaki kütüphanenin kullanılması gerekmektedir.

- `testImplementation "org.mockito:mockito-inline:2.23.16""`
- `testImplementation "org.mockito:mockito-core:2.23.16"` bu satırı siliyoruz.

Kotlin dilinin nimetlerinden yararlanarak test yazımı sırasında mockitonun kullanımını kolaylaştırmak için aşağıdaki kütüphaneyi kullanabilriz

- https://github.com/nhaarman/mockito-kotlin
- `testImplementation "com.nhaarman.mockitokotlin2:mockito-kotlin:2.1.0"`

---

# Test Piramit

<img src="/image/test_piramid.jpeg" height="400px" />

> Unit Tests

Birim testleri, yazabileceğiniz en hızlı ve en ucuz testlerdir. Uygulamanızın tüm mantığını (ancak bağımlılıklarını değil) çoğunu kapsamaları gerekir. Bağımlılıklar, mock objeler ile değiştirilmelidir. Unit testleri, yazılım geliştirme sürecinin düzenli bir parçası olarak sıkça çalıştırılabilir. **Android JUnit, Mockito** yerel JVM'de hızlı birim tarzı testler yapmak için güzel bir araç kombinasyonu sunuyor.

> Integration Tests

Entegrasyon testleri, kodunuzun, sistemin diğer bölümleriyle nasıl etkileşime girdiğini doğrular ancak bir UI çerçevesinin ek karmaşıklığı olmadan. Bu katman için Robolectric kullanılabilir.

> UI Tests

UI testleri, en yavaş ve en pahalı test türüdür. Bunlar yavaştır, çünkü uygulamanızı bir emülatöre veya cihaza dağıtmanız ve kullanıcı arayüzünü kullanarak sürmeniz gerekir. Bunlar pahalıdır çünkü bu genellikle dahili bir cihaz laboratuarını ve CI sunucusunu korumak veya bir bulut test hizmeti için ödeme yapmak anlamına gelir. Bununla birlikte, UI, donanım, ürün yazılımı ve geriye dönük uyumluluk ile ilgili sorunları ortaya çıkarabildiklerinden, otomatik UI testleri herhangi bir test stratejisinin önemli bir bileşeni olabilir. Android'de UI testleri için sahip olduğumuz başlıca araçlar **Espresso, Robotium veya UI Automator**'dur. UI testleri, piramidin diğer katmanlarındaki çatlaklardan kaymış olabilecek sorunları yakalamak için değerli bir ikinci savunma hattı olabilir.

---

# Small - Medium - Large Tests

Testler 3 başlık altında kategorilendirilebilir.

**Small Test:** Unit testleri bu kategori altına alabiliriz. *test* package ı altına yazılan ve çalışma süresi kısa olan testler de Android ve UI ile alakalı olmayan sınıfların/metodların testi yazılır. Dökümantasyon,filtreleme vs. için small testlerin çalıştırılacağı sınıf **@SmallTest** olarak etiketlenir.

**Medium Test:** Dosya sistemine, database'e vs erişimi olan sınıfların testini bu kategoriye koyabiliriz. Dökümantasyon,filtreleme vs. için medium testlerin çalıştırılacağı sınıf **@MediumTest** olarak etiketlenir.

**Large Test:** Medium'dan farkı ağ erişimi gibi dış kaynaklara erişim gerektiren sınıfları bu kategoriye koyabiliriz. Dökümantasyon,filtreleme vs. için large testlerin çalıştırılacağı sınıf **@LargeTest** olarak etiketlenir.

---

# MockWebServer

Unit test yazarken api rest isteklerinin simüle edilmesini sağlar.

```bash
testImplementation 'com.squareup.okhttp3:mockwebserver:3.8.1'
```

- https://github.com/square/okhttp/blob/master/mockwebserver/README.md

Mockito gibi bir kullanım şekli vardır.

<script src="https://gist.github.com/aykuttasil/8c4367fa073b6e92b24c34e11fe7e814.js"></script>

<script src="https://gist.github.com/aykuttasil/7c85a90aed4c0cefe92cc66889f40401.js"></script>

SocketTimeoutException testi çalıştırılırken aşağıdaki düzenlemeler de yapılmalıdır:

- `mockResponse.throttleBody(1024, 1, TimeUnit.SECONDS)` Her saniye için sadece 1024 bayt göndermesini söylüyoruz.
- Client için okuma/yazma zaman sürelerini düzenliyoruz.

<script src="https://gist.github.com/aykuttasil/dcf702649fdd1f1b97a52fc03d50eae3.js"></script>

> getJson(path = "json/blog/blogs.json") ?

![Mock Web Server](/image/mockwebserber.png "Mock Web Server")

## Dispatcher

Her bir response'u ayrı ayrı mocklamak yerine aşağıdaki gibi bir yapıda kullanılabilir.

<script src="https://gist.github.com/aykuttasil/fc8e49413b7d5cc4e8857102796cea0e.js"></script>

---

# Gradle Yapılandırmaları

Unit testlerin yazımı sırasında mocklanmaya gerek olmayan nesnelerin default değerini dönmesini belirtmek için aşağıdaki şekilde düzenleme yapılmalıdır.

<script src="https://gist.github.com/aykuttasil/108d9400fb421101d852c287da2afe01.js"></script>

---

> Kaynaklar

- https://developer.android.com/training/testing/
- http://www.vogella.com/tutorials/AndroidTesting/article.html
- https://medium.com/android-testing-daily/the-3-tiers-of-the-android-test-pyramid-c1211b359acd
- https://github.com/codepath/android_guides/wiki/Unit-Testing-with-Robolectric
- https://github.com/codepath/android_guides/wiki/Android-Testing-Options
- https://www.javainuse.com/java/mockSpy
- https://proandroiddev.com/robolectric-testing-with-androidjunitrunner-86292bceef25
- http://robolectric.org/blog/2018/05/09/robolectric-4-0-alpha/
- https://github.com/googlesamples/android-testing
- https://github.com/square/okhttp/blob/master/mockwebserver/
- https://medium.com/appunite-edu-collection/ui-testing-on-android-with-dagger-espresso-and-mockito-12d37e5f613d
- https://medium.com/@rafael_toledo/setting-up-an-unified-coverage-report-in-android-with-jacoco-robolectric-and-espresso-ffe239aaf3fa
- https://github.com/arturdm/jacoco-android-gradle-plugin
- https://medium.com/@fabioCollini/android-testing-using-dagger-2-mockito-and-a-custom-junit-rule-c8487ed01b56
- https://codelabs.developers.google.com/codelabs/android-testing/index.html?index=..%2F..index#
- https://medium.com/androiddevelopers/write-once-run-everywhere-tests-on-android-88adb2ba20c5
- https://github.com/googlesamples/android-testing-templates