+++
coverImage = "https://c8.staticflickr.com/8/7421/9339731831_9ba94f287c_k.jpg"
title = "RxJava scan() Kullanımı"
# url = "rxjava-scan"
thumbnailImage = ""
thumbnailImagePosition = "top"
date = "2017-01-11T18:49:21+03:00"
metaAlignment = "center"
tags = [
  "software","android","java","rxjava"
]
keywords = [
  "yazilim",
  "sofware","android","java","rxjava"
]
autoThumbnailImage = false
desciption = ""
categories = [
  "yazilim",
  "android",
  "rxjava"
]

+++

# RxJava scan() Kullanımı

RxJava da scan() kullanımı iki şekilde olur.

- 1. İlk değer ataması yapılarak

```java
private void scan() {
    Observable.just(3, 5, 7, 9)
            .scan(10,(val1, val2) -> {
                //
                Log.i(TAG, "val1: " + val1.toString());
                Log.i(TAG, "val2: " + val2.toString());
                return val1 + val2;
            }).subscribeOn(Schedulers.io())
            .subscribe(success -> {
                Log.i(TAG, "Sonuc:" + success.toString());
            });
}
```

- 2. İlk değer ataması yapılmadan

```java
private void scan() {
    Observable.just(3, 5, 7, 9)
            .scan((val1, val2) -> {
                //
                Log.i(TAG, "val1: " + val1.toString());
                Log.i(TAG, "val2: " + val2.toString());
                return val1 + val2;
            }).subscribeOn(Schedulers.io())
            .subscribe(success -> {
                Log.i(TAG, "Sonuc:" + success.toString());
            });
}
```
 

**Açıklama:**

Observable nesnesinin içindeki her bir item a fonksiyon uygulamamızı sağlar. Ve her iterasyon sonrası sonucu yayınlar yani subscribe.OnSuccess metoduna yollar. Başka bir deyişle scan() kod bloğu içeresinde tanımlamış olduğumuz fonksiyonu item lara sırayla uygulayarak onNext() fonksiyonunu çağırır.

 
```java
private void scan() {
    Observable.just(3, 5, 7, 9)
            .scan(10,(val1, val2) -> {
                //
                Log.i(TAG, "val1: " + val1.toString());
                Log.i(TAG, "val2: " + val2.toString());
                return val1 + val2;
            }).subscribeOn(Schedulers.io())
            .subscribe(success -> {
                Log.i(TAG, "Sonuc:" + success.toString());
            });
}
```

Yukarıda ki örnek üzerinden gidecek olursak;

```
I/MainActivity: Sonuc:10
I/MainActivity: val1: 10
I/MainActivity: val2: 3
I/MainActivity: Sonuc:13
I/MainActivity: val1: 13
I/MainActivity: val2: 5
I/MainActivity: Sonuc:18
I/MainActivity: val1: 18
I/MainActivity: val2: 7
I/MainActivity: Sonuc:25
```

böyle bir çıktı ile karşılaşırız.

İlk değer ataması yaptığımız için (10) ilk olarak onSuccess in içine bu değer düşer. Daha sonra Observable nesnemizin ilk item ı olan 3 değeri ile toplama işlemi yapılır (10 + 3) ve sonuç onSuccess e  yollanır (13). Daha sonra 13 ile ikinci item olan 5 toplanır (13 + 5) ve sonuç (18) onSuccess e aktarılır. Tüm item lar işlem görünceye kadar devam eder.

 

RxJava candır 😉

