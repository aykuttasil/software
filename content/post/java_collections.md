+++
metaAlignment = "center"
autoThumbnailImage = false
desciption = ""
date = "2017-01-11T16:10:10+03:00"
thumbnailImage = ""
coverImage = "https://c8.staticflickr.com/8/7421/9339731831_9ba94f287c_k.jpg"
categories = [
  "yazilim",
  "android",
  "java"
]
keywords = [
  "yazilim",
  "sofware","android","java","collections","swap"
]
title = "Java Collections"
tags = [
  "software","android","java","collections","swap"
]
thumbnailImagePosition = "top"

+++

### **Collections.swap**

**Swap** kelime anlamı ile takas anlamına gelmektedir. Mevcut dizimiz içerisinde elemanların yerlerini değiştirmeye yarar.

```java
private static final String[] STRINGS = new String[]{
        "1", "2", "3", "4", "5"
};

private final List mItems = new ArrayList<>();
public void setArray()
{
  mItems.addAll(Arrays.asList(STRINGS));
}
mItems listemizi yazdırdığımızda sonuç şu şekilde olacaktır.

//  1,2,3,4,5

private void elemanYerDegistir()
{
  Collections.swap(mItems, 0, 4);
}
mItems listemizi yazdırdığımızda sonuç şu şekilde olacaktır.

//  5,2,3,4,1
```
