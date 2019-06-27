---
title: "Android FileProvider"
date: 2019-06-27T14:00:59+03:00
lastmod: 2019-06-27T14:00:59+03:00
draft: false
keywords: ["android","fileprovider","filesystem","uri","share-data","permission","security"]
description: "Android FileProvider"
tags: ["android","fileprovider","filesystem","uri","share-data","permission","security"]
categories: ["android","security"]
author: "Aykut Asil"
comment: true
toc: true
autoCollapseToc: false
postMetaInFooter: true
hiddenFromHomePage: false
---

## Android FileProvider

### Internal Storage vs External Storage

> Internal Storage

- Her zaman erişilebilir.
- Sadece uygulamanın kendisi tarafından erişebilir. Buraya kaydedilen dosyalar başka bir uygulama tarafından görünmez. USB ile bilgisayara bağlanıldığında bu dosyalara erişim sağlanamaz.
- Uygulama silindiğinde internal storage temizlenir.

> External Storage

- Her zaman erişilebilir durumda olmayabilir. External storage cihaza dahili olarak bulunabileceği gibi USB cihazı olarak sonradan takılmış bir cihaz da olabilir. Bu nedenle erişebilir olup olmadığı kontrol edilmelidir (`getExternalStorageState()`).
- External Storage'a kayıt edilen dosyalar başka uygulamalar tarafından okunabilir ve görülebilir durumda olabilir. Oluşturma şekline göre farklılık gösterebilir.
- `Context.getExternalFilesDir()`ile ulaşılan path'e kayıt edilen dosyalar uygulama silindiğinde otomatik olarak temizlenir.

---

- `getFilesDir()` -> /data/user/0/com.aykuttasil.myapp/files
- `getExternalFilesDir(Environment.DIRECTORY_DCIM)` -> /storage/emulated/0/Android/data/com.aykuttasil.myapp/files/DCIM
- `getExternalCacheDir()` -> /storage/emulated/0/Android/data/com.aykuttasil.myapp/cache
- `Environment.getExternalStorageDirectory()` -> /storage/emulated/0
- `Environment.getExternalStoragePublicDirectory(Environment.DIRECTORY_DCIM)` -> /storage/emulated/0/DCIM

## Environment.getExternalStorageDirectory()

> **Android Q** ve sonrasını hedefleyen uygulamalar için bu fonksiyon **deprecated** olmuştur.

> - <https://developer.android.com/reference/android/os/Environment#getExternalStorageDirectory()>

**External** kelimesi kafa karıştırıcı olabiliyor. Çoğu zaman için bu alan SD kartı ifade eder fakat android cihazların çok büyük bir kısmında dahili olarak **external** için ayrılmış alan bulunur. 
Bir cihazda birden fazla **external** hafızayı temsil eden alan var ise bu alanlara

## Kaynaklar

- <https://developer.android.com/training/data-storage/files.html>
