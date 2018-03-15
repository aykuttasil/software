+++
thumbnailImagePosition = "top"
coverImage = "https://c8.staticflickr.com/8/7421/9339731831_9ba94f287c_k.jpg"
date = "2017-01-11T01:43:53+03:00"
autoThumbnailImage = false
title = "Android GcmNetworkManager Kullanımı"
#url = "android-gcmnetworkmanager-kullanimi"
metaAlignment = "center"
categories = [
  "yazilim",
  "java",
  "android",
]
desciption = ""
tags = [
  "software","java","android","gcmnetworkmanager"
]
thumbnailImage = ""
keywords = [
  "yazilim",
  "sofware",
  "gcmnetworkmanager",
  "oneoftask",
  "periodictas",
  "internet",
  "charge"
]

+++


GcmNetworkManager kullanarak Android de network tabanlı işlemlerinizi olabildiğince kontrollü bir şekide yapabilirsiniz.

GcmNetworkManager ın kullanım alanı daha çok asenkron ve periodic network işlemlerinizi yapılandırmaktır.

Tek sefer çalışacak veya Periodic olarak çalışacak işlemleriniz için 2 tip belirlenmiştir.

- OneoffTask
- PeriodicTask


---

**OneoffTask Kullanımı**

```java
OneoffTask oneoff = new OneoffTask.Builder()
        // Uygulamanızı kapatsanız bile tekrar açtığınız da network işleminiz işleme konulur.
        // Yani isteğinizin kalıcı olup olmamasını bu parametre ile ayarlayabilirsiniz.
        .setPersisted(true)
        // Belirtilen network işlemlerinin çalışacağı service i belirtir.
        .setService(MyGcmTaskService.class)
        // İşleminiz için tag belirleyebilirsiniz.
        // Aynı tag ile yeni bir istek yaptığınız da '.setUpdateCurrent(true)' olarak belirlenmiş ise isteğinizi yeni istek ile günceller. Yani eski isteğiniz geçersiz olacaktır. 'setUpdateCurrent(false)' olarak belirlenir ise aynı tag ile yeni istekte bulunsanız bile her iki isteğiniz de çalışacaktır. 
        .setTag(tag)
        // Network isteğinizin çalışma zamanı parametrelerini belirler
        .setExecutionWindow(0, 10)
        // İşleminiz için internet gerekliliği veya wireless gerekliliği parametrelerini belirler
        .setRequiredNetwork(required_network_state)
        // İşleminiz için cihazın şarja bağlı olup olmaması gerekliliğini belirler
        .setRequiresCharging(false)
        // Aynı tag ile yeni istek yapıldığı takdirde güncellenip güncellenmemesi durumunu belirler
        .setUpdateCurrent(true)
        // Bundle ile işleminiz için gerekli parametreleri belirleyebilirsiniz
        .setExtras(bundle)
        // build eder
        .build();

GcmNetworkManager.getInstance(context).schedule(oneoff);
```

---


**MyGcmTaskService**

```java
// Cihazınızı yeniden başlatma gibi durumlar da önceden belirlenmiş task lerinizin çalışmasını devam ettirmesini istiyorsanız
// burada tekrar belirlemelisiniz.
@Override
public void onInitializeTasks() {
    
}

// GcmNetworkManager ın kendi algoritması ile seçilen, tanımlamış olduğunuz istek bu kod bloğuna girecek.
@Override
public int onRunTask(TaskParams taskParams) {
    try {
        Bundle bundle = taskParams.getExtras();
        return SchedulerRandevuTask(bundle);        
    } catch (Exception e) {
        ErrorEvent errorEvent = new ErrorEvent();
        errorEvent.setErrorContent(e.getMessage());
        EventBus.getDefault().post(errorEvent);
        return GcmNetworkManager.RESULT_RESCHEDULE;
    }
}

private int SchedulerZiyaretTask(Bundle bundle) {
    try {
        // isteğimiz sırasında bundle olarak yüklemiş olduğumuz nesneyi burada yakalıyoruz
        String jsonModel = bundle.getString(Const.IZTOP_TASK_BUNDLE);
        ZiyaretRequest request = new Gson().fromJson(jsonModel, ZiyaretRequest.class);
        ZiyaretResponse ziyaretResponse = sendZiyaretProcess(request);
        switch (ziyaretResponse.getCode()) {
            case 0: {
                EventBus.getDefault().post(ziyaretResponse);                
                // işlemlerimiz başarılı bir şekilde gerçekleşmiş ise result değeri olarak RESULT_SUCCESS dönüyoruz.
                // GcmNetworkManager bu işlemi başarılı olduğu için listeden silecek ve tekrar çalıştırmayı denemicektir.
                return GcmNetworkManager.RESULT_SUCCESS;
            }
            default: {
                // işlemlerimiz başarısız olması durumunda ( burada sunucuya istekte bulunulmuş ve dönen değer 0 değil ise başarısız olarak belirlenmiştir )isteğin tekrar çalışması için result değeri olarak RESULT_RESCHEDULE dönüyoruz.
                return GcmNetworkManager.RESULT_RESCHEDULE;
            }
        }
    } catch (Exception e) {
        SuperHelper.CrashlyticsLog(e);
        e.printStackTrace();
        return GcmNetworkManager.RESULT_RESCHEDULE;
    }


}
private ZiyaretResponse sendZiyaretProcess(ZiyaretRequest request) {
    return ApiManager.getInstance(getApplicationContext()).Ziyaret(request);
}

```

**ApiManager / Ziyaret**

```java
public ZiyaretResponse Ziyaret(ZiyaretRequest request) {
    try {
        RestClient restClient = RestClient.getInstance();
        Call<ZiyaretResponse> responseCall = restClient.getApiService().Ziyaret(request);
        ZiyaretResponse ziyaretResponse = responseCall.execute().body();
        return ziyaretResponse;
    } catch (Exception ex) {
        return null;
    }
}
```

**Retrofit ApiService**

```java
@POST("sunucu/ziyaret/api/adresi")
Call<ZiyaretResponse> Ziyaret(@Body ZiyaretRequest ziyaretRequest);</pre>
```

---
