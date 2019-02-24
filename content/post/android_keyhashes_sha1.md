+++
keywords = [
  "yazilim",
  "sofware",
  "sha1",
  "keyhashes",
  "bash"
]
autoThumbnailImage = false
coverImage = "https://c8.staticflickr.com/8/7421/9339731831_9ba94f287c_k.jpg"
date = "2017-01-10T23:56:35+03:00"
categories = [
  "yazilim",
  "android",
]
tags = [
  "software",
  "android",
  "java",
  "sha1",
  "keyhashes",
  "bash"
]
desciption = ""
metaAlignment = "center"
thumbnailImagePosition = "top"
thumbnailImage = ""
title = "Android Key Hashes & SHA1"

+++

# Android Key Hashes & SHA1

Windows komut satırına aşağıdaki komutu yazarak ulaşabilirsiniz.

```bash
keytool -exportcert -alias androiddebugkey -keystore %HOMEPATH%\.android\debug.keystore | openssl sha1 -binary | openssl base64
```

openssl hatası alırsanız https://code.google.com/p/openssl-for-windows/downloads/detail?name=openssl-0.9.8k_X64.zip adresindeki dosyayı indirdikten sonra çıkan dosyadaki bin klasörünü ortam değişkenlerindeki PATH kısmına eklerseniz sorun ortadan kalkacaktır.

Ortam değişkenlerine ulaşmak için : **Denetim Masası > Sistem > Gelişmiş Sİstem Ayarları** 

SHA1 ulaşmak için komut satırına aşağıdaki kodu yazmanız yeterli olacaktır.

**your_user_name** yerine kendi kullanıcı adınızı yazmayı unutmayın.

```bash
keytool -list -v -keystore c:\users\your_user_name\.android\debug.keystore -alias androiddebugkey -storepass android -keypass android
```
