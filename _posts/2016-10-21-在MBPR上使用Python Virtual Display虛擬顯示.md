---
bg: "rails.jpg"
layout: post
title:  在MBPR上使用Python Virtual Display虛擬顯示
crawlertitle: "在MBPR上使用Python Virtual Display虛擬顯示"
summary: "什麼都看不到，什麼都做得到"
date:   2016-10-21
categories: posts
tags: ['Mac','Python']
author: nps798
---

以前是在Rpi上做這件事情，現在搬到Mac來試試看！

## 必裝Python套件清單

{% highlight linenos %}
pyvirtualdisplay
pyautogui
selenium
Xlib
{% endhighlight %}

## 1.安裝Selenium

{% highlight linenos %}
sudo pip3 install selenium
{% endhighlight %}

#如果已經安裝，請記得更新更新的版本！


## 2.基礎程式測試

{% highlight linenos %}
from selenium import webdriver

browser = webdriver.Firefox()
browser.get('http://blog.medcode.in/') 

{% endhighlight %}

出現問題...
[Errno 2] No such file or directory: 'geckodriver'
卡關gg....
