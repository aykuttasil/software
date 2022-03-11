+++
author = "Aykut Asil"
autoCollapseToc = false
categories = ["dependency-injection", "android"]
comment = true
date = "2018-10-12T11:48:08+03:00"
description = "Android Dagger Kullanımı"
draft = false
hiddenFromHomePage = false
keywords = ["dagger", "di", "dependency-injection", "android"]
lastmod = "2018-10-12T11:48:08+03:00"
postMetaInFooter = true
tags = ["dagger", "di", "dependency-injection", "android"]
title = "Android Dagger"
toc = true
url = "android-dagger"

+++

## @BindsInstance

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
