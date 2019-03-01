---
title: "Android Service"
date: 2019-02-14T13:10:18+03:00
lastmod: 2019-02-14T13:10:18+03:00
draft: false
keywords: ["android","service"]
description: ""
tags: ["android","service"]
categories: ["android","mobile"]
author: "Aykut Asil"
comment: true
toc: true
autoCollapseToc: false
postMetaInFooter: true
hiddenFromHomePage: false
---

# Android Service

Android'in temel bileşenlerinden biri olan **Service**'ler kısaca **UI(arayüz)** olmayan **Activity**'lere benzetebiliriz. Tabi ki kendine göre ek özellikleri bulunmaktadır.
**Service** tanımlaması **AndroidManifest.xml** içerisinde tanımlı olmalıdır. Aksi takdirde çalışmaz. **\<service\>** elementinin alabileceği özellikler aşağıdaki gibidir.

## Niçin kullanılır?

Temel olarak ifade etmek gerekirse **Service**'lerin kullanım amacı uzun süren ve arka planda(background) çalışan taskler oluşturmaktır. Bununla birlikte diğer uygulamalar tarafından çağırılabilen iletişim imkanı sağlar.

## Service tanımlanması

```xml
<service android:description="string resource"
         android:directBootAware=["true" | "false"]
         android:enabled=["true" | "false"]
         android:exported=["true" | "false"]
         android:icon="drawable resource"
         android:isolatedProcess=["true" | "false"]
         android:label="string resource"
         android:name="string"
         android:permission="string"
         android:process="string" >
    . . .
</service>
```

### android:description

**Service**'in amacını kullanıcıya açıklamak için kullanılır.

### android:directBootAware

Android 7 ile birlikte gelen **Direct Boot** mode özelliği ile ilgilidir. Android sistemi, güvenlik konusunda çeşitli varsayımlar dahilinde şifreleme seçenekleri sunar. Örneğin, cihaz ekran kilidi aktif iken uygulamanın erişebileceği dosyalar ile ekran kilidi pasif iken erişebileceği dosyalar farklıdır. Bu sınırlamalar ve bu sınırlamaların aşılması gereken durumlar için aşağıdaki linke bakabilirsiniz.

- <https://developer.android.com/training/articles/direct-boot>

### android:enabled

Belirtilen servisin sistem tarafından çalıştırılabilir olup olmamasını belirler. Varsayılan değeri = true.

### android:exported

Diğer uygulamaların belirtilen servisi çalıştırabilir veya etkileşim kurabilir olup olmamasını belirler. 
Eğer **true** olarak belirlenmiş ise başka uygulamalar tarafından erişilebilir demektir. **false** olarak belirtilmesi durumunda başka uygulamalar tarafından erişilemez fakat aynı **user ID** ile imzalanmış uygulamalar tarafından erişebilir demektir.

Default değeri aşağıdaki durumlara göre belirlenir.

- Eğer servis tanımı içerisinde **intent-filter** belirlenmiş ise default olarak **true** olacaktır.
- Diğer durumlarda default değer **false** olacaktır.

### android:icon

Servisi temsil eden icon belirlemek için kullanılır. Eğer bu özelliğe bir atama yapılmaz ise **application** elementinde belirlenen **icon** özelliği default olarak alınır.

### android:isolatedProcess

Bu özelliğin **true** olarak belirlenmesi, ilgili servisin diğer uygulama processlerinden tamamen farklı izole bir process'de çalışacak olması anlamına gelir.

### android:label

Kullanıcı tarafından gözükecek olan service adını belirlemek için kullanılır. Eğer atama yapılmaz ise **application** elementi içerisindeki **label** özelliği değeri default olarak atanır.

### android:name

Service class name'i bu etiket altında belirlenir. Örneğin: com.example.project.MyService

### android:permission

Service çağırıldığında uygulama tarafından bu etiket altın belirtilen izinlerin sağlanmış olması gerekmektedir. Aksi takdirde ilgili service çalışmayacaktır.
Örneğin, service içerisinde location API'si kullanılıyor ise `android:permission="android.permission.ACCESS_FINE_LOCATION"` şeklinde tanımlama yapılması ilgili servisi koruma altına alacak yani ilgili iznin sağlanması dahilinde çalışacağını garanti etmiş olacaktır.

### android:process

Normalde uygulamanın tüm bileşenleri, **uygulama.package.name** adında tek bir process'de çalışır. Fakat componentler bu default özelliği ezerek kendi process'inde koşabilir ve bu sayede uygulama işlemlerinin birden fazla process'de çalışmasını sağlayabilir. 

Bu özelliğe atama yapılır iken `android:process=":MyServiceProcess"` şeklinde **:** ile başlayan bir ifade girilecek olursa, yeni oluşturulacak process sadece kendi uygulamamız tafafından kullanılabilir olacaktır.

Fakat `android:process="myServiceProcess"` şeklinde yani **:** olmadan ve küçük harfle başlayan bir değer girilmesi durumunda, yeni oluşturulacak process başka uygulamalar tarafından da kullanılabilir olacaktır. Bu sayede kaynak kullanımı azaltılmış olup ilgili bileşenlerin aynı process'i paylaşmaları sağlabilir.

# Kaynaklar

- <https://developer.android.com/guide/topics/manifest/service-element>