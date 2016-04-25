---
layout: post
title: Storyboard中一些关注度较低的属性一
date: '2016-04-16 11:26'
tags:
  - iOS
  - storyboard
  - Strecting,First Responder
---

随着苹果 Sdk 不断的更迭，storyboard 变得越来越好用，但是仍然有很多人都不愿意使用 storyboard,如果使用 storyboard 我们可以更快的构建应用，而且使得我们的 controller 变的没那么 Massive,在这篇文章中，我将介绍一些 storyboard 中使用频次不是那么高，但是很常用的一些属性，提升我们的开发效率。
<!-- more -->

### 1.Strectching相关的属性
在 storyboard中，每一个 View 的属性中都会有一个 Strecting 属性，这个属性的主要作用是保护图片的边缘，拉伸指定的部分。但是这个属性在我们调节的时候并没有起到任何作用，但是从它的意思可以看出它的功能是拉伸的作用，但是并没有起效果，是因为它只对 ImageView 这个 View 起作用,下图显示设置到 Button 上并没有什么卵用。

![Storyboard_strecting](http://7xl8rl.com1.z0.glb.clouddn.com/Storyboard_strecting.png)

**Strecting** 对应 UIView 的 **contentStretch** 属性，默认值为{%raw%}{{0,0} {1,1}}{%endraw%},上图中设置{0.5,0.5,0.1,0.1}，为设置从图片中间的位置取{0.1,0.1}大小拉伸。

>@property(nonatomic)                 CGRect            contentStretch NS_DEPRECATED_IOS(3_0,6_0) __TVOS_PROHIBITED; // animatable. default is unit rectangle {%raw%}{{0,0} {1,1}}{%endraw%}. Now deprecated: please use -[UIImage resizableImageWithCapInsets:] to achieve the same effect.

由苹果的属性描述可以看到该属性已被弃用，改为使用图片的 `resizableImageWithCapInsets`方法达到相同的效果。这个`resizableImageWithCapInsets`在`NS_AVAILABLE_IOS(5_0)`开始可以使用。但是 iOS7在xcassets 中给图片引入Slicing 功能，可以以可视化的效果直接在设置图片拉伸。

![storyboard_assetSlicing](http://7xl8rl.com1.z0.glb.clouddn.com/storyboard_assetSlicing.png)

总结下如果想在iOS7及以后设置图片拉伸效果，请使用 Asset中的效果。如果想在 iOS7以前设置**resizableImageWithCapInsets**。storyboard中的view属性建议永远不要动，估计以后苹果会移除它。

### 2.First Responder

在 storyboard中每一个 **ViewController** 都有一个First Responder 属性，但是我们很少拿它来做一些事情。在 Storyboard中选择 First Responder 右键会发现如下默认属性:

![Storyboard_FirstResponder](http://7xl8rl.com1.z0.glb.clouddn.com/Storyboard_FirstResponder.png)

First Responder 直译为第一响应者，在我们的视图中存在着一个响应链(ps:对响应链不熟悉的童鞋可以看下苹果的[Event Handling Guide for iOS](https://developer.apple.com/library/ios/documentation/EventHandling/Conceptual/EventHandlingiPhoneOS/Introduction/Introduction.html#//apple_ref/doc/uid/TP40009541))，

未完待续。。。
