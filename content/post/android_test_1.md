---
title: "Android Test"
date: 2018-09-18T11:51:07+03:00
lastmod: 2018-09-18T11:51:07+03:00
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

> Not: Bu yazıyı hazırlarken ben de öğrenme sürecinde olucam. Araştırdığım ve öğrendiğim tüm teknik ve yöntemleri, best-practice leri gelişi güzel(karmakarışık değil) bir şekilde yazıcam. Sonrasında zaman bulduğum vakit bir düzenleme yapıcam.

# Unit Test ve Instrumentation Test

## Unit Test

**Unit Test:** Android framework ünden bağımsız olan sınıfları/metodları test etmek için kullanılır. **Robolectric** ve **JUnit** popüler unit test araçlarıdır.

>If you run local unit tests, a special version of the android.jar (also known as the Android mockable jar) is created by the tooling. This modified JAR file is provided to the unit test so that all fields, methods and classes are available. Any call to the Android mockable JAR results, by default, in an exception, but you can configure Android to return default values. See Activating default return values for mocked methods in android.jar for details.

## Instrumentation Test (on-device)

Android framework ü ile ilişkili olan sınıfların testi için kullanılır. Örneğin bir Activity nin testi yazılacak ise Instrumentation Test yapılacak demektir. **Espresso**,**UIAutomator**,**Robotium** popüler instrumentation test araçlarıdır.

- Unit Test lerin yazımı için aşağıdaki resimde görülen **test** package ı kullanılmalıdır.
- Instrumentation Test lerin yazılımı için aşağıdaki resimde görülen **androidTest** package ı kullanılmalıdır.
- **app/src/test/java** - for any unit test which can run on the JVM
- **app/src/androidTest/java** -> for any test which should run on an Android device

<img src="/image/unit_vs_instrumential.png" height="400px" />

> Android bağımlılıklarını mock edemiyorsak Instrumentation Test yazılır. Eğer mock edebileceğimiz bir yapıya sahip ise unit test yazılır. Bu sayede hızlı bir şekilde testler koşturulabilir.

- Instrumentation test yapılırken sınıf **@AndroidJUnitRunner** annotations ı ile etiketlenmelidir.
- Instrumentation testleri JVM yerine gerçek bir cihazda veya emülatör de koşturulur.

AndroidJunitRunner provides access to the instrumentation API, via the InstrumentationRegistery.**

- **InstrumentationRegistry.getInstrumentation()**, returns the Instrumentation currently running.
- **InstrumentationRegistry.getContext()**, returns the Context of this Instrumentation’s package.
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

Sonuç, bir emülatöre ya da cihaza dağıtma yükü olmadan daha gerçekçi bir test tertibatıdır. Gerçek Android çerçevenin çalışan bir sürümünü kullanmak, test yürütmeyi yavaşlatır ve saf birim testleri ile karşılaştırıldığında bir derece kırılganlık ekler.

Roboloctric ile neredeyse her android componentinin shadow hali üretileblir. Ve bu üretilen shadow nesneleri normal componentlerin sahip olmadığı bazı fonksiyonlara sahiptir.

Mockito, Espresso gibi diğer test bileşenleri ile birlikte çalıştırılabilir.

```java
Activity activity = Robolectric.buildActivity(MyAwesomeActivity.class).create().get();
```

şeklinde test edeceğimiz **Activity** nin oluşturulmasını sağlıyoruz.

Aşağıdaki gibi activity nin lifecycle ına uygun testler yazılabilir. Burada **onResume** sonrası çalışan kod bloğu için test yazımını görebiliriz.

```java
ActivityController controller = Robolectric.buildActivity(MyAwesomeActivity.class).create().start();
Activity activity = controller.get();
// assert that something hasn't happened
activityController.resume();
// assert it happened!
```

>**visible()**

Gerçek bir Android uygulamasında, bir Etkinliğin görünüm hiyerarşisi, **onCreate()** çağrıldıktan sonraya kadar window'a eklenmez. Bu gerçekleşene kadar Activity'nin görünümleri görünür olarak bildirilmez. Bu, onları tıklayamayacağınız anlamına gelir. Ne zaman görünür olacağı varsayımları yapmak yerine, Robolectric testleri yazarken gücü geliştiricinin ellerine verir.

Ne zaman ihtiyacımız olacak? Activity içindeki görünümlerle etkileşime girdiğinizde.

**Robolectric.clickOn()** gibi yöntemler, işlev görmesi için view'in görünür ve düzgün şekilde eklenmesini gerektirir. Bunun için **create()** öğesinden sonra **visible()** öğesini çağırmalıyız.

## Robolectric 4

gradle.properties

```bash
android.enableUnitTestBinaryResources=true
```

build.gradle

```bash
android
{
    testOptions {
        unitTests {
            includeAndroidResources = true
        }
    }
}

dependency
{
    testImplementation 'com.android.support.test:rules:1.0.2'
    testImplementation 'com.android.support.test:runner:1.0.2'
    testImplementation 'com.android.support.test.espresso:espresso-core:3.0.2'
    testImplementation "org.robolectric:robolectric:4.0-alpha-1"
}
```

test.kt (test package'ı altında)

```kotlin
@RunWith(AndroidJUnit4::class)
class MainActivityTest {
    @get:Rule
    val rule = ActivityTestRule(MainActivity::class.java)

    @Test
    fun testAssertHelloText() {
        onView(withId(R.id.hello)).check(matches(withText("Hello World!")))
    }
}
```

Yukarıda **RobolectricRunner** yerine **AndroidJunit4** kullanılmıştır. Ve **Espreesso**'nun tüm yetenekleri kullanılabilir. Normalde **Espresso** testi yazılması için **androidTest** package'ı altında tanımlama yapmak ve gerçek bir cihazda ya da emülatör de çalıştırmamız gerekecekti.

---

# Mockito

```bash
dependencies {
    // Required -- JUnit 4 framework
    testImplementation 'junit:junit:4.12'
    // Optional -- Mockito framework
    testImplementation 'org.mockito:mockito-core:1.10.19'
}
```

- Yukarıda ki bağımlılıklar eklenir.
- **@RunWith(MockitoJUnitRunner.class)** ile sınıf etiketlenir. Bu sayade mock objelerinin(@Mock) otomatik olarak inject edilmesi sağlanır.
- **@Mock** etiketi ile objeler etiketlenir.

> @Mock ve @Spy arasındaki fark

- https://www.javainuse.com/java/mockSpy
- Test sürecinde bir objenin tüm alanlarını/metodlarını mocklamamız gerekiyor ise **@Mock** kullanılır.
- Eğer sınıf içerisinde sadece belli metodları mocklamamız gerekiyor(Partial Mocking) ve diğer alanların gerçek değerleri ile işlem yapılacak ise **@Spy** ile etiketlenir.
- Partial mocking can also be achieved using mock thenCallRealMethod()

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
testImplementation 'com.squareup.okhttp3:mockwebserver:(insert latest version)'
```

- https://github.com/square/okhttp/blob/master/mockwebserver/README.md

Mockito gibi bir kullanım şekli vardır.

```kotlin
val mockServer = MockWebServer()
mockServer.start()


val mockedResponse = MockResponse()
mockedResponse.setResponseCode(200)
mockedResponse.setBody("{}") // sample JSON

mockServer.enqueue(mockedReponse)


val recordedRequest = mockServer.takeRequest()
mockedRequest.path // /blogs
```

```kotlin
@RunWith(JUnit4::class)
class BlogRepositoryUTest {
  
    lateinit var blogRepository : BlogRepository
    lateinit var mockServer : MockWebServer
    lateinit var blogService : BlogService
  
    @Before @Throws fun setUp() {
        // Initialize mock webserver
        mockServer = MockWebServer()
        // Start the local server
        mockServer.start()

        // Get an okhttp client
        val okHttpClient = OkHttpClient.Builder()
                .build()

        // Get an instance of Retrofit
        val retrofit = Retrofit.Builder()
                .addCallAdapterFactory(RxJava2CallAdapterFactory.create())
                .baseUrl("https://api.blogs.com")
                .addConverterFactory(GsonConverterFactory.create())
                .client(okHttpClient)
                .build()

        // Get an instance of blogService
        blogService = retrofit.create(BlogService::class.java)
        // Initialized repository
        blogRepository = BlogRepository(blogService)
    }

    @After @Throws fun tearDown() {
        // We're done with tests, shut it down
        mockServer.shutdown()
    }

    @Test fun testBlogsReturnsListOfBlogs() {
        val testObserver = TestObserver<List<Blog>>()
        val path = "/blogs"

        // Mock a response with status 200 and sample JSON output
        val mockReponse = MockReponse()
                            .setResponseCode(200)
                            .setBody(getJson("json/blog/blogs.json"))
        // Enqueue request
        mockServer.enqueue(mockResponse)

        // Call the API
        blogRepository.blogs().subscribe(testObserver)
        testObserver.awaitTerminalEvent(2, TimeUnit.SECONDS)

        // No errors
        testObserver.assertNoErrors()
        // One list emitted
        testObserver.assertValueCount(1)

        // Get the request that was just made
        val request = mockServer.takeRequest()
        // Make sure we made the request to the required path
        assertEquals(path, request.path)
    }

    // Network ağ durumu testi
    @Test fun testBlogsReturnsError() {
        val testObserver = TestObserver<List<Blog>>()
        val path = "/blogs"

        // Mock a response with status 200 and sample JSON output
        val mockReponse = MockReponse()
                            .setResponseCode(500) // Simulate a 500 HTTP Code

        // Enqueue request
        mockServer.enqueue(mockResponse)

        // Call the API
        blogRepository.blogs().subscribe(testObserver)
        testObserver.awaitTerminalEvent(2, TimeUnit.SECONDS)

        // No values
        testObserver.assertNoValues()
        // One error recorded
        assertEquals(1, testObserver.errorCount())

        // Get the request that was just made
        val request = mockServer.takeRequest()
        // Make sure we made the request to the required path
        assertEquals(path, request.path)
    }

    // SocketTimeoutException
    @Test fun testBlogsReturnsError1() {
        val testObserver = TestObserver<List<Blog>>()

        val path = "/blogs"

        // Mock a response with status 200 and sample JSON output
        val mockReponse = MockReponse()
                            .setResponseCode(200)
                            .throttleBody(1024, 1, TimeUnit.SECONDS) // Simulate SocketTimeout
                            .setBody(getJson("json/blog/blogs.json"))

        // Enqueue request
        mockServer.enqueue(mockResponse)

        // Call the API
        blogRepository.blogs().subscribe(testObserver)
        testObserver.awaitTerminalEvent(2, TimeUnit.SECONDS)

        // No values
        testObserver.assertNoValues()
        // One error recorded
        assertEquals(1, testObserver.errorCount())

        // Get the request that was just made
        val request = mockServer.takeRequest()
        // Make sure we made the request to the required path
        assertEquals(path, request.path)
    }

}
```

SocketTimeoutException testi çalıştırılırken aşağıdaki düzenlemeler de yapılmalıdır:

- mockResponse.throttleBody(1024, 1, TimeUnit.SECONDS) Her saniye için sadece 1024 bayt göndermesini söylüyoruz.
- Client için okuma/yazma zaman sürelerini düzenliyoruz.

```kotlin
val okHttpClient = OkHttpClient.Builder()
        .connectTimeout(2, TimeUnit.SECONDS) // For testing purposes
        .readTimeout(2, TimeUnit.SECONDS) // For testing purposes
        .writeTimeout(2, TimeUnit.SECONDS)
        .build()
```

> getJson(path = "json/blog/blogs.json") ?

<img src="/image/mockwebserber.png" height="400px" />

## Dispatcher

Her bir response'u ayrı ayrı mocklamak yerine aşağıdaki gibi bir yapıda kullanılabilir.

```java
final Dispatcher dispatcher = new Dispatcher() {

    @Override
    public MockResponse dispatch(RecordedRequest request) throws InterruptedException {

        if (request.getPath().equals("/v1/login/auth/")){
            return new MockResponse().setResponseCode(200);
        } else if (request.getPath().equals("v1/check/version/")){
            return new MockResponse().setResponseCode(200).setBody("version=9");
        } else if (request.getPath().equals("/v1/profile/info")) {
            return new MockResponse().setResponseCode(200).setBody("{\\\"info\\\":{\\\"name\":\"Lucas Albuquerque\",\"age\":\"21\",\"gender\":\"male\"}}");
        }
        return new MockResponse().setResponseCode(404);
    }
};
server.setDispatcher(dispatcher);
```

---

# Gradle Yapılandırmaları

Unit testlerin yazımı sırasında mocklanmaya gerek olmayan nesnelerin default değerini dönmesini belirtmek için aşağıdaki şekilde düzenleme yapılmalıdır.

```bash
android {
    // ...

    testOptions {
        unitTests.returnDefaultValues = true
    }
}
```

---

> Kaynaklar

- http://www.vogella.com/tutorials/AndroidTesting/article.html
- https://medium.com/android-testing-daily/the-3-tiers-of-the-android-test-pyramid-c1211b359acd
- https://github.com/codepath/android_guides/wiki/Unit-Testing-with-Robolectric
- https://github.com/codepath/android_guides/wiki/Android-Testing-Options
- https://www.javainuse.com/java/mockSpy
- https://proandroiddev.com/robolectric-testing-with-androidjunitrunner-86292bceef25
- http://robolectric.org/blog/2018/05/09/robolectric-4-0-alpha/
- https://github.com/googlesamples/android-testing
- https://github.com/square/okhttp/blob/master/mockwebserver/