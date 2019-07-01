+++
metaAlignment = "center"
keywords = [
  "yazilim",
  "sofware","android","java","palette","glide"
]
autoThumbnailImage = false
thumbnailImagePosition = "top"
tags = [
  "software","android","java","palette","glide"
]
coverImage = "https://c8.staticflickr.com/8/7421/9339731831_9ba94f287c_k.jpg"
title = "Android Glide ve Palette Kullanımı"
#url = "android-glide-ve-palette-kullanimi"
categories = [
  "yazilim",
  "android","java"
]
thumbnailImage = ""
date = "2017-01-11T17:11:30+03:00"
desciption = ""

+++

Google ın resim işlemleri için geliştirmiş olduğu Glide kütüphanesi Android kaynaklarını, cache mekanizmasını vs. verimli şekilde kullanarak uygulamanıza hız ve kalite kazandırır.

Sizde projenizde resimlerle ilgili herhangi bir işlem yapıyorsanız bu kütüphaneyi incelemenizi tavsiye ediyorum.

- **Github :** https://github.com/bumptech/glide

- **Gradle :** `compile ‘com.github.florent37:glidepalette:1.0.6’`

Google ın geliştirmiş olduğu Palette kütüphanesi ise, resimlerinizin renkleriyle ilgilenir. Daha farklı işlemler içinde kullanılabilir resim boyutlandırma vs. gibi. Ama renklerle ilgili işlemler için oldukça güzel bir kütüphanedir.

Link : [Android Developer](http://developer.android.com/reference/android/support/v7/graphics/Palette.html)

- `Gradle : compile ‘com.android.support:palette-v7:23.1.1’`

Bu iki güzel kütüphanenin birlikte kullanımı ile oldukça güzel işler çıkabilir

Bunun için de bir kütüphane mevcut 🙂

- **Github :** <https://github.com/florent37/GlidePalette>

```java
 Glide.with(mContext).load("ImageAdress")
                         // Her resim için farklı bir signature belirtmeliyiz. Bu sayede resimlerin tekrar tekrar yüklenmesini engellemiş oluruz.
                        .signature(new StringSignature("ImageSignature"))
                        .centerCrop()
                        .listener(GlidePalette.with("ImageAdress")
                                        .use(GlidePalette.Profile.VIBRANT_LIGHT)
                                        .intoTextColor(txt, GlidePalette.Swatch.BODY_TEXT_COLOR)
                                        .crossfade(true)

                                         // Belirttiğimiz ImageAdress den gelen resmimiz kullanılmaya hazır olduğunda burada yakalayabilir ve 
                                         // istediğimiz özelleştirmeyi yapabiliriz.
                                         // Biz burada yüklenen resmin palette.getDarkMutedColor(DefaultColor) fonksiyonu ile rengini yakalıyoruz ve CollapsingToolbar ın expand olduğu durumda ki title rengini değiştiriyoruz.
                                        .intoCallBack(new BitmapPalette.CallBack() {
                                            @Override
                                            public void onPaletteLoaded(@Nullable Palette palette) {
                                                mCollapsingToolbar.setExpandedTitleColor(palette.getDarkMutedColor(Color.BLACK));
                                            }
                                        })
                                        /*
                                        .setGlideListener(new RequestListener<String, GlideDrawable>() {
                                            @Override
                                            public boolean onException(Exception e, String model, Target<GlideDrawable> target, boolean isFirstResource) {
                                                return false;
                                            }

                                            @Override
                                            public boolean onResourceReady(GlideDrawable resource, String model, Target<GlideDrawable> target, boolean isFromMemoryCache, boolean isFirstResource) {
                                                return false;
                                            }
                                        })
                                        // optional: do stuff with the builder
                                        .setPaletteBuilderInterceptor(new BitmapPalette.PaletteBuilderInterceptor() {
                                            @NonNull
                                            @Override
                                            public Palette.Builder intercept(Palette.Builder builder) {
                                                return builder.resizeBitmapSize(100);
                                            }
                                        })
                                        */
                        )
                        .into(mCollapsingImageView);
```
