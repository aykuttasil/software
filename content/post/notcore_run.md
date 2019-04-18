---
title: ".Net Core Run"
date: 2019-04-15T22:08:14+03:00
lastmod: 2019-04-15T22:08:14+03:00
draft: false
keywords: ["netcore",".net",",netcore","aspnet","aspnetcore"]
description: ""
tags: ["netcore","aspnetcore","csharp","webapi","api"]
categories: ["netcore","csharp","aspnetcore","webapi"]
author: "Aykut Asil"
comment: true
toc: true
autoCollapseToc: false
postMetaInFooter: true
hiddenFromHomePage: false
---

## ASP.NET Core Run Method

**ASP.NET Core** ile birlikte gelen **middleware** konsepti sayesinde **request-response** süreçleri arasına girerek kendi mantıksal devremizi yazmamız çok kolay hale gelmiştir.

<script src="https://gist.github.com/aykuttasil/1c8ba890f705d3c982b9381a3792800b.js"></script>

Yukarıda ki gibi **Startup.cs** sınıfı içerisindeki **Configure** fonksiyonuna `app.Run()` diyerek **middleware**'ımızı ekliyoruz. Peki tam olarak **Run** metodu nasıl çalışıyor?


**app.Run()** kendine gelen request'i bir sonraki **middleware**'a iletmez. Yani yukarı

## Kaynak

- <https://www.tutorialsteacher.com/core/aspnet-core-middleware>