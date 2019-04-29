---
title: "ASP.Net Core Filters"
date: 2019-04-29T10:05:05+03:00
lastmod: 2019-04-29T10:05:05+03:00
draft: false
keywords: ["aspnetcore","netcore","filters"]
description: "ASP.Net Core Filters"
tags: ["aspnetcore","netcore","filters"]
categories: ["aspnetcore","netcore"]
author: "Aykut Asil"
comment: true
toc: true
autoCollapseToc: false
postMetaInFooter: true
hiddenFromHomePage: false
---

> Not: Bu makale hazırlanırken kullanılan **.net core** versiyonu: **2.2**

**ASP.Net Core**'daki **filter** yapısı, **middleware** bileşenine benzer fakat bazı farklılıkları vardır. **Filter**'lar ile sadece **request-response** süreci arasına girilerek istenilen kod blokları çalıştırılabilir. **Filter** tipine göre **request-response** sürecinin hangi aşamasında çalıştırılacağı belirlenebilir. Oldukça özelleştirebilir bir yapı sunar bize **Filter**'lar.

## Filter Tipleri

![filter-pipeline](/image/filter-pipeline-2.png "Filter Pipeline")

### Authorization Filters

İlk çalışan **filter**'dır. İlgili mvc action metoduna erişmek için **user**'ın yetkisi olup olmadığını kontrol eder. Eğer koşullar sağlanıyor ise ilgili **mvc action** metodu çalışır. Yetki problemi var ise **short-circuit** diye tabir edilen kısa devre yapılarak direk olarak bir **response** mesajı döndürülür. Yani kodun geri kalanının çalışmasına izin verilmez.

### Resource Filters

Eğer ki kullanıcı **Authorization Filter**'ından geçmiş ise **filter pipeline**'nında ki diğer filter'lardan önce çalıştıralacak ilk filterdır. Ve aynı zamanda response yönü için **filter pipeline**'nında ki diğer tüm filter'lardan sonra çalışacak filter'dır. Yani kod bloğuna ilk giriş ve son çıkış mekanizması kurulmasını sağlar. Genellikle **short-circuit** mekanizmanısı sağlamak için kullanılır. Örnek olarak **caching** akışını bu filter ile oluşturabiliriz.

### Action Filters

İlgili **Action** metodunun hemen öncesinde ve hemen sonrasında çalışacak olan **filter** tipidir. Genellikle **Action** metoduna gelen argüman değerlerinin ve response çıktısının **manipülasyon**'u için kullanılır.

## Kaynaklar

- <https://docs.microsoft.com/en-us/aspnet/core/mvc/controllers/filters>