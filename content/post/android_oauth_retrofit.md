+++
metaAlignment = "center"
autoThumbnailImage = false
title = "Retrofit Oauth"
desciption = "Retrofit Oauth Entegrasyonu"
thumbnailImage = ""
date = "2017-05-17T11:58:43+03:00"
#url = "retrofit_oauth"
tags = [
  "software","oauth","retrofit","okhttp"
]
thumbnailImagePosition = "top"
coverImage = "https://c8.staticflickr.com/8/7421/9339731831_9ba94f287c_k.jpg"
categories = [
  "yazilim","android"
]
keywords = [
  "yazilim",
  "sofware","oauth","retrofit","okhttp","android","oauth2"
]

+++

## Retrofit Oauth Entegrasyonu

Oauth, kısaca anlatmak gerekirse; Kullanıcı ile ilgili her türlü (izin,yetki,güvenlik vs.) etkileşimi standartlaştıran bir yapı diyebiliriz. Google amcaya sorarsanız neler yapabileceğiniz ile ilgili birçok kaynak bulabilirsiniz. Veya [buraya](https://oauth.net/) bakabilirsiniz.

Burada bahsedecek olduğumuz şey Android mobil uygulamanızdan bir istek yaptığınızda tüm bu oauth işlemlerinizi otomatize etmek üzerine olacak.

#### Nasıl Çalışıyor?

[Kısaca](http://bahadir.almaci.com/2010/02/oauth-nedir-nasil-calisir/) anlatılan bu yazıyı okuduğunuzda günün sonunda elimizde bir token olduğunu ve bu tokenı request headerımıza ekleyerek backend kısmında kontrolünü sağladığımızı ve duruma göre cevap döndüğümüzü göreceksiniz. Bu tokenın işlevsellik ömürleri vardır. Yani 10 dakika süre ile bu tokenı yetkilendir deriz. Çünkü sonuçta asıl amacımız güvenlik. Kullanıcı ad ve şifre bilgisi birilerinin eline geçmesi ile token bilgisinin birilerinin eline geçmesi aynı anlama gelir. Tabi eğer tokena bir ömür biçmezsek. 

**Peki elimizde ki token ın bir ömrünün olması uygulamamıza nasıl bir akış olmasını gerektiriyor ?**

- Zorunlu olmamakla beraber bazen elimizde token ile beraber birde refresh token bilgisi olur. Eğer token süresi dolmuşsa refresh token kullanılarak yeni taze bir token isteği yapılır ve yeni isteklerde bu yeni token kullanılır.

- Refresh token yok ise ilk başta tokenımızı nasıl elde ediyor isek(tüm header bilgileri tekrar girilecek; cliend_id,secret_id vs.) o şekilde süreci baştan başlatmamız gerekecek ve elde ettiğimiz tokenı yeni isteklerimizde kullanmamız gerekecek.

Biz ikinci kısım için [Retrofit](http://square.github.io/retrofit/) yapılandırmasını anlatıcaz.

**RestClient**

```java
public class RestClient {

    private static RestClient _instance;
    private OkHttpClient client;
    private ApiService apiService;

    @DebugLog
    private RestClient(Context context) {

        HttpLoggingInterceptor httpLoggingInterceptor = new HttpLoggingInterceptor();
        httpLoggingInterceptor.setLevel(HttpLoggingInterceptor.Level.BODY);

        OkHttpClient.Builder clientBuilder = new OkHttpClient.Builder();

        clientBuilder.addInterceptor(chain -> {
            Request original = chain.request();

            Request request = original.newBuilder()
                    .addHeader("Content-Type", "application/json")
                    .addHeader("X-App-Name", "Dokar") // Optional
                    .addHeader("X-api-version", "1.0") // Optional
                    .method(original.method(), original.body())
                    .build();

            return chain.proceed(request);
        });

        clientBuilder.addInterceptor(chain -> {
            Request original = chain.request();

            Request.Builder builder = original.newBuilder();

            String tokenType = "Bearer";
            String authToken = AppDataHelper.getInstance(context).getAccessToken(); // Veritabanından ya da SharedPreference vs. den tokenımızı çekiyoruz

            // Eğer token bilgisi var ise request imizin header ına "Authorization: Bearer tokenanahtari" şeklinde ekleme yapıyoruz.
            if (authToken != null) {
                builder.header("Authorization", tokenType + " " + authToken);
            }

            Request request = builder
                    .method(original.method(), original.body())
                    .build();

            return chain.proceed(request);
        });

        // Yapılan istekleri loglamak için interceptor
        clientBuilder.addInterceptor(httpLoggingInterceptor);

        // *****************************
        // ÖNEMLİ
        // Otomatize ettiğimiz kısım burası
        clientBuilder.authenticator(new TokenAuthenticator(context));

        clientBuilder.connectTimeout(60, TimeUnit.SECONDS)
                .readTimeout(60, TimeUnit.SECONDS)
                .writeTimeout(60, TimeUnit.SECONDS);

        Gson gson = new GsonBuilder()
                .setDateFormat("yyyy-MM-dd'T'HH:mm:ss")
                .excludeFieldsWithoutExposeAnnotation() // Android 23 için düzenleme
                .create();

        client = clientBuilder.build();

        Retrofit retrofit = new Retrofit.Builder()
                .addConverterFactory(GsonConverterFactory.create(gson))
                .addCallAdapterFactory(RxJava2CallAdapterFactory.create())
                .baseUrl(BuildConfig.BACKEND_URL)
                .client(client)
                .build();

        apiService = retrofit.create(ApiService.class);
    }

    @DebugLog
    public static RestClient getInstance(Context context) {
        if (_instance == null) {
            _instance = new RestClient(context);
        }
        return _instance;
    }

    @DebugLog
    public ApiService getApiService() {
        return apiService;
    }

    @DebugLog
    public OkHttpClient getOkHttpClient() {
        return client;
    }
}
```

Yukarıda **ÖNEMLİ** yazan alanda `clientBuilder.authenticator(new TokenAuthenticator(context))` şeklinde bir authenticator işlevi ekledik.

Bu sınıfın şunu yapıyor:

- Belirttiğimiz header ve body bilgileri doğrultusunda istek yaptık ve sonuç olarak **401** response code döndü. Yani kimlik doğrulanamadı. Eğer  **401** döner ise `TokenAuthenticator(context` sınıfı devreye girecek ve bizim requestimizin istediğimiz alanlarını düzenleyerek yine aynı isteği otomatik olarak yapıcak.

**TokenAuthenticator**
```java
public class TokenAuthenticator implements Authenticator {

    private Context mContext;

    public TokenAuthenticator(Context context) {
        this.mContext = context;
    }

    @DebugLog
    @Override
    public Request authenticate(Route route, Response response)
            throws IOException {

        // İlk başta tokenımızı nasıl elde ettiysek yine aynı şekilde ama senkron şekilde elde ediyoruz.
        AccessTokenResponse accessTokenResponse = RestClient.getInstance(mContext)
                .getApiService()
                .getAccessTokenImmediate(AccessTokenRequest.buildParams())
                .execute()
                .body();

        String accessToken = accessTokenResponse.getAccessToken();

        // Veritabanımızda ki token bilgisi güncelliyoruz
        AppDataHelper.getInstance(mContext).saveAccessToken(accessToken);

        String tokenType = "Bearer";

        Request original = response.request();
        Request.Builder newRequestBuilder = original.newBuilder();

        // Eski isteğimizde ki Authorization header alanını siliyoruz. Eğer silmezsek birden fazla aynı keye sahip header alanımız olur.
        if (original.headers().get("Authorization") != null) {
            Logger.i("Authorization Header is: " + original.headers().get("Authorization"));
            newRequestBuilder.removeHeader("Authorization");
        }

        // Ve yeni token değeri ile tekrar Authorization headerı ekliyoruz.
        Request newRequest = newRequestBuilder
                .addHeader("Authorization", tokenType + " " + accessToken)
                .method(original.method(), original.body())
                .build();

        return newRequest;
    }
}

```

- **401** response kod döndü
- Yukarıda ki kod çalıştı
- Yeni token elde edildi
- Yapılan request header bilgileri güncellendi
- İsteğimiz aynı parametreler ile(**body** alanı vs.) tekrar yapıldı

Ve gülen surat..
:)

Sorunuz varsa sorabilirsiniz.






