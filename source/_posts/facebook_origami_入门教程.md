
---
layout: post
title: "FaceBook Origami 入门教程"
date: 2014-02-12 08:55
comments: true
tags: 
	- Origami 
	- 设计
---


#### 1.Origami 的简介

&nbsp;&nbsp;&nbsp;&nbsp;首先 origami 是 facebook 的一款制作动效的软件，它可以很简单的实现很多动效，可以实现几乎所有的手机上和网页端的动画效果，它相对于其它的动效制作软件更贴近于程序，基本上程序上所支持的手势，基本上 origami 都支持，而且它很好的融入了 facebook 的 [pop动画库](https://github.com/facebook/pop)。

#### 2.Origami 的安装

&nbsp;&nbsp;&nbsp;&nbsp;基本上[Facebook](https://code.facebook.com) 所有出的产品和开源库都可以在 facebook 的官网上找到，包括 Origami 安装等的所有的信息。（***PS:faceBook的网站需要自备 vpn***）

&nbsp;&nbsp;&nbsp;&nbsp;在 FaceBook 中查找 Origami 的流程是这样的，首先**IOS->DesginTool->origami**，这里最后直接给出 [origami 的官方网站](http://facebook.github.io/origami/)  

这里我们点击 origami 的官方网站，然后有个 download 按钮，点击后出现如下：  
![origami](http://m2.img.srcdd.com/farm5/d/2015/0521/20/2D9D71387EA6E2BB16806947423040C8_B500_900_500_243.png)

>1.安装流程如下第一步成为一个 mac 开发者.点击进去苹果的官网注册后，才可以下载苹果相关的开发工具，这里注册成为开发者主要就是为了下载 QuartsComposer 这个软件，而 origami 只是它的一直插件.  

>2.下载 Quartzs Composer,这个软件下载之后w文件名字是这样：`graphics_tools_for_xcode_5.1__march_2014`，这个是根据 Xcode 版本来的，现在已经出到 for Xcode 6.1了。这个解压完之后，可以不会默认拖到 Application 中，需要我们将全部的需要的拖到 Application 中。    

>3.下载 Origami,目前出到2.02,这个安装之后会安装到/System/Library/Graphics/Quartz Compose，以 Quartzs Composer 的插件形式安装，这个安装之后我们只能在电脑上看到，如果想要哦映射到手机上，需要在手机上Appstore 安装手机端 origami 软件。

#### 3.Origami 的初使用

&nbsp;&nbsp;&nbsp;&nbsp;在安装完之后，我们当然要第一步熟悉打开一个案例 demo，见证一下它的威力，所有的 demo,在这里可以找到，[origamiDemo](http://facebook.github.io/origami/examples/)案例。如果嫌翻墙麻烦，可以登陆 UI 中国查看[MartinRGB大神的 demo](http://i.ui.cn/ucenter/88338.html)，在他这里也可以找到相关的 origami 教程。
下载之后，Prototype 的后缀名是.qtz，在我们修改之后会默认出现~XXX.qtz，这个~波浪线是默认软件为修改之后的Prototype生成 的副本。（***第一次见不要感到惊奇哦***）运行方式同 xcode ，直接⌘+R 运行。

#### 4.Origami 的相关教程。  
1.首先推荐看 faceBook 的官方视频[tutorials](http://facebook.github.io/origami/tutorials/)，可能你的英语不是特别好，不要怕，MartinRGB 大神早就料到了，他在自己的UI 中国主页上放了自己翻译的字幕和相应的视频到百度云上，可以前去下载。[视频+QC文件+安装包+案例](http://pan.baidu.com/s/1kTxJmar),字幕下载[链接](http://pan.baidu.com/s/1mgzJb8s)

2.当然还是 MartinRGB 大神的Wayfinder，[Quartz Composer学习](http://wayfinder.is/martinrgb/Quartz-Composer--/5461a948224bfd1600b47a4b),这个我不知道为什么老是刷不出来主页，即使我翻墙也不可以，看你运气吧！  
3.推荐这个收费教程，也是视频教学，但是收费略贵，[courese 地址](http://designers.how/courses)  
![course](http://m1.img.srcdd.com/farm4/d/2015/0521/21/E075BD419014C824E6A3B6797C2E3153_B500_900_500_181.png)

#### 5.Origami 的入门必学
&nbsp;&nbsp;&nbsp;&nbsp;前面体验了 Origami 的新鲜感之后，我们要记住千里之行始于足下，还是要从最基础的一步步学习，首先要熟悉 Origami的工具，推荐先看完这个Origami 的 [Document](http://facebook.github.io/origami/documentation/),MartinRGB的 [翻译版本](http://www.ui.cn/detail/43369.html)！

## 结束语
&nbsp;&nbsp;&nbsp;&nbsp;写着一篇博客的主要目的是给公司的 UI 设计妹子看的，程序猿你看到了就看到了吧！下一篇主要是介绍 Origami 的使用技巧，在介绍个简单的案例。（ps:写这么详细妹子也该看懂了吧，这么拼也不容易，设计妹子都可以交流啊）。
