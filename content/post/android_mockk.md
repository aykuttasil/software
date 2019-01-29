---
title: "Android Mockk Kullanımı"
date: 2019-01-29T15:22:56+03:00
lastmod: 2019-01-29T15:22:56+03:00
draft: true
keywords: ["android","mockk","mock","test","android-test","unit-test","instrumentation-test"]
description: "Android Mock"
tags: ["android","mock","test","intrumentation-test"]
categories: ["android","test"]
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

**Final** tipindeki sınıfların veya metodların mocklama işlemi sırasında çıkan sorunun çözmenin birkaç farklı yöntemi vardır

## Çözüm 1

Kotlin **all-open** pluginini kullanmak.



