---
title: "DNS over TLS Nedir?"
date: 2018-08-13T00:50:25+03:00
lastmod: 2018-08-13T00:50:25+03:00
draft: false
keywords: ["dns","tls","ssl","dns-over-tls"]
description: "DNS over TLC nedir?"
tags: ["dns","tls","ssl","dns-over-tls"]
categories: ["network"]
author: "Aykut Asil"
comment: true
toc: true
autoCollapseToc: false
postMetaInFooter: true
hiddenFromHomePage: false
---

# DNS over TLS Nedir?

Kısaca açıklamak gerekirse, **İnternet Servis Sağlayıcılarının**, hangi siteye erişmek istediğimizi görmesini engelleme yöntemidir.

## TLS/SSL protokolü zaten bunu yapmıyor mu?

TLS/SSL protokolüne sahip siteler üzerinden yapılan veri alış verişleri şifreli yapılır fakat hala hangi siteye erişmek istediğimiz açık/şifresiz bir şekilde bellidir ve internet servis sağlayıcıları bunu görebilir.

Bunu engellemek için DNS over TLS kullanılır.

## Özet

- TLS üzerinden DNS, tüm DNS isteklerinin güvenli bir şekilde yapılmasını sağlayan bir protokoldür.
- Bu uygulama, İnternet Servis Sağlayıcılarının hangi web sitelerine erişmeye çalıştığınızı görmesini engeller.
- DNS'i TLS üzerinden kullanmak için DNS servisinizin bunu desteklemesi gerekir.

## Kaynak

- <https://www.thesslstore.com/blog/what-is-dns-over-tls/>
- <https://developers.cloudflare.com/1.1.1.1/dns-over-tls/>