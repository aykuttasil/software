---
title: "Android Backstack Navigate"
date: 2018-05-04T17:38:15+03:00
lastmod: 2018-05-04T17:38:15+03:00
draft: false
keywords: ["android","navigate","backstack"]
description: "Android BackStack Control"
tags: ["android","backstack","navigate"]
categories: ["android"]
author: "Aykut Asil"

comment: true
toc: true
autoCollapseToc: false
postMetaInFooter: false
hiddenFromHomePage: false

---
# Android Navigate

## Örnek Senaryo

Kullanıcıya bir ürün ile ilgili notification yolladınız. Kullanıcı bu notification a tıkladığında direk olarak **Ürün Detay** sayfasına yönlendiriliyor. Kullanıcı sayfada işi bittiğinde geri tuşuna basıyor ve siz bu durumda kullanıcıyı uygulamanın **Anasayfa**sına yönlendirmek istiyorsunuz. Ek bir geliştirme yapmadığınız takdirde bu senaryo için geri tuşuna basıldığında uygulama kapanır. Çünkü geri gidecek ekranı yoktur. Direk olarak **Detay** sayfası açılmıştır.

---

## Çözüm

İlk olarak yapmamız gereken **Manifest.xml** dosyasında **DetailActivity** tanımlamasını yaptığımız yerde **parentActivityName** i belirtmek. Yani geri tuşuna basıldığında hangi Activity'nin açılmasını istiyorsak **parentActivityName** alanına bu activity i yazıyoruz. 

```xml
<application ... >
    ...
    <!-- The main/home activity (it has no parent activity) -->
    <activity
        android:name="com.example.myfirstapp.MainActivity" ...>
        ...
    </activity>
    <!-- A child of the main activity -->
    <activity
        android:name="com.example.myfirstapp.DetailActivity"
        android:label="@string/title_activity_display_message"
        android:parentActivityName="com.example.myfirstapp.MainActivity" >
        <!-- Android uygulamamız 4.1 ve öncesini destekliyorsa meta-data ile de belirtmek durumundayız.-->
        <meta-data
            android:name="android.support.PARENT_ACTIVITY"
            android:value="com.example.myfirstapp.MainActivity" />
    </activity>
</application>
```

Sonrasında notification gösterimi için hazırladığımız kodu aşağıdaki şekilde düzenliyoruz.

```java
// Intent for the activity to open when user selects the notification
Intent detailsIntent = new Intent(this, DetailsActivity.class);

// Use TaskStackBuilder to build the back stack and get the PendingIntent
PendingIntent pendingIntent = TaskStackBuilder.create(this)
                        // add all of DetailsActivity's parents to the stack,
                        // followed by DetailsActivity itself
                        .addNextIntentWithParentStack(detailsIntent)
                        .getPendingIntent(0, PendingIntent.FLAG_UPDATE_CURRENT);

NotificationCompat.Builder builder = new NotificationCompat.Builder(this);
builder.setContentIntent(pendingIntent);
```

---

Artık notification ile açılan DetailsActivity sayfasında kullanıcı geri tuşuna bastığında Mainactivity sayfası açılacaktır.

> Ayrıntılı Bilgi için

- https://developer.android.com/training/implementing-navigation/temporal
- https://developer.android.com/training/design-navigation/ancestral-temporal
