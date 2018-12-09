---
title: "Bash Scripts"
date: 2018-12-10T00:47:41+03:00
lastmod: 2018-12-10T00:47:41+03:00
draft: false
keywords: ["bash","script","sh","zsh","komut","command"]
description: "Bash Scripts"
tags: ["bash","script","sh"]
categories: ["bash","script"]
author: "Aykut Asil"
comment: true
toc: true
autoCollapseToc: false
postMetaInFooter: true
hiddenFromHomePage: false
---

# cat <<EOF

`cat <<EOF` komutu ile multiline bash script yazabiliriz. Son satır **EOF** oluncaya kadar satır oluşturulmaya devam eder.

**EOF** yerine herhangi bir belirteç yazılabilir. Genelde **EOF veya STOP** yazılır.


```bash
$ sql=$(cat <<EOF
SELECT foo, bar FROM db
WHERE foo='baz'
EOF
)
```

```bash
$ cat <<EOF > print.sh
#!/bin/bash
echo \$PWD
echo $PWD
EOF
```

Yukarıda ki bash çalıştırıldığında **print.sh** adında ve içeriği

```bash
#!/bin/bash
echo $PWD
echo /home/user
```

olan bir dosya oluşur.

```bash
$ cat <<EOF | grep 'ay' | tee ay-words.txt
foo
bar
baz
.
.
.
```

Yukarıda ki script ile son satır **EOF** olana dek yazılan tüm herşey içerisinden içinde **aykut** geçen kelimeler bulunarak **ay-words.txt** dosyası içine kayıt edilir.

---

# find

```bash
find . -type f -name '*debug*.apk'
```

Yukarıda ki komut ile, **komutun yazıldığı klasör içerisinde** tipi **f(dosya)** olan ve adında **debug** geçen tüm dosyalar listelenir.
