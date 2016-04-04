---
layout: post
title: " Origami 的一些快速常识"
date: 2014-02-12 08:55
comments: true
tags: 
	- 设计 
	- Origami
---
### 1.Origami的基本知识介绍
&nbsp;&nbsp;&nbsp;&nbsp;Origami 的patch 相当于组件的概念，通过它来构建基本的界面和交互。Patch 也就相当于 PS 中的工具箱，里面包含了创建所有动效的任何组件。
### 2.快捷键相关操作
`⌘+⌥+N` 快速创建一个Origami 的文件。    

`⌃+⎇+⌘+0`调节Editor 和 Viewer 到合适的位置。  

`⌘+shift+E` 快速显示 Editor(即编辑项，相当于 ps 的 canvans)    

`⌘+shift+V` 快速显示 Viewer（效果查看面板）    

`L` 创建一个 Layer，即显示一个 Layer,即相当于一个正方形的 view,里面可以调节它的各项属性。（记忆方法去 layer,取首字母)    

`T` 快速创建一个Transition.    

`⌘+T` 快速调节一个 Patch的各项参数。    

`shift+T` 快速创建一个 TextLayer  

`C`创建一个Conditional Patch 条件表达式  

`R`创建 Reverse Progress 反转进度 Patch  

`delete`直接快速删除一个 patch

`I`快速创建一个Interaction的 Patch(这个 patch 相当于一个 tap)

`A` 创建一个 POP Animation 的 Patch

`S` 创建一个Virtual Splitter

`D` 创建一个 Delay 的 Patch 这个在延迟动画中很常用

`G` 创建一个 Layer Group 的 Patch

`H` 创建一个 HitArea的 Patch

`K` 创建一个 Keyboard的 Patch

`M`创建一个Virtual Multiplexer的 patch

`shift+ A/O/N`快速创建与或非

`shift+S` 快速创建一个 switch 的开关 Patch

`shift +L` 快速创建一个 Live Image Patch    

`Enter` 键去重命名一个 patch

附赠快捷键原文地址需翻墙：[origami 快捷键](http://facebook.github.io/origami/documentation/concepts/KeyboardShortcuts.html)

#### 3. 关于 Origiami 的一些基本常识
&nbsp;&nbsp;&nbsp;&nbsp;Origami的坐标系与 Quarts Composer 相比默认是(eg. Layer / Text Layer / Layer Group)使用设备的最中心点作为屏幕的（0，0）的坐标系。  

&nbsp;&nbsp;&nbsp;&nbsp;所有的坐标系默认都是向左 x 轴数值减少，向右 X 值增加，向下 Y 值数值增加，向上 Y 值数值减少。
下面的插图就是一个高400，宽300 的 LayerGroup。

![默认坐标系](http://m1.img.srcdd.com/farm4/d/2015/0523/11/55469FB893973CFE8229C665D8639173_B500_900_500_667.png)

&nbsp;&nbsp;&nbsp;&nbsp;默认坐标系的 anchor point 是 center，我们也可以修改patch 的 anchor point ，来修改自身的坐标系统，但不会影响到其它的 patch,数值变化的规律保持不变。

![image](http://m1.img.srcdd.com/farm4/d/2015/0523/11/601CFD14333DA6BD955DDAA0CBBAA425_B500_900_500_657.png)
下面的例子是一个修改了 anchor point 为 Top Left 的 TextLayer

![image](http://m2.img.srcdd.com/farm5/d/2015/0523/11/B1B4A1CB2E7FD81147A23D064AD44A7E_B500_900_500_666.png)
总结一下吧，这个anchor Point 调为 Top Left 就和我们平时使用的坐标系是一样的，其实这些坐标系很好理解的。你在软件上试一下立马可以理解（ps：帆哥你要是不理解，就是🐷了）

#### 4.Origami 常用的一些 patch，以及对应的功能.
`interaction Patch` 这个patch 就是单击的 patch，相当于 iOS 的 button吧，但是还不一样。  

`pop Patch` 做 pop 动画,可以使动画看着舒畅。  
  


