---
title: "Firebase Dynamic Links & iOS"
date: 2023-02-21T12:00:00+03:00
weight: 1
tags: ["software", "firebase", "dynamiclink"]
categories: ["firebase","deeplink","applink","dynamic-link","ios"]
author: "Me"
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "Firebase Dynamic Links & iOS"
disableHLJS: false # to disable highlight.js
disableShare: false
disableHLJS: false
hideSummary: false
searchHidden: true
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
cover:
    image: "img/ios_dynamiclinks.png"  # image path/url
    alt: "iOS Firebase Dynamic Links" # alt text
    caption: "From Midjourney" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: false # only hide on current single page
editPost:
    URL: "https://github.com/aykuttasil/software/blob/master/content/"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link
url: "firebase-ios-dynamiclink"
---

## Firebase Dynamic Links

**Firebase Dynamic Links** nedir ne neden kullanılır sorularının cevabı için [buraya](/firebase-dynamiclink-deeplink-applink) tıklayabilirsiniz.

## iOS Konfigürasyonu

- **Apple** geliştirici hesabı açmalıyız.
- **Firebase Console** da proje oluşturmalı ve **Proje Ayarlarından** iOS ile ilgili kısmı (**team id, bundle id, apple id**) doğru şekilde doldurmalıyız. 
- Uygulamamız için Apple geliştirici konsolundan **Provisioning Profile** oluşturmalı ve bu sırada **Associated Domain** yeteneğinin aktif hale getirmeliyiz.
- Uygulamamızı **XCode** ile açmalıyız.
  - **TARGETS** sayfasına gitmeliyiz.
  - **Signing & Capabilities** sayfasında iken **Team** ve **Provisioning Profile** bölümlerinin doğru yapılandırdığına emin olmalıyız.
  - **Signing & Capabilities** sayfasında iken **Add Capability** butonuna basmalı ve **Associated Domains** seçeneğini seçtikten sonra Firebase Dynamic Links sayfasında kullandığımız domaini aşağıdaki şekilde eklemeliyiz.
  
    ```txt
        applinks:customsubdomain.aykutasil.com
    ```
  - **Info** tabından **URL Type** eklemeliyiz. Eklerken **Identifier** alanına **Bundle ID** yazabiliriz ve **URL Schemes** alanına uygulamamızı bundle id sini girmeliyiz. (co.a2a.app)
  - Eğer **Firebase Dynamic Links** oluştururken **google**'ın bize sunduğu **customsubdomain.page.link** şeklinde ki linki kullanmıyor ve kendi web adresimizi kullanıyor isek **Info.plist** dosyasını aşağıdaki şekilde güncellemeliyiz.

    ```xml
        <?xml version="1.0" encoding="UTF-8"?>
        <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
        <plist version="1.0">
        <dict>
        <key>FirebaseDynamicLinksCustomDomains</key>
        <array>
            <string>https://customsubdomian.aykutasil.com</string>
        </array>

        ...other settings

        </dict>
        </plist>
    ```

- Artık uygulamamız **dynamic links** için belirlediğimiz url lere tıklandığında direkt olarak açılacak ve uygulama içerisinde istediğimiz sayfalara yönlendirme yapabiliyor olacağız.


---

## Yaşadığım sorunlar


![Custom Firebase Hosting Domain](/img/dynamiclinks_skip_preview.png)

- **Firebase Console** kullanarak **Dynamic link** üretirken **Skip the app preview page(not recommend)** seçeneğini seçmek zorunda kaldım. Bu seçenek seçili olmadığında **iOS** cihazlarda öncelikle bir preview web sayfası çıkıyor. Sonra **Open App** butonuna basmamızı bekliyor. Ve basıldığında uygulamamız açılıyor ama **FirebaseDynamicLinks** kütüphanesi ile bu linki yakalayamadım.

- **Simulator** dan denerken sorun yaşadım. **Fiziksel cihazda** denemek gerekiyor.
- **Custom domain** belirlediğimde farklı farklı davranışlar sergiledi. Google'ın default olarak vermiş olduğu **customsubdomain.page.link** şeklindeki adresi daha sorunsuz çalıştı.
- **Google Chrome** üzerinden linki yazdığımda istediğim şekilde uygulama içerisinde yönlendirme yapabildim. Fakat **Safari** den linke gitmeye çalıştığımda **App Store** a yönlendiriyor. Fakat örneğin **mail** üzerinden gönderilmiş linke uzun tıklayıp **Safari** ile aç dediğimde doğru çalışıyor.

Anladığım kadarıyla bu durumların yaşanmasının sebebi **Universel Link** ile **Firebase Dynamic Link** in çakışıyor olması. Ve bazı iOS kısıtlamaları.


> Sonuç olarak;
- **customsubdomain.page.link** şeklinde üretilen **Dynamic Link **ler daha tutarlı çalışıyor.
- **Safari** ile **Google Chrome** çalışma şekli arasında farklılıklar var
- **Dynamic Link** üretirken **Skip the app preview page(not recommend)** seçeneğinin seçili olması, linkin çalışma şeklini değiştiriyor.

---

## Kaynaklar

- <https://www.kodeco.com/21376846-firebase-dynamic-links-getting-started>
- <https://medium.com/@musmein/associated-domains-78843817f1eb>
- <https://morioh.com/p/89f560dcaabd>
- <https://firebase.google.com/docs/dynamic-links>
- <https://firebase.google.com/docs/dynamic-links/flutter/receive>
- <https://blog.devgenius.io/firebase-flutter-dynamic-links-step-by-step-guide-630402ee729b>