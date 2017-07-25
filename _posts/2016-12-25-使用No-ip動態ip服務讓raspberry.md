--- 
image: "img/rails.jpg" 
layout: post 
title: 使用No-ip動態ip服務讓raspberry 
summary: "" 
date: 2016-12-25 
categories: posts 
tags: ['RaspberryPi'] 
author: nps798
---

最近想把 Raspberry Pi 放在家中跑監視器功能，回學校可以透過網路慢慢監看它的運作情況。為此，有必要來使用一下已經發展很久的Dynamic Ip 對應網域的服務，從我小時候就有的啦ＲＲＲ
 
## 註冊 No-ip 服務

請先到 [no-ip官網](http://www.noip.com/) 註冊好一個帳號。
 
## Add Hostname

接著我們要來新增 Hostname (就是以後要連上RPi所使用的外部網域名稱Ｒ)，我們使用No-ip內部提供的免費網域。透過左選邊選擇Dynamic DNS - Hostname進入以下畫面。

[![]({{ site.images }}/161225_noip.png)]({{ site.images }}/161225_noip.png) 
 
填妥各種網域名字之後，Record Type保持A，新增完成。至此，可以測試一下，該網域名稱是否可以連得上，雖然現在還只是靜態相對，沒有動態連結。

確認OK沒問題，進行下一步，進入 Rpi 的設定方面

## 在Pi上安裝好 Dynamic Update Client (DUC)

新增folder for DUC，並進入該資料夾下載 DUC 完成，解壓縮，進入「noip-2.1.9-1」folder
{% highlight linenos %} 
sudo mkdir ~/noip
cd /home/pi/noip
sudo wget http://www.no-ip.com/client/linux/noip-duc-linux.tar.gz
sudo tar vzxf noip-duc-linux.tar.gz
cd noip-2.1.9-1
{% endhighlight %} 

使用以下指令安裝，第一個指令可能有問題，不管它，今天先直接跳到第二行。
{% highlight linenos %} 
sudo make
sudo make install
{% endhighlight %} 


接著會出現許多設置手續（No-ip之Email, password, 網址, ip更新時間等等)，若它有問 Please enter an update interval:[30] ，他是在問你執行這個no-ip duc之後，幾分鐘要重新更新一次ip（注意單位是分鐘），我先填上五分鐘測試一下。


若今日設定不滿意，未來還可以使用以下指令
第一行表示全部重新設定一遍
第二行表示設定時間（分鐘），設定為一分鐘更新一次。
{% highlight linenos %} 
sudo /usr/local/bin/noip2 -C
sudo /usr/local/bin/noip2 -U 1
{% endhighlight %}

當出現以下提示時，即代表安裝成功。

{% highlight linenos %}
Please enter the script/program name  noipDUC

New configuration file '/tmp/no-ip2.conf' created.

mv /tmp/no-ip2.conf /usr/local/etc/no-ip2.conf
{% endhighlight %} 

以下第一行為啟用 No-ip DUC 之指令。
啟用後，第二行可以讓我們查詢程式是否正在運行，若在運行，應該會看到第三行。
{% highlight linenos %}
sudo /usr/local/bin/noip2
ps aux|grep no
nobody    7649  0.0  0.1   2604  1748 ?        Ss   15:54   0:00 /usr/local/bin/noip2
{% endhighlight %} 

這時候可以故意去No-ip官網手動改一下ip，看這個 DUC 是否運作順利（可以等五分鐘等它實現，或是直接關閉程式重新 sudo /usr/local/bin/noip2 開啟程式執行更新ip）。

## Raspberry pi 3 開機就執行 No-ip DUC 服務

最後一哩路...最後一哩路...。若想要開機就自動執行 DUC 的話，就需要再 /etc/init.d 建立自動執行的 script。所以，我們先用以下指令，開啟一個新的 script。
{% highlight linenos %}
sudo nano /etc/init.d/noip_whatever_name
{% endhighlight %}

開啟文件後，複製以下程式碼貼上：

{% highlight linenos %}
### BEGIN INIT INFO
# Provides:          noip2
# Required-Start:    $syslog
# Required-Stop:     $syslog
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: noip.com client service
### END INIT INFO

# . /lib/lsb/init-functions
case "$1" in
    start)
        echo "Starting noip2."
        /usr/local/bin/noip2
    ;;
    stop)
        echo "Shutting down noip2."
        killall noip2
        #killproc /usr/local/bin/noip2
    ;;
    *)
        echo "Usage: $0 {start|stop}"
        exit 1
esac

exit 0
{% endhighlight %}


最後更改執行權限，設定一些東西，就可使用啦。

{% highlight linenos %}
sudo chmod +x /etc/init.d/noip_whatever_name
sudo update-rc.d noip_whatever_name defaults
{% endhighlight %}


測試部分與問題：
- [OK] 開啟 noip2 瞬間可以更新一次 ip
- [NOT OK] 開啟 noip2 一段時間後檢查更新ip
- [NOT OK] 開啟自動啟動 noip2 程式
