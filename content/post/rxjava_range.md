+++
date = "2017-01-11T18:52:51+03:00"
title = "RxJava range() Kullanımı"
description = "RxJava range() Kullanımı"
url = "rxjava-range"

keywords = [
  "yazilim",
  "sofware","android","java","rxjava"
]
categories = [
  "yazilim",
  "android",
  "rxjava"
]
tags = [
  "software","android","java","rxjava"
]
+++

## RxJava range() Kullanımı

**Örnek kod:--

```java
private void range() {
    Observable.range(3, 5).subscribeOn(Schedulers.io())
            .subscribe(success -> {
                Log.i(TAG, "val: " + success.toString());
            });
}
```

**Açıklama:**

İlk değer 3 kabul edilerek sonraki 5 sayı için teker teker onNext() çağırılır. Yani 3 , 4, 5, 6, 7

Yukarıda ki kodun çıktısı aşağıdaki gibidir.

```console
I/MainActivity: val: 3
I/MainActivity: val: 4
I/MainActivity: val: 5
I/MainActivity: val: 6
I/MainActivity: val: 7
```

RxJava candır. 😉
