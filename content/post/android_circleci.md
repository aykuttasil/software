---
title: "Android CircleCI Configuration"
date: 2018-12-01T14:20:45+03:00
lastmod: 2018-12-03T14:20:45+03:00
draft: false
keywords: ["android","ci","cd","continuous-integration","continuous-deployment","circleci","build","test","deployment"]
description: "Android Circle CI/CD"
tags: ["android","ci","cd","continuous-integration","continuous-deployment","circleci","build","test","deployment"]
categories: ["CI/CD","android","mobile"]
author: "Aykut Asil"
comment: true
toc: true
autoCollapseToc: false
postMetaInFooter: true
hiddenFromHomePage: false
---

# CircleCI Android Yapılandırması

## .circleci/config.yml

```yml
version: 2
jobs:
  build:
    working_directory: ~/code
    docker:
      - image: circleci/android:api-28-alpha
    environment:
      JVM_OPTS: -Xmx3200m
    steps:
      - checkout
      - run:
          name: Initialize Keystore File
          command: echo $KEYSTORE_BASE64 | base64 --decode > app/aykutasilkeystore
      - run:
          name: Initialize Fabric Properties
          command: |
            echo "apiKey=$FABRIC_API_KEY" >> app/fabric.properties
            echo "apiSecret=$FABRIC_API_SECRET" >> app/fabric.properties
            cat app/fabric.properties
      - run:
          name: Initialize Keystore File
          command: |
             echo "signingKeyAlias=$KEYSTORE_KEY_ALIAS" >> keystore.properties
             echo "signingKeyAliasPassword=$KEYSTORE_KEY_ALIAS_PASSWORD" >>  keystore.properties
             echo "signingStoreFile=$KEYSTORE_STORE_FILE" >>  keystore.properties
             echo "signingStorePassword=$KEYSTORE_STORE_PASSWORD" >>  keystore.properties
             cat keystore.properties
      - restore_cache:
          key: jars-{{ checksum "build.gradle" }}-{{ checksum  "app/build.gradle" }}
      - run:
          name: Download Dependencies
          command: ./gradlew androidDependencies
      - save_cache:
          paths:
            - ~/.gradle
          key: jars-{{ checksum "build.gradle" }}-{{ checksum  "app/build.gradle" }}
      - store_artifacts:
          path: app/build/reports
          destination: reports
      - store_test_results:
          path: app/build/test-results
      - run:
          command: echo "Current Branch:" ${CIRCLE_BRANCH}
      - run:
          name: Initial build
          command: ./gradlew assembleDebug assembleRelease --no-daemon --stacktrace
      - store_artifacts:
          path: app/build/outputs/apk/
          destination: apks/
      - deploy:
          name: Deploy to Fabric
          command: ./gradlew crashlyticsUploadDistributionRelease --stacktrace --debug --no-daemon
workflows:
  version: 2
  workflow:
    jobs:
    - build
```

## app/build.gradle

```bash
apply plugin: 'com.android.application'
apply plugin: 'kotlin-android'
apply plugin: 'kotlin-android-extensions'
apply plugin: 'io.fabric'


android {
    compileSdkVersion 28

    def fabricPropertiesFile = rootProject.file("app/fabric.properties")
    def fabricProperties = new Properties()
    if (fabricPropertiesFile.exists()) {
        fabricProperties.load(new FileInputStream(fabricPropertiesFile))
    }

    defaultConfig {
        applicationId "com.aykutasil.circlecitest"
        minSdkVersion 21
        targetSdkVersion 28
        versionCode 1
        versionName "1.0"
        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"

        ext.betaDistributionEmails = "aykuttasil@hotmail.com"
        ext.betaDistributionNotifications = true
        resValue "string", "io.fabric.ApiKey", fabricProperties['apiKey']
    }

    def keystorePropertiesFile = rootProject.file("keystore.properties")
    def keystoreProperties = new Properties()
    if (keystorePropertiesFile.exists()) {
        keystoreProperties.load(new FileInputStream(keystorePropertiesFile))
    }

    signingConfigs {
        config {
            keyAlias keystoreProperties['signingKeyAlias']
            keyPassword keystoreProperties['signingKeyAliasPassword']
            storeFile file(keystoreProperties['signingStoreFile'])
            storePassword keystoreProperties['signingStorePassword']
        }
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
            signingConfig signingConfigs.config
        }
    }
}

dependencies {
    implementation fileTree(include: ['*.jar'], dir: 'libs')
    implementation "org.jetbrains.kotlin:kotlin-stdlib-jdk7:$kotlin_version"
    implementation 'com.android.support:appcompat-v7:28.0.0'
    implementation 'com.android.support.constraint:constraint-layout:1.1.3'
    testImplementation 'junit:junit:4.12'
    androidTestImplementation 'com.android.support.test:runner:1.0.2'
    androidTestImplementation 'com.android.support.test.espresso:espresso-core:3.0.2'
    implementation('com.crashlytics.sdk.android:crashlytics:2.9.6@aar') {
        transitive = true
    }
}
```

## Ne Yaptık ?

Yukarıda ki örnek **gradle** dosyasında log için kullanılan **Crashlytics**' apiKey bilgilerini ve **apk** imzalama için gerekli bilgileri başka dosyalardan okuyayarak gizliliği sağladığımızı görüyorsunuz.

Bu bilgileri okuğumuz **keystore.properties** ve **fabric.properties** dosyalarını **VCS** içerisine eklemiyoruz. Yani **.gitignore** dosyamıza ekleyerek **github, bitbucket** gibi **VCS** platformlarına gönderimini engelliyoruz.

Peki bu dosyaları **VCS** platformuna göndermemiş isek **CI/CD** platformu projemizi derlemeye çalışırken **gradle** dosyası içerisinde ki bu bilgileri nasıl okuyacak?

İşte bunun için farklı yöntemler mevcut. Bunlardan ikisi yukarıda ki örnekte mevcut.

### 1. Base64 Yöntemi

Yukarıda ki **.yml** dosyasını incelediğinizde `command: echo $KEYSTORE_BASE64 | base64 --decode > app/aykutasilkeystore` şeklinde bir satır göreceksiniz. Burada yaptığımız şey mevcut **keystore** dosyamızın **base64** değerini alarak **CI/CD** platformuna **environment variable** olarak kayıt etmek. Ve daha sonra **yml** dosyasında bu **environment variable ($KEYSTORE_BASE64)** değerini okuyarak `base64 --decode` komutu ile **decode** ederek `> app/aykutasilkeystore` yoluna bu dosyayı ekliyoruz. Ve artık **gradle** dosyamız ilgili dosyayı(keystore) bulacağı için sorun oluşmayacak ve düzgün bir şekilde projemiz derlenecektir.

### 2. Environment Variable

Aslında burada da bir öncekine benzer bir yapılandırma söz konusu. Tek fark okunan **environment variable** değerini ilgili dosyaya direk olarak yazmak.

Yukarıda ki **yml** dosyasında `echo "apiKey=$FABRIC_API_KEY" >> app/fabric.properties` satırında direk olarak **$FABRIC_API_KEY** değeri okunup **app/fabric.properties** yolunda **fabric.properties** isimli dosyanın oluşması sağlanmıştır.