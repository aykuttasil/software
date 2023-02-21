---
title: "Firebase Dynamic Links & Android"
date: 2023-02-20T12:00:00+03:00
weight: 1
tags: ["software", "firebase", "dynamiclink"]
categories: ["firebase","deeplink","dynamic-link","android"]
author: "Me"
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: true
description: "Firebase Dynamic Links & Android"
disableHLJS: false
disableShare: false
hideSummary: false
searchHidden: true
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
cover:
    image: "img/firebase-dynamiclinks-android.png"  # image path/url
    alt: "Android Firebase Dynamic Links" # alt text
    caption: "From Midjourney" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: false # only hide on current single page
editPost:
    URL: "https://github.com/aykuttasil/software/blob/master/content/"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link
url: "firebase-android-dynamiclink"
---

<!-- +++
autoThumbnailImage = false
categories = ["yazilim"]
coverImage = "https://c8.staticflickr.com/8/7421/9339731831_9ba94f287c_k.jpg"
date = "2017-04-17T01:53:03+03:00"
desciption = ""
keywords = ["yazilim", "sofware", "firebase", "dynamiclink"]
metaAlignment = "center"
tags = ["software", "firebase", "dynamiclink"]
thumbnailImage = ""
thumbnailImagePosition = "top"
title = "Firebase Dynamic Links"
url = "firebase_dynamiclink"

+++ -->


## Firebase Dynamic Links

Öyle bir link olsun ki;

- Masaüstü bilgisayarımda linke tıkladığımda kişisel web sitem açılsın,
- Eğer mobil cihazımdan linke tıklarsan;
    * Uygulama cihazda yüklü ise uygulamam açılsın (Belirtmiş olduğum Activity vs.),
    * Uygulama cihazda yüklü değilse Google Play Store veya  App Store açılsın,
    * Uygulama cihazda yüklü olsa bile eğer belirttiğim versiyon kodundan eski bir sürüm varsa yine Google Play Store vs. açılsın,
    * ...

gibi sorulara tek bir link ile cevap verebilirsiniz.

Burada açıklayacağım kısım **Android** uygulaması üzerine olacaktır.

### Nasıl başlarım?

Öncelikle https://console.firebase.google.com/ adresinden bir proje oluşturuyoruz.

Dynamic Links tabına tıkladığımızda <https://example.page.link> benzeri bir adres görürüz. Bu adresi daha sonra oluşturacağımız dynamic linkler için kullanacağız.

### Dynamic Link oluşturma

- [Firebase Console](https://console.firebase.google.com/project/_/durablelinks/links/) kullanılarak

- [iOS Builder API](https://firebase.google.com/docs/dynamic-links/ios/create) kullanılarak

- https://app_code.app.goo.gl/?link=your_deep_link&apn=package_name[&amv=minimum_version][&al=android_link][&afl=fallback_link]

gibi bir linkte gerekli parametreleri uygun şekilde değiştirerek dynamic linkimizi oluşturabiliriz.

---

> POST https://firebasedynamiclinks.googleapis.com/v1/shortLinks?key=api_key
  Content-Type: application/json

```json
{
    "dynamicLinkInfo": {
        "domainUriPrefix": "https://example.page.link",
        "link": "https://play.google.com/store/apps/details?id=com.coderockets.referandumproject", // Masaüstü bilgisayarımızdan bu linke tıkladığımızda gidilecek adres
        "androidInfo": {
            "androidPackageName": "com.coderockets.referandumproject", // Android uygulamamızın package name i
            "androidMinPackageVersionCode": "21", // Linki alan kişide uygulamamız yüklü fakat eski versiyon yüklü ise (<21) direk olarak Google Play sayfasına yönlendirilir.
            "androidLink": "http://referandum?questionId=xyz" // Android cihazından linke tıklanıldığında bu linke yönlendirilme yapılır. Uygulamamızda gerekli ayarlamaları yaparak direk gerekli sayfaya(Activity) yönlendiririz.
        },
        "socialMetaTagInfo": {
            "socialTitle": "Question text", // Sosyal medyada bu link paylaşıldığında görünecek başlık ayarlaması
            "socialDescription": "Hemen Referandum topluluğuna katılarak cevap verebilirsin.", // Sosyal medya paylaşımında görünecek açıklama
            "socialImageLink": "Question Image Url" // Sosyal medya paylaşımında görünecek resim
        }
    },
    "suffix": {
        "option": "SHORT" // shortLink sonundaki id nin daha kısa olmasını sağlar. UNGUESSABLE yaparsak daha uzun karakterli bir unique id ile link oluşturulur. shortLink: https://c49ss.app.goo.gl/5TI3
    }
}
```

şeklinde bir REST isteği yaparak dynamic linkimizi oluşturabiliriz ve aşağıdaki gibi bir response döner.

```json
{
  "shortLink": "https://c49ss.app.goo.gl/5TI3",
  "previewLink": "https://c49ss.app.goo.gl/?link=https://play.google.com/store/apps/details?id%3Dcom.coderockets.referandumproject&al=http://referandum?questionId%3Dxyz&apn=com.coderockets.referandumproject&amv=21&st=Question+text&sd=Hemen+Referandum+toplulu%C4%9Funa+kat%C4%B1larak+cevap+verebilirsin.&si=Question+Image+Url&d=1"
}
``` 

**shortLink** ile **previewLink** aynı görevi üstlenir. Aralarında bir fark yok. 

---

### Android Uygulamamızı Yapılandıralım

Öncelikle ilgili **Firebase** kütüphanelerini eklememiz gerekiyor.

```gradle
dependencies {
    // Import the BoM for the Firebase platform
    implementation platform('com.google.firebase:firebase-bom:31.2.0')

    // Add the dependencies for the Dynamic Links and Analytics libraries
    // When using the BoM, you don't specify versions in Firebase library dependencies
    implementation 'com.google.firebase:firebase-dynamic-links-ktx'
    implementation 'com.google.firebase:firebase-analytics-ktx'
}
```

- **AndroidManifest.xml** dosyamızda ilgili Activity tanımı içerisinde aşağıdaki gibi düzenleme yapıyoruz.

```xml
        <activity
            android:name=".DetailActivity">

            <intent-filter>
                <action android:name="android.intent.action.VIEW" />
                <category android:name="android.intent.category.DEFAULT" />
                <category android:name="android.intent.category.BROWSABLE" />

                <data android:host="referandum" android:scheme="http" />
                <data android:host="referandum" android:scheme="https" />
            </intent-filter>

        </activity>
```

- **Firebase** kullanarak aşağıdaki gibi dynamic linkimize erişebiliriz.

```kotlin
Firebase.dynamicLinks
    .getDynamicLink(intent)
    .addOnSuccessListener(this) { pendingDynamicLinkData: PendingDynamicLinkData? ->
        // Get deep link from result (may be null if no link is found)
        var deepLink: Uri? = null
        if (pendingDynamicLinkData != null) {
            deepLink = pendingDynamicLinkData.link
        }

        // Handle the deep link. For example, open the linked
        // content, or apply promotional credit to the user's
        // account.
        // ...

    }
    .addOnFailureListener(this) { e -> Log.w(TAG, "getDynamicLink:onFailure", e) }
```

### Analytics

Oluşturmuş olduğumuz Dynamic Linkler ile ilgili analytics datasına Firebase Console üzerinden erişebiliriz. Eğer Google Analytics entegrasyonu var ise oradan da erişim sağlayabiliriz.

Varsayılan olarak aşağıdaki parametrelere ait datalar tutulmaktadır.

- dynamic_link_app_open
- dynamic_link_first_open
- dynamic_link_app_update

---

## Kaynaklar

- <https://firebase.google.com/docs/dynamic-links/>
- <https://firebase.google.com/docs/dynamic-links/rest>
- <https://firebase.google.com/docs/dynamic-links/android/receive>
