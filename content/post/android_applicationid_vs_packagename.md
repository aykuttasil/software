---
author: "Aykut Asil"
date: 2020-06-21T19:36:41+03:00
lastmod: 2020-06-21T19:36:41+03:00
title: "Android applicationId vs package name"
url: "android-applicationid-vs-packagename"
weight: 10
keywords: ["aykuttasil","software"]
description: "Android applicationId vs package name"
tags: ["software","android","configuration"]
categories: ["android"]
---


**Android** projesi oluşturduğunuzda aşağıdaki gibi **AndroidManifest.xml** ve **build.gradle** dosyaları oluşur. **AndroidManifest.xml** içerisinde **package** tagı ve **build.gradle** içerisinde **applicationId** niteliği bulunur. 

Peki bu ikisi arasındaki fark nedir?

**package="com.example.myapp"** ile belirttiğimiz kısım aslında projemizin klasör yapısını ifade eder. Yani **com** > **example** > **myapp** şeklinde bir tree yapısı vardır. Ve biz proje dosyalarımızı bu dizin altında oluşturmaya başlarız. Örneğin **MainActivity** dosyası oluşturduğumuzda aslında bu dosyanın yolu **com.example.myapp.MainActivity** olur. Projemiz derlenme sırasında **package** değerine bakar ve dosyaları bu adrese göre bulur. **AndroidManifest.xml** dosyasına bir `<activity>` tanımlaması yaparken `name=".MainActivity"` şeklinde belirtiriz. Aslında demek istediğimiz `com.example.myapp.Mainactivity` şeklinde olur. Ve ayrıca **R** sınıfları yine bu **package** değerine göre oluşturulur.

**applicationId**, projemizi temsil eden tekil değerdir. Bir projenin **applicationId** değeri sonradan değiştirilemez. Ama **package** değeri değiştirilebilir. Projemizi belli bir **applicationId** ile derledikten sonra **Play Store**'a deploy ettiğimizde artık bu **id**'yi değiştiremeyiz. Çünkü projemiz artık bu id ile eşleştirilmiştir. Tüm değerleri aynı kalmakla beraber(signing değeri dahil = aynı keystore) sadece applicationId değerini değiştirip tekrar **Play Store**'a yükleyecek olursak bu proje tamamen farklı bir proje olarak algılanır.

Bununla birlikte projemiz derlendiğinde, **Android Studio** package değerine bakarak dosya yollarını bulacak, işini halledecek ve sonrasında bu **package** değeri **applicationId** değeri ile değiştirilecek. Günün sonunda **package** değeri, asıl bakılan yer oluyor ama bu değer de aslında **applicationId** ile belirtiliyor.

> Although you may have a different name for the manifest package and the Gradle applicationId, the build tools copy the application ID into your APK's final manifest file at the end of the build. So if you inspect your AndroidManifest.xml file after a build, don't be surprised that the package attribute has changed. The package attribute is where Google Play Store and the Android platform actually look to identify your app; so once the build has made use of the original value (to namespace the R class and resolve manifest class names), it discards that value and replaces it with the application ID.

Ayrıntlı bilgi için [şuraya](https://developer.android.com/studio/build/application-id) bakabilirsiniz.


> **Best practice**, **package** ve **applicationId** değerlerinin aynı olmasıdır.


**build.gradle**

```gradle
android {
    defaultConfig {
        applicationId "com.example.myapp"
        minSdkVersion 15
        targetSdkVersion 24
        versionCode 1
        versionName "1.0"
    }
    ...
}
```

**AndroidManifest.xml**

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.myapp"
    android:versionCode="1"
    android:versionName="1.0" >
```


### Kaynaklar

- <https://developer.android.com/studio/build/application-id>