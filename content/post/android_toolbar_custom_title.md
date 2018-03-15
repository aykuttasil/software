+++
thumbnailImage = ""
title = "Android Custom Toolbar Title"
tags = [
  "software","android","java","toolbar"
]
date = "2017-01-11T16:01:33+03:00"
desciption = ""
categories = [
  "yazilim",
  "android",
]
keywords = [
  "yazilim",
  "sofware","android","java","toolbar"
]
autoThumbnailImage = false
metaAlignment = "center"
thumbnailImagePosition = "top"
coverImage = "https://c8.staticflickr.com/8/7421/9339731831_9ba94f287c_k.jpg"

+++


Android Toolbar bileşeni çok yönlü bir yapıya sahiptir. Android Design Library  kullanarak ve AppBarLayout içerisinde tanımlanarak oldukça farklı şekillere bürünebilir.

Genel kullanımı aşağıdaki gibidir.

 
```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.design.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
 xmlns:app="http://schemas.android.com/apk/res-auto"
 xmlns:tools="http://schemas.android.com/tools"
 android:layout_width="match_parent"
 android:layout_height="match_parent"
 android:background="@color/primary_dark"
 tools:context=".MainActivity">

 <FrameLayout
 android:id="@+id/container"
 android:layout_width="match_parent"
 android:layout_height="match_parent">

 </FrameLayout>


 <android.support.design.widget.FloatingActionButton
 android:id="@+id/fab"
 android:layout_width="wrap_content"
 android:layout_height="wrap_content"
 android:layout_gravity="bottom|end"
 android:layout_margin="@dimen/fab_margin"
 android:src="@android:drawable/ic_dialog_email" />


 <android.support.design.widget.AppBarLayout
 android:layout_width="match_parent"
 android:layout_height="wrap_content"
 app:elevation="0dp">

 <android.support.v7.widget.Toolbar
 android:id="@+id/toolbar"
 android:layout_width="match_parent"
 android:layout_height="wrap_content"
 android:background="@color/primary_dark"
 android:minHeight="?attr/actionBarSize"
 app:popupTheme="@style/AppTheme.PopupOverlay"/>


 </android.support.design.widget.AppBarLayout>

</android.support.design.widget.CoordinatorLayout>
```

`app:elevation="0dp"`  // AppBarLayout içerisinde ki bu tanımlama default olarak tanımlanmış gölgeyi kaldırır.

Tanımlamış olduğumuz Toolbar a Activity içerisinden (ya da Fragment vb) erişerek gerekli tanımlamaları vs yapabilirsiniz.

 

**Toolbar Custom Title**

Toolbarınızı tanımladığınız yerde aşağıdaki değişiklikleri yaparak Title ınızı istediğiniz gibi özelleştirebilirsiniz.

```xml
<android.support.design.widget.AppBarLayout
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    app:elevation="0dp">

    <android.support.v7.widget.Toolbar
        android:id="@+id/toolbar"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="@color/primary_dark"
        android:minHeight="?attr/actionBarSize"
        app:popupTheme="@style/AppTheme.PopupOverlay">

        <TextView
            android:id="@+id/toolbar_title"
            style="@style/ToolbarTitleStyle"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_gravity="center" />
    </android.support.v7.widget.Toolbar>


</android.support.design.widget.AppBarLayout>
```

**FragmentX**

```java
 private void setToolbar() {
 Toolbar toolbar = (Toolbar) mActivity.findViewById(R.id.toolbar);
 mActivity.setSupportActionBar(toolbar);
 mActivity.getSupportActionBar().setHomeButtonEnabled(true);
 mActivity.getSupportActionBar().setDisplayHomeAsUpEnabled(true);
 mActivity.getSupportActionBar().setDisplayShowTitleEnabled(false); // Default olarak tanımlanmış Title ın gösterilmemesini belirtiyoruz.
 ((TextView) toolbar.findViewById(R.id.toolbar_title)).setText("SORU SOR"); // Custom olarak belirlenmiş TextView e text ataması yapıyoruz.
}
```
 
