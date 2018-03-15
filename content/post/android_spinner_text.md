+++
thumbnailImagePosition = "top"
thumbnailImage = ""
metaAlignment = "center"
tags = [
  "software",
  "android",
  "java",
  "spinner"

]
desciption = "Dinamik Olarak Spinner Text Güncelleme"
coverImage = "https://c8.staticflickr.com/8/7421/9339731831_9ba94f287c_k.jpg"
date = "2017-01-10T23:00:03+03:00"
categories = [
  "yazilim",
  "android"
]
keywords = [
  "yazilim",
  "sofware",
  "android",
  "java",
  "spinner"
]
autoThumbnailImage = false
title = "Android Spinner Text Güncelleme"
#url = "android-spinner-dynamic-text"

+++


## Dinamik Olarak Spinner Text Güncelleme

Android de Spinner yapısı açılır menü (dropdown) olarak kullanlan kullanışlı bir componenttir.

Farklı ihtiyaçlarınız doğrultusunda Spinner ınızın elemanlarının değerini değiştirmek isteyebilirsiniz.

Örneğin bir ListView iniz var.Ve içerisinde aynı kategoriden ama değişik durumlara sahip itemlar var ve siz bu itemları durumuna göre gruplandırıp sayısını Spinner da göstermek istiyorsunuz. Bunun için aşağıda belirtecek olduğum yapıyı kullanabilirsiniz.

 

Öncellikle String.xml dosyasına Spinner ımızda göstereceğimiz elemanları tanımlıyoruz.

**String.xml**

```xml
<string name="pazartesi">Pazartesi</string>
<string name="sali">Salı</string>
<string name="carsamba">Çarşamba</string>
<string name="persembe">Perşembe</string>
<string name="cuma">Cuma</string>
<string name="cumartesi">Cumartesi</string>
<string name="pazar">Pazar</string>


<string-array name="array_gonderi_list_haftalik">
    <item>@string/pazartesi</item>
    <item>@string/sali</item>
    <item>@string/carsamba</item>
    <item>@string/persembe</item>
    <item>@string/cuma</item>
    <item>@string/cumartesi</item>
    <item>@string/pazar</item>
</string-array>
``` 

**SpinnerHelper.java**

```java
public static String[] getChangedSpinnerItemText(Context context) {
    String[] originalList = context.getResources().getStringArray(R.array.array_gonderi_list_haftalik);

    Resources resources = context.getResources();
    for (int a = 0; a < originalList.length; a++) {
        int count = 0;
        if (originalList[a].equals(resources.getString(R.string.pazartesi))) {
            count = 2;
            originalList[a] += " ( " + count + " )";
        } else if (originalList[a].equals(resources.getString(R.string.sali))) {
            count = 4;
            originalList[a] += " ( " + count + " )";
        } else if (originalList[a].equals(resources.getString(R.string.carsamba))) {
            count = 7;
            originalList[a] += " ( " + count + " )";
        } else {
            count = 8
            originalList[a] += " ( " + count + " )";
        }
    }
    return originalList;
}
```

Daha sonra spinnerımızı tanımladığımız yere giderek spinnerımızı yapılandırıyoruz.

**FragmentX.java**

```java
public void setSpinnerNavToolbar() {

 // Guncelledikten sonra spinner görünümü örneği
 // Pazartesi ( 2 )
 // Salı ( 4 )
 // ... 
 
 // Spinner itemlarının güncellenmiş halini barındıran listeyi getiriyoruz.
 String[] changedList = getChangedSpinnerItemText(context);

 // Spinner adapterımıza eklemeler yapılmış String Arrayi veriyoruz.
 ArrayAdapter<CharSequence> adapter = new ArrayAdapter<CharSequence>(mContext, R.layout.spinner_nav_item_layout,changedList);
 Spinner spinner = (Spinner) findViewById(R.id.spinner_nav);
 spiner.setAdapter(adapter);
 spinner.setVisibility(NavigationView.VISIBLE);
 spinner.setOnItemSelectedListener(new AdapterView.OnItemSelectedListener() {
 @Override
 public void onItemSelected(AdapterView<?> parent, View view, int position, long id) {

 // Original Liste elemanlarımızı getiriyoruz 
 String[] originalList = context.getResources().getStringArray(R.array.array_gonderi_list_haftalik);
 
 // Original Liste elemanını baz alarak işlem yapmak 
 // Bu sayede Pazartesi , Salı gibi değerleri alıyoruz. Değiştirilmiş (güncellenmiş) değerleri sadece görünüm için kullanıyoruz.
 String secilendeger = originalList[position];


 // Pazartesi seçildiğinde yapmak istediğimiz işlemleri burada belirtebiliriz.
 if (secilendeger.equals(getResources().getString(R.string.pazartesi))) {

 } 
 // Salı seçildiğinde yapmak istediğimiz işlemleri burada belirtebiliriz.
 else if(secilendeger.equals(getResources().getString(R.string.pazartesi)))
 {

 }
 }

 @DebugLog
 @Override
 public void onNothingSelected(AdapterView<?> parent) {

 }
 });
}
```
 


