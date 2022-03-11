+++
author = "Aykut Asil"
autoCollapseToc = false
categories = []
comment = true
date = "2019-01-08T23:20:28+03:00"
description = "Find Server IP"
draft = false
hiddenFromHomePage = false
keywords = ["ip", "server", "find-ip"]
lastmod = "2019-01-08T23:20:28+03:00"
postMetaInFooter = true
tags = ["ip"]
title = "Server Ip"
toc = true
url = "server-ip"

+++

## Problem

Domain'i barındıran server'ın IP'sini bulmak istiyoruz.

## Çözüm

## Linux

- Terminali aç
- `host www.aykutasil.com` şeklinde istediğin domain'i yaz
- Server IP:

```text
www.aykutasil.com has address 104.24.114.254
www.aykutasil.com has address 104.24.115.254
www.aykutasil.com has IPv6 address 2606:4700:30::6818:73fe
www.aykutasil.com has IPv6 address 2606:4700:30::6818:72fe
```

## Windows

- Komut penceresini aç. (**cmd.exe**)
- `nslookup www.aykutasil.com` şeklinde istediğin domain'i yaz
- Server IP:

```text
Name:    www.aykutasil.com
Addresses:  2606:4700:30::6818:73fe
      2606:4700:30::6818:72fe
      104.24.115.254
      104.24.114.254
```
