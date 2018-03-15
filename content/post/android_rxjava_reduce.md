+++
desciption = ""
categories = [
  "yazilim",
  "android",
  "rxjava"
]
title = "RxJava reduce() Kullanımı"
# url = "rxjava-reduce"
autoThumbnailImage = false
thumbnailImage = ""
date = "2017-01-11T18:42:04+03:00"
thumbnailImagePosition = "top"
keywords = [
  "yazilim",
  "sofware","android","java","rxjava","reduce"
]
coverImage = "https://c8.staticflickr.com/8/7421/9339731831_9ba94f287c_k.jpg"
metaAlignment = "center"
tags = [
  "software","android","java","rxjava","reduce"
]

+++
# RxJava reduce() Kullanımı

**reduce()** fonskyionu iki şekilde çalışır;

- 1

```java
.reduce(new BiFunction<Integer, Integer, Integer>() {
    @Override
    public Integer apply(Integer val1, Integer val2) throws Exception {
        return null;
    }
})
```
- 2 

```java
.reduce(10,new BiFunction<Integer, Integer, Integer>() {
    @Override
    public Integer apply(Integer val1, Integer val2) throws Exception {
        return null;
    }
})
```
1 ile 2 nin farkı, 2 de görüldüğü üzere fonksiyona başlangıç değeri atanabilmesidir.

Aşağıdaki örneklerde daha net görebilirsiniz.

RxJava’nın reduce fonksiyonunu tanımlıcak olursak;

Observable nesnesine ait her bir item a (1 , 3, 5) fonksiyon uygulanmasını sağlar. Bunu map() gibi fonksiyonlarda sağlıyor. Ama tabi reduce bunu farklı bir şekilde yapıyor.
 

Eğer ilk değer (seed) atanmamış ise ilk değer olarak (val1) ilk item ı (1) alıyor.

Daha sonra biz her item a yapması gereken işlem olarak iki değeri toplamasını söylediğimiz için, ilk değer ve ikinci değeri toplayıp bunu bir sonraki işlem için ilk değer olarak atıyor. İkinci iterasyonda kaldığı yerden devam ederek, ilk değer olarak bir önceki işlemin sonucu ve ikinci değer olarak 2. item ı alıyor. Yine toplama işlemi yaparak bir sonraki işlem için ilk değer ataması yapıyor.

Tüm item lar ile işlem yapıncaya kadar devam ediyor ve sonuç subscribe.onSuccess in içine düşüyor.

 
```java
private void reduce() {
    Observable.just(1, 3, 5)
            .reduce((val1, val2) -> {
                Log.i(TAG, "val1: " + val1.toString());
                Log.i(TAG, "val2: " + val2.toString());
                return val1 + val2;
            }).retry()
            .subscribeOn(Schedulers.computation())
            .subscribe(success -> {
                Log.i(TAG, "Sonuc:" + success.toString());
            });
}
```

**Çıktı:**

```
I/MainActivity: val1: 1
I/MainActivity: val2: 3
I/MainActivity: val1: 4
I/MainActivity: val2: 5
I/MainActivity: Sonuc:9
``` 

 

Burda yukarıda ki işlemden farklı olarak ek bir iterasyon daha yapılıyor. Çünkü ilk değer atamasını biz kendimiz yapıyoruz. İkinci değer olarak da her item sırayla bu görevi üstleniyor. Ve toplama işlemi yapılıp, toplam sonucunu ilk değer olarak atadıktan sonra döngü devam ediyor. Ve sonuç subscribe.onSuccess e düşüyor.

 
```java
private void reduce() {
    Observable.just(1, 3, 5).reduce(10, (val1, val2) -> {
        Log.i(TAG, "val1: " + val1.toString());
        Log.i(TAG, "val2: " + val2.toString());
        return val1 + val2;
    }).retry()
            .subscribeOn(Schedulers.computation())
            .subscribe(success -> {
                Log.i(TAG, "Sonuc:" + success.toString());
            });
}
```

Çıktı:

```
I/MainActivity: val1: 10
I/MainActivity: val2: 1
I/MainActivity: val1: 11
I/MainActivity: val2: 3
I/MainActivity: val1: 14
I/MainActivity: val2: 5
I/MainActivity: Sonuc:19
``` 

RxJava candır 😉
