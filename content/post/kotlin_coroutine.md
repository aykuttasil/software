---
title: "Kotlin Coroutine"
date: 2019-03-02T19:22:51+03:00
lastmod: 2019-03-02T19:22:51+03:00
draft: false
keywords: ["kotlin","coroutine","concurrency"]
description: "Kotlin Coroutine"
tags: ["kotlin","coroutine","concurrency"]
categories: ["kotlin"]
author: "Aykut Asil"
comment: true
toc: true
autoCollapseToc: false
postMetaInFooter: true
hiddenFromHomePage: false
---

# Kotlin Coroutine

**Coroutine**'i thread'e benzetebiliriz. Amaçları aynıdır fakat işleyiş şekillleri farklıdır.

**Thread**'ler **CPU** üzerinde başlatılan birbirinden izole yapılardır. Ve **thread**lerin yönetimini **CPU** düzenler. Tek çekirdekli bir bilgisayarda oluşturulan 5 thread düşünelim. **CPU** bu 5 thread arasında teker teker ama sadece her zaman için tek bir tanesinde işlem yapar. Yani aynı anda yapmaz. Bu geçişleri çok hızlı yaptığı için son kullanıcı gözünden sanki aynı anda yapıyormuş gibi görünür. 4 çekirdekli bilgisayarda ise **aynı anda** 4 thread çalışabilir demektir.

![Thread](/image/concurrency_is_not_parallelism.png "Thread")

**Coroutine**'lerin çalışma biçimi **thread**ler gibi değildir. **Coroutine**'lerin yönetimi kotlin runtime tarafından yapılır. Arka tarafta **thread**'leri kullanır fakat nasıl kullanılacağını kotlin runtime belirler. Bir thread'de başlatılan **coroutine** başka bir thread'de devam edebilir. Bunun yönetimi dediğimiz gibi kotlin runtime tarafından otomatik yapılır.

![Coroutine](/image/coroutine.png "Coroutine")

---

# Kaynaklar

- <https://proandroiddev.com/kotlin-coroutines-channels-csp-android-db441400965f>