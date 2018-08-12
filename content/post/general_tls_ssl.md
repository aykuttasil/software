---
title: "TLS vs SSL"
date: 2018-08-12T23:12:07+03:00
lastmod: 2018-08-12T23:12:07+03:00
draft: false
keywords: ["tls","ssl","encrypted","security"]
description: "What is TLS/SSL"
tags: ["ssl","tls","encrypted-data","network"]
categories: ["genel","network"]
author: "Aykut Asil"
comment: true
toc: true
autoCollapseToc: false
postMetaInFooter: true
hiddenFromHomePage: false
---

# TLS/SSL nedir?

**TLS (Transport Layer Securtiy)** , **SSL(Secure Sockets Layer)**'ın modernize edilmiş halidir diyebiliriz. Yani günümüzde SSL den bahsedildiğinde aslında TLS den bahsediliyor olduğunu söyleyebiliriz.

**TLS** protokolü, makinelerin web üzerindeki iletişimini kimlik doğrulama ve şifreleme mekanizmaları ile güvenli hale getirem bir standarttır. Günümüz dijital dünyasında güvenliğin önemi her geçen gün arttığı için iletişim yöntemleri ve güvenliği de beraberinde değişmekte ve gelişmektedir. Bu nedenle web iletişiminin güvenli hale gelmesi zorunluluk ve ihtiyaçtır.

## TLS/SSL Gelişim Süreci

- **1995: SSL v2** ilk olarak Netscape firması tarafından yayınlandı. v1 nerede diye soracak olursak hiç yayınlanmadı diyebiliriz. Neden? > Google :) 
- **1996: SSL v3** v2 nin çeşitli açıklarının kapatılmış sürümü,
- **1999: TLS v1.0** SSL v3 ün çeşitli açıklarının kapatılmış sürümü,
- **2006: TLS v1.1** TLS v1 in çeşitli açıklarının kapatılmış sürümü,
- **2008: TLS v1.2** Günümüzde kullanılan standart
- **TLS v1.3** Gelişim sürecinde..

## Ne anlama gelir?

**TLS/SSL** web server ile web browser lar arasındaki data alışverişinin özel ve güvenli bir şekilde yapıldığı ağ protokolüdür.

Teknik olarak iki ana bölümü vardır:

1. **TLS el sıkışma katmanı**
  - Hangi şifrenin kullanılacağını (şifreleme algoritmasının türü), kimlik doğrulamayı (etki alanı adınıza ve kuruluşunuza özgü bir sertifika kullanarak) ve anahtar değişimini (sertifikadaki genel-özel anahtar çiftine göre) yönetir. El sıkışma işlemi, her iki taraf için güvenli bir ağ bağlantısı kurmak için yalnızca bir kez gerçekleştirilir.
2. **TLS kayıt katmanı** 
  - kullanıcı uygulamalarından veri alır, şifreler, uygun bir boyuta (şifre tarafından belirlendiği gibi) parçalara ayırır ve bunu ağ aktarım katmanına gönderir.

## TLS/SSL Faydaları

- Davetsiz misafirlerin web sitenizle web tarayıcıları arasındaki iletişimi kurcalamasını engelleyin. Kullanıcı giriş bilgileri, kredi kartı bilgileri gibi hassas verilerin açık bir şekilde internet üzerinde dolaşmasını engelleyin.

- Davetsiz misafirlerin sunucunuzla iletişimi pasif olarak dinlemelerini önleyin. Bu biraz zor ama büyüyen bir güvenlik tehdidi (Snowden sızıntıları tarafından da onaylandı).

## TLS/SSL Eksi Yanları

- TLS site traiğinde gecikmelere sebebiyet verir
- El şıkışma mekanizması yoğun kaynak tüketimine sebebiyet verir.
- TLS server yönetimini komplike hale getirir. (Bununla ilgili modern ve kolay çözümler üretilmiştir.)
- CPU kullanımını %2 artırır. (https://www.maxcdn.com/blog/ssl-performance-myth/)

## TLS/SSL Sertifika Tipleri Nelerdir?

- Extended Validation (EV)
  - **EV** sertifikaları, yeşil adres çubuğuna sahiptir. İnternet'te tanınan güven sembolüdür.

- Organization Validation (OV)
  - **OV** sertifikaları yeşil adres çubuğuna sahip değildir, ancak bazı tarayıcılar güven göstergesini etkinleştirir. OV sertifikası, bir işletmenin Sertifika Yetkilisi tarafından doğrulanmasını gerektirir. Kuruluşun adı, güvenini pekiştiren sertifikada listelenecektir.

- Domain Validation (DV)
  - **DV** sertifikaları endüstri standardı şifrelemeyi (diğer sertifika türleri ile aynı seviyede) sunar. Düşük (ya da ücretsiz) maliyetinin yanı sıra, **DV** sertifikasının bir başka avantajı da sadece birkaç dakika içinde verilebilmesidir. Çünkü Sertifika Yetkilisi sadece güvenmek istediğiniz alana sahip olduğunuzu doğrulamak zorundadır. Otomatik bir süreçtir.

## Kaynaklar

> Not: Bazı cümleler kaynaklardan direk Türkçe çeviri olarak eklenmiştir.

- https://www.hostingadvice.com/how-to/tls-vs-ssl/
- https://www.globalsign.com/en/blog/ssl-vs-tls-difference/
- https://letsencrypt.org/getting-started/
- https://www.thesslstore.com/blog/what-is-dns-over-tls/
