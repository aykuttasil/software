---
title: "AMP"
date: 2018-10-10T11:57:46+03:00
lastmod: 2018-10-10T11:57:46+03:00
draft: false
keywords: ["amp"]
description: ""
tags: ["amp"]
categories: ["amp"]
author: "Aykut Asil"
comment: true
toc: true
autoCollapseToc: false
postMetaInFooter: true
hiddenFromHomePage: false
---

> Not: Bu yazı öğrenme sürecinde hazırlanmış olup kısa kısa notlar içerir.

## Nedir bu AMP

## Kurallar

- `<!doctype html>` ile başlamalı
- `<html amp>` şeklinde düzenlenmeli 
- `<head>` tagı içerisine ilk sırada `<meta charset="utf-8">` şeklinde meta tagı eklenmeli
- `<head>` tagı içerisine mümkün olduğunda erken sırada `<script async src="https://cdn.ampproject.org/v0.js"></script>` script tagı eklenmeli
- `<head>` tagı içerisine sayfanın AMP olmayan halinin linkini ya da sadece AMP li hali var ise kendi url i `<link rel="canonical" href="$SOME_URL">` şeklinde belirtilmeli
- `<head>` tagı içerisine `<meta name="viewport" content="width=device-width,minimum-scale=1">` meta tagı eklenmeli
- AMP js dosyası yüklenene kadar mevcut içeriği gizlemek için `<style amp-boilerplate>body{-webkit-animation:-amp-start 8s steps(1,end) 0s 1 normal both;-moz-animation:-amp-start 8s steps(1,end) 0s 1 normal both;-ms-animation:-amp-start 8s steps(1,end) 0s 1 normal both;animation:-amp-start 8s steps(1,end) 0s 1 normal both}@-webkit-keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}@-moz-keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}@-ms-keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}@-o-keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}@keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}</style><noscript><style amp-boilerplate>body{-webkit-animation:none;-moz-animation:none;-ms-animation:none;animation:none}</style></noscript>` tagı eklenmeli

---

- Bir çok HTMl tagı direk olarak AMP HTML içerisinde kullanılabilir fakat bazılarının (**img** gibi) AMP için özel versiyonları bulunmaktadır. Bazı html tagları ise AMP HTML için yasaklanmıştır.(iframe gibi)

- Web sayfasının doğru bir şekilde dağıtılması ve bulunması için aşağıdaki düzenlemeler yapılmalıdır.
    1. Normal html sayfasına AMP li sayfanın bilgisi, AMP li sayfaya da normal html sayfasının bilgisi eklenmelidir.
        - Normal sayfaya `<link rel="amphtml" href="https://www.example.com/url/to/amp/document.html">`
        - AMP li sayfaya `<link rel="canonical" href="https://www.example.com/url/to/full/document.html">` eklenmelidir.
        - Eğer sayfanın sadece AMP versiyonu var ise kendi linki canonical olarak belirtilmelidir.
            - `<link rel="canonical" href="https://www.example.com/url/to/amp/document.html">`

---

## AMP-HTML

- `<amp-img>`

Örnek: `<amp-img src="welcome.jpg" alt="Welcome" height="400" width="800"></amp-img>`

---

## Style

```html
<style amp-custom>
  /* any custom style goes here */
  body {
    background-color: white;
  }
  amp-img {
    background-color: gray;
    border: 1px solid black;
  }
</style>
```

Normak html sayfalarındaki gibi amp html sayfalarında da mevcut taglara style uygulanabilir. Tabi bazı kısıtlamalar ve düzenlemeler yapmak gerekmekdtedir.

- Yukarıda görüldüğü üzere `<style amp-custom>` şeklinde düzenleme yapılmalıdır.
- `<head>` tagı içerisine eklenmelidir
- Harici bir dosyadan çağırılmamalıdır.
- Tek bir `<style>` tagı bulunabilir ve inline styles olmalıdır.

---

## Validate

AMP url inin sonuna `#development=1` eklenerek (`http://localhost:8000/released.amp.html#development=1`) valide olup olmadığı kontrol edilebilir. Chrome DevTools açılarak console a yazılan loglar takip edilebilir.

---

## Update or Delete Cache

Sayfamızda bir değişiklik oldu ve bunu google a söylememiz gerekiyor. Hiç bir etkileşimde bulunmaz isek [max-age](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control) meta tagına göre güncelleme yapılır.

Bir değişiklik olduğunu ve google ın mevut cache i güncellemesi gerektiğini söylememiz için

- https://example-com.<cache.updateCacheApiDomainSuffix>/update-cache/c/s/example.com/article?amp_action=flush&amp_ts=<ts_val>&amp_url_signature=<sig_val> şeklinde bir istekte bulunmamız gerekiyor

adresine, linkteki alanları uygun şekilde doldurduktan sonra request atmamız gerekiyor. Nasıl doldurmamız gerektiği ile ilgili açıklamaları aşağıdaki linkte bulabilirsiniz.

- <https://developers.google.com/amp/cache/update-cache>

Google AMP Cache mekanizması otomatik güncelleme için de bir yapıya sahiptir. Kullanıcı daha önce cache lenmiş bir AMP dökümanına istekte bulunduğunda Google AMP cache mekanizöası taze datayı sağlamak için update request inde bulunur. Aynı dökümana istekte bulunan birkaç kullanıcıdan sonra taze içerik gösterilmeye başlanır.

---

## Kaynaklar

- <https://www.ampproject.org/docs/>
