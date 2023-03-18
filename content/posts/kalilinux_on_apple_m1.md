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

# Apple M1 işlemcili bilgisayara Kali Linux kurmak mümkün mü?

Apple M1 işlemcili bilgisayarlarda Kali Linux kurulumunun birkaç farklı yolu var. Benim kullandığım yöntem ise sanallaştırma işlemi için **Virtual Box** yerine özel olarak Apple için geliştirilen [UTM](https://mac.getutm.app/) sanallaştırma uygulamasını kullanmak.

Virtual Box indirme linki: [https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads)

**Yöntem şu şekildedir:**

1. İlk olarak [Kali Linux](https://www.kali.org/get-kali/#kali-installer-images) adresinden **Apple Silicon** seçeneğini belirleyerek **Installer** olanı indiriyoruz.
2. Daha sonra, **[UTM](https://mac.getutm.app/)** uygulamasını indiriyoruz.
3. **UTM** uygulamasında "+" işaretine tıklayarak "Virtualize" seçeneğine tıklıyoruz.
4. Ardından, Linux işletim sistemini seçiyoruz.
5. İlgili ISO dosyasını seçmek için "Browse" seçeneğine tıklıyoruz.
6. RAM ve CPU ayarlarını yaptıktan sonra "OK" butonuna tıklıyoruz.
7. Oluşturulan sanal makine ayarlarına girdikten sonra **Devices** bölümünden **Serial**'ı seçiyoruz. Bu adım oldukça önemlidir.
8. Ve son olarak, sanal makineyi başlatıyoruz.
9. İki ekran açılacak. Bir tanesinde sadece cursor yanıp sönecek. Diğerinde Kali Linux kurulum ekranı gelecektir.
10. Ülke, bölge, kullanıcı adı gibi bilgileri girdikten sonra en son adıma gelindiğinde boot yöntemi olarak seçili olan **ISO** dosyasını silmeliyiz.
11. Daha sonra ilgili UTM penceresini kapatıp tekrar açmalıyız.
12. Artık **Kali Linux** kullanıma hazırdır.

Arayüz olarak ben henüz çok stabil bir sonuç alamadım. Ancak en azından bu yöntemle Kali Linux kurulumunu başarabildim. Bununla birlikte, daha detaylı bir anlatım için [https://www.kali.org/get-kali/](https://www.kali.org/get-kali/) adresine göz atabilirsiniz. Ayrıca, [https://www.youtube.com/watch?v=9zdjQ9w_v_4](https://www.youtube.com/watch?v=9zdjQ9w_v_4) adresindeki YouTube videosu da faydalı olabilir.

## Kaynaklar

- [https://www.kali.org/get-kali/](https://www.kali.org/get-kali/)
- [https://mac.getutm.app/](https://mac.getutm.app/)
- [https://www.youtube.com/watch?v=9zdjQ9w_v_4](https://www.youtube.com/watch?v=9zdjQ9w_v_4)