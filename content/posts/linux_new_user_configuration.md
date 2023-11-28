---
title: "Linux İşletim Sisteminde Yeni Kullanıcı Oluşturma ve Sudo Yetkisi Verme"
date: 2023-11-28T11:11:56+03:00
weight: 1
# aliases: ["/first"]
tags: ["software","linux","user","sudo","adduser","usermod","su","ls","root"]
categories: ["software","linux"]
author: "Me"
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "Linux işletim sistemlerinde yeni bir kullanıcı oluşturmak ve bu kullanıcıya sudo yetkisi vermek oldukça yaygın bir ihtiyaçtır. Bu yazıda, adım adım bu işlemleri gerçekleştirmenin nasıl yapıldığını öğreneceksiniz."
# canonicalURL: "https://canonical.url/to/page"
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
url: "linux-new-user-configuration"    
---

# Linux İşletim Sisteminde Yeni Kullanıcı Oluşturma ve Sudo Yetkisi Verme

Linux işletim sistemlerinde yeni bir kullanıcı oluşturmak ve bu kullanıcıya **sudo** yetkisi vermek oldukça yaygın bir ihtiyaçtır. Bu yazıda, adım adım bu işlemleri gerçekleştirmenin nasıl yapıldığını öğreneceksiniz.

## Adım 1: Yeni Kullanıcı Oluşturma

Yeni bir kullanıcı oluşturmak için `adduser` komutunu kullanabiliriz. Örneğin, yeni kullanıcı adı "yeni_kullanici" olarak belirlenebilir:

```bash
sudo adduser yeni_kullanici
```

Bu komut, sizden yeni kullanıcı için bir şifre girmenizi ve diğer bazı bilgileri sağlamanızı isteyecektir.

## Adım 2: Sudo Yetkisi Verme
Yeni oluşturulan kullanıcıya sudo yetkisi vermek için, kullanıcıyı "sudo" grubuna ekleyebiliriz. Bu işlem için `usermod`` komutunu kullanabiliriz:

```bash
sudo usermod -aG sudo yeni_kullanici
```

Bu komut, "yeni_kullanici" adlı kullanıcıyı "sudo" grubuna ekler. Artık bu kullanıcı sudo yetkisine sahip olacaktır.

## Adım 3: Test Etme

Yeni kullanıcı ve sudo yetkisi başarıyla oluşturulduktan sonra, bu kullanıcı ile sudo yetkisini test edebiliriz. Örneğin:

```bash
su - yeni_kullanici
```

Yeni kullanıcının şifresini girdikten sonra, sudo yetkisini kullanarak bir komut çalıştırabilirsiniz:

```bash
sudo ls /root
```

Bu komut, "/root" dizinindeki dosyaları listeleyecektir. Eğer her şey yolunda gittiyse, yeni kullanıcınız sudo yetkisini başarıyla kullanabilmelidir.

Bu kadar! Artık Linux işletim sisteminizde yeni bir kullanıcı oluşturabilir ve bu kullanıcıya sudo yetkisi verebilirsiniz. Bu işlemleri takip ederek, sistem güvenliğinizi artırabilir ve ayrıcalıklı komutları kontrol altında tutabilirsiniz.