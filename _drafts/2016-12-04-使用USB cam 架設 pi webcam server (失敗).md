--- 
bg: "rails.jpg" 
layout: post 
title: 使用USB Webcam架設Pi 3家用監視器系統
summary: "期中考QQ" 
date: 2016-12-04 
categories: posts 
tags: ['Raspberry Pi','Webcam'] 
author: nps798 
---

好久沒用這個Webcam了，趁期中考結束、心血管Block還沒開始的假日，趕緊來嘗試新的應用。Webcam是用一般的USB接口的Webcam，聽說Pi支援這類鏡頭有一個清單，大家有空記得去看看自己的Webcam有沒有被支援到，我是沒去看啦，之前使用這個Webcam插上去就可以用，我猜Pi支援應該很廣....不囉唆，先來下載軟體的部分

## 軟體的部分

這邊我用別人先寫好的Server試試看，暫時不用motion、mjpg的原因是因為未來如果要做更多應用（動作偵測、夜間攝影、Email通知、OpenCV等）還是要用一個「我看得懂」的程式碼的程式來用。

#Python Webcam Server連結：[https://github.com/patrickfuller/camp](https://github.com/patrickfuller/camp) 

大家可以先上去看看，先別下載，依照上面的指示，我先做下面前導套件安裝：


{% highlight linenos %} 
 
sudo apt-get install python-dev python-pip python-opencv libjpeg-dev
sudo pip3 install tornado Pillow picamera
 
{% endhighlight %} 
 
中途若出現 404  Not Found 而 Fail，請善用 sudo apt-get update 進行套件來源的更新，再重新執行上述指令。

會花一點時間，完成後，請執行下方程式碼，下載python軟體
並執行

{% highlight linenos %} 
git clone https://github.com/patrickfuller/camp.git
python3 camp/server.py
{% endhighlight %} 



## 錯誤排除

執行python後發生錯誤，


{% highlight linenos %}
mmal: mmal_vc_component_create: failed to create component 'vc.ril.camera' (1:ENOMEM)
mmal: mmal_component_create_core: could not create component 'vc.ril.camera' (1)
Traceback (most recent call last):
  File "/usr/lib/python3/dist-packages/picamera/camera.py", line 522, in _init_camera
    prefix="Failed to create camera component")
  File "/usr/lib/python3/dist-packages/picamera/exc.py", line 191, in mmal_check
    raise PiCameraMMALError(status, prefix)
picamera.exc.PiCameraMMALError: Failed to create camera component: Out of memory

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "camp/server.py", line 102, in <module>
    camera = picamera.PiCamera()
  File "/usr/lib/python3/dist-packages/picamera/camera.py", line 488, in __init__
    self.STEREO_MODES[stereo_mode], stereo_decimate)
  File "/usr/lib/python3/dist-packages/picamera/camera.py", line 526, in _init_camera
    "Camera is not enabled. Try running 'sudo raspi-config' "
picamera.exc.PiCameraError: Camera is not enabled. Try running 'sudo raspi-config' and ensure that the camera has been enabled.
{% endhighlight %} 

...orz 後來找了好久才發現，這個python背後使用的picamera並不支援usb webcam....

[官方說法](http://picamera.readthedocs.io/en/release-1.4/faq.html#out-of-memory-when-initializing-the-camera)



