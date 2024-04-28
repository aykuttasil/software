---
title: "macOS'ta Zamanı Akıllıca Kullanın: launchd ile Otomatik İşlemler"
date: 2023-09-16T13:50:33+03:00
weight: 1
# aliases: ["/first"]
tags: [ "macOS", "Otomatik Görev Yönetimi", "launchd", "Daemon", "Agent", "Sistem Yöneticisi", "plist Dosyası"]
categories: ["mac"]
author: "Me"
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "macOS'ta launchd, otomatik işlemleri düzenlemek için güçlü bir araçtır. Bu kılavuzda, ajanlar ve daemonlar oluşturarak sistem başlangıcında veya kullanıcı oturumu başladığında çalışacak işlemleri yapılandırmayı öğreneceksiniz."
canonicalURL: "https://canonical.url/to/page"
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


## macOS'ta launchd: Otomatik Görev Yönetimi

macOS işletim sistemi, arkada çalışan görevleri düzenlemek ve otomatikleştirmek için çeşitli araçlar sunar. Bu araçlardan biri de **launchd**'dır. **"launchd"**, macOS'ta otomatik olarak başlatılan işlemleri yöneten bir sistem yöneticisidir.

## launchd Nedir?

**launchd**, macOS ve diğer BSD türevi işletim sistemlerinde kullanılan bir sistem yöneticisidir. Sistem başlangıcında çalışan süreçleri başlatır, sürekli çalışan hizmetleri denetler ve gerektiğinde yeniden başlatır. Bu, otomatik görevlerin yönetimini kolaylaştırır.

### Temel Kavramlar

**Agent ve Daemonlar:** `launchd`, iki tür süreci yönetir: **ajanlar (agents)** ve **daemonlar**. 

- **Ajanlar**, kullanıcı oturumunu başlatırken çalışır ve kullanıcı bağlantısı kapandığında durur. 
- **Daemonlar** ise sistem başladığında çalışır ve kapanana kadar aktif kalır.

**Property List (plist) Dosyaları:** launchd konfigürasyonu, XML tabanlı plist dosyaları aracılığıyla yapılır. Bu dosyalar, hangi programın çalıştırılacağı, nasıl çalıştırılacağı ve ne zaman çalıştırılacağı gibi bilgileri içerir.

## Kullanımı

### Ajan Oluşturma:

Aşağıdaki gibi bir plist dosyası oluşturarak bir ajan tanımlayabilirsiniz:


```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
  <dict>
    <key>Label</key>
    <string>com.example.myagent</string>
    <key>ProgramArguments</key>
    <array>
      <string>/usr/bin/python3</string>
      <string>/path/to/your/script.py</string>
    </array>
    <key>RunAtLoad</key>
    <true/>
  </dict>
</plist>
```

### Daemon Oluşturma

Daemon için bir plist dosyası oluşturmak, ajan oluşturmaya benzer. Sadece "Label" alanının farklı olması gerekebilir.


> **launchd**, macOS işletim sisteminde otomatik işlemleri yönetmek için güçlü bir araçtır. Ajanlar ve daemonlar aracılığıyla kullanıcı ve sistem düzeyinde görevlerinizi düzenleyebilir ve otomatikleştirebilirsiniz.

**Not:** Özellikle plist dosyalarının doğru şekilde yapılandırılması önemlidir. Hatalı yapılandırma, beklenmeyen sonuçlara yol açabilir.