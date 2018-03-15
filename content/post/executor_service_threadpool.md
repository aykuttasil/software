+++
autoThumbnailImage = false
desciption = ""
tags = [
  "software","Executors","newCachedThreadPool","newFixedThreadPool","newSingleThreadExecutor","thread","multithread"
]
keywords = [
  "yazilim",
  "sofware","Executors","newCachedThreadPool","newFixedThreadPool","newSingleThreadExecutor","thread","multithread"
]
date = "2017-01-11T18:55:08+03:00"
metaAlignment = "center"
categories = [
  "yazilim",
  "java"
]
thumbnailImage = ""
coverImage = "https://c8.staticflickr.com/8/7421/9339731831_9ba94f287c_k.jpg"
title = "Executor newCachedThreadPool() newFixedThreadPool(n) newSingleThreadExecutor()"
#url = "java-executor"
thumbnailImagePosition = "top"

+++

# Executor newCachedThreadPool() newFixedThreadPool(n) newSingleThreadExecutor()

**Thread** kullanımı, Java ile multi-threading yazılım geliştiriyorsanız mutlaka ama mutlaka bilmeniz gereken konulardan biridir.

 
## ThreadPool

Gün geldi çattı ve uygulamanız ilk kurulduğu amacın evrimleşmesi sonucu bambaşka bir hale büründü 🙂

Yani birçok uygulamada gidişat bu yöndedir ve olması gerekende budur.

Aynı anda 1000 den fazla kullanıcıya cevap verme gereksinimiz var artık. Sadece patron kullanmıyor sonuçta. Ya da patron öyle bir uygulama istemişki ağır işler gerektiriyor. Uzun süreler gerektiren işlemler sonucunda akış tamamlanıyor vs.

Sözün kısası;

Uygulamamız içerisinde ki akışları ayrı parçalara bölerek daha hızlı çalışmasını ve aynı anda birden fazla iş yapmamızı sağlayan yapıdır bu Thread ler.
 

Peki en basitinden nasıl kullanırız bu yapıyı ? Tabi hödük gibi değil, olması gerektiği gibi.. 😉

## Executors

Executors sınıfı içerisinde statik olarak tanımlanmış ve kolayca thread pool oluşturabileceğimiz yapılar mevcuttur.


### EXECUTORS.NEWCACHEDTHREADPOOL()

`
Creates a thread pool that creates new threads as needed, but will reuse previously constructed threads when they are available. These pools will typically improve the performance of programs that execute many short-lived asynchronous tasks. Calls to execute will reuse previously constructed threads if available. If no existing thread is available, a new thread will be created and added to the pool. Threads that have not been used for sixty seconds are terminated and removed from the cache. Thus, a pool that remains idle for long enough will not consume any resources. Note that pools with similar properties but different details (for example, timeout parameters) may be created using ThreadPoolExecutor constructors.
`

Resmi kaynaklardan yukarıdaki tanımlama yapılmış.

Yani özetlicek olursak;

- Kısa süren işlemlerimizde bu yapıyı kullanmalıyız
- Bu yapı bizim için otomatik olarak bir thread oluşturur ve ihtiyacı oldukça yeni bir thread oluşturulmasını sağlar
- Oluşturulan thread ler boş kaldığında thread i kapatır. (60 saniye)
- Oluşturulan thread le işi bitti ve kapatılması için belli bir süre gerekiyor (60 saniye). Ama bu süre tamamlanmadan yeni bir iş geldi. Bu işi yapmak için yeni bir thread açmaz. Eğer önceden oluşturulan thread ler arasında uygun durumda olan varsa yeni gelen işi orada çalıştırır.

Peki süper. Herşey çok iyi. Olması gerektiği gibi. Peki aynı anda 10000 kişi işlem yapmaya çalıştı. Ve ağır işlemler olduğu için sürekli yeni thread oluşturmak zorunda kaldı. 10000 tane yeni thread. Thread oluşturmak da belli bir süre ve emek gerektiriyor unutma. Peki CPU ? … vs. vs.
İşte bu yüzden kısa süreli ve hafif işler için bu yapıyı kullanıyoruz. 😉
 

Aga bana örnek söyle.

Tamam sakin ol..  :*

```java
private void executorServiceNewCachedThreadPool() {
    
    ExecutorService executorService = Executors.newCachedThreadPool();

    for (int a = 0; a < 40; a++) {
        executorService.submit(new Runnable() {
            @Override
            public void run() {
                Log.i(TAG, "ExecuterService: " + Thread.currentThread().getName());
            }
        });
    }
}
```

**Çıktı:**

```
I/MainActivity: ExecuterServicee: pool-7-thread-43
I/MainActivity: ExecuterServicee: pool-7-thread-10
I/MainActivity: ExecuterServicee: pool-7-thread-111
I/MainActivity: ExecuterServicee: pool-7-thread-47
I/MainActivity: ExecuterServicee: pool-7-thread-91
...
```

Yukarıda ki çıktıdan görüldüğü üzere 111 thread oluşturmuş. Ben 10000 satırın sadece bi kısmını yapıştırdım buraya. Belki 200 tane fln de oluşturmuş olabilir. Yani makineye ve işe bağlı olarak değişiyor. Kodun çalışmasıda ~6-7 saniye kadar sürdü. Yani baya uzun. Eee bu kadar thread açmak kapamak kolay değil.

Ama 60 saniye sonra tüm thread ler kapatılacak. Bu yönden de bakabilirsin.

Hangi yönden bakman gerektiğini projen söylücek sana. Kulak ver..

Neyse..

 
```java
public static ExecutorService newCachedThreadPool() {
 return new ThreadPoolExecutor(0, Integer.MAX_VALUE,
 60L, TimeUnit.SECONDS,
 new SynchronousQueue<Runnable>());
}
```

**Not:**

Yukarıda ki fonksiyonu kullanarak kendimize uygun cachedThreadPool oluşturabiliriz.

Tabi şuraya bakmakta fayda var : http://stackoverflow.com/a/1800583/3448461



### EXECUTORS.NEWFIXEDTHREADPOOL(N)

`
Creates a thread pool that reuses a fixed number of threads operating off a shared unbounded queue. At any point, at most nThreads threads will be active processing tasks. If additional tasks are submitted when all threads are active, they will wait in the queue until a thread is available. If any thread terminates due to a failure during execution prior to shutdown, a new one will take its place if needed to execute subsequent tasks. The threads in the pool will exist until it is explicitly shutdown.
`

Yani diyor ki;

- Uygulaman da daha fazla kontrol sahibi olmak istiyorsan bu yapıyı kullan.
- Thread e göndereceğin işler daha ağır ve uzun işler ise bu yapıyı kullan
- Ben senin tanımlamış olduğun kadar Thread oluşturucam. Ve bana iş yolladıkça hangi thread müsaitse onda çalıştırıcam. Tüm Thread ler dolu iken iş gelirse sıraya sokacam. Sen raad ol. Ben halledicem hepsini diyor.

```java
private void executorServiceNewFixedThreadPool() {

    ExecutorService executorService = Executors.newFixedThreadPool(5);

    for (int a = 0; a < 10000; a++) {
        executorService.execute(new Runnable() {
            @Override
            public void run() {
                Log.i(TAG, "ExecuterServicee: " + Thread.currentThread().getName());
            }
        });
    }
}
```

**Çıktı:**

```
I/MainActivity: ExecuterServicee: pool-7-thread-5
I/MainActivity: ExecuterServicee: pool-7-thread-5
I/MainActivity: ExecuterServicee: pool-7-thread-1
I/MainActivity: ExecuterServicee: pool-7-thread-3
I/MainActivity: ExecuterServicee: pool-7-thread-3
I/MainActivity: ExecuterServicee: pool-7-thread-3
I/MainActivity: ExecuterServicee: pool-7-thread-3
```
Ortalama ~2 saniye sürdü.

 

 

### EXECUTORS.NEWSINGLETHREADEXECUTOR()

```java
private void executorServiceNewSingleThreadExecutor() {
    ExecutorService executorService = Executors.newSingleThreadExecutor();

    for (int a = 0; a < 10000; a++) {
        executorService.execute(new Runnable() {
            @Override
            public void run() {
                Log.i(TAG, "ExecuterServicee: " + Thread.currentThread().getName());
            }
        });
    }
}
```

**Çıktı:**

```
I/MainActivity: ExecuterServicee: pool-7-thread-1
I/MainActivity: ExecuterServicee: pool-7-thread-1
I/MainActivity: ExecuterServicee: pool-7-thread-1
I/MainActivity: ExecuterServicee: pool-7-thread-1
I/MainActivity: ExecuterServicee: pool-7-thread-1
```
Ortalama ~2 saniye sürdü.

Tek bir Thread oluşturarak tüm gelen işleri sıraya sokarak bu Thread üzerinde işlemi gerçekleştirir.

 

**Peki bir soru ?**

**Executors.newSingleThreadExecutor() ile Executors.newFixedThreadPool(1) arasında fark var mı ?**

Ufakta olsa var tabi.

Ama ikiside tek bir Thread oluşturur ve gelen tüm işleri bu Thread üzerinden gerçekleştirir.

**Fark :** 

`
Similirity
newSingleThreadExecutor() returns ExecutorService with single thread worker and newFixedThreadPool(1) also returns ExecutorService with single thread worker. In both cases if thread terminates, new thread will be created.
`

`
Difference
ExecutorService returned by newSingleThreadExecutor(), can never increase its thread pool size more than one. ExecutorService returned by newFixedThreadPool(1), can increase its thread pool size more than one at run time by setCorePoolSize() of the class ThreadPoolExecutor.
`

Yani **newFixedThreadPool(1)** ile oluşturulan tek Thread yapısı sonradan artırılabilir. Ama **newSingleThreadExecutor()** ile oluşturulan yapı her zaman tek Thread üzerinden çalıştırılır.

**Nasıl arttırabiliriz ?**

```java
private void changeThreadSize() {

    ExecutorService executorService = Executors.newFixedThreadPool(1);
    ThreadPoolExecutor threadPoolExecutor = (ThreadPoolExecutor) executorService;
    threadPoolExecutor.setCorePoolSize(2);
    threadPoolExecutor.setMaximumPoolSize(2);

    for (int a = 0; a < 10000; a++) {
        threadPoolExecutor.execute(new Runnable() {
            @Override
            public void run() {
                Log.i(TAG, "ExecuterServicee: " + Thread.currentThread().getName());
            }
        });
    }
}
```

**Çıktı:**

```
I/MainActivity: ExecuterServicee: pool-7-thread-1
I/MainActivity: ExecuterServicee: pool-7-thread-1
I/MainActivity: ExecuterServicee: pool-7-thread-2
I/MainActivity: ExecuterServicee: pool-7-thread-2
I/MainActivity: ExecuterServicee: pool-7-thread-2
```