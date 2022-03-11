+++
author = "Aykut Asil"
categories = ["android"]
comment = true
date = "2019-07-20T01:04:53+03:00"
description = "ADB komutu ile cihaz IP'si nasıl öğrenilir?"
draft = false
keywords = ["android", "abd", "ip", "shell"]
lastmod = "2019-07-20T01:04:53+03:00"
tags = ["android", "abd", "ip", "shell"]
title = "ADB Komutu ile Cihaz IP'sini Öğren"
url = "deviceip-with-adb"

+++

Aşağıdaki komutu çalıştırarak **android** cihaz ip'sini öğrenebilirsiniz.

```shell
> adb shell ip -f inet addr show wlan0
```
