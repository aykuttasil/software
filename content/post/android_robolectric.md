+++
autoThumbnailImage = false
coverImage = "https://c8.staticflickr.com/8/7421/9339731831_9ba94f287c_k.jpg"
date = "2017-01-11T00:31:52+03:00"
thumbnailImagePosition = "top"
title = "Robolectric ile UnitTest Yazımı"
#url = "android-robolectric-test"
keywords = [
  "yazilim",
  "sofware",
  "robolectric",
  "test"
]
tags = [
  "software","robolectric","android test"
]
thumbnailImage = ""
desciption = ""
metaAlignment = "center"
categories = [
  "yazilim",
  "test",
  "android",
  "java"
]

+++

Unit Test birçok yazıılımcı tarafından es geçillen ama bir o kadar da önemli ve yapılması gerekli olan bir durumdur.

Proje büyüdükçe ve ilerledikçe teste duyulan ihtiyaç ta doğru orantılı olarak artmaktadır.

Unit Test neden yapılır sorusunun daha ayrıntılı cevabı için unit test nedir nicin ve nasil yapilir bu yazıyı okuyabilirsiniz.

Android Studio da Robolectric kullanarak Unit Test Yazımı

Aşağıdaki adımları sırası ile ve düzgün bir şekilde uygularsanız herhangi bir sorun çıkmadan testi çalıştırabileceksiniz.(Ben gerektiğinden uzun bi zaman harcadım, siz harcamayın !)

 
**build.gradle**

```gradle
apply plugin: 'com.android.application'

android {
    compileSdkVersion 22
    buildToolsVersion "23.0.0"

    defaultConfig {
        applicationId "com.a.aykut.tryrobolectric1"
        minSdkVersion 16
        targetSdkVersion 21
        versionCode 1
        versionName "1.0"
    }
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
}

dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile 'com.android.support:appcompat-v7:22.2.1'
    compile 'com.jakewharton:butterknife:7.0.1'

    testCompile 'junit:junit:4.12'
    testCompile 'com.squareup.assertj:assertj-android:1.1.0'
    testCompile 'org.robolectric:robolectric:3.0'
}
```

**MainActivity**

```java
@Bind(R.id.button)
Button button;

@Bind(R.id.textView)
TextView textView;

@Override
protected void onCreate(Bundle savedInstanceState) {
  super.onCreate(savedInstanceState);
  setContentView(R.layout.activity_main);
  ButterKnife.bind(this);
}

@OnClick(R.id.button)
public void buttonClick(View vi) {
  textView.setText("tiklandi");
}
```
 

AndroidManifest.xml  dosyasında herhangi bir değişiklik yapmanıza gerek yok.

Test sınıfımızı yazmaya başlayalım.

İlk olarak Android Studio nun sol tarafında yer alan **Build Variant** tabından **Test Artifact** kısmını **Unit Tests** olarak değiştirin.

<a href="http://imgur.com/rEPljpe"><img src="http://i.imgur.com/rEPljpe.jpg" title="source: imgur.com" /></a>

Unit Test yazmak için **src** klasörüne test isminde bir klasör ve test klasörünün içine de **java** isimli bir klasör oluşturmanız gerekmektedir.

yani **src/test** ve **src/test/java** klasörlerini oluşturmalısınız.

<a href="http://imgur.com/gztY5gr"><img src="http://i.imgur.com/gztY5gr.jpg" title="source: imgur.com" /></a>


Oluşturmuş olduğunuz java klasörüne sağ tıklayarak new > Package diyin ve normal Package isminizle aynı isimde bir Package oluşturun.Burda biz com.a.aykut.tryrobolectric1 ismini kullandık.

Oluşturmuş olduğunuz Package sağ tıklayarak new > Java Class diyin ve test sınıfını oluşturun. Anlaşılabilirlik açısından Test*(Test Edilecek Sınıf ismi) olarak isimlendirebilirsiniz.

 

**TestMainActivity**

```java
import android.app.Activity;

import android.widget.Button;
import android.widget.TextView;

import org.junit.Before;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.robolectric.Robolectric;
import org.robolectric.RobolectricGradleTestRunner;
import org.robolectric.annotation.Config;

import static junit.framework.Assert.assertEquals;
import static junit.framework.Assert.assertTrue;

@RunWith(RobolectricGradleTestRunner.class)
@Config(constants = BuildConfig.class,sdk = 21)
public class TestMainActivity {

  Activity activity;
  TextView textView;
  Button button;

  @Before
  public void setUp() {
    activity = Robolectric.setupActivity(MainActivity.class);
    textView = (TextView) activity.findViewById(R.id.textView);
    button = (Button) activity.findViewById(R.id.button);
  }

  @Test
  public void shouldMainActivityNotBeNull() throws Exception {
    Robolectric.buildActivity(MainActivity.class).create().pause().resume().get();
    assertTrue(Robolectric.buildActivity(MainActivity.class).create().get() != null);
    String hello = new MainActivity().getResources().getString(R.string.hello_world);
    assertEquals(hello, "Hello world!");


  }

  @Test
  public void buttonClickChangeTextView() throws Exception {
    String text = textView.getText().toString();
    assertEquals(text, "Hello world!");
    button.performClick();
    text = textView.getText().toString();
    assertEquals(text, "tiklandi");

  }
}
```

Evet.Test sınıfımızı da yazdık.Artık çalıştırabiliriz.

Testimizi çalıştırmak için yazmış olduğumuz Test Sınıfına sağ tıklayarak Run diyoruz.

<a href="http://imgur.com/7STeLHy"><img src="http://i.imgur.com/7STeLHy.jpg" title="source: imgur.com" /></a>

 
Run dediğiniz de Test çalışacaktır. Fakat aşağıdaki hataya benzer bir hata alırsanız endişelenmeyin.

```
java.lang.RuntimeException: build\intermediates\bundles\debug\AndroidManifest.xml not found or not a file; it should point to your project's AndroidManifest.xml

at org.robolectric.manifest.AndroidManifest.validate(AndroidManifest.java:121)

at org.robolectric.manifest.AndroidManifest.getResourcePath(AndroidManifest.java:469)

at org.robolectric.manifest.AndroidManifest.getIncludedResourcePaths(AndroidManifest.java:475)

at org.robolectric.RobolectricTestRunner.createAppResourceLoader(RobolectricTestRunner.java:491)

at com.intellij.rt.execution.application.AppMain.main(AppMain.java:140)
.

.

.
```

<a href="http://imgur.com/1TpiUpG"><img src="http://i.imgur.com/1TpiUpG.jpg" title="source: imgur.com" /></a>

Resim de görüldüğü gibi **Edit Configurations** a tıklayın.

 
<a href="http://imgur.com/pFGTznR"><img src="http://i.imgur.com/pFGTznR.jpg" title="source: imgur.com" /></a>


Resim deki gibi **Working directory** yolunun sonuna **\app** ekleyin.

 

Ve şimdi tekrar Test Sınıfına sağ tıklayarak Run diyin.

Ve sonuç aşağıdaki gibi olmalıdır.


<a href="http://imgur.com/zIgjUDT"><img src="http://i.imgur.com/zIgjUDT.jpg" title="source: imgur.com" /></a>
 

Kaynak kodlarını aşağıdkai linkten indirebilirsiniz.

https://github.com/aykuttasil/RobolectricUnitTest
