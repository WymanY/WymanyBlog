title: Facebook 的lldb来调试二
date: 2015-03-10 19:07:43
tags: lldb
---
## LLdb篇2教你使用faceBook的chisel来提高调试效率
这次真是久违的第二篇了，过年的时候一直在帮家里带孩子，顺便用webStorm这个神器重新问下了前端的知识。然后最近刚来北京又是重感冒，又是找房子，整个来说效率极低又苦不堪言。

首先如果使用lldb，最好你要学着使用chisel来提高效率，否则你会浪费很多的时间，除非你自己会写python脚本，自己封装一些lldb的命令。  
<!-- more -->

### 安装chisel
[chisel](https://github.com/facebook/chisel)的安装是十分简单，它是在终端通过brew安装的，具体可以点击链接参考github的安装说明，唯一需要注意的一点就是命令行安装完之后，它会在安装完之后显示出chisel的安装地址path.在执行下面的命令时候要记得替换/path/to/fblldb.py这一块。

```
# ~/.lldbinit
...  
command script import /path/to/fblldb.py
script fblldb.loadCommandsInDirectory('/magical/    
commands/')
```

如果安装成功的话，那么你就会看到如下图的这些命令。    

![lldb help](http://m2.img.srcdd.com/farm4/d/2015/0308/22/E835901C12DB9A2018B1FF573B161069_B500_900_500_213.png)

这里大概会有30个命令吧，我记得我第一次装的时候没那么多命令的，facebook又更新了很多。其实这些封装的命令，就是使用python封装了一下函数然后调用。凡是这些封装的命令，你都可以通过多个lldb命令打出来，所以如果你会使用python的话，那么你可以根据自己的使用习惯封装一些常用的lldb命令。我使用了也有一段时间的chisel了，但是感觉并不是所有的命令都很常用，而且有写使用的场景也不是很清楚，所以在这里给大家普及一下，如果有谬误，请大家及时指正。（ps:和大家说个快捷键，cmd+k快速清楚console的信息。）

一般我们使用chisel的命令的时候，我们可以通过 help + chisel命令，譬如 help + pvc，得到如何具体使用这个命令，但是有时候你看了help信息也不一定就会用呢。  

![image](http://m1.img.srcdd.com/farm5/d/2015/0308/22/11C139A4BF9F53F540953BB2F6B307B1_B500_900_500_160.png)    

### pviews  
这个命令是我最常使用的命令。它能够帮助我们看到view的层级，即使我们并没有触发到一个断点。操作如下：    

![pviews](http://m2.img.srcdd.com/farm5/d/2015/0308/22/2032B982DBAF0EB09D1C49D01D64CD63_ORIG_938_714.gif)    

* 如图我没有设置任何断点，只是点击控制台的暂停图标，就可以呼出lldb控制台了。然后再这里输出pviews这个命令。
* 然后这个命令主要可以看到当前的view层级，如果我们写了一个控件没有显示。我们就可以通过这个命令来排查。
* 排查首先看有没有我们添加的这个view，如button,如果内存地址里没有这个button，说明没有添加到view中（没调用addSubview方法）
* 然后可以看到这个button的地址，我们可以看到这个button的frame属性，根据属性判断是否是位置或者大小不合适。
* 再次，我们要看是否hidden被设置成了yes，如果设置了yes的话，在打印信息中会打印出来。因为默认view的isHidden是no,所以没被打印。
* 最后如果是button可以检查下是否设置了图片，如果是view，就可以查看下颜色是否与后面的控件一致，这就引入到了下一个命令border。    

### border&unborder

![border](http://m3.img.srcdd.com/farm5/d/2015/0308/23/2FDA13630E75FB9AA1CD89CBE9B81597_B500_900_500_337.png)
这个命令可以直接给border 添加边框颜色和边框的宽度，使用如下：    

`border 0x79ec3140 -c green -w 2`    

border这个命令常常在我们需要查看边框的边缘的问题，常常用到，而且我们想要设置的直接在lldb中设置，完全不需要重新写代码再次运行。我就是通过直接暂停程序，并且通过pviews命令找到的控件的地址，并且调用命令显示的。当我们不需要的时候可以通过`unborder`这个命令去掉边框。整个过程一气呵成。   

### pinternals
![pinternals](http://m3.img.srcdd.com/farm4/d/2015/0308/23/9817F08BEB31BF19E4A374E2B26113B4_B500_900_500_682.png)  

这个命令就是打印出来的一个控件（`id`）类型的内部结构，详细到令人发指！甚至是你自定义的控件中的类型，譬如这个`styleView`就是我自定义的，内部有个iconView的属性，其中的值它也会打印出来。好处，你们自己琢磨吧。（ps：这个demo，我会在下一篇博客中放出来，下篇博客是说transform的。
  
###   presponder  
打印出一个集成于UIResponder控件的消息传递链。
![presponder](http://m1.img.srcdd.com/farm4/d/2015/0308/23/08D1D5B28A97BC3CDB6C29A65F71C9FB_B500_900_500_377.png)
这个也方便我们了解消息是如何传递的，打印的时候是倒叙打印的。  
###   visualize  
可以使用mac下的预览app打开我们的图片UIImage, CGImageRef格式的图片，甚至view和layer的图片  。

`visualize 0x79ec3140//或者变量名，此地址是id类型的`

### pclass

pclass可以打印出一个对象的继承关系。![pclass](http://m1.img.srcdd.com/farm4/d/2015/0308/23/7DF9C2A62984C55AD2A1237B5BEA6908_B500_900_500_116.png,"pclass命令")
### taplog
这个命令是模拟敲击一下屏幕，并且打印出你敲击屏幕时候事件接收的对象。
![image](http://m3.img.srcdd.com/farm4/d/2015/0308/23/F2F93B95D430C3270D65AC3405BE9327_B500_900_500_101.png)
### hide&show
hide命令可以直接隐藏一个对象,移除当前遮挡的对象便于你观察后面的对象。show命令会让它再次显示出来。

### bmessage

这个命令就是lldb添加一个断点，譬如-viewWillAppear:这个方法，在当前控制器中你没有实现它，但是你又想在调用它的时机触发中断。  

```
Arguments:
  <expression>; Type: string; Expression to set a breakpoint on, e.g. "-[MyView setFrame:]", "+[MyView awesomeClassMethod]" or "-[0xabcd1234 setFrame:]"
```

这个我就不解释了，需要补充一点的是oc的方法是带`：`的。
### 其他命令
其它命令我用着并不是太多，并不代表他们不常用。只是我用的不太好而已，而且我认为用到是需要特殊的场景的，这个里说几个我感觉有很大作用但是我用的又不好的。  
1. `wivar`,这个命令是加watchPoint，用的好，就相当于使用lldb写了kvo了。（ps:恕我没研究明白）  
2. `pvc`这个命令的作用是打印出当前的控制器层级，（ps:有时好使，有时又很坏，似魔鬼的步伐.😢，没研究明白）  
3. `vs`，`fv`,`fvc`，这几个命令都需要正则表达式的知识背景，因为我正则表达式从来都是百度，也没自己真正学过。所以对我不常用，但是对那些会正则的可能会很大作用。（ps:希望你们研究出来有什么好的技巧分享下）  

###   参考

1. [南峰子的技术博客，工具篇：LLDB调试器](http://southpeak.github.io/blog/2015/01/25/gong-ju-pian-:lldbdiao-shi-qi/)  
2. [ Dancing in the Debugger — A Waltz with LLDB](http://www.objc.io/issue-19/lldb-debugging.html)
3. [LLDB调试命令初探](http://www.starfelix.com/blog/2014/03/17/lldbdiao-shi-ming-ling-chu-tan/)
4. [Xcode LLDB Debug教程](http://my.oschina.net/notting/blog/115294)

----

