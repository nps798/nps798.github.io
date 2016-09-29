---
bg: "rails.jpg"
layout: post
title:  安裝Alamofire小記錄
crawlertitle: "安裝Alamofire小記錄"
summary: "天阿！Swift上安裝libraries真的頗複雜...."
date:   2016-09-29
categories: posts
tags: ['Swift']
author: nps798
---

一開始先從Alamofire在git上的教學下手  

1.以下方指令安裝 Cocoapods 

sudo gem install cocoapods

#但是這個指令莫名其妙執行很久，查了 Stack OverFlow 才知道其實它有在跑，只是跑很久。
根據 [這篇](http://stackoverflow.com/questions/14355165/ xcode-installing-cocoapods-no-response) 所說，完成 gem update 後，就可以順利安裝 Cocoapods 了。

等等....出現此錯誤

{% highlight bash linenos %}
DEVELOPER:~ Yu-Ju$ sudo gem install cocoapods
Password:
ERROR:  While executing gem ... (Errno::EPERM)
    Operation not permitted - /usr/bin/fuzzy_match
{% endhighlight %}

聽說這是因為 OS X El Caption 的安全新設定所導致，要用其他方法解決。
根據 [github的討論](https://github.com/CocoaPods/CocoaPods/issues/3692) 找到以下解決辦法：

{% highlight bash linenos %}
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

2.設定 Cocoapods

pod setup --verbose




