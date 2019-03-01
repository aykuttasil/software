---
title: "Android Kotlin Test - Final Type Problem"
date: 2019-01-29T15:38:29+03:00
lastmod: 2019-01-29T15:38:29+03:00
draft: false
keywords: ["android","kotlin","final-type","mock","kotlin","test"]
description: ""
tags: ["android","kotlin","test"]
categories: ["android","kotlin","final-type","mock","kotlin","test"]
author: "Aykut Asil"
comment: true
toc: true
autoCollapseToc: false
postMetaInFooter: true
hiddenFromHomePage: false
---

# Sorun

**Kotlin** dili ile geliştirilen **Android** projelerinin test yazımı sırasında sınıfların veya metodların **mock**lanması, **Java** ile geliştirilen projelere göre bazı farklılıklar göstermektedir. Bu farklılığın sebeplerinden biri **Kotlin** ile oluşturulan sınıf veya metodların default olarak **final** olarak işaretlenmiş olmasıdır. Ve **final** tipindeki sınıfların **mock**lanması bazı sorunlar çıkarmaktadır.

# Çözüm

**Final** tipindeki sınıfların veya metodların mocklama işlemi sırasında çıkan sorunu çözmenin birkaç farklı yöntemi vardır.

## Çözüm 1 (open ClassName)

Test edeceğimiz sınıf ve metodun başına **open** ifadesini ekleyerek **extend** edilebilir hale getirmek. Böylece **mock** kütüphaneleri kotlin final problemine yakalanmadan mocklama işlemini başarıyla gerçekleştirebilecekler.

## Çözüm 2 (all-open)

Kotlin **all-open** pluginini kullanmak. Ki bu **plugin**'de aslında sınıfların, metodların başında **open** ifadesi ekliyor.

## Çözüm 3 (Mockk)

[Mockk](https://mockk.io/) kütüphanesini kullanmak.

> **Not:** Eğer test sınıfımızı **Instrumentation Test** bağlamında yani **androidTest** package'ı altında yazıyorsak testi çalıştıracağımız emulator veya cihazın android versiyonuna göre bazı farklılıklar vardır.

> - Android P ve sonrası (>= 28) için birşey yapmanıza gerek yok
> - Android P öncesi (< 28) için **all-open** pluginini aktifleştirmeniz gerekmektedir.
> - <https://mockk.io/ANDROID>

**Not:** Kişisel olarak **Mockk** kullanmanızı tavsiye ederim. 

## Çözüm 4 (Mockito)

[Mockito](https://site.mockito.org/) kütüphanesini kullanmak.

### **Unit Test için**

- Mockito kütüphanesini eklerken **mockito-core** yerine **mockito-inline** bağımlılığını kullanmalısınız.

```bash
// testImplementation "org.mockito:mockito-core:2.23.16"
testImplementation "org.mockito:mockito-inline:2.23.16"
```

Aslında **mockito-inline** bağımlılığının yaptığı **resources/mockito-extensions/org.mockito.plugins.MockMaker** dosyasına **mock-maker-inline** satırını eklemek.

- <https://github.com/mockito/mockito/blob/release%2F2.x/subprojects/inline/src/main/resources/mockito-extensions/org.mockito.plugins.MockMaker>

> **mockito-inline** ile **java byte code** manipilasyonu sağlanır. Bu sayede **final** tanımlanmış sınıf veya metodlar mocklanabilir hale gelir. Yazdığımız **Unit Test**ler **JVM**'de çalışacağı için bu yöntem işe yarar.
> Fakat **Android runtime**'ı **Dalvik Byte Code** kullanır. Ve bu yüzden **mockito-inline** yöntemi işe yaramaz.

### **Instrumentation Test için**

- `androidTestImplementation "org.mockito:mockito-android:2.23.16"` bağımlılığını eklemelisiniz.
- [Buradaki](https://github.com/mockito/mockito/issues/1082#issuecomment-301646307) yöntemlerden birini izleyebilirsiniz. Aslında burada bahsedilen yöntem de **all-open** pluginini aktifleştirmekten ibaret.
- <https://medium.com/androiddevelopers/mock-final-and-static-methods-on-android-devices-b383da1363ad>

> **Not**: **dexmaker-mockito**'nun çalışması için **Android P** ve sonrası gerekmektedir. Aşağıdaki tabloda görebilirsiniz.

Eğer projenizi **kotlin** ile geliştirmiş ve test için **Mockito** kullanmaya karar vermişseniz [Mockito Kotlin](https://github.com/nhaarman/mockito-kotlin) kütüphanesini kullanmanızda fayda var. Bu kütüphaneyi kullanarak daha hızlı ve kolay şekilde Mockito'yu kullanabilir ve bazı kotlin özelliklerinden dolayı alınacabilecek hataları (Kotlin NullPointerException hatası) engelleyebilirsiniz.

---

![Mockito Benchmark](/image/mockito_benchmark.png "Mockito Benchmark")

# Kaynaklar

- <https://medium.com/androiddevelopers/mock-final-and-static-methods-on-android-devices-b383da1363ad>