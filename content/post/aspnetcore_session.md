---
title: "Asp.Net Core Session"
date: 2019-11-12T10:17:17+03:00
lastmod: 2019-11-12T10:17:17+03:00
draft: false
keywords: ["aspnetcore","webapi","session"]
description: ""
tags: ["aspnetcore","webapi","session"]
categories: ["aspnetcore","web","api"]
author: "Aykut Asil"
comment: true
toc: true
autoCollapseToc: false
postMetaInFooter: true
hiddenFromHomePage: false
---

## Asp.Net Core Session Kullanımı

**Asp.Net Core 2.1** ve üstü sürüme sahip projelerinizde **Session** kullanmak için **Startup.cs** dosyanızı aşağıdaki yöntemlerden birini seçerek güncellemeniz gerekmektedir.

### Yöntem 1

<script src="https://gist.github.com/aykuttasil/204736c317535297aee8ff59f9a8dcc1.js"></script>

<script src="https://gist.github.com/aykuttasil/9583bc680c3b2189b50aab6b146a3f4f.js"></script>

### Yöntem 2

<script src="https://gist.github.com/aykuttasil/d5e344d17baad11966fed8b6cb815bb8.js"></script>

<script src="https://gist.github.com/aykuttasil/b211a5434eb22824af799ce5cce8d914.js"></script>

## Neden Asp.Net Core 2.1 ve sonrası?

2018'de uygulanması zorunlu hale gelen [Avrupa Veri Koruma Kanunu](https://eugdpr.org) ile birlikte, şirketlere kişisel verileri kullanması konusunda bazı kısıtlamalar ve uygulaması gereken bazı zorunluluklar getirilmiştir. Bu zorunluluklardan şu an için bizi ilgilendiren kısmı **cookie**lerin kullanımı. Web siteleri **cookie** kullanımı için son kullanıcının onayını almak zorundadır.

Tüm bu gelişmeler sonucunda **Microsoft .Net Core** ekibi de version **2.1 ve sonrası** için **cookie** kullanımına dair bazı özelleştirmeler yapmış.

**Session**'lar da bir anlamda **cookie** sayıldığından bu kısıtlamaya maruz kalmış durumda. Default olarak **Non-Essential** kategorisine giren **Session** kullanımı için kullanıcının rızası alınmak zorundadır. Ya da yukarıda gösterilen yöntemlerden biri kullanılarak bu durum iptal edilebilir.

## Kaynaklar

- <https://andrewlock.net/session-state-gdpr-and-non-essential-cookies>
- <https://www.microsoft.com/tr-tr/microsoft-365/growth-center/resources/5-things-to-know-about-gdpr-before-its-too-late>
- <https://eugdpr.org/>
- <https://docs.microsoft.com/en-us/aspnet/core/security/gdpr?view=aspnetcore-2.2>
