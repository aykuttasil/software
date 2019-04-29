---
title: "ASP.Net Core app.Run() Middleware"
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

![middleware](/image/middleware-1.png "Asp.Net Core Middleware")

<script src="https://gist.github.com/aykuttasil/1c8ba890f705d3c982b9381a3792800b.js"></script>

Yukarıda ki gibi **Startup.cs** sınıfı içerisindeki **Configure** fonksiyonuna `app.Run()` diyerek **middleware**'ımızı ekleyebiliriz.

## Peki tam olarak `app.Run()` metodu nasıl çalışıyor?

**app.Run()** middlaware'ı ile istenilen bir kod bloğunu çalıştırabilir ve uygulamamıza kısa devre yaptırabiliriz. Yani **pipeline** akışı eğer **Run()** içerisine girerse, sonraki kod blokları çalışmayacaktır.

<script src="https://gist.github.com/aykuttasil/3e3741c99ebafe52d2b6bd4ccae00972.js"></script>

Yukarıda ki kodun çıktısı sadece `Response 1` olacak ve sonraki kod bloğu çalışmayacaktır.

## Kaynak

- <https://www.tutorialsteacher.com/core/aspnet-core-middleware>
- <http://www.nazimdemir.net/asp-net-core-mvc-middleware-kavrami/>