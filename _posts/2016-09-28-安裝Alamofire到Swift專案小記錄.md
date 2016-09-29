---
bg: "0929 swift.jpg"
layout: post
title:  安裝Alamofire小記錄
crawlertitle: "安裝Alamofire小記錄"
summary: "天阿！Swift上安裝libraries真的頗複雜...."
date:   2016-09-29
categories: posts
tags: ['Swift']
author: nps798
---

從Alamofire在git上的教學下手，先暫時記錄在此。

# 以 Cocoapods 安裝 Alamofire
  

## 1.以下方指令安裝 Cocoapods  

{% highlight linenos %}
sudo gem install cocoapods
{% endhighlight %}

#但是這個指令莫名其妙執行很久，查了 Stack OverFlow 才知道其實它有在跑，只是跑很久。
根據 [這篇](http://stackoverflow.com/questions/14355165/ xcode-installing-cocoapods-no-response) 所說，完成 gem update 後，就可以順利安裝 Cocoapods 了。

等等....出現此錯誤

{% highlight linenos %}
DEVELOPER:~ Yu-Ju$ sudo gem install cocoapods
Password:
ERROR:  While executing gem ... (Errno::EPERM)
    Operation not permitted - /usr/bin/fuzzy_match
{% endhighlight %}

聽說這是因為 OS X El Caption 的安全新設定所導致，要用其他方法解決。
根據 [github的討論](https://github.com/CocoaPods/CocoaPods/issues/3692) 找到以下解決辦法：

{% highlight linenos %}
$ mkdir -p $HOME/Sofrware/ruby
$ export GEM_HOME=$HOME/Sofrware/ruby
$ gem install cocoapods
[...]
1 gem installed
$ export PATH=$PATH:$HOME/Sofrware/ruby/bin
$ pod --version
0.37.2
{% endhighlight %}

上面寫 Sofrware 是我不小心打錯，將錯就錯吧......

## 2.設定 Cocoapods

在 terminal 輸入：
{% highlight linenos %}
pod setup --verbose
{% endhighlight %}

## 3.切換到 xcode project 根目錄，開始設定 pod

{% highlight linenos %}
pod init
{% endhighlight %}

如此，使用 Finder 開啟project根目錄即可發現，多了一個 podfile 檔案

## 4.以文字編輯器打開 podfile 檔案，並設定引入 frameworks 細節。

{% highlight linenos %}
# Uncomment this line to define a global platform for your project
# platform :ios, ‘8.0’

target 'HealthEdu' do
  # Comment this line if you're not using Swift and don't want to use dynamic frameworks
  use_frameworks!

  # Pods for HealthEdu

  target 'HealthEduTests' do
    inherit! :search_paths
    pod 'Alamofire'
    # Pods for testing
  end

  target 'HealthEduUITests' do
    inherit! :search_paths
    # Pods for testing
  end

end
{% endhighlight %}

## 5.使用 pod 安裝 alamofire。

同一個目錄下，
{% highlight linenos %}
pod init
{% endhighlight %}

這個可能會拖很久orz，一個指令就卡在那邊很尷尬。  
這時可以到 Finder 打開專案根目錄看看，是真的當機，還是有在下載。  
像我這邊就有看到專案底下多了 pod 字眼的的檔案跟資料夾，你可以看檔案資訊，看他大小有沒有增加，  
有增加就代表command有在跑，下載在背後進行。




