---
author: "Aykut Asil"
date: 2021-11-02T20:17:23+03:00
lastmod: 2021-11-02T20:17:23+03:00
title: "Android SMS User Consent API Kullanımı"
url: "android_sms_user_consent_api"
weight: 10
keywords: ["aykuttasil","software","android","sms-user-consent-api","android-sms","one-tap sms"]
description: ""
tags: ["aykuttasil","software","android","sms-user-consent-api","sms"]
categories: ["android","software"]
---

## Problem?

**Android** işletim sisteminin ilk versiyonlarında **SMS**'lere erişim oldukça kolaydı. Fakat şimdi sadece özel izne sahip uygulamalar bu izne sahip olabiliyor. Google'a bir form doldurarak ve neden SMS okuma özelliğine kesin olarak ihtiyacınız olduğunu anlatarak bu izni talep ediyorsunuz. Peki bizim amacımız tek seferlik bir SMS okumak ise yine bu süreçten geçmeli miyiz? **Hayır!**

**Google** tek seferlik **SMS okuması** yapabilmek için çeşitli API'ler çıkardı. Bu API'lere sayfanın en altında bulunan **Kaynaklar** bölümünden erişebilirsiniz.

Kısaca bu API'lerden biri süreci tamamen otomatize ederken, diğeri bazı manuel işlemler yapmamıza gerek duyuyor.

Bizim kullanacağımız **SMS User Consent API**. Yani bazı manuel süreçlere gereksinim duyan modeli inceleyeceğiz. 

---

## SMS User Consent API

SMS dinleme akışı kısaca aşağıdaki gibi olacaktır.

<img src="/img/user-consent-1.png" width="300" />
<img src="/img/user-consent-2.png" width="300" />
<img src="/img/user-consent-3.png" width="300" />

**SMS User Consent API** ile içeriği belli şartları sağlayan ve dinlemeye start verildikten sonra gelecek olan sms'in tüm içeriğini yakalayabilirsiniz.

### Nedir bu şartlar?

1. The message contains a 4-10 character alphanumeric string with at least one number. **(İçinde en az 1 rakam bulunan ve en az 4-10 karakter olan)**
2. The message was sent by a phone number that's not in the user's contacts. **(Rehber'de kayıtlı olmayan)**
3. If you specified the sender's phone number, the message was sent by that number. **(SMS'in hangi numaradan geleceğini biliyorsak, dinlemeyi bu telefon numarası özelinde yapabiliriz)**

---

### Kodlama

<script src="https://gist.github.com/aykuttasil/8c99cc63429f9434c8f27d15070d4519.js"></script>

<script src="https://gist.github.com/aykuttasil/824ab91b683910b3c141c642cd00706d.js"></script>    


## Kaynaklar
- <https://developers.google.com/identity/sms-retriever>
- <https://developers.google.com/identity/sms-retriever/user-consent/overview>
- <https://developers.google.com/identity/sms-retriever/choose-an-api>