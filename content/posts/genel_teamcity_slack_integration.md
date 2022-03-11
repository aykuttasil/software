---
author: "Aykut Asil"
date: 2021-01-05T23:50:00+03:00
lastmod: 2021-01-05T23:50:00+03:00
title: "Teamcity Slack Entegrasyonu"
url: "teamcity-slack-integration"
weight: 10
keywords:
  ["aykuttasil", "software", "ci", "cd", "automation", "teamcity", "slack"]
description: "Teamcity Slack Entegrasyonu"
tags: ["software", "ci", "cd", "automation", "teamcity", "slack"]
categories: ["genel"]
---

## Senaryo

Kendi sunucunuz üzerine kurmuş olduğunuz **Teamcity**'e **Slack** entegrasyonu yaparak, **build > deploy** adımlarını **Slack** üzerinden nasıl izleriz?

## Çözüm

- Öncelikle **Slack** yapılandırmaları ile başlayalım.

https://api.slack.com/apps adresine giriyoruz ve yeni bir **App** oluşturuyoruz.

<video controls>
    <source src="/video/slack_app_step1.mov">
</video>

- **OAuth & Permissions** tabına geçerek ilgili yetkilendirmeleri yapıyoruz.

<video controls>
    <source src="/video/slack_app_step2.mov">
</video>

- **Bot**umuza, ihtiyaç duyacağı tüm izinleri verdikten sonra **Install to Workspace** diyerek yetkilendirmeyi tamamlıyoruz. Oluşan **token**ı **Teamcity** tarafında kullanıcaz.

<video controls>
    <source src="/video/slack_app_step3.mov">
</video>

- **Slack** tarafında **App**'imizi oluşturduktan sonra şimdi **Teamcity** tarafında gerekli ayarlamaları yapabiliriz.
Öncelikle **Teamcity** içerisindeki tüm projelerimizin **parent**ı olan **Root Project**'e **Slack** connection bilgisini ekliyoruz. Böylece tüm **child** projeler bu connection'ı kullanabilecek.

<video controls>
    <source src="/video/slack_app_step4.mov">
</video>

- **Slack** uygulaması üzerinden oluşturduğumuz **App**'i artık görebiliriz ve sonrasında bildirimlerin nereye düşmesini istiyorsak bunu belirtmemiz gerekiyor.

<video controls>
    <source src="/video/slack_app_step5.mov">
</video>

- Ve son olarak **Teamcity** tarafında, hangi projemizin bildirim göndermesini istiyorsak gerekli düzenlemeyi yapıyoruz.

<video controls>
    <source src="/video/slack_app_step6.mov">
</video>

## Kaynaklar

- <https://www.jetbrains.com/help/youtrack/standalone/Slack-Integration-HTML.html>
- <https://www.jetbrains.com/help/teamcity/notifications.html#Slack+Notifier>
- <https://api.slack.com/apps>
