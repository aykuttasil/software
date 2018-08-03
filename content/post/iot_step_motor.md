---
title: "Step Motor Nedir?"
date: 2018-08-03T22:21:48+03:00
lastmod: 2018-08-03T22:21:48+03:00
draft: false
keywords: ["motor","step-motor","iot"]
description: "Step Motor Nedir?"
tags: ["arduino","iot","motor"]
categories: ["iot","motor"]
author: "Aykut Asil"

# You can also close(false) or open(true) something for this content.
# P.S. comment can only be closed
comment: true
toc: true
autoCollapseToc: false
postMetaInFooter: true
hiddenFromHomePage: false

---

> **Step motoru** bir diğer adıyla adım motorları; hızlı,  Doğrusal ve kademeli hareket istenilen uygulamalarda kullanılan fırçasız  DC elektrik motorudur.

Kademeli motorlar düşük devirlerde yüksek tork ve düşük titreşim ve hassasiyetle çalışmaktadırlar. Step  motorları rotor denilen sabit bir manyetik dönen şafta ve stator olarak adlandırılan motoru çevreleyen sabit kısımdaki elektromıknatıslardan oluşur. step motorlar, yarim adim modunda çalistiklarin da hassasiyetleri daha da artar.

## STEP MOTORUN SÜRÜCÜ İLE ÇALIŞTIRILMASI

Step motorlar mikro denetleyici, arduino ve step motor sürücü yardımı yöntemi ile sürülmektedir. Sürücü ve mikro denetleyici encoder ve plc’ den motora sırasıyla kare dalga ( darbe sinyali ) gönderilerek step motorun adım adım ( derece ) dönmesini sağlar.

Step motorlarda adımlar derece olarak adlandırılır. Motorun adım derecesi motor’un imalat tasarıma bağlıdır. Derece açıları motorun teknik dökümanlarından öğrenebilir. Adım motorlarının derece açıları 0,6’dan 62,5 dereceye kadar imal edilirler. Örneğin: 400 adımlık bir motor tam dönüşte  (360 derece ) 400 adım yapar. Motorun her adım açısı 360/400 = 0,9 derecedir.  Adım motoruna uygulanan kare dalga ( pals ) sinyal adeti ile doğru orantıda çıkış sinyali üreterek motor milini istediğimiz derecede döndürebiliriz. Motorun hız ayarı motora birim zamanda ( sn ) gönderilen sinyallere ( pals ) göre değişmektedir.

Birden fazla step motorun bulunduğu uygulamalarda adım motorları bir birleri ile senkron çalışabilmektedir. Step motora sürekli dc voltaj uygulandığı zaman motor sürekli dönmektedir. Adım motorları geri besleme ( encoder ) ve geri besleme olmadan da kolaylıkla kontrol edilebilmektedir.

### STEP MOTOR KULLANIM ALANLARI

Gıda paketleme, Tıbbi cihazlar, Cnc tezgahları, yazıcılar, Robotlar, Hareket konumlandırma, Fabrika otomasyonlarında

## STEP MOTOR KULLANIM AVANTAJLARI

- Elde edilebilecek güç ve moment sınırlıdır.
- Sayısal olarak kontrol edilebildiklerinden bilgisayar veya mikro işlemci gibi elemanlarla kontrol edilebilir.
- Hız, giriş darbelerinin frekansı ile orantılı olduğu için geniş bir dönme hızı aralığı gerçekleştirilebilir.
- Başlama, durma ve tersine mükemmel tepki gösterirler.