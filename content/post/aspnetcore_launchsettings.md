---
author: "Aykut Asil"
date: 2020-04-17T21:02:16+03:00
lastmod: 2020-04-17T21:02:16+03:00
title: "Asp.Net Core launchSettings.json Kullanımı"
url: "aspnetcore-launchsettings-file"
weight: 10
keywords: ["aykuttasil","software"]
description: "Asp.Net Core launchSettings.json dosyası kullanımı"
tags: ["software","aspnetcore","dotnet"]
categories: ["aspnetcore","dotnet"]
draft: false
---

> **launchSettings.json** dosyası **sadece local**'de **development** yaparken kullanılan bir dosyadır. **Visual Studio** veya **dotnet cli** ile birlikte kullanılır.

**Not: Eğer** uygulamamızı sunucuyu deploy ettikten sonra ulaşmak istediğimiz ayarlar var ise bunun belirtilmesi gereken yer **launchSettings.json** dosyası **değildir**. Bu tür ayarlar genelde **appSettings.json** dosyası içerisinde tanımlanır.

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

**FirstCoreWebApplication** uygulamamızın ismidir. Varsayılan olarak, eğer **Visual Studio** kullanarak **run** edersek `"commandName": "IISExpress"` bloğu, eğer **CLI** kullanarak yani `dotnet run` şeklinde komut yazarak **run** edersek `"commandName": "Project"` bloğu ayarları uygulanacaktır.

İhtiyacımıza göre yeni **profile** oluşturulabilir ve **Visual Studio** üzerinden **profile** seçilerek hangi ayarların uygulanmasını istediğimizi seçebiliriz.

**commandName** parametresi 3 değer alabilir:

1. IISExpress
2. IIS
3. Project

Aşağıdaki tablo ile projenin **internal web server** ve **external web server** yapılandırmasının nasıl olacağını görebilirsiniz.

<img src="/img/launchSettings_commandName_vs_aspnetcorehostingmodel.png" />

**InProcess**: IIS host the app (w3wp.exe or iisexpress.exe)

**OutOfProcess**: Kestrel host the app, IIS is a proxy to kestrel.

**commandName:Project** olarak ayarlanan **profile** bloğu ile çalıştırılan proje her zaman **kestrel** kullanır.

---

Yukarıdaki ifadelerin ne anlama geldiğini biraz daha açık şekilde belirtelim.

**Asp.Net Core** uygulamasını iki şekilde barındırabiliriz: 

- IIS
- Kestrel

Projemizi **windows** üzerinde koşturacak isek, eskiden olduğu gibi **IIS** üzerinden bunu sağlayabiliriz. Arada **kestrel** web server olmadan direkt olarak istekleri **IIS** karşılar ve projemize iletir. 

Bununla beraber bir de **OutOfProcess** hosting modeli vardır. Bu modelde istekler yine **IIS** tarafından karşılanır. Ve projemizin iç haberleşmesi için ayağa kaldırdığı **kestrel** web server'a bu istekler iletilir. Yani **IIS -> Kestrel -> Projemiz** şeklinde istekler işlenir ve geri döndürülür.

> **Windows** üzerinde koşturulan **Asp.Net Core** projelerinin arada **Kestrel** olmadan host edilmesi performans anlamında büyük fark oluşturur.

Bununla beraber projemiz **windows** üzerinde koşmayacak ise zaten **IIS** yapılandırması olmayacağı için **kestrel** üzerinden host edilir. Fakat burada projemizi sadece **kestrel** web sunucusu ile host etmekten ziyade yine **IIS**'in **reverse proxy görevini** üstlenecek olan **nginx, apache** gibi bir çok özelliği barındıran(cache vb.) web server'lar kullanılabilir. 

Yani örneğin ön tarafta **nginx** yapılandırılmış olan projemize gelen istekler **Nginx -> Kestrel -> Projemiz** şeklinde olacaktır. 

**Linux** serverlarda **nginx** kullanarak **asp.net core** uygulamasının koşturulması için [şu](https://docs.microsoft.com/tr-tr/aspnet/core/host-and-deploy/linux-nginx?view=aspnetcore-3.1) sayfayı ziyaret edebilirsiniz.

---

### Kaynaklar

- <https://dotnettutorials.net/lesson/asp-net-core-launchsettings-json-file/>
- <https://docs.microsoft.com/tr-tr/aspnet/core/host-and-deploy/linux-nginx?view=aspnetcore-3.1>
