---
author: "Aykut Asil"
date: 2020-01-17T18:13:00+03:00
lastmod: 2020-01-17T18:13:00+03:00
next: /tutorials/github-pages-blog
prev: /tutorials/automated-deployments
title: "Andoroid 10 Background Location"
url: "android-background-location"
weight: 10
keywords: ["aykuttasil","software"]
description: "Android 10 Background Location kısıtlaması nedir ve nasıl "
tags: ["software","android","mobile","location"]
categories: ["android","mobile","location"]
---

**Android 10** ile birlikte gelen [değişiklerden](https://developer.android.com/about/versions/10/privacy/changes) biri de **Location** dinleme ile alakalıdır. Uygulamamız arka plandayken **(background)** ve ön plandayken **(foreground)** konum dinleme şekilleri farklılık göstermektedir. Uygulamımız bize görünür vaziyette ise yani **foreground**'da ise her hangi bir değişiklik yapmamıza gerek yoktur. Fakat **Home** tuşuna basarak veya başka bir uygulama açarak uygulamamızı arka plana atıyorsak ve bu sırada konum dinlemesinin devam etmesini istiyorsak bazı düzenlemeler yapmamız gerekmektedir.

İki şekilde **background** konum dinlemesi yapılabilir.

1. **Foreground Service Kullanımı**

```xml
        <!-- https://developer.android.com/reference/android/R.attr.html#foregroundServiceType -->
        <service
            android:name=".services.MyLocationService"
            android:foregroundServiceType="location"/>
```

- **Manifest** dosyasına **service** tanımlaması yaparken, `android:foregroundServiceType="location"` şeklinde ekleme yapıyoruz.
- Servisimizi `ContextCompat.startForegroundService(app,serviceIntent)` şeklinde başlatıyoruz.
- Servisimizi **location** dinleceyecek şekilde yapılandırıyoruz.
- Servisimiz içerisinde uygun şekilde `startForeground()` metodunu çağırarak, kullanıcının bildirim ekranında uygulamamıza ait bir bildirim görmesini sağlıyoruz.

> **Service** kullanımı ile alakalı problem yaşıyorsanız [buradan](https://developer.android.com/guide/components/services) ayrıntılı bilgi edinebilirsiniz.

2. **ACCESS_BACKGROUND_LOCATION**

Eğer **location** dinlemek için bir **servis** yapılandırmak istemiyorsanız ve uygulamanız arka planda iken **location** dinlemeye devam etmek istiyorsanız normal **location** izinleri haricinde ek olarak `ACCESS_BACKGROUND_LOCATION` iznini de almanız gerekmektedir.

Bu izni eklediğinizde **Android 10 ve üstü** cihazlarda aşağıdaki gibi bir **dialog** görünür. Normal izinler için görünen dialog penceresinden farkı **Allow all the time** seçeneğinin olmasıdır. Bu seçenek uygulama **background**'da olsa dahi her zaman location dinlemesine izin ver demektir.

<img src="/img/request-device-location.svg" width="300" />

- İlk olarak **AndroidManifest** dosyanızı aşağıdaki gibi düzenlemelisiniz.

```xml
<manifest ...>
  <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
  <uses-permission android:name="android.permission.ACCESS_BACKGROUND_LOCATION" />
</manifest>
```

- Sonrasında uygulamanıza aşağıdaki gibi `ACCESS_BACKGROUND_LOCATION` iznini de isteyen bir akış kurmalısınız.

```kotlin
val permissionAccessCoarseLocationApproved = ActivityCompat
    .checkSelfPermission(this, permission.ACCESS_COARSE_LOCATION) ==
    PackageManager.PERMISSION_GRANTED

if (permissionAccessCoarseLocationApproved) {
   val backgroundLocationPermissionApproved = ActivityCompat
       .checkSelfPermission(this, permission.ACCESS_BACKGROUND_LOCATION) ==
       PackageManager.PERMISSION_GRANTED

   if (backgroundLocationPermissionApproved) {
       // App can access location both in the foreground and in the background.
       // Start your service that doesn't have a foreground service type
       // defined.
   } else {
       // App can only access location in the foreground. Display a dialog
       // warning the user that your app must have all-the-time access to
       // location in order to function properly. Then, request background
       // location.
       ActivityCompat.requestPermissions(this,
           arrayOf(Manifest.permission.ACCESS_BACKGROUND_LOCATION),
           your-permission-request-code
       )
   }
} else {
   // App doesn't have access to the device's location at all. Make full request
   // for permission.
   ActivityCompat.requestPermissions(this,
       arrayOf(Manifest.permission.ACCESS_COARSE_LOCATION,
               Manifest.permission.ACCESS_BACKGROUND_LOCATION),
       your-permission-request-code
   )
}
```

Uygulama arka planda iken **location** isteğinde bulunur ise **bildirim ekranında** aşağıdaki gibi bir bildirim görünecektir.

<img src="/img/location-access-reminder.svg" width="300" />

## Kaynaklar

- <https://developer.android.com/about/versions/10/privacy/changes>
- <https://developer.android.com/training/location/receive-location-updates>
