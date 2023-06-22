---
title: "Android Reverse Engineering"
date: 2023-03-10T00:15:50+03:00
weight: 1
# aliases: ["/first"]
tags: ["software","android","reverse-engineering","root","jailbreak","tersine mühendislik","güvenlik","safety"]
categories: ["software","safety"]
author: "Me"
showToc: true
TocOpen: false
draft: true
hidemeta: false
comments: false
description: "Desc Text."
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
url: android-reverse-engineering
---

## Mac Kurulumu

[Apktool](https://ibotpeaches.github.io/Apktool/install/) ve [jadx](https://github.com/skylot/jadx) araçlarını kurmamız gerekiyor.

**Apktool** için kurulum adımları biraz daha farklı. **MacOS** için kurulum bölümünde gerekli adımlar yazıyor.

**apktool.jar** ve **apktool** dosyasını `/usr/local/bin` e taşımak için **root** yetkisine ihtiyaç var.

Bunun için komut satırına `sudo su` yazmanız ve şifrenizi girmeniz gerekmektedir.

Daha sonra yine komut satırından `mv apktool.jar /usr/local/bin` ve `mv apktool /usr/local/bin` demeniz yeterli olacaktır.

Son olarak `chmod +x apktool` ve  `chmod +x apktool.jar` komutlarını çalıştırarak **apktool** aracının kurulumunu tamamlıyoruz. 


## Araçlar
- [apktool](https://github.com/iBotPeaches/Apktool)
- [jadx](https://github.com/skylot/jadx)


## Kaynaklar
- <https://www.udemy.com/course/etik-hacker-olmak-mobil-uygulamalar-ve-telefonlar>
- <https://ibotpeaches.github.io/Apktool/install/>
