---
title: "Apple M1 Kali Linux Kurulumu"
date: 2023-03-08T16:33:06+03:00
weight: 1
# aliases: ["/first"]
tags: ["software","linux","apple","m1","kalilinux","utm","virtualize"]
categories: ["software","linux","m1"]
author: "Me"
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: true
description: "Apple M1 Kali Linux Kurulumu"
canonicalURL: "kalilinux"
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
url: "kalilinux-kurulumu-apple-m1"
---

##  Apple M1 işlemcili bilgisayara Kali Linux kurmak?

Ben **Virtual Box** kullanarak sanallaştırma işinde başarılı olamadım. Apple için özel olarak geliştirilen [UTM](https://mac.getutm.app/) sanallaştırma uygulamasını kullandım.

- [Kali Linux](https://www.kali.org/get-kali/#kali-installer-images) adresinden **Apple Silicon** seçerek **Installer** olanı indirmeliyiz
- **UTM** uygulamasını indirmeliyiz
- **UTM** uygulamasında + işaretine basarak **Virtualize** demeliyiz.
- Linux seçmeliyiz
- Browse diyerek ilgili ISO dosyasını seçmeliyiz
- RAM ve CPU ayarlamaları yaptıktan OK demeliyiz.
- Oluşturulan sanal makine ayarlarına girdikten sonra **Devices** bölümünden **Serial**'ı seçmeliyiz. **Burası önemli.**
- Ve sanal makineyi başlatabiliriz. 
- İki ekran açılacak. Bir tanesinde sadece cursor yanıp sönecek. Diğerinde linux kurulum ekranı gelecek.
- Ülke, bölge, kullanıcı adı vs bilgilerini girdikten sonra en son adıma gelindiğinde boot yöntemi olarak seçili olan **ISO** dosyasını silmeliyiz. 
- Daha sonra ilgili UTM penceresini kapatıp tekrar açmalıyız
- **Kali Linux** kullanıma hazır.

Arayüz olarak şu aşamada çok stabil sonuç alamadım. Ama en azından bu şekilde kurulumunu sağlayabildim.
Kaynaklar bölümündeki youtube videosu faydalı olabilir.


## Kaynaklar

- <https://www.kali.org/get-kali/>
- <https://mac.getutm.app/>
- <https://www.youtube.com/watch?v=9zdjQ9w_v_4>