---
title: "ASP.NET Core Filters"
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

**ASP.NET Core**'daki **filter** yapısı, **middleware** bileşenine benzer fakat bazı farklılıkları vardır. **Filter**'lar ile sadece **request-response** süreci arasına girilerek istenilen kod blokları çalıştırılabilir. **Filter** tipine göre **request-response** sürecinin hangi aşamasında çalıştırılacağı belirlenebilir. Oldukça özelleştirebilir bir yapı sunar bize **Filter**'lar.

## Filter Tipleri

![filter-pipeline](/image/filter-pipeline-2.png "Filter Pipeline")

### Authorization Filters

İlk çalışan **filter**'dır. İlgili mvc action metoduna erişmek için **user**'ın yetkisi olup olmadığını kontrol eder. Eğer koşullar sağlanıyor ise ilgili **mvc action** metodu çalışır. Yetki problemi var ise **short-circuit** diye tabir edilen kısa devre yapılarak direk olarak bir **response** mesajı döndürülür. Yani kodun geri kalanının çalışmasına izin verilmez.

### Resource Filters

Eğer ki kullanıcı **Authorization Filter**'ından geçmiş ise **filter pipeline**'nında ki diğer filter'lardan önce çalıştırılacak ilk **filter**dır. Ve aynı zamanda response yönü için **filter pipeline**'nında ki diğer tüm filter'lardan sonra çalışacak filter'dır. Yani kod bloğuna ilk giriş ve son çıkış mekanizması kurulmasını sağlar. Genellikle **short-circuit** sağlamak için kullanılır. Örnek olarak **caching** akışını bu filter ile oluşturabiliriz.

### Action Filters

İlgili **Action** metodunun hemen öncesinde ve hemen sonrasında çalışacak olan **filter** tipidir. Genellikle **Action** metoduna gelen argüman değerlerinin ve response çıktısının **manipülasyon**'u için kullanılır.

### Exception Filters

### Result Filters

---

## Filter scopes and order of execution

Filter'ların 3 farklı **scope** seçeneği vardır. **Controller, Action** ve **Global**. **Controller** scope'una sahip bir **filter**, ilgili controller'a gelen tüm isteklerde çalışacaktır. **Action** scope'una sahip bir filter sadece ilgili action'a gelen isteklerde ve **Global** scope'una sahip filter ise tüm controller ve action'lara gelen isteklerde çalışacaktır. Projenin ihtiyacına göre filter scope'ları belirlenir.

### Global Filter Scope

Tüm **controller** ve **action**'larda çalışmasını istediğimiz filter'ların scope'unu **global** olarak kayıt etmeliyiz.

Örneğin hangi controller çalışırsa çalışsın, response'a bir **header** eklemek istiyoruz. Ve bu **filter** kodunun ilgili tüm işlemler yapıldıktan sonra **response** döneceği sırada çalışmasını istiyoruz. Response dönmeden hemen önce ve hemen sonra yani tüm işlemler başarılı ise çalışmasını istediğimiz kodlar için filter tipi, **IResultFilter** olması gerekmektedir.

> Eğer kodumuzun ilgili **action**'a istek gelir gelmez çalışmasını isteseydik o zaman filter'ımızın **IActionFilter** tipinde olması gerekecekti.

Oluşturduğumuz filter'ın **global scope**'a sahip olduğunu belirtmek için aşağıdaki şekilde belirtmemiz gerekmektedir.

<script src="https://gist.github.com/aykuttasil/434979505600cda99bd4b84ccfa048f1.js"></script>

<script src="https://gist.github.com/aykuttasil/ae2f921f38f8a2222b73b27c82e12ae7.js"></script>

<script src="https://gist.github.com/aykuttasil/71b3a33f3ee562826efbc3dacf23001c.js"></script>

![resultfilter](/image/resultfiltersample.png "Global Scope Result Filter")

### Dependency injection

Filter'lar **type (typeOf())** veya **instance** alınarak eklenebilir. Eğer **instance** alınarak eklenir ise tüm isteklerde bu instance çalışacaktır. Eğer **type** şeklinde eklenir ise **type-activated** olacak ve her request sonrası filter sınıfının **constructer**'ı çağırılarak yeni bir instance'ı oluşturulacaktır. Ve bu sayede **DI** container'ından istenilen sınıflar **inject** edilebilir olacaktır. **Type** kullanılarak eklenen filter'lar `filters.Add(new TypeFilterAttribute(typeof(MyFilter)))` şeklinde container'a eklenmiş gibi davranacaktır.

Eğer oluşturduğumuz **Filter** sınıfı **DI** container'ından bir bağımlılık içeriyor ise aşağıdaki yöntemlerden birini kullanarak **controller** veya **action** metoduna filter eklenebilir:

- ServiceFilterAttribute
- ServiceFilterAttribute
- IFilterFactory implementasyonu

#### ServiceFilterAttribute

## Kaynaklar

- <https://docs.microsoft.com/en-us/aspnet/core/mvc/controllers/filters>
