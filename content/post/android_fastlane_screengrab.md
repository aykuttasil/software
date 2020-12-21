---
author: "Aykut Asil"
date: 2020-11-04T13:14:18+03:00
lastmod: 2020-11-04T13:14:18+03:00
title: "Android Fastlane Screengrab Kurulumu"
url: "fastlane-screengrab"
weight: 10
keywords: ["software","android","mobil","fastlane","screengrab","automation","ci/cd"]
description: "Fastlane "
tags: ["software","android","mobil","fastlane","screengrab","automation","ci/cd"]
categories: ["android","mobil"]
---

**Fastlane** kurulumunu henüz tamamlamadıysanız sizi [buraya](/fastlane-setup) alalım.

### Screengrab

- Mobil **ekran görüntüsü** alımını otomatize eden bu aracın kullanımı için öncelikle aşağıdaki komut ile ilgili aracı yüklüyoruz.

```bash
    sudo gem install screengrab
```

- Daha sonra **AndroidManifest.xml** dosyasına aşağıdaki bloğu eklemeliyiz.

```xml
 <!-- Allows unlocking your device and activating its screen so UI tests can succeed -->
  <uses-permission android:name="android.permission.DISABLE_KEYGUARD" />
  <uses-permission android:name="android.permission.WAKE_LOCK" />

  <!-- Allows for storing and retrieving screenshots -->
  <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
  <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />

  <!-- Allows changing locales -->
  <uses-permission xmlns:tools="http://schemas.android.com/tools"
    android:name="android.permission.CHANGE_CONFIGURATION"
    tools:ignore="ProtectedPermissions" />
```

- **Gradle** dosyasını aşağıdaki gibi güncellemeliyiz.

```gradle

android {

    ...

    defaultConfig {
        applicationId "com.example.myapplication"
        minSdkVersion 21
        targetSdkVersion 30
        versionCode 1
        versionName "1.0"

        testInstrumentationRunner "androidx.test.runner.AndroidJUnitRunner"
    }

    ...
}

dependencies {
    implementation "org.jetbrains.kotlin:kotlin-stdlib:$kotlin_version"
    implementation 'androidx.core:core-ktx:1.3.2'
    implementation 'androidx.appcompat:appcompat:1.2.0'
    implementation 'com.google.android.material:material:1.2.1'
    implementation 'androidx.constraintlayout:constraintlayout:2.0.4'

    testImplementation 'junit:junit:4.13.1'

    androidTestImplementation 'androidx.test.ext:junit:1.1.2'
    androidTestImplementation 'androidx.test:rules:1.3.0'
    androidTestImplementation 'androidx.test.espresso:espresso-core:3.3.0'

    androidTestImplementation 'tools.fastlane:screengrab:2.0.0'
}
```

- Daha sonra **app/src/androidTest/** klasörü altında, **ExampleInstrumentedTest.kt** isimli dosyamızı oluşturuyoruz ve aşağıdaki gibi düzenliyoruz.


```kotlin
package com.example.myapplication

import androidx.test.espresso.Espresso
import androidx.test.espresso.action.ViewActions
import androidx.test.espresso.assertion.ViewAssertions
import androidx.test.espresso.matcher.ViewMatchers
import androidx.test.platform.app.InstrumentationRegistry
import androidx.test.rule.ActivityTestRule
import org.junit.Assert.assertEquals
import org.junit.Rule
import org.junit.Test
import org.junit.runner.RunWith
import org.junit.runners.JUnit4
import tools.fastlane.screengrab.Screengrab
import tools.fastlane.screengrab.UiAutomatorScreenshotStrategy
import tools.fastlane.screengrab.locale.LocaleTestRule

/**
 * Instrumented test, which will execute on an Android device.
 *
 * See [testing documentation](http://d.android.com/tools/testing).
 */
@RunWith(JUnit4::class)
class ExampleInstrumentedTest {

    // JVMField needed!
    @Rule
    @JvmField
    val localeTestRule = LocaleTestRule()

    @get:Rule
    var activityRule = ActivityTestRule(MainActivity::class.java, false, false)

    @Test
    fun testTakeScreenshot() {
        activityRule.launchActivity(null)
        //1
        Screengrab.setDefaultScreenshotStrategy(UiAutomatorScreenshotStrategy())

        Espresso.onView(ViewMatchers.withId(R.id.askButton))
            .check(ViewAssertions.matches(ViewMatchers.isDisplayed()))

        //2
        Screengrab.screenshot("beforeFabClick")

        //3
        Espresso.onView(ViewMatchers.withId(R.id.askButton)).perform(ViewActions.click())

        //4
        Screengrab.screenshot("afterFabClick")
    }

    @Test
    fun useAppContext() {
        // Context of the app under test.
        val appContext = InstrumentationRegistry.getInstrumentation().targetContext
        assertEquals("com.example.myapplication", appContext.packageName)
    }
}
```

**MainActivity** layout dosyası aşağıdaki gibidir.

**activity_main.xml**

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World!"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <Button
        android:id="@+id/askButton"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Button"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/textView" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

- Daha sonra aşağıdaki komut ile **debug** ve **androidTest** apk'larının oluşturulmasını sağlarız.

```bash
./gradlew assembleDebug assembleAndroidTest
```

- Aşağıdaki komut ile **fastlane/Screengrabfile** dosyasının oluşmasını sağlıyoruz.

```bash
bundle exec fastlane screengrab init
```

Ve aşağıdaki gibi ilgili yerleri uygulamamıza yönelik olarak düzenliyoruz.

**Screengrabfile**

```
# remove the leading '#' to uncomment lines

app_package_name('com.example.myapplication')
# use_tests_in_packages(['your.screenshot.tests.package'])

app_apk_path('app/build/outputs/apk/debug/app-debug.apk')
tests_apk_path('app/build/outputs/apk/androidTest/debug/app-debug-androidTest.apk')

locales(['tr-TR'])

# clear all previously generated screenshots in your local output directory before creating new ones
clear_previous_screenshots(true)

# For more information about all available options run
#   fastlane screengrab --help

```

### Screengrab komutunun çalıştırılması

> Note: If you run an emulator with API 24 or above, you must configure it with the Google APIs target. An emulator with Google Play won’t work because adb needs to run as root. That’s only possible with the Google APIs target.
However, if you run a device or emulator with API 23 or below, either option will work. See comment #15788 under fastlane issues for more information.

- Yani, Eğer **apk**'nızı bir android emülatörde koşturacak iseniz, emülatörünüzü **Google APIs** target olarak kurulmuş olması gerekmekte.

- Eğer fiziksel cihazınızda çalıştıracak iseniz **Geliştirici Seçenekleri > USB debugging(Security Settings)** seçeneğinin açık olması gerekmektedir.

- Komut satırının gerekli komutları çalıştırabilmesi için aşağıdaki şekilde **PATH** güncellenmiş olmalıdır.

```bash
# Path to Android SDK
export ANDROID_HOME=$HOME/Library/Android/sdk

# Path to Android platform tools (adb, fastboot, etc)
export ANDROID_PLATFORM_TOOLS="$ANDROID_HOME/platform-tools"

# Path to Android tools (aapt, apksigner, zipalign, etc)
export ANDROID_TOOLS="$ANDROID_HOME/build-tools/30.0.1/"

# Add all to the path
export PATH="$PATH:$ANDROID_PLATFORM_TOOLS:$ANDROID_TOOLS"
```

Ve tüm ayarlamalar bittiğine göre artık komutumuzu çalıştırabiliriz.

```bash
bundle exec fastlane screengrab

```


Tüm bu adımları **Fastfile** dosyasına **lane** olarak ekleyebiliriz.

```
desc "Build debug and test APK for screenshots"
lane :build_for_screengrab do
  gradle(
    task: 'clean'
  )
  gradle(
    task: 'assemble',
    build_type: 'Debug'
  )
  gradle(
    task: 'assemble',
    build_type: 'AndroidTest'
  )
end
```

Ve aşağıdaki komut ile tüm akışı çalıştırabiliriz.

```
bundle exec fastlane build_for_screengrab && bundle exec fastlane screengrab
```

> Örnek proje için;

- <https://github.com/aykuttasil/fastlane-sample>

### Kaynaklar

- <https://fastlane.tools/>
- <https://www.raywenderlich.com/10187451-fastlane-tutorial-for-android-getting-started>
- <https://docs.fastlane.tools/getting-started/android/screenshots/>
- <https://docs.fastlane.tools/actions/capture_android_screenshots/>