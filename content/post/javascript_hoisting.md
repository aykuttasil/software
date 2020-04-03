---
author: "Aykut Asil"
date: 2020-04-03T15:13:51+03:00
lastmod: 2020-04-03T15:13:51+03:00
title: "Javascript Hoisting"
url: "javascript_hoisting"
weight: 10
keywords: ["aykuttasil","software","javascript","hoisting"]
description: "Javascript nasıl yorumlanır ve Hoisting nedir?"
tags: ["software","javascript","hoisting"]
categories: ["javascript"]
---

**Javascript** dilinin nasıl yorumlandığı ile alakalı bir durum olan **Hoisting** kavramını bilmekte fayda var diye düşünüyorum. Örneğin aşağıdaki gibi bir kod yazdığımızda;

```javascript
console.log(username);
var username = "aykuttasil";
```

alacağımız çıktı `undefined` şeklinde olur. Normal şartlarda beklediğimiz sonuç hata fırlatmasıdır. Çünkü `username` değişkeni tanımlanmadan önce yazdırılmaya çalışılmıştır. Ama beklediğimiz şekilde olmaz ve ekrana `undefined` yazılır.

## Peki Neden?

Yukarıdaki gibi bir kod bloğu çalıştırılmadan önce **javacsript** tarafından **hoisting** işlemi uygulanır ve tüm **var** değişkenleri kod bloğunun(**scope**) en üstüne taşınır ve default olarak **undefined** atanır. Yani aslında kod aşağıdaki hale dönüşür:

```javascript
var username = undefined;
console.log(username);
username = "aykuttasil";
```

Bu nedenle ekrana `undefined` ,yani **username** değişkeni diye bir değişken var ama henüz tanımlama yapılmamış, yazılır.

## Peki ne yapmak gerekir?

Javascript yazarken çok dikkatli oluyoruz :D Ve bu sorunu çözmek için **ES2015(ES6)** ile birlikte gelen **let** ve **const** tipinde değişken oluşturmaya özen gösteriyoruz.

<iframe height="400px" width="100%" src="https://repl.it/@aykuttasil/javascript-hoisting?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>

---

Bir de şuraya **let,const ve var** değişken farkları ve hosting işlevini özetleyen maddeleri bırakalım.

- var declarations are globally scoped or function scoped while let and const are block scoped.
- var variables can be updated and re-declared within its scope; let variables can be updated but not re-declared; const variables can neither be updated nor re-declared.
- They are all hoisted to the top of their scope. But while var variables are initialised with undefined, let and const variables are not initialised.
- While var and let can be declared without being initialised, const must be initialised during declaration.

## Kaynaklar

- <https://www.freecodecamp.org/news/var-let-and-const-whats-the-difference/>
