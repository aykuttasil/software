---
title: "Mac Bilgisayarını Belirli Bir Saatte Otomatik Açma"
date: 2023-09-16T12:01:10+03:00
weight: 1
# aliases: ["/first"]
tags: ["Mac","MacOtomatikAçma", "EnerjiTasarrufu", "Terminal", "MacKullanımı", "Otomasyon", "İpuçları", "Teknoloji"]
categories: ["software","mac"]
author: "Me"
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "Bu makalede, Mac bilgisayarınızı belirli bir saatte otomatik olarak açmanın adımlarını adım adım öğreneceksiniz. Enerji tasarrufu ayarlarından başlayarak, Terminal komutlarıyla bilgisayarınızın otomasyonunu sağlamak için gereken adımları detaylı bir şekilde açıklıyoruz."
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
---


Günümüzde bilgisayarlarımızı belirli bir saatte otomatik olarak açma ihtiyacı oldukça yaygındır. Bu, bilgisayarın belirli bir zaman diliminde çalışmaya başlaması gerektiğinde oldukça kullanışlıdır. Mac kullanıcıları için, bu işlemi Terminal aracılığıyla gerçekleştirmek oldukça basittir. Bu makalede, belirli bir saatte Mac bilgisayarını otomatik olarak açmak için kullanılan komutun nasıl çalıştığını öğreneceksiniz.

1. **Enerji Tasarrufu Ayarları**

Enerji tasarrufu ayarları, bilgisayarın uyku ve uyanma zamanlarını belirlemenizi sağlar. Bu adımı şu şekilde gerçekleştirin:
- **Apple** menüsünden **Tercihler**i seçin.
- **Enerji Tasarrufu**na tıklayın.
- **Uyku** ve **Bilgisayar Uyanmasına İzin Ver** seçeneklerini işaretleyin.

2. **Zamanlanmış Açılış**

Terminal uygulamasını açarak, komutu kullanarak belirli bir saatte bilgisayarın otomatik olarak açılmasını sağlayabilirsiniz.
Terminal açıldığında, aşağıdaki komutu yapıştırın ve Enter tuşuna basın:

bash
```
sudo pmset repeat wakeorpoweron MTWRFSU 09:00:00
```

Bu komut, bilgisayarın Pazartesi'den Pazar gününe kadar her gün saat 09:00'da otomatik olarak açılmasını sağlar.

3. **Terminal'e Yetki Verme**

```sudo pmset repeat``` komutu, yönetici **(sudo)** izinleri gerektirir. Terminal size parola soracak. Parolanızı girin ve Enter'a basın.

4. **Test Etme**

Şimdi bilgisayarınızı kapatın. Belirlediğiniz saatte bilgisayarınızın otomatik olarak açılıp açılmadığını kontrol edin.

**Sonuç**

Artık **belirli bir saatte** Mac bilgisayarınızın otomatik olarak açılmasını sağlamak için gerekli adımları tamamladınız. Bu işlem sayesinde, bilgisayarınız belirttiğiniz saatte kendiliğinden çalışmaya başlayacak ve size zaman kazandıracak.

### Kaynaklar
- <https://support.apple.com/tr-tr/guide/mac-help/mchl40376151/mac>

