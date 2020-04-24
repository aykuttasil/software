---
author: "Aykut Asil"
date: 2020-04-17T21:02:16+03:00
lastmod: 2020-04-17T21:02:16+03:00
title: "AspNetCore launchSettings.json File"
url: "aspnetcore-launchsettings-file"
weight: 10
keywords: ["aykuttasil","software"]
description: "Asp.Net Core launchSettings.json dosyası kullanımı"
tags: ["software","aspnetcore","dotnet"]
categories: ["aspnetcore","dotnet"]
draft: true
---

> **launchSettings.json** dosyası **sadece local**'de **development** yaparken kullanılan bir dosyadır. **Visual Studio** veya **dotnet cli** ile birlikte kullanılır.

**Not: Eğer** uygulamamızı sunucuyu deploy ettikten sonra ulaşmak istediğimiz ayarlar var ise bunun belirtilmesi gereken yer **launchSettings.json** dosyası değildir. Bu tür ayarlar genelde **appSettings.json** dosyası içerisinde tanımlanır.

<img src="/img/launchSettings.png" />

---

### launchSettings.json dosyası

```json
{
  "iisSettings": {
    "windowsAuthentication": false, 
    "anonymousAuthentication": true, 
    "iisExpress": {
      "applicationUrl": "http://localhost:59119",
      "sslPort": 0
    }
  },
  "profiles": {
    "IIS Express": {
      "commandName": "IISExpress",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    },
    "FirstCoreWebApplication": {
      "commandName": "Project",
      "launchBrowser": true,
      "applicationUrl": "http://localhost:5000,https://localhost:5000",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
```

Yukarıdaki dosyayı inceleyecek olursak **profiles** tagı altında **IIS Express** ve **FirstCoreWebApplication** tagları olduğunu görürüz. Ve bu tagların **commandName** değerleri **IISExpress** ve **Project** olarak tanımlanmıştır. 

**FirstCoreWebApplication** uygulamamızın ismidir. Eğer **Visual Studio** kullanarak **run** edersek `"commandName": "IISExpress"` bloğu, eğer **CLI** kullanarak yani `dotnet run` şeklinde komut yazarak **run** edersek `"commandName": "Project"` bloğu ayarları uygulanacaktır.

**commandName** parametresi 3 değer alabilir:

1. IISExpress
2. IIS
3. Project


InProcess : IIS host the app (w3wp.exe or iisexpress.exe)

OutOfProcess: Kenstrel host the app, IIS is a proxy to kestrel.

### Kaynaklar

- <https://dotnettutorials.net/lesson/asp-net-core-launchsettings-json-file/>
