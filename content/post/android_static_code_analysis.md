+++
author = "Aykut Asil"
autoCollapseToc = false
categories = ["android", "ci/cd", "code-quality"]
comment = true
date = "2019-01-24T23:55:53+03:00"
description = "Android Static Code Analysis"
draft = true
hiddenFromHomePage = false
keywords = ["android", "static-code-analysis", "static-code"]
lastmod = "2019-01-24T23:55:53+03:00"
postMetaInFooter = true
tags = ["android", "static-code-analysis", "static-code", "ci", "cd", "ci/cd"]
title = "Android Static Code Analysis"
toc = true
url = "android-static-code-analysis"

+++

# Nedir ?

Statik kod analizi, uygulama kodlarınızın mevcut standartlara uygunluğu, uygulama kodları içerisindeki değişken isimlerinin ilk harfinin küçük olması, sınıf isimlerinin büyük harfle başlaması gibi genelde kod görünümü ile alakalı sorunların bulunmasını sağlar. Bununla birlikte bazı statik kod analiz araçları uygulama kodlarınızı mantıksal çerçevede değerlendirmesini de sağlayabilir. Veya yazılan platform/dil özelinde çok sık yapılan bazı hatalı kodlamaların farkedilmesini sağlayabilir.

Bir diğer faydası da uygulama kodlarını kaç kişi geliştiriyor olursa olsun sanki tek bir kişinin elinden çıkmış gibi düzenin korunmasını sağlar.

# Android statik kod analizi nasıl yapılır?

![Android Studio Code Inspect](/img/android_studio_code_inspect.png "Android Studio Code Inspect")

Android Studio içerisinde statik kod analizi yapmak için dahili olarak gelen bazı araçlar vardır. Bunlardan biri resimdeki gibi IDE üzerisinde **Inspect Code** diyerek çalıştırabilir. Analiz sonrası mevcut kodunuz üzerinde çeşitki önerimler, iyileştirmele vs. göreceksiniz ve bu iyileştirmeleri yapmanız uygulama kodunuzun ileriye dönük bakımı, sağlamlığı vs. üzerinde önemli etkilere sahip olacaktır.

