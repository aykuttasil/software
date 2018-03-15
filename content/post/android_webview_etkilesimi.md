+++
categories = [
  "yazilim","android","java"
]
keywords = [
  "software","android","java","webview","addJavascriptInterface","WebViewClient","JavascriptInterface"
]
autoThumbnailImage = false
thumbnailImagePosition = "top"
metaAlignment = "center"
title = "Android ile WebView Etkileşimi"
url = "android-webview-etkilesimi"
date = "2017-01-11T19:59:08+03:00"
tags = [
  "sofware","android","java","webview","addJavascriptInterface","WebViewClient","JavascriptInterface"
]
thumbnailImage = ""
coverImage = "https://c8.staticflickr.com/8/7421/9339731831_9ba94f287c_k.jpg"

+++

Kısa Hikaye : Üstünde çalışmakta olduğum bir projede kredi kartı ile ödeme yapısı kurmam gerekti. Kısaca projenin yapısından bahsedecek olursak

- Backend : .Net
- Client : Android (Java)
- Ödeme altyapısı : İyzico

Client tarafında rest isteğiyle tüm işlemlerimizi backend tarafında yapıyoruz. Ödeme yapımızı da bu doğrultuda geliştirdik.

Problem : 3DS ile ödeme almaya çalıştığımızda malumunuz işin içerisine bankanın bize telefonumuza gelen şifreyi girmemiz için göndermiş olduğu web sayfası vs. giriyor. Bu durumda client ile web sayfası iletişimini de bir şekilde sağlamamız gerekiyor.

--- 

Yukarıda ki hikayeyi anlatarak kafanızda bir problemi canlandırmaya çalıştım. Benzer bir çok durum ile karşı karşıya kalabilirsiniz.

Ulan ben webview de ki butonu vs nasıl android tarafında kontrol edicem ? gibi akılda deli sorular oluşabilir..

Tabiri caiz ise : PhoneGap tarzı bişiler.. 🙂 😉

Neyse.

Örnek ver hacı abi;

## Layout.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <WebView
        android:id="@+id/WebView"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />

</LinearLayout>
``` 

## Activity – Fragment

```java
WebView webView = (WebView) findViewById(R.id.WebView);

WebSettings settings = webView.getSettings();
settings.setDefaultTextEncodingName("utf-8");
settings.setJavaScriptEnabled(true);

webView.setWebViewClient(new MyWebViewClient());
webView.addJavascriptInterface(new MyWebViewJavascriptInterface(mContext, webView, callbackDialog), "Android"); // Buraya dikkat
webView.loadDataWithBaseURL(null, response.getData().getThreeDSHtmlContent(), "text/html", "utf-8", null); // elimizde bulunan bir html dökümanı webview e basıyoruz. URL de verebilirsin istersen
webView.addJavascriptInterface(new MyWebViewJavascriptInterface(mContext, webView, callbackDialog), "Android");
```

Yukarıda ki “Android” kısmına istediğimiz ismi verebiliriz. WebView yapısı, burada belirtmiş olduğumuz isim ile sayfaya yüklenen içeriğe bir eklenti yapıyor, yani bir instance alıyor. Biz bu instance ı kullanarak iletişimimizi sağlıyoruz.

Az sabır. Nasıl olduğunu görüceksin..

## MyWebViewClient

```java
class MyWebViewClient extends WebViewClient {

    @Override
    public void onPageFinished(WebView view, String url) {
        super.onPageFinished(view, url);
        //view.loadUrl("javascript:window.Android.processHTML('<html>'+document.getElementsByTagName('html')[0].innerHTML+'</html>');");
    }
}
```

Bu blok ne yapıyor ?

**window.Android.processHTML** de ki Android bizim yukarıda belirtmiş olduğumuz aracı.

**processHtml** ise aşağıda tanımlıcak olduğumuz fonksiyon.

Diğer kısımlar javascript fonksiyonu zaten. Yani sayfada ki ilk html tag ını bularak içindeki html kodu al.

Aşağıda ki processHtml e bakarsan görüceksin ki htmlContent parametresi bekliyor.Biz de bu parametreye sayfa içeriğinin html kodunu gönderiyoruz. Almış olduğun bu html içeriğiyle ne yaparsın sana kalmış.

## MyWebViewJavascriptInterface

```java
class MyWebViewJavascriptInterface {

    Context mContext;
    WebView mWebView;

    MyWebViewJavascriptInterface(Context c, WebView webView) {
        this.mContext = c;
        this.mWebView = webView;
    }

    @JavascriptInterface
    public void processHTML(String htmlContent) {
        // işlem
        // Toast ile html içeriğini gösterebiliriz       
    }

    @JavascriptInterface
    public void islemeDevamEt() {
       // işlem
    }

    @JavascriptInterface
    public void pencereyiKapat() {
        // işlem
        // dialog.dismiss();
    }
}
```

WebView miz ile Android native kodumuzun iletişimini bu sınıf sağlıyor.

Aşşağıda ki  kod ile bunu söylüyoruz.

***webView.addJavascriptInterface(new MyWebViewJavascriptInterface(mContext, webView), "Android");***

***Not*** : Android 4.2 den sonra **@JavascriptInterface** annotations ını eklememiz gerekmekte. Yoksa nolur ? Çalışmaz 😉


## Custom Html Content

WebView de gösterdiğimiz html contentinde bir buton ekleyelim.

```html
 <button class="btn btn-danger" onclick="PencereyiKapat()">Pencereyi Kapat</button>
```
WebView içinde ki bu butona basıldığını Android anlasın ve (WebView imizi Dialog içerisinde gösterildiğini varsayarak) dialog penceresini kapasın.

Butonumuzun olduğu sayfaya  javascript kodumuzu ekliyoruz.

```javascript
<script type="text/javascript">
  function PencereyiKapat() {
     Android.pencereyiKapat();
  }
</script>
```

Yukarıda ki Android i hatırladınız dimi ? 🙂

HTML sayfamız bu kadar.

WebView içerisinde ki butona basıldığında javascript fonksiyonu çalışacak ve Android.pencereyiKapat() çalışacak. Yani native tarafta tanımlamış olduğumuz pencereyiKapat() fonksiyonu.

 
Yukarıda ki yapıyı kullanarak WebView – Android Native etkileşimli yapı kurabilirsin.

Her zaman ki gibi hayal gücüne ve projeye kalmış 😉

Kalın sağlıcakla..
