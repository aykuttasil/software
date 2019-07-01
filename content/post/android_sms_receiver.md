---
title: "Android SMS Receiver"
date: 2017-01-11T01:42:54+03:00
lastmod: 2018-09-25T11:51:07+03:00
draft: false
keywords: ["yazilim","sofware","sms-receiver","android","java"]
description: "Android Test"
tags: ["software","android","java","sms","sms-receiver"]
categories: ["yazilim","java","android"]
author: "Aykut Asil"
comment: true
toc: true
autoCollapseToc: false
postMetaInFooter: true
hiddenFromHomePage: false
---

## Android SMS Receiver

Android de gelen sms leri dinlemek ve uygulamanızın akışını gelen sms lere göre şekillendirmek için aşağıdaki yapıyı kullanabilirsiniz.

İlk olarak AndroidManifest.xml dosyanızda receiver tanımlamalısınız. Fakat biz bu receiver ı dinamik olarak tanımlıcaz. Bunu yapmamızın sebebi SMS i dinledikten sonra bu receiver ı silmek ve daha sonra gelen SMS lerin dinlenmesini önlemek.

Siz uygulamınız da sürekli bir SMS dinlemeye ihtiyaç duyarsanız receiver ı AndroidManifest.xml dosyanızda tanımlamalısınız.

Biz burda SMS dinlemesi yaparken önlem amaçlı olarak CPU nun uyumasını önlüyoruz. Eğer bir işlem sonucunda sms gelmesini bekliyorsak, beklediğimiz SMS  geciktiği takdirde ve bu sırada cihazın ekranını vs. kapattığımız da Android cihazımız kendini uyku moduna almak isteyecektir. Bunu önlemek için **WakefulBroadcastReceiver** yapısını kullanıyoruz. Bu yapı aslında Android in WakeLock özelliğini kullanan serviceler için özel olarak tasarlanmış  bir yapıdır. Cihazı uyanık tutar ve işimiz bittiği takdirde bu WakeLock u kaldırmamız gerekir. Bunu da service in içerisinde tanımlarız.

Aşağıdaki fonksiyonu Activity miz içerisinde ihtiyacımız olan yerde çağırır ve SMS receiver ı çalıştırmıış oluruz.

### LoginActivity

```java
public void RegisterSmsReceiver() {
    SmsReceiver smsReceiver = new SmsReceiver();
    IntentFilter intentFilter = new IntentFilter("android.provider.Telephony.SMS_RECEIVED");
    android.os.Handler handler = new android.os.Handler();
    registerReceiver(smsReceiver, intentFilter, "android.permission.GET_TASKS", handler);
}
```

### SmsReceiver

```java
public class SmsReceiver extends WakefulBroadcastReceiver {

    private static final String TAG = "SmsReceiver";

    public SmsReceiver() {
        super();
    }

    @Override
    public void onReceive(Context context, Intent intent) {
        Intent myIntent = intent;
        myIntent.setClass(context, SmsReceiverService.class);

        // startWakeFulService ile AndroidManifest.xml dosyamız içerisinde tanımlamış olduğumuz Service e yönlendiriyoruz.
        // Ve cihazın uyanık kalmasını sağlıyoruz.
        startWakefulService(context, myIntent);

    }
}
```

**AndroidManifest.xml** dosyasında  Service imizi  tanımlıyoruz.

```xml
<service
    android:name=".SmsReceiverService"
    android:exported="false" />
```

`exported="false"` tanımı servisin cihazda ki diğer uygulamalar tarafından çalıştırılamayacağını belirtir.

### SmsReceiverService

```java
public class SmsReceiverService extends IntentService {

    private static final String TAG = "SmsReceiverService";
    private Context context;

    public SmsReceiverService() {
        super(TAG);
    }

    @Override
    public void onCreate() {
        super.onCreate();
        context = getApplicationContext();
    }


    @Override
    protected void onHandleIntent(Intent intent) {

        String msg_from = null;
        String msgBody = null;
        if (intent.getAction().equals("android.provider.Telephony.SMS_RECEIVED")) {
            Bundle bundle = intent.getExtras();           //---get the SMS message passed in---
            SmsMessage[] msgs = null;

            if (bundle != null) {
                //---retrieve the SMS message received---
                try {
                    Object[] pdus = (Object[]) bundle.get("pdus");
                    msgs = new SmsMessage[pdus.length];
                    for (int i = 0; i < msgs.length; i++) {
                        msgs[i] = SmsMessage.createFromPdu((byte[]) pdus[i]);
                        msg_from = msgs[i].getOriginatingAddress();
                        msgBody = msgs[i].getMessageBody();

                    }
                } catch (Exception e) {
                     // Log.d("Exception caught",e.getMessage());
                }
            }
        }

        SmsEvent smsEvent = new SmsEvent();
        smsEvent.setActivationCode("357");
        smsEvent.setMsgBody(msgBody);
        smsEvent.setMsgFrom(msg_from);

        // EventBus ile sonucu istediğimiz yere gönderebiliriz.
        EventBus.getDefault().post(smsEvent);

        // Service in işini tamamladığını ve artık cihazın WakeLock u serbest bırakabileceğini söylüyoruz.
        SmsReceiver.completeWakefulIntent(intent);
    }
}
```

**SmsEvent**

```java
public class SmsEvent {
    private String activationCode;
    private String msgFrom;
    private String msgBody;


    public String getActivationCode() {
        return activationCode;
    }

    public void setActivationCode(String activationCode) {
        this.activationCode = activationCode;
    }

    public String getMsgFrom() {
        return msgFrom;
    }

    public void setMsgFrom(String msgFrom) {
        this.msgFrom = msgFrom;
    }

    public String getMsgBody() {
        return msgBody;
    }

    public void setMsgBody(String msgBody) {
        this.msgBody = msgBody;
    }
}
```
