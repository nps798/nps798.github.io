---
bg: "rails.jpg"
layout: post
title:  Firebase入門使用與python結合
crawlertitle: "Firebase入門使用與python結合"
summary: "不用再自建mysql資料庫惹XD"
date:   2016-10-09
categories: posts
tags: ['Firebase','Python',''ㄈ]
author: nps798
---


# Firebase入門使用與python結合
  

## 1. 初始註冊與設定。

請先到Firebase官方網站，註冊一個帳號，並依據指示，建立一個新專案。

[Firebase官方網站](https://www.firebase.com/)

## 2. 於 Raspberry pi 上安裝 python-firebase。

{% highlight linenos %}
pip install -e git://github.com/mikexstudios/python-firebase.git#egg=python-firebase
{% endhighlight %}


## 2. 於 Raspberry pi 上安裝 pyrebase。


{% highlight linenos %}
sudo pip3 install pyrebase
{% endhighlight %}


## 3. 出現錯誤 ImportError: cannot import name 'IncompleteRead'

根據Stack Overflow 的解釋，以以下指定更新 pip 套件即可。

{% highlight linenos %}
sudo easy_install3 -U pip
{% endhighlight %}


## 4. 撰寫 python 連線程式碼。

{% highlight linenos python %}

import pyrebase

config = {

  "authDomain": "<APPNAME>.firebaseio.com",
  "databaseURL": "https://<APPNAME>.firebaseio.com/",

}

firebase = pyrebase.initialize_app(config)

db = firebase.database()
print(db.child("BTCbuy"))
{% endhighlight %}

## 5. 出現錯誤 ImportError: No module named 'requests.packages.urllib3.contrib.appengine'

使用 sudo pip3 uninstall python-firebase，卻跟我說不能移除。只好手動至 /usr/local/lib/python3.4/dist-packages/ 與 /home/pi/src/ 移除相關 python-firebase 檔案，再以原指令重新安裝 python-firebase 套件。

執行 firebase 的 HTTP 連線過程中，如果出現以下問題
TypeError: __init__() got an unexpected keyword argument 'strict'

可以藉由更新 requests 套件解決：

{% highlight linenos %}
sudo pip3 install requests --upgrade
{% endhighlight %}
