+++
title = "Android Internet Connection Receiver "
metaAlignment = "center"
categories = [
  "yazilim",
  "java",
  "android"
]
date = "2017-01-11T01:42:36+03:00"
desciption = ""
keywords = [
  "yazilim",
  "sofware",
  "java",
  "android",
  "receiver",
  "broadcast receiver",
  "internet connection"
]
coverImage = "https://c8.staticflickr.com/8/7421/9339731831_9ba94f287c_k.jpg"
tags = [
  "software","java","android","broadcast receiver","internet control"
]
autoThumbnailImage = false
thumbnailImagePosition = "top"
thumbnailImage = ""

+++

Uygulamanızın akışını internet kontrolü yaparak yönetmeniz gerekebilir.

Bunun için ilk olarak AndroidManifest.xml dosyasına receiver tanımı yapmalısınız.

```xml
<receiver android:name=".InternetConnectionReceiver">
    <intent-filter>
        <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
    </intent-filter>
</receiver>
```

**InternetConnectionReceiver**

```java
public class InternetConnectionReceiver extends BroadcastReceiver {


    @Override
    public void onReceive(Context context, Intent intent) {

        if (CheckConnection(context)) {

        }
    }
}
```

Yukarıda tanımlanmış olan receiver, cihazın ağ yapısında herhangi bir değişiklik olduğunda bunu yakalar. Örneğin wireless açıkken kapattığınız da veya kapalıyken açtığınız da bunu yakayabilirsiniz.

**CheckConnection**

```java
public static boolean checkConnection(Context con) {
    ConnectivityManager cm = (ConnectivityManager) con.getSystemService(Context.CONNECTIVITY_SERVICE);
    NetworkInfo netInfo = cm.getActiveNetworkInfo();
    return netInfo != null && netInfo.isConnectedOrConnecting();
}
```

Yukarıda ki kod bloğu da cihazın internete bağlı olması durumunu kontrol eder.
