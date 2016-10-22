---
bg: "rails.jpg"
layout: post
title:  在Raspberry Pi GPIO 控制 LED 初體驗
crawlertitle: "在Raspberry Pi GPIO 控制 LED 初體驗 (terminal+Python)"
summary: "上次做這種事好像是高二zzz"
date:   2016-10-22
categories: posts
tags: ['Raspberry pi','Maker','Python']
author: nps798
---

一直想來嚐試 Rpi 上的 GPIO，想要做溫濕度感測器或是遙控之類的東西。
今天到電子材料行先從 LED 入口（台南電子材料行還真的只有那幾家zzz...）

## 所需材料

- 麵包板
- 杜邦線？
- LED 數個
- 220 Ω 電組幾個
- Raspberry pi 3


## Step1 LED 插上麵包板備妥

[輸出]
正極接到 GPIO 任何一個 GPIO 出口
負極接到 GND

[電路]
很簡單的，正極過來接上一個電阻（我今天才知道電阻不分正負），接著LED燈，負極回來，回到Rpi GND。

＃注意：LED二極體，長端代表正極，短端代表負極。


## Step2 使用 Rpi Terminal 測試 GPIO 輸出

這一關可以直接用 命令讓 GPIO turn on/off，也可以趁機看看 LED 燈有沒有損壞。

先用以下指令，看一下現在各個 GPIO 的狀況。
{% highlight linenos %}
gpio readall
{% endhighlight %}

 +-----+-----+---------+------+---+---Pi 3---+---+------+---------+-----+-----+
 | BCM | wPi |   Name  | Mode | V | Physical | V | Mode | Name    | wPi | BCM |
 +-----+-----+---------+------+---+----++----+---+------+---------+-----+-----+
 |     |     |    3.3v |      |   |  1 || 2  |   |      | 5v      |     |     |
 |   2 |   8 |   SDA.1 |  OUT | 0 |  3 || 4  |   |      | 5V      |     |     |
 |   3 |   9 |   SCL.1 |   IN | 1 |  5 || 6  |   |      | 0v      |     |     |
 |   4 |   7 | GPIO. 7 |   IN | 1 |  7 || 8  | 0 | OUT  | TxD     | 15  | 14  |
 |     |     |      0v |      |   |  9 || 10 | 1 | ALT5 | RxD     | 16  | 15  |
 |  17 |   0 | GPIO. 0 |   IN | 0 | 11 || 12 | 0 | IN   | GPIO. 1 | 1   | 18  |
 |  27 |   2 | GPIO. 2 |   IN | 0 | 13 || 14 |   |      | 0v      |     |     |
 |  22 |   3 | GPIO. 3 |   IN | 0 | 15 || 16 | 0 | OUT  | GPIO. 4 | 4   | 23  |
 |     |     |    3.3v |      |   | 17 || 18 | 0 | IN   | GPIO. 5 | 5   | 24  |
 |  10 |  12 |    MOSI |   IN | 0 | 19 || 20 |   |      | 0v      |     |     |
 |   9 |  13 |    MISO |   IN | 0 | 21 || 22 | 0 | IN   | GPIO. 6 | 6   | 25  |
 |  11 |  14 |    SCLK |   IN | 0 | 23 || 24 | 1 | IN   | CE0     | 10  | 8   |
 |     |     |      0v |      |   | 25 || 26 | 1 | IN   | CE1     | 11  | 7   |
 |   0 |  30 |   SDA.0 |   IN | 1 | 27 || 28 | 1 | IN   | SCL.0   | 31  | 1   |
 |   5 |  21 | GPIO.21 |   IN | 1 | 29 || 30 |   |      | 0v      |     |     |
 |   6 |  22 | GPIO.22 |   IN | 1 | 31 || 32 | 0 | IN   | GPIO.26 | 26  | 12  |
 |  13 |  23 | GPIO.23 |   IN | 0 | 33 || 34 |   |      | 0v      |     |     |
 |  19 |  24 | GPIO.24 |   IN | 0 | 35 || 36 | 0 | IN   | GPIO.27 | 27  | 16  |
 |  26 |  25 | GPIO.25 |   IN | 0 | 37 || 38 | 0 | IN   | GPIO.28 | 28  | 20  |
 |     |     |      0v |      |   | 39 || 40 | 0 | IN   | GPIO.29 | 29  | 21  |
 +-----+-----+---------+------+---+----++----+---+------+---------+-----+-----+
 | BCM | wPi |   Name  | Mode | V | Physical | V | Mode | Name    | wPi | BCM |
 +-----+-----+---------+------+---+---Pi 3---+---+------+---------+-----+-----+

 如上表，目前我是將正極，接到 physical 15 的位置。因此我對照 Physical 15 到左邊的 wPi 欄位，顯示我是 wPi 3。

 為什麼這個重要呢？因為從 Terminal 如果要調整 GPIO 的 開與關，就要用 wPi 的號碼(3)
當然凡事有例外，在 Python 中，我們要使用的是 Physical 號碼(15)。

 {% highlight linenos %}
gpio mode 3 out
gpio write 3 1
gpio write 3 0
{% endhighlight %}

[講解]
第一行將 gpio 3 號改成 out
第二行將 3 號打開（此時LED會亮）
第三行將 3 號關閉（此時LED關閉）

## Step3 使用 Python 控制 GPIO

 {% highlight linenos Python%}
import time
import RPi.GPIO as GPIO

GPIO.setmode(GPIO.BOARD)

#設定LED pin變數
# 若是用 terminal 指令，則直接使用 
LED0 = 15

counter = 0

#設定為輸出

try:  
	GPIO.setup(LED0,GPIO.OUT)

	#迴圈10次
	while(counter < 10):
		GPIO.output(LED0,GPIO.HIGH)
		print("high")
		time.sleep(2)
		GPIO.output(LED0,GPIO.LOW)
		print("low")
		time.sleep(2)
		counter = counter + 1
  
except:  
	print("Other error or exception occurred!")
  
finally:
	print('final')

	# 一定要有這行，不然下次執行會 RuntimeWarning: This channel is already in use, continuing anyway.  
	GPIO.cleanup()

{% endhighlight %}

應該不難看懂喔！要注意的是這裡的 PIN 號碼要用 Physical ，所以我這裡改成用 15。




