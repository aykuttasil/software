---
title: "Android File System"
date: 2019-06-27T14:00:59+03:00
lastmod: 2019-06-27T14:00:59+03:00
draft: false
keywords: ["android","fileprovider","filesystem","uri","share-data","permission","security","file-system"]
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

## Internal Storage vs External Storage

### Internal Storage

- Her zaman erişilebilir.
- Sadece uygulamanın kendisi tarafından erişebilir. Buraya kaydedilen dosyalar başka bir uygulama tarafından görünmez. **USB ile bilgisayara bağlanıldığında bu dosyalara erişim sağlanamaz.**
- **Uygulama silindiğinde internal storage temizlenir.**

### External Storage

- Her zaman erişilebilir durumda olmayabilir. **External storage** cihaza dahili olarak bulunabileceği gibi USB cihazı olarak sonradan takılmış bir cihaz da olabilir. Bu nedenle erişebilir olup olmadığı kontrol edilmelidir (`getExternalStorageState()`).
- External Storage'a kayıt edilen dosyalar başka uygulamalar tarafından okunabilir ve görülebilir durumda olabilir. Oluşturma şekline göre farklılık gösterir.
  - `Environment.getExternalStorageDir()` ile belirtilen klasöre kayıt edilen dosyalar tüm uygulamalar tarafından erişilebilir ve değiştirilebilir.
  - `Context.getExternalFilesDir()` ile belirtilen klasöre kayıt edilen dosyalar yalnızca uygulamanın kendisi tarafından görülebilir ve değiştirilebilir. Fakat cihaz **usb** ile bilgisayara bağlandığında bu dosyalar **internal storage**'a kaydedilen dosyaların aksine erişilebilir olacaktır.
- `Context.getExternalFilesDir()`ile ulaşılan path'e kayıt edilen dosyalar uygulama silindiğinde otomatik olarak temizlenir.

---

- `getFilesDir()` -> /data/user/0/com.aykuttasil.myapp/files
- `getExternalFilesDir(Environment.DIRECTORY_DCIM)` -> /storage/emulated/0/Android/data/com.aykuttasil.myapp/files/DCIM
- `getExternalCacheDir()` -> /storage/emulated/0/Android/data/com.aykuttasil.myapp/cache
- `Environment.getExternalStorageDirectory()` -> /storage/emulated/0
- `Environment.getExternalStoragePublicDirectory(Environment.DIRECTORY_DCIM)` -> /storage/emulated/0/DCIM

![android_external_storage_apis](/image/android_external_storage_apis.png "android_external_storage_apis")

## Environment.getExternalStorageDirectory()

> **Android Q** ve sonrasını hedefleyen uygulamalar için bu fonksiyon **deprecated** olmuştur. [getExternalStorageDirectory()](https://developer.android.com/reference/android/os/Environment#getExternalStorageDirectory())

**External** kelimesi kafa karıştırıcı olabiliyor. Çoğu zaman için bu alan SD kartı ifade eder fakat android cihazların çok büyük bir kısmında dahili olarak **external** için ayrılmış alan bulunur.

Bir cihazda birden fazla **external** hafızayı temsil eden alan var ise bu alanlara `ContextCompat.getExternalFilesDirs(this,null);` şeklinde erişilebilir. **Android 4.4 (19)** öncesi ve sonrasında bu dizine erişim farklılık gösterdiği için **ContextCompat** sınıfı bize yardımcı olur.

---

![android_external_storage_example_result](/image/android_external_storage_example_result.png "android_external_storage_example_result")

---

## Kaynaklar

- <https://developer.android.com/training/data-storage/files.html>
- <https://developer.android.com/reference/android/os/Environment>
- <https://www.codevoila.com/post/46/android-tutorial-android-external-storage>
