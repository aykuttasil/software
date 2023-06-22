---
title: "Android AAB Dosyasını Local Olarak Nasıl Çalıştırabilirsiniz"
date: 2023-06-22T10:50:32+03:00
weight: 1
# aliases: ["/first"]
tags: ["software", "android", "aab", "apk", "bundletool", "cli", "command-line"]
categories: ["software","android","mobile"]
author: "Me"
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
# description: "Desc Text."
canonicalURL: "running-aab-file-locally"
disableHLJS: true # to disable highlightjs
disableShare: false
disableHLJS: false
hideSummary: false
searchHidden: true
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
# cover:
#     image: "<img/flutter-firebase-dynamiclinks.png>" # image path/url
#     alt: "aykutasil.com" # alt text
#     caption: "<text>" # display caption under cover
#     relative: false # when using page bundles set this to true
#     hidden: true # only hide on current single page
editPost:
    URL: "https://github.com/aykuttasil/software/blob/master/content/"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link
---

# Android AAB Dosyasını Yerel Olarak Nasıl Çalıştırabilirsiniz

Bir Android AAB dosyasını yerel olarak çalıştırmak için, aşağıdaki adımları izleyerek komut satırı arayüzünü (CLI) kullanabilirsiniz:

1. Bilgisayarınıza Android SDK komut satırı araçlarını yükleyin.
2. Yerel olarak çalıştırmak istediğiniz AAB dosyasını indirin.
3. Bir komut istemi veya terminal penceresi açın ve AAB dosyasını indirdiğiniz dizine gidin.
4. Aşağıdaki komutu çalıştırarak bir APK dosyası oluşturun:
`<path-to-aab-file>` yerine AAB dosyanızın yolunu, `<path-to-output-apks-file>` ise çıkış APK dosyası için istediğiniz yolunu değiştirin.
    
    ```
    $ bundletool build-apks --bundle=<path-to-aab-file> --output=<path-to-output-apks-file> --mode=universal
    
    ```
    
5. USB kablosu kullanarak Android cihazınızı bilgisayarınıza bağlayın.
6. "Ayarlar" > "Geliştirici seçenekleri" > "USB hata ayıklama" yolunu izleyerek cihazınızda USB hata ayıklamayı etkinleştirin.
7. Aşağıdaki komutu çalıştırarak APK dosyasını cihazınıza yükleyin:
`<path-to-output-apks-file>` yerine çıkış APK dosyanızın yolunu değiştirin.
    
    ```
    $ adb install <path-to-output-apks-file>
    
    ```
    
8. Uygulama şimdi cihazınızda yüklü ve çalışmaya hazır olmalıdır.

Not: Yükleme işlemi sırasında herhangi bir sorunla karşılaşırsanız, Android SDK komut satırı araçlarının en son sürümünü ve cihazınızın uyumlu bir Android sürümü çalıştırdığından emin olun.