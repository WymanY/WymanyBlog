---
layout: post
title: "iOS 面试大全从简单到复杂1(简单篇)"
date: 2014-02-12 08:55
comments: true
tags: 
	- 面试 
	- iOS
---
![Interview](https://raw.githubusercontent.com/WymanY/PicBed/master/img/interview.png)
面试是在开发中必须经历的一环，三分运气七分实力，需要我们日常的总结，同时是考验记忆的能力，毕竟可能平时我们会查很多资料，面试的时候可不会给你机会去查
<!-- more -->

#### 1.1UIWindow和UIView和 CALayer 的联系和区别?
* UIView是视图的基类，UIViewController是视图控制器的基类，UIResponder是表示一个可以在屏幕上响应触摸事件的对象；   
* UIwindow是UIView的子类，UIWindow的主要作用：一是提供一个区域来显示UIView，二是将事件（event）的分发给UIView，一个应用基本上只有一个UIWindow.  
* 万物归根，UIView和CALayer都是的老祖都是NSObjet。可见 UIResponder是用来响应事件的，也就是UIView可以响应用户事件。  

###  CALayer 和 UIView 的区别：  
> 1.1 UIView的继承结构为: UIResponder : NSObject。
> CALayer的继承结构为： NSObject。可见 UIResponder是用来响应事件的，也就是UIView可以响应用户事件，CALayer直接从 NSObject继承，因为缺少了UIResponder类，不能响应任何用户事件  
> 1.2 所属框架,UIView是在 /System/Library/Frameworks/UIKit.framework中定义的,UIKit主要是用来构建用户界面，并且是可以响应事件的。CALayer是在/System/Library/Frameworks/QuartzCore.framework定义的。而且CALayer作为一个低级的，可以承载绘制内容的底层对象出现在该框架中。    
> 
> 1.3 UIView相比CALayer最大区别是UIView可以响应用户事件，而CALayer不可以。UIView侧重于对显示内容的管理，CALayer侧重于对内容的绘制。UIView是基于CALayer的高层封装。    
> 
> 1.4 相似支持1：相似的树形结构2：显示内容绘制方式3: 布局约束  
总结一下就是：UIView是用来显示内容的，可以处理用户事件.CALayer是用来绘制内容的，对内容进行动画处理依赖与UIView来进行显示，不能处理用户事件  
为啥有两套体系 并不是两套体系？UIView和CALayer是相互依赖的关系。UIView依赖与calayer提供的内容，CALayer依赖uivew提供的容器来显示绘制的内容。归根到底CALayer是这一切的基础，如果没有CALayer，UIView自身也不会存在，UIView是一个特殊的CALayer实现，添加了响应事件的能力。UIView本身，更像是一个CALayer的管理器，访问它的跟绘图和跟坐标有关的属性，例如frame，bounds等等，实际上内部都是在访问它所包含的CALayer的相关属性。  
UIView的layer树形在系统内部，被系统维护着三份copy（这段理解有点吃不准）。
第一份，逻辑树，就是代码里可以操纵的，例如更改layer的属性等等就在这一份。
第二份，动画树，这是一个中间层，系统正在这一层上更改属性，进行各种渲染操作。
第三份，显示树，这棵树的内容是当前正被显示在屏幕上的内容。
这三棵树的逻辑结构都是一样的，区别只有各自的属性。  
UIView的主layer以外，对它的subLayer，也就是子layer的属性进行更改，系统将自动进行动画生成。  
CALayer的坐标系系统和UIView有点不一样，它多了一个叫anchorPoint的属性，它使用CGPoint结构，但是值域是0~1，也就是按照比例来设置。这个点是各种图形变换的坐标原点，同时会更改layer的position的位置，它的缺省值是{0.5, 0.5}，也就是在layer的中央。

哈哈，这下够说一壶的了把，虽然说完感觉其实没什么卵用，但是记住一定要说的绘声绘色。  

参考链接如下：  
1. [http://o0o0o0o.iteye.com/blog/1728599](http://o0o0o0o.iteye.com/blog/1728599)  
2. [http://www.cnblogs.com/pengyingh/articles/2381673.html](http://www.cnblogs.com/pengyingh/articles/2381673.html)  

#### 2. property 都有哪些常见的字段 
strong，weak，retain，assign，copy nomatic,readonly,


####3. strong，weak，retain，assign，copy nomatic 等的区别。

> assign： 简单赋值，不更改索引计数（Reference Counting）对基础数据类

>copy： 建立一个索引计数为1的对象，然后释放旧对象。对NSString 

>retain：释放旧的对象，将旧对象的值赋予输入对象，再提高输入对象的索引计数为1 ,对其他NSObject和其子类

`weak` 和`strong`的区别：`weak`和`strong`不同的是 当一个对象不再有strong类型的指针指向它的时候 它会被释放  ，即使还有weak型指针指向它。一旦最后一个`strong`型指针离去 ，这个对象将被释放，所有剩余的`weak`型指针都将被清除。    

`copy`与`retain`：  
1. `copy`其实是建立了一个相同的对象，而retain不是.  
2.  `copy`是内容拷贝，`retain`是指针拷贝.  
3. `copy`是内容的拷贝 ,对于像NSString,的确是这样，如果拷贝的是 `NSArray`这时只是`copy`了指向array中相对应元素的指针.这便是所谓的"浅复制".

`atomic`是Objc使用的一种线程保护技术，基本上来讲，是防止在写未完成的时候被另外一个线程读取，造成数据错误。而这种机制是耗费系统资源的，所以在iPhone这种小型设备上，如果没有使用多线程间的通讯编程，那么nonatomic是一个非常好的选择。 

对于 NSString 为什么使用 copy 参考这篇链接
[http://southpeak.github.io/blog/2015/05/10/ioszhi-shi-xiao-ji-di-%5B%3F%5D-qi-2015-dot-05-dot-10/](http://southpeak.github.io/blog/2015/05/10/ioszhi-shi-xiao-ji-di-%5B%3F%5D-qi-2015-dot-05-dot-10/)

#### 4.`__block`和`__weak`修饰符的区别： 

1. `__block`不管是ARC还是MRC模式下都可以使用，可以修饰对象，还可以修饰基本数据类型。 
2. `__weak`只能在ARC模式下使用，也只能修饰对象（NSString），不能修饰基本数据类型（int）。 
3. `__block`对象可以在block中被重新赋值，`__weak`不可以。  

#### 4.常见的 Http 状态🐴有哪些？
http状态吗 ：302 是请求重定向。500以上是服务器错误。400以上是请求链接错误或者找不到服务器。200以上是正确。100以上是请求接受成功。

2-3问题参考链接

#### 5.单例的写法。在单例中使用数组要注意什么？
```
static PGSingleton *sharedSingleton;

+ (instancetype)sharedSingleton
{
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        sharedSingleton = [[PGSingleton alloc] init];
    });
    return sharedSingleton;
}
```

其实上面的还不是标准的单例方法，标准的单例方法需要重写 `copyWithZone`,`allocWithZone`,`init`,确保以任何方式创建出来的对象只有一个，这里就不详细写了。

单例使用 NSMutableArray 的时候，防止多个地方对它同时遍历和修改的话，需要加原子属性。并且property用strong，并且写一个遍历和修改的方法。加上锁. Lock,UnLock.（PS:考虑性能问题尽量避免使用锁，在苹果的文档张看到的不要问我为什么，我也忘了自己查去。。)

关于单例的参考，和不要滥用单例的参考  
1. [http://www.jianshu.com/p/7486ebfcd93b](http://www.jianshu.com/p/7486ebfcd93b)  
2. [http://blog.codingcoder.com/singletons/](http://blog.codingcoder.com/singletons/)

#### 6.static 关键字的作用 
1.函数体内 static 变量的作用范围为该函数体，不同于 auto 变量，该变量的内存只被分配一次， 
因此其值在下次调用时仍维持上次的值；   
2.在模块内的 static 全局变量可以被模块内所用函数访问，但不能被模块外其它函数访问；   
3.在模块内的 static 函数只可被这一模块内的其它函数调用，这个函数的使用范围被限制在声明 
它的模块内；  
4.在类中的 static 成员变量属于整个类所拥有，对类的所有对象只有一份拷贝；   
5.在类中的 static 成员函数属于整个类所拥有，这个函数不接收 this 指针，因而只能访问类的static 成员变量。 

#### 7.iOS 中的事件的传递:响应链
> 简要说一下: 
> 事件沿着一个指定的路径传递直到它遇见可以处理它的对象。 首先一个UIApplication 对象从队列顶部获取一个事件并分发(dispatches)它以便处理。 通常，它把事件传递给应用程序的关键窗口对象，该对象把事件传递给一个初始对象来处理。 初始对象取决于事件的类型。  
>> 触摸事件。 对于触摸事件，窗口对象首先尝试把事件传递给触摸发生的视图。那个视图被称为hit-test(点击测试)视图。 寻找hit-test视图的过程被称为hit-testing, 参见 “Hit-Testing Returns the View Where a Touch Occurred.”  
**运动和远程控制事件**。 对于这些事件，窗口对象把shaking-motion(摇晃运动)或远程控制事件传递给第一响应者来处理。第一响应者请参见 “The Responder Chain Is Made Up of Responder Objects.”  

>iOS 使用hit-testing来找到事件发生的视图。 Hit-testing包括检查触摸事件是否发生在任何相关视图对象的范围内， 如果是，则递归地检查所有视图的子视图。在视图层次中的最底层视图，如果它包含了触摸点，那么它就是hit-test视图。等 iOS决定了hit-test视图之后，它把触摸事件传递给该视图以便处理。

参考链接：[http://yishuiliunian.gitbooks.io/implementate-tableview-to-understand-ios/content/uikit/132event-chains.html](http://yishuiliunian.gitbooks.io/implementate-tableview-to-understand-ios/content/uikit/132event-chains.html)

#### 8堆和栈的区别 
> 管理方式：对于栈来讲，是由编译器自动管理，无需我们手工控制；对于堆来说，释放工作由程序员控制，容易产生memory leak。   
> 
> 申请大小：   
> 栈：在Windows下,栈是向低地址扩展的数据结构，是一块连续的内存的区域。这句话的意思是栈顶的地址和栈的最大容量是系统预先规定好的，在 WINDOWS下，栈的大小是2M（也有的说是1M，总之是一个编译时就确定的常数），如果申请的空间超过栈的剩余空间时，将提示overflow。因此，能从栈获得的空间较小。   
> 
> 堆：堆是向高地址扩展的数据结构，是不连续的内存区域。这是由于系统是用链表来存储的空闲内存地址的，自然是不连续的，而链表的遍历方向是由低地址向高地址。堆的大小受限于计算机系统中有效的虚拟内存。由此可见，堆获得的空间比较灵活，也比较大。     
> 
> 碎片问题：对于堆来讲，频繁的new/delete势必会造成内存空间的不连续，从而造成大量的碎片，使程序效率降低。对于栈来讲，则不会存在这个问题，因为栈是先进后出的队列，他们是如此的一一对应，以至于永远都不可能有一个内存块从栈中间弹出   
> 分配方式：堆都是动态分配的，没有静态分配的堆。栈有2种分配方式：静态分配和动态分配。静态分配是编译器完成的，比如局部变量的分配。动态分配由alloca函数进行分配，但是栈的动态分配和堆是不同的，他的动态分配是由编译器进行释放，无需我们手工实现。   
> 分配效率：栈是机器系统提供的数据结构，计算机会在底层对栈提供支持：分配专门的寄存器存放栈的地址，压栈出栈都有专门的指令执行，这就决定了栈的效率比较高。堆则是C/C++函数库提供的，它的机制是很复杂的。


