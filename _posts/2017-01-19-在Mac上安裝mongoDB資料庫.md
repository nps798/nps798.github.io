--- 
bg: "tools.jpg" 
layout: post 
title: 在Mac上安裝mongoDB資料庫 
summary: "NoSQL來惹" 
date: 2017-01-19 
categories: posts 
tags: ['MongoDB'] 
author: nps798 
---


NoSQL是近年來興盛的資料庫架構，不同於傳統的關聯式資料庫（ex. MySQL）等，NoSQL具有支援大量存取、高擴充的特性。以下就來嘗試看看。

[![][image1]][image1]

[image1]: {{ site.images }}/2017-01-19-在Mac上安裝mongoDB資料庫_monogodblogo.jpg


## 第一步 使用 brew 安裝 mongoDB

{% highlight linenos %} 
brew update 
brew install mongodb
{% endhighlight %} 


## 第二步 設定資料庫儲存路徑

預設之下，還要設定存放資料庫的目錄，預設為 **/data/db**

{% highlight linenos %} 
sudo mkdir -p /data/db
sudo chown -R 你的使用者名稱 /data/db
{% endhighlight %} 

第二行是為了避免接下來啟動mongoDB因為權限被拒絕
***exception in initAndListen: 20 Attempted to create a lock file on a read-only directory: /data/db, terminating***
網路上有人說用 chmod 改變 /data/db 存取權限，我這邊先用 chown 改變該資料夾擁有者，一樣可以用。（只是不知道哪個方法比較好）



## 第三步 啟用 mongoDB

使用以下指令啟動資料庫服務：

{% highlight linenos %} 
mongod
{% endhighlight %}

接著，terminal 會顯示...

NETWORK  [thread1] waiting for connections on port 27017

代表資料庫服務已經在運行，這時候按下「⌘ + N」開啟另外一個terminal，使用 mongo 指令連線上資料庫。使用 show dbs 顯示所有資料庫。

## MongoDB 終端機指令小記錄

show dbs：顯示目前所有資料庫
use TEST：使用TEST資料庫，如果TEST不存在，則自動建立之。
db.TEST.save({name: "Jack", "age": 20}); 將左方之 json儲存進資料庫
db.TEST.find(); 顯示所有資料庫現存內容
db.TEST.findOne({"age":20}); 顯示 age 欄位等於 20 者
db.TEST.find({$where: "this.age > 19"}); 顯示 age 欄位數值大於19者

