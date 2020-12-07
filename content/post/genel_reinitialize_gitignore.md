---
author: "Aykut Asil"
date: 2020-12-07T14:32:26+03:00
lastmod: 2020-12-07T14:32:26+03:00
title: "Düzeltilmiş .gitignore ile Projenin Yeniden Yapılandırılması"
url: "reinitialize-project-with-gitignore"
weight: 10
keywords: ["aykuttasil","software","git","gitignore","git-cache"]
description: "Zaten halihazırda **.gitignore** dosyası eklenmiş şekilde bir **git** repunuz var. Sonradan **.gitignore** dosyasınızı yapılandırdınız fakat yine de bu dosyalar **track(izlenmeye)** devam ediyor."
tags: ["software","git","gitignore","git-cache"]
categories: ["genel"]
---

## Problem

Zaten halihazırda **.gitignore** dosyası eklenmiş şekilde bir **git** repunuz var. Sonradan **.gitignore** dosyasınızı yapılandırdınız fakat yine de bu dosyalar **track(izlenmeye)** devam ediyor. 

## Sebep

Proje dosyalarını ilk **commit** yaptığınızda mevcut **.gitignore** yapılandırmanız baz alınarak dosyalar cachelenir. Ve sonrasında bu dosyalarda yapılan tüm değişikler izlenmeye devam eder. Sonradan **.gitignore** dosyanızda değişiklik yapsanız dahi cachelenmiş dosyalarda değişiklik olmaz.

## Çözüm

**git** cache'ini temizleyerek izlenenen tüm dosyaları yeni **.gitignore** yapılandırmanıza göre tekrardan cachelenmesini sağlayabilirsiniz.

```bash
git rm -r --cached .
git add .
git commit -am "new gitignore conf"
```

**Not:** Mevcut **.git** history'nizde bir değişiklik olmayacaktır.


