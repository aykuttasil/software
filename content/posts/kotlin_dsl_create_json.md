+++
author = "Aykut Asil"
categories = ["kotlin"]
comment = true
date = "2020-01-03T17:19:13+03:00"
description = "Kotlin DSL kullanarak Json Objesi nasıl oluşturulur ?"
draft = false
keywords = ["kotlin", "dsl", "json"]
lastmod = "2020-01-03T17:19:13+03:00"
tags = ["kotlin", "dsl", "json"]
title = "Kotlin DSL ile Json"
url = "create-json-with-kotlin-dsl"

+++

## Kotlin DSL kullanarak Json Objesi nasıl oluşturulur ?

Normalde **Json** objesi oluşturmak için aşağıdaki gibi bir yöntem izleriz.

```kotlin
val jsonObject = JSONObject()
jsonObject.put("name","Aykut")
jsonObject.put("age",20)
```

Bunu **Kotlin DSL** ile çok daha ergonomik bir şekilde hazırlayabiliriz.

Öncelikle **Json** isminde bir sınıf oluşturuyoruz ve **JsonObject** sınıfından kalıtım alıyoruz. Ve **DSL** kullanabilmek için gerekli custom **constructor** fonksiyonumuzu yazıyoruz.

```kotlin
class Json() : JSONObject() {
    constructor(json: Json.() -> Unit) : this() {
        this.init()
    }
}
```

**Json** objemizi hazırlarken kolaylık olsun diye bir **infix** fonksiyonu ekliyoruz. Bu sayede `"name" to "Aykut"` , `"age" to 20` gibi değerler girebiliriz.

```kotlin
class Json() : JSONObject() {
    constructor(json: Json.() -> Unit) : this() {
        this.init()
    }

    infix fun <T> String.to(value: T) {
        put(this, value)
    }
}
```

**Json** objemizi doldurmak aşağıdaki gibi hazırlayabiliriz.

```kotlin
val json = Json {
    "name" to "Aykut"
    "age" to 20
}
```

Oluşturduğumuz nesneyi print edersek aşağıdaki gibi bir çıktı elde ederiz.

```json
{"name":"Aykut","age":20}
```

## Kaynaklar

- <https://blog.mindorks.com/mastering-kotlin-dsl-in-android-step-by-step-guide>
