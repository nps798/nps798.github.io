---
bg: "rails.jpg"
layout: post
title:  於Raspberry pi 3 安裝 realVNC Server紀錄
crawlertitle: "於Raspberry pi 3 安裝 realVNC Server紀錄"
summary: "以後就不用再用實體螢幕"
date:   2016-10-08
categories: posts
tags: ['Raspberry Pi']
author: nps798
---

打在這裡做個紀錄，方便日後回找。

  

## 1. 在樹莓派的終端機執行底下兩行敘述，下載最新版的RealVNC軟體並解壓縮： 

{% highlight linenos %}
curl -L -o VNC.tar.gz https://www.realvnc.com/download/binary/latest/debian/arm/ 
tar xvf VNC.tar.gz 
{% endhighlight %}


## 2. 解壓完成後，安裝 Server 版本

// 其實去看目錄會有 Server（伺服器）和Viewer（用戶端）兩個套件，安裝 Server 版本是讓你的 Raspberry pi 可以給別的電腦連上來看，安裝 Viewer 版本是讓你可以用 Raspberry pi 以 VNC 連上去看別台電腦。

{% highlight linenos %}
sudo dpkg -i VNC-Server-5.3.2-Linux-ARM.deb 
{% endhighlight %}


## 3. 輸入Free授權碼

{% highlight linenos %}
sudo vnclicense -add 這裡放授權碼
{% endhighlight %}

## 4. 設定密碼For Service Mode

{% highlight linenos %}
sudo vncpasswd -user
{% endhighlight %}

接著他會請你輸入兩次密碼，做為確認。

## 5. 啟動 VNC Server
 
按照官方的步驟，啟動 VNC Server 應該用 sudo /etc/init.d/vncserver-x11-serviced start 指令。但不知道為什麼我安裝起來在 /etc/init.d 就是沒 vncserver-x11-serviced 這個檔案，因此看了相關說明，改用以下指令啟動 Server

{% highlight linenos %}
sudo systemctl start vncserver-x11-serviced.service
{% endhighlight %}

## 5. 設定開機自動啟動

同理，官方給的 sudo update-rc.d vncserver-x11-serviced defaults 指令皆顯示 not found。因此改用以下方法設定。

{% highlight linenos %}
sudo systemctl enable vncserver-x11-serviced.service

{% endhighlight %}


 
## 參考教學

https://www.realvnc.com/docs/man/vncserver-x11-serviced.html
