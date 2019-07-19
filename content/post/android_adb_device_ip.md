---
title: "ADB Komutu ile Cihaz IP'sini Öğren"
date: 2019-07-20T01:04:53+03:00
lastmod: 2019-07-20T01:04:53+03:00
draft: false
keywords: ["android","abd","ip","shell"]
description: ""
tags: ["android","abd","ip","shell"]
categories: ["android"]
author: "Aykut Asil"
comment: true
toc: true
autoCollapseToc: false
postMetaInFooter: true
hiddenFromHomePage: false
---

Aşağıdaki komutu çalıştırarak **android** cihaz ip'sini öğrenebilirsiniz.

```shell
> adb shell ip -f inet addr show wlan0
```
