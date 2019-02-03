---
title: "Android Static Code Analysis"
date: 2019-01-24T23:55:53+03:00
lastmod: 2019-01-24T23:55:53+03:00
draft: true
keywords: ["android","static-code-analysis","static-code"]
description: "Android Static Code Analysis"
tags: ["android","static-code-analysis","static-code","ci","cd","ci/cd"]
categories: ["android","ci/cd","code-quality"]
author: "Aykut Asil"
comment: true
toc: true
autoCollapseToc: false
postMetaInFooter: true
hiddenFromHomePage: false
---

# Nedir ?

Statik kod analizi, uygulama kodlarınızın mevcut standartlara uygunluğu, uygulama kodları içerisindeki değişken isimlerinin ilk harfinin küçük olması, sınıf isimlerinin büyük harfle başlaması gibi genelde kod görünümü ile alakalı sorunların bulunmasını sağlar. Bununla birlikte bazı statik kod analiz araçları uygulama kodlarınızı mantıksal çerçevede değerlendirmesini de sağlayabilir. Veya yazılan platform/dil özelinde çok sık yapılan bazı hatalı kodlamaların farkedilmesini sağlayabilir.

Bir diğer faydası da uygulama kodlarını kaç kişi geliştiriyor olursa olsun sanki tek bir kişinin elinden çıkmış gibi düzenin korunmasını sağlar.

# Android statik kod analizi nasıl yapılır?

![Android Studio Code Inspect](/image/android_studio_code_inspect.png "Android Studio Code Inspect")

Android Studio içerisinde statik kod analizi yapmak için dahili olarak gelen bazı araçlar vardır. Bunlardan biri resimdeki gibi IDE üzerisinde **Inspect Code** diyerek çalıştırabilir. Analiz sonrası mevcut kodunuz üzerinde çeşitki önerimler, iyileştirmele vs. göreceksiniz ve bu iyileştirmeleri yapmanız uygulama kodunuzun ileriye dönük bakımı, sağlamlığı vs. üzerinde önemli etkilere sahip olacaktır.

