+++
categories = [
  "yazilim"
]
tags = [
  "software","firebase","dynamiclink"
]
coverImage = "https://c8.staticflickr.com/8/7421/9339731831_9ba94f287c_k.jpg"
desciption = ""
thumbnailImage = ""
thumbnailImagePosition = "top"
metaAlignment = "center"
keywords = [
  "yazilim",
  "sofware","firebase","dynamiclink"
]
autoThumbnailImage = false
date = "2017-04-17T01:53:03+03:00"
title = "Firebase Dynamic Links"
#url = "firebase_dynamiclink"

+++

## Firebase Dynamic Links

Öyle bir link olsun ki;

 - Bilgisayarımda ki browserdan linke tıkladığımda kişisel web sitem açılsın,
 - Eğer mobil cihazımda ki browserdan linke tıklarsan;
    * Uygulama cihazda yüklü ise uygulamam açılsın (Belirtmiş olduğum Activity vs.),
    * Uygulama cihazda yüklü değilse Google Play Store veya  App Store açılsın,
    * Uygulama cihazda yüklü olsa bile eğer belirttiğim versiyon kodundan eski bir sürüm varsa yine Google Play Store vs. açılsın,
    * ...

gibi sorulara tek bir link ile cevap verebilirsiniz.

Burda açıklayacağım kısım Android uygulaması üzerine olacaktır.

### Nasıl başlarım?

Öncelikle https://console.firebase.google.com/ adresinden bir proje oluşturuyoruz.

Dynamic Links tabına tıkladığımızda https://abcd.app.goo.gl/ benzeri bir adres görürüz. Bu adresi daha sonra oluşturucağımız dynamic linkler için kullanıcaz.

### Dört şekilde Dynamic Link oluşturabiliriz

- [Firebase Console](https://console.firebase.google.com/project/_/durablelinks/links/) kullanılarak

- [iOS Builder API](https://firebase.google.com/docs/dynamic-links/ios/create) kullanılarak

- https://app_code.app.goo.gl/?link=your_deep_link&apn=package_name[&amv=minimum_version][&al=android_link][&afl=fallback_link]

gibi bir linkte gerekli parametreleri uygun şekilde değiştirerek dynamic linkimizi oluşturabiliriz.

- 

```
POST https://firebasedynamiclinks.googleapis.com/v1/shortLinks?key=api_key
Content-Type: application/json

{
    "dynamicLinkInfo": {
        "dynamicLinkDomain": "c49ss.app.goo.gl",
        "link": "https://play.google.com/store/apps/details?id=com.coderockets.referandumproject", // Masaüstü browserlarından bu linke tıkladığımızda gidilecek adres
        "androidInfo": {
            "androidPackageName": "com.coderockets.referandumproject", // Android uygulamamızın package name i
            "androidMinPackageVersionCode": "21", // Linki alan kişide uygulamamız yüklü fakat eski versiyon yüklü ise (<21) direk olarak Google Play sayfasına yönlendirilir.
            "androidLink": "http://referandum?questionId=xyz" // Android cihazından linke tıklanıldığında bu linke yönlendirilme yapılır. Uygulamamızda gerekli ayarlamaları yaparak direk gerekli sayfaya(Activity) yönlendiririz.
        },
        "socialMetaTagInfo": {
            "socialTitle": "Question text", // Sosyal medyada bu link paylaşıldığında görünecek başlık ayarlaması
            "socialDescription": "Hemen Referandum topluluğuna katılarak cevap verebilirsin.", // Sosyal medya paylaşımında görünecek açıklama
            "socialImageLink": "Question Image Url" // Sosyal medya paylaşımında görünecek resim
        }
    },
    "suffix": {
        "option": "SHORT" // shortLink sonundaki id nin daha kısa olmasını sağlar. UNGUESSABLE yaparsak daha uzun karakterli bir unique id ile link oluşturulur. shortLink: https://c49ss.app.goo.gl/5TI3
    }
}
```
şeklinde bir REST isteği göndererek dynamic linkimizi aşağıdaki response şeklinde elde edebiliriz.
```
{
  "shortLink": "https://c49ss.app.goo.gl/5TI3",
  "previewLink": "https://c49ss.app.goo.gl/?link=https://play.google.com/store/apps/details?id%3Dcom.coderockets.referandumproject&al=http://referandum?questionId%3Dxyz&apn=com.coderockets.referandumproject&amv=21&st=Question+text&sd=Hemen+Referandum+toplulu%C4%9Funa+kat%C4%B1larak+cevap+verebilirsin.&si=Question+Image+Url&d=1"
}
``` 
**shortLink** ile **previewLink** aynı görevi üstlenir. Aralarında bir fark yok. 

---

## Android Uygulamamızı Yapılandıralım

- AndroidManifest.xml dosyamızda ilgili Activity tanımı içerisinde aşağıdaki gibi düzenleme yapıyoruz.
```
        <activity
            android:name=".activity.UniqueQuestionActivity"
            android:parentActivityName=".activity.MainActivity">

            <intent-filter>
                <action android:name="android.intent.action.VIEW" />
                <category android:name="android.intent.category.DEFAULT" />
                <category android:name="android.intent.category.BROWSABLE" />

                <data android:host="referandum" android:scheme="http" />
                <data android:host="referandum" android:scheme="https" />
            </intent-filter>

            <meta-data android:name="android.support.PARENT_ACTIVITY"
             android:value=".activity.MainActivity" />
        </activity>
```         
- İlgili Activity içerisinde aşağıdaki tanımlamaları yapıyoruz. 
```
@Override
protected void onCreate(Bundle savedInstanceState) {
    // ...

    // Build GoogleApiClient with AppInvite API for receiving deep links
    mGoogleApiClient = new GoogleApiClient.Builder(this)
            .enableAutoManage(this, this)
            .addApi(AppInvite.API)
            .build();

    // Check if this app was launched from a deep link. Setting autoLaunchDeepLink to true
    // would automatically launch the deep link if one is found.
    boolean autoLaunchDeepLink = false;
    AppInvite.AppInviteApi.getInvitation(mGoogleApiClient, this, autoLaunchDeepLink)
            .setResultCallback(
                    new ResultCallback<AppInviteInvitationResult>() {
                        @Override
                        public void onResult(@NonNull AppInviteInvitationResult result) {
                            if (result.getStatus().isSuccess()) {
                                // Extract deep link from Intent
                                Intent intent = result.getInvitationIntent();
                                String deepLink = AppInviteReferral.getDeepLink(intent);

                                // Handle the deep link. For example, open the linked
                                // content, or apply promotional credit to the user's
                                // account.

                                // ...
                            } else {
                                Log.d(TAG, "getInvitation: no deep link found.");
                            }
                        }
                    });
}
``` 

Yukarıda ki kodu dynamic link ile açılabilen her Activity içerisinde tanımlamalısınız.

Aslında aşağıda ki gibi de link ve parametrelere ulaşabilirsiniz.
``` 
@Override
protected void onCreate(Bundle savedInstanceState) {
 if (getIntent() != null && getIntent().getAction() != null) {
            
            // Link : http://referandum/?questionId=xxx
            String questionId = "";
            switch (getIntent().getAction()) {
                case Intent.ACTION_VIEW: {
                    if (getIntent().getData().getHost().equals("referandum")) {
                        questionId = getIntent().getData().getQueryParameter("questionId");
                    }
                    break;
                }
            }
        }
}        
``` 

Ama ilk durumda ki gibi **getInvitation()** ile dynamic linki işlerseniz Firebase arka tarafta başka işlemlerde yapıyor olacaktır. Mesela Firebase Analytics tanımlamışsanız Firebase Console undan;

* dynamic_link_app_open
* dynamic_link_first_open
* dynamic_link_app_update

parametrelerine ait istatistikleri görebilirsiniz.

---

Kaynaklar:

- https://firebase.google.com/docs/dynamic-links/
- https://firebase.google.com/docs/dynamic-links/rest
- https://firebase.google.com/docs/dynamic-links/android/receive







