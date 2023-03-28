---
title: "iOS Jailbreak ve Android Rooting"
date: 2023-03-28T13:32:12+03:00
weight: 1
# aliases: ["/first"]
tags: ["software","android", "hack", "ios", "jailbreak", "reverse-engineering"]
categories: ["software","security"]
author: "Me"
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "iOS Jailbreak ve Android Rooting"
canonicalURL: "https://canonical.url/to/page"
disableHLJS: true # to disable highlightjs
disableShare: false
disableHLJS: false
hideSummary: false
searchHidden: true
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
cover:
    image: "<img/flutter-firebase-dynamiclinks.png>" # image path/url
    alt: "aykutasil.com" # alt text
    caption: "<text>" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page
editPost:
    URL: "https://github.com/aykuttasil/software/blob/master/content/"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link
url: "ios-jailbreak-android-rooting"
---

## iOS Jailbreak

Cihazımızdaki iOS sürümüne uygun olan Jailbreak uygulamasını (`.ipa` dosyasını) bulmamız gerekiyor. Sonrasında bu dosyayı indirip cihazımıza yüklememiz gerekiyor. Bunun için [harici bir uygulama](http://www.cydiaimpactor.com/) kullanmalıyız.

Cihazımızda Jailbreak uygulamasını açarak işlemi başlatmamız gerekiyor. Bu işlem 10-15 dakika kadar sürebilir. Jailbreak işlemi tamamlandıktan sonra [Cydia](https://www.cydiafree.com/) otomatik olarak yüklenecektir. Cydia, App Store dışında uygulama indirmemizi sağlar.

### IPA Dosyasını iOS'a Yüklemek

- [http://www.cydiaimpactor.com/](http://www.cydiaimpactor.com/) adresinden ilgili dosyayı indirin.
- Not: **Developer Account** gereklidir.
- USB kablosuyla cihazı bilgisayara bağlayın.
- Uygulamayı açtıktan sonra bağlı olan cihaz görünecektir.
- Apple Developer Account bilgilerinizi girmeniz gerekiyor. Email kısmına developer account emailinizi girin. Parola için [https://appleid.apple.com/account/manage](https://appleid.apple.com/account/manage) adresinden **App-Specific Passwords** sayfasına girin ve bu uygulama için parola oluşturun.

### Cydia'dan İndirilmesi Gereken Uygulamalar

- Search kısmından **openSSH** ve **Cycript** uygulamalarını indirebilirsiniz.
- **openSSH** ile kablosuz ağ üzerinden cihaz ile iletişim kurabilirsiniz.

### openSSH ile Kablosuz İletişim Kurmak

- Cihazın IP adresini öğrenin. Wi-Fi ayarları > IP Address alanından ulaşabilirsiniz.
- Bilgisayarınızda terminal'i açın.
- Aşağıdaki komutu çalıştırarak bağlantı kurabilirsiniz. Cihazın IP adresini girin.
    
    ```
    ssh root@192.168...
    ```
    
- Şifre olarak genellikle **alpine** kullanılır. Eğer olmazsa Jailbreak için kullandığınız uygulamanın web sitesinden ilgili bilgilere erişebilirsiniz.
- Giriş yaptıktan sonra şifrenizi değiştirmek için komut satırına **passwd** yazarak yeni şifre belirleyebilirsiniz.

## Android Rooting

iOS Jailbreak gibi bir süreç yoktur. Ancak istediğimiz APK dosyasını [https://apkpure.com/](https://apkpure.com/) gibi sitelerden indirip doğrudan emülatör üzerine sürükleyerek yükleyebiliriz.

Ayrıca, admin yetkilerine sahip olmak, gelmeyen güncellemeleri yüklemek gibi amaçlarla rooting yapmak isteyebilirsiniz. Ancak bu işlem garantiyi devre dışı bırakır.

[https://www.tenorshare.com/android-root/top-6-rooting-apps-to-root-android-without-pc.html](https://www.tenorshare.com/android-root/top-6-rooting-apps-to-root-android-without-pc.html) adresinden herhangi birini indirerek deneyebilirsiniz. Root butonuna basmanız yeterlidir.