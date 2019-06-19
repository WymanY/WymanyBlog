---
layout: post
title: "如何优雅的使用 Mac"
date: 2014-02-12 08:55
comments: true
tags: 
	- Mac 
	- Apple电脑
---
#### 1.不使用第三方工具给我们的 Finder 集成一个右键或快捷键拷贝路径的方法

我们经常需要拷贝一个文件的地址，每次拷贝的时候都十分苦恼，如何快速拷贝文件的路径呢。
<!-- more -->
这个时候我们需要使用苹果的 Automator 工具了
Open Automator>Create Service>choose Utility(实用工具)->copy To ClipBoard ->Choose File Or folders In Finder.>Save(copy Path)
(大致显示结果如下吧)  

![image](http://m1.img.srcdd.com/farm5/d/2015/0726/00/08DB0B57DDC2678DDFFC45493A122A12_LARGE_984_859.png)    

然后我们已经创建了一个服务了。然后我们随便找到一个文件夹，右键找到服务，里面有个 Copy Path，这样我们就可以 Copy Path了。    

![image](http://m1.img.srcdd.com/farm5/d/2015/0726/00/E277DF4A50F61FE8017CC726FACD4321_LARGE_635_617.png)    

然后这样结束了嘛？还不够简单，我们打开键盘的偏好设置，然后我们找到快捷键选项，在左侧面板选择服务，右侧面板选择到文件或者文件夹，设置快捷键，因为 Mac 没有剪切的功能，所以我将其设置成了  ⌘+X，这个时候我们需要重启动下 Finder，快捷键才能生效
![image](http://m3.img.srcdd.com/farm4/d/2015/0726/00/EDDB18FB7E552BC194626206EF9E97B1_LARGE_638_526.png)  
#### 2.默认打开 mac 的隐藏文件夹

`显示：defaults write com.apple.finder AppleShowAllFiles -bool true  `

`隐藏：defaults write com.apple.finder AppleShowAllFiles -bool false `  

#### 3. mac 下关闭 .DS_store 自动生成
这是个 OS X 下一个用来保存文件夹显示信息的隐藏文件，它保存了这个目录的文件清单，在开发的时候很容易上传到 Web 目录下。当我们开启隐藏文件选项的时候就会出现它，很烦。

[sepsis](http://asepsis.binaryage.com/)，一个针对 .DS_store 文件的处理工具  

1. 把当前用户下的所有 ds 文件放入集中营
`asepsisctl migratein`
2. 将集中营的 ds 文件移回本身的位置   `asepsisctl migrateut`
3. 在后台持续运行   `asepsisctl launch_daemon`  

禁止生成的命令：  
`defaults write com.apple.desktopservices DSDontWriteNetworkStores -bool TRUE`  
(关闭以后比如视图设置、文件图标的摆放位置这些信息的将会失去。）  
找到已有的：  
`sudo find / -name ".DS_Store" -depth`  
#### 4.关于snippet的备份  
我们xcode的snippet Library 一般放在
> ~/Library/Developer/Xcode/UserData/CodeSnippets/  

 通过右键-》服务，-》在Find中显示，-》直接打开。  
 然后我们备份到我们的iCloud中就可以了，推荐一个Matt大神的Snippet的[repo](https://github.com/Xcode-Snippets/Objective-C)(ps:如果你说你都不知道snippet是什么，你不适合看我的博客。)  
 好吧，我开玩笑，你不知道snippet可以看下这个文章[snippet 简单介绍](http://mobile.51cto.com/hot-431722.htm)
 
####  5、桌面壁纸网站
[desktopper](https://www.desktoppr.co/wallpapers)高清桌面壁纸网站，这个网站里面全都是高清的图，而且里面有羞羞的图片哦，需要连接 DropBox，需翻墙。  
![image](http://m2.img.srcdd.com/farm4/d/2015/0726/00/D8D142DE844529891865F85992641DA6_LARGE_2282_1322.png)  

#### 6.桌面清理软件  
呵呵，我的壁纸一直都狠酷毙的，而且我的桌面就完全是空的。为什么我的桌面是空的呢，因为我使用了这么一款软件[DeskTop Tidy](https://itunes.apple.com/us/app/desktop-tidy/id468622130?mt=12) 软件，玩不习惯的人，不建议如，我也是用了几天才习惯的。当然镜像的地方建议不要选择默认的地方,找起来哭死。  
`/Users/~~wuyun~~/Library/Application Support/Desktop Tidy/Shadow Desktop`  
![image](http://m2.img.srcdd.com/farm5/d/2015/0726/00/235677CD376EA7EAA98DC717F92C5298_LARGE_962_1174.png)    

#### 7.The Clock 软件
一款设置定时每半小时提醒你休息两分钟的的软件,然后也可以设置提醒时间,然而它是收费的，其实并没有什么卵用。(不过确实因为这个，我没有休息，愧疚感已经相当严重了，所以现在休息有所改善)

#### 8.The Bartender
这款软件可以有效的管理你的菜单栏，让你的菜单栏看起来没那么乱.![image](http://m2.img.srcdd.com/farm4/d/2015/0726/00/499946B645F1BF74C61F6E5E3F963511_LARGE_186_192.png)

以前是这么的  
![image](http://m2.img.srcdd.com/farm4/d/2015/0726/00/12954A725459E8647B94C84ED815B12A_LARGE_1814_42.png)
  
 现在是这样的    
 ![image](http://m3.img.srcdd.com/farm4/d/2015/0726/00/E6B2D1963E3B6FB46AF10F5B45570A8E_LARGE_1750_48.png)  
 当然我的菜单栏本身东西也不是特别多，但是我依然有强迫症。甚至我可以让菜单栏上没有东西，这个软件当然也是收费的。。

## 结束语 
这些软件只是我的一部分反复考察的感觉常用的一些软件，下次记录一些有助于我们开发者提高开发效率的的一些软件吧，妈蛋，好久没写博客了，人啊果然不能懈怠，虽然写博客会耗费一定的精力，但是带来的好处是无穷的，希望自己不要断了。



 
