---
title: "Android Dagger"
date: 2018-10-12T11:48:08+03:00
lastmod: 2018-10-12T11:48:08+03:00
draft: false
keywords: ["dagger","di","dependency-injection","android"]
description: "Android Dagger Kullanımı"
tags: ["dagger","di","dependency-injection","android"]
categories: ["dependency-injection","android"]
author: "Aykut Asil"
comment: true
toc: true
autoCollapseToc: false
postMetaInFooter: true
hiddenFromHomePage: false
---

# @BindsInstance

Bağımlılıklarınızı oluşturma esnasında belirtmeniz gereken bir değişkenininiz var ve bu değişkeni diğer module leriniz içerisinde kullanıcaksınız. Aşağıdaki gibi `@BindsInstance` kullanarak bu bağımlılığınızı dependency graph içerisine ekleyerek diğer modüller içerisinde kullanımını sağlayabilirsiniz.

```java
@Component(modules = AppModule.class)
interface AppComponent {
  App app();

  @Component.Builder
  interface Builder {
    @BindsInstance Builder apiUrl(@ApiUrl String apiUrl);
    AppComponent build();
  }
}
```

Component build edilirken `apiUrl` değeri bind edilir.

```java
 App app = DaggerAppComponent
      .builder()
      .apiUrl("http://....")
      .build()
```

---
