+++
autoThumbnailImage = false
categories = ["yazilim", "android", "java"]
coverImage = "https://c8.staticflickr.com/8/7421/9339731831_9ba94f287c_k.jpg"
date = "2017-01-11T16:39:33+03:00"
desciption = ""
draft = true
keywords = ["yazilim", "sofware", "android", "java", "recyclerview"]
metaAlignment = "center"
tags = ["software", "android", "java", "recyclerview"]
thumbnailImage = ""
thumbnailImagePosition = "top"
title = "Android RecyclerView"
url = "android-recyclerview"

+++


Android **RecyclerView** yapısı **ListView** in oldukça özelleşmiş bir halidir. ListView kullanarak yaptığınız işlemleri bu yapı ile çok daha kaliteli ve kolay yapabilirsiniz. ListView kullanırken karşılaşmış olduğumuz sorunları, ( ViewHolder yapısının kullanılmasının çoğu durumda zorunlu olması , Scroll durumunda liste elemanlarının birbirine karışması gibi… ) kendi iç yapısı ile ve çalışma mantığı ile çözmüştür. ViewHolder yapısını kullanmak zorunludur 🙂

Ayrıca **RecyclerView**, **CoordinatorLayout** bileşeni ile koordine şekilde çalışır.Ek kodlamaya ihtiyaç duymaz.


ListView in özelleştirilmesi için birçok kütüphane mevcuttur.

- Drag-Drop işlemleri için https://github.com/bauerca/drag-sort-listview
- Swipe işlemleri https://github.com/timroes/EnhancedListView

Ama bu kütüphane ve benzerleri ömürlerini doldurmuşlar. Çünkü artık RecyclerView var.

Evet bu kadar tanıtımdan sonra artık örnek kodlara geçebiliriz.

---

Android CardView yapısı için oldukça güzel hazırlanmış bir kütüphane mevcut. Biz de bu kütüphane ile, RecyclerView içersinde göstermek istediğimiz liste elemanlarımızı CardView içine gömücez. Bu sayede hem daha güzel görünüm elde edicez hem de CardView bileşeninin nimetlerinden yararlanıcaz.

CardsLib Link : [CardsLib](https://github.com/gabrielemariotti/cardslib)



