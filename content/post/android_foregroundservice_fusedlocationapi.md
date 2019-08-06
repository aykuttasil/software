+++
thumbnailImagePosition = "top"
title = "Foreground Service ile FusedLocationApi Kullanımı"
#url = "foregroundservice-ve-fusedlocationapi-kullanimi"
metaAlignment = "center"
categories = [
  "yazilim",
  "java",
  "android"
]
tags = [
  "software","android","java","fusedlocationapi","foreground service"
]
coverImage = "https://c8.staticflickr.com/8/7421/9339731831_9ba94f287c_k.jpg"
desciption = ""
autoThumbnailImage = false
date = "2017-01-11T15:27:59+03:00"
thumbnailImage = ""
keywords = [
  "yazilim",
  "sofware","android","java","fusedlocationapi","foreground service"
]

+++


Android service yapısı, Android’in temel bileşenlerinden olup genel kullanım amacı yan iş parçacığı oluşturmaktır. Uzun süren işlemler (download vb.) için olmazsa olmaz bileşendir. Android in service bileşenini kendi ihtiyaçlarınız doğrultusunda özelleştirebilir ve uygulamanızı modern bir yapıya kavuşturabilirsiniz.

Burada yapacağımız örnekte kısaca şu aşamaları görücez;

- Service i foreground olarak başlatmak
- Ongoing(devamlı gözüken) notification oluşturmak
- GoogleApiClient a bağlanmak
- Location bilgisini notification da göstermek
 

İlk olarak Service imiz kodlarını yazalım. Gerekli açıklamalar kodların arasında mevcuttur.

**ForegroundService.java**

```java
public class ForegroundService extends Service implements GoogleApiClient.ConnectionCallbacks, GoogleApiClient.OnConnectionFailedListener {

 private static final int LOCATION_NOTIF_ID = 859;
 private static int LocationPeriod = 1000 * 30 ;
 
 private static Notification notification;
 private static GoogleApiClient _googleApiClient = null;
 private static Context mContext;
 private static NotificationCompat.Builder mBuilder;



 @Override
 public void onCreate() {
 super.onCreate();
 mContext = this;
 mBuilder = new NotificationCompat.Builder(mContext);
 }

 @Nullable
 @Override
 public IBinder onBind(Intent intent) {
 return null;
 }

 @Override
 public int onStartCommand(Intent intent, int flags, int startId) {


 // Eğer Service ilk defa çalıştırılıyor ise buildGoogleApiClient fonksiyonu ile GoogleApiClient build ediliyor
 if (_googleApiClient == null) {
 buildGoogleApiClient();
 }
 // GoogleApiClient a bağlanılıyor
 _googleApiClient.connect();

 // Notificaiton gösteriyoruz
 LocationNotification(null, "Konum Bilgisi Yok");

 // Service imizi foreground olarak başlatıyoruz
 // Foreground service bizden bir notification parametresi ister. Bunun sebebi sürekli ayakta olduğunu kullanıcıya bildirmektir.
 startForeground(LOCATION_NOTIF_ID, notification);

 // Herhangi bir sebeple service imizi durması halinde kendiliğinden tekrar başlaması için START_STICKY dönüyoruz.
 return START_STICKY;
 }


 public synchronized void buildGoogleApiClient() {
 if (_googleApiClient == null) {
 _googleApiClient = new GoogleApiClient.Builder(this)
 .addConnectionCallbacks(this)
 .addOnConnectionFailedListener(this)
 .addApi(LocationServices.API)
 .build();
 }
 }


 @Override
 public void onConnected(@Nullable Bundle bundle) {
 // GoogleApiClient a bağlanıldığında Location isteğinde bulunuyoruz.
 LocationRequest();
 }


 @Override
 public void onConnectionSuspended(int i) {

 }


 @Override
 public void onConnectionFailed(ConnectionResult result) {
 // tekrar bağlanmayı deniyoruz
 _googleApiClient.connect();
 }


 private void LocationRequest() {


 // PRIORITY_BALANCED_POWER_ACCURACY -> hassas konum bilgisine ihtiyacımız yok ise (Network, Cell Tower)
 // PRIORITY_HIGH_ACCURACY -> hassas konum bilgisine ihtiyacımız var ise (GPS)

 final LocationRequest locationRequest = LocationRequest.create()
 .setPriority(LocationRequest.PRIORITY_HIGH_ACCURACY)
 .setInterval((LocationPeriod * 1000)) 
 .setFastestInterval(LOCATION_FASTEST_INTERVAL);


 //final LocationRequest locationRequest1 = LocationRequest.create()
 // .setPriority(LocationRequest.PRIORITY_BALANCED_POWER_ACCURACY)
 // .setInterval((LocationPeriod * 1000))
 // .setFastestInterval(LOCATION_FASTEST_INTERVAL);

 // İhtiyaçlarımız doğrultusunda yapılandırdığımız LocationRequest in gerekliliklerini kontrol ederek, cihazın davranışının otomatik olarak ayarlanması için
 // gerekli Dialog penceresinin çıkmasını sağlar ve Kullanıcının onayını ister

 // Örnek olarak eğer PRIORITY_HIGH_ACCURACY olrarak belirlenmişse ve cihazın GPS i kapalı ise bir Dialog çıkartarak GPS i açmanızı ister
 LocationSettingsRequest locationSettingsRequest = new LocationSettingsRequest.Builder()
 .addLocationRequest(locationRequest)
 .setAlwaysShow(true)
 .build();

 PendingResult<LocationSettingsResult> result =
 LocationServices.SettingsApi.checkLocationSettings(_googleApiClient, locationSettingsRequest);

 // Gösterilen Dilag penceresi ile Kullanıcının etkileşimi sonucunun yakalar
 result.setResultCallback(new ResultCallback<LocationSettingsResult>() {
 @DebugLog
 @Override
 public void onResult(LocationSettingsResult locationSettingsResult) {

 // locationSettingsStates ile cihazın durumunu kontrol ederek akışımızı yönlendirebiliriz
 LocationSettingsStates locationSettingsStates = locationSettingsResult.getLocationSettingsStates();
 
 // Konum dinlenmeye başlanır
 // mLocationListener ile konum değişikliği yakalanır.
 FusedLocationApi.requestLocationUpdates(_googleApiClient, locationRequest, mLocationListener);
 
 //FusedLocationApi.requestLocationUpdates(_googleApiClient, locationRequest1, mLocationListener);
 
 }
 });


 }


 private static LocationListener mLocationListener = new LocationListener() {
 @Override
 public void onLocationChanged(Location location) {


 // Location değişikliğini burada yakalayabiliriz.
 // Belirlemiş olduğumuz periyodik zaman aralığında Location değişikliği kontrol edilecektir.
 }
 };


 
 public ForegroundService() {
 }

 // Bu fonksiyon yardımı ile başka bir sınıftan konum bilgisine erişebiliriz.
 // static olarak tanımlanmış olması bize kolaylık sağlıcaktır.
 public static Location getLastLocation() {
 if (_googleApiClient != null) {
 return FusedLocationApi.getLastLocation(_googleApiClient);
 } else {
 return null;
 }
 }


 // GoogleApiClient nesnemize erişmek istersek bu kod bloğunu kullanabiliriz.
 public static GoogleApiClient getGoogleApiClient() {
 return _googleApiClient;
 }

 
 public static void removeLocationUpdates() {
 if (_googleApiClient != null) {
 FusedLocationApi.removeLocationUpdates(_googleApiClient, mLocationListener);
 }
 }

 @DebugLog
 private static void LocationNotification(Location location, String subText) {
 NotificationManager notificationManager = (NotificationManager) mContext.getSystemService(Context.NOTIFICATION_SERVICE);
 //

 mBuilder.setSmallIcon(R.drawable.ic_stat_device_gps_fixed);
 mBuilder.setTicker("Konum");
 //.setSound(RingtoneManager.getDefaultUri(RingtoneManager.TYPE_NOTIFICATION))
 mBuilder.setSubText(subText);
 mBuilder.setOnlyAlertOnce(true);
 mBuilder.setPriority(Notification.PRIORITY_HIGH);


 if (location != null) {
 mBuilder
 .setContentTitle("Sapma: " + String.valueOf(location.getAccuracy()))
 .setContentText("Lat: " + String.valueOf(location.getLatitude()) + " , " +
 "Long: " + String.valueOf(location.getLongitude()))
 .setWhen(System.currentTimeMillis())
 .setShowWhen(true)
 .setUsesChronometer(true)
 .setContentIntent(pendingIntent);


 } else {
 mBuilder
 .setContentTitle("")
 .setContentText("");
 }


 notification = mBuilder.build();
 notificationManager.notify(LOCATION_NOTIF_ID, notification);
 }

 @DebugLog
 @Override
 public void onDestroy() {
 if (_googleApiClient != null) {
 if (_googleApiClient.isConnected()) {
 FusedLocationApi.removeLocationUpdates(_googleApiClient, mLocationListener);
 _googleApiClient.disconnect();
 }
 }
 PeriodicHandler.removeMessages(1);
 stopForeground(true);
 super.onDestroy();
 }


}
```

Tanımlamış olduğumuz servisi başlatmak için istediğimiz yerden ( Activity , Fragment vb.) aşağıdaki kod bloğunu çalıştırmamız yeterli olucaktır.

```java
Intent serviceIntent = new Intent(context, ForegroundService.class);
startService(serviceIntent);
```