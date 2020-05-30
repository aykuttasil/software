---
author: "Aykut Asil"
date: 2020-05-30T16:30:05+03:00
lastmod: 2020-05-30T16:30:05+03:00
title: "Telefonun Kamerasini Web Kamerası Gibi Kullanmak"
url: "telefonun-kamerasini_web-kamerasi-gibi-kullanmak"
weight: 10
keywords: ["aykuttasil","genel","telefon-kamerasi","phone-camera"]
description: "Telefonun Kamerasini Web Kamerası Gibi Kullanmak"
tags: ["genel"]
categories: ["genel"]
---

> Gerekli Uygulamaların Kurulması

Öncelikle [şu](https://iriun.com/) siteye giderek hem bilgisayarımız hem de telefonumuz için **IRION** uygulamasını indirip kuruyoruz.

Daha sonra bilgisayarımız ile telefonumuzun aynı ağa bağlı olduğundan emin oluyoruz.

Bu kadar. :)

Artık telefonun kamerasını sanki bilgisayarımıza takılı bir **webcam** gibi kullanabilir ve kaliteli görüntü aktarabiliriz.

---

**Zoom** uygulamasında kamera kaynağı olarak telefon kamerası **(Virtual Cam)** görünmüyor. Çözüm için **terminal**'ı açarak aşağıdaki komut satırını yazmamız yeterli olacaktır. Yazmadan önce **Zoom** uygulamasını kapattığımızdan emin olalım.

```bash
codesign --remove-signature /Applications/zoom.us.app
```






### Kaynaklar

- <https://fatihhayrioglu.com/telefonu-bilgisayarin-web-kamerasi-olarak-tanimlamak/>
- <https://iriun.com/>
