
---
layout: post
title: "ios开发的一些小技巧3"
date: 2014-11-15 14:48
comments: true
tags:
	- iOS
	- 技巧
---
 很久没写博客了，最近一堆破事和工作上的事，导致博客断更一阵子了，这篇博客既是一些技巧篇也是一些我在最近的日常的开发中使用到的技巧和笔记的记录。  （ps:希望大家不要像我一样，事情一忙就忘了做笔记）。

1.神器计算图片位置的函数：`AVMakeRectWithAspectRatioInsideRect（）`

通过这个函数，我们可以计算一个图片放在另一个 view 按照一定的比例居中显示，可能说的我比较抽象，还是用图来显示，可以说它可以直接一个 image 以任何的比例显示显示在 imageview 中居中所处的位置，拿 UIViewContontAspectFit来演示，

```
    UIImageView *imageView = [[UIImageView alloc] initWithFrame:CGRectMake(100, 100, 300, 300)];
    imageView.center = self.view.center;
    imageView.backgroundColor = [UIColor redColor];
    imageView.contentMode = UIViewContentModeScaleAspectFit;
    UIImage *image = [UIImage imageNamed:@"mm.jpg"];
    imageView.image = image;

     CGRect iamgeAspectRect = AVMakeRectWithAspectRatioInsideRect(image.size, imageView.bounds);
    NSLog(@"iamgeAspectRect = %@, imageView =%@",NSStringFromCGRect(iamgeAspectRect),NSStringFromCGRect(imageView.frame));
    [self.view addSubview:imageView];
    
```


图片显示如下:（ps:这妹子是我公司一个管理 公司weibo账号 的妹纸，目前我在勾搭，你们不要羡慕，生活依然很美好的嘛！）

![呵呵](http://m3.img.srcdd.com/farm5/d/2015/0423/22/BDE718EF7F219155B5445157E8F20D34_B500_900_500_889.png)
log 打因结果如下：
```
iamgeAspectRect = {{37.563884156729145, 0}, 
{224.87223168654171, 300}}, imageView ={{37.5, 183.5}, {300, 300}}
```
可以从 log 得出 对应的 image 以 aspectFit 的方式在 imageView 的位置，在 imageView 中的位置是（37.5，0）。这样你根本不需要任何多的代码来计算了。（ps:这个函数是在 AV框架的，童鞋们自行导入。）

具体它的其他的好处，如果你是做相机或者图片处理的你就知道它的好处了，什么处理横屏照片了，16:9，1:1,4:3图片在控件中的位置，控件上的点对应图片上的点的位置拉，等等。

2.关于 如果一个矩形如果做了平移旋转缩放等一系列操作之后，上下左右的四个点（甚至矩形上任意一个点）的位置。


```
  CGPoint originalCenter=CGPointApplyAffineTransform(_mStyleLeftEyeView.center,
                CGAffineTransformInvert(_mStyleLeftEyeView.transform));
    //1左眼内眼角
    CGPoint bottomRight = originalCenter;
    bottomRight.x += _mStyleLeftEyeView.bounds.size.width / 2;
    bottomRight.y += _mStyleLeftEyeView.bounds.size.height / 2;
    bottomRight = CGPointApplyAffineTransform(bottomRight, _mStyleLeftEyeView.transform);

	```

首先这个 styleLeftView 就是一个矩形的 view,这里以右下角的点做示范，做无论做了任何的 tranform 之后都可以得到它的点的位置，具体它用在什么位置，我就不方便透露了，（妈蛋，说多了，就相当于把公司我写的代码开源了）

3.在使用 pinch 的时候我们设置 pinch 缩放的最大值和最小值(系统默认没有提供最大值和最小值的 api)，设置 pinch的 maxValue,minValue.


```

    if([gestureRecognizer state] == UIGestureRecognizerStateBegan)
    {
        // Reset the last scale, necessary if there are multiple objects with different scales
        mLastScale = [gestureRecognizer scale];
    }

    if ([gestureRecognizer state] == UIGestureRecognizerStateBegan ||
        [gestureRecognizer state] == UIGestureRecognizerStateChanged)
    {
        CGFloat currentScale = gestureRecognizer.view.transform.a;   

        //计算出 缩放平移的 scale
        CGFloat deletaScale = (mLastScale - [gestureRecognizer scale]);

        CGFloat newScale = 1 -  deletaScale;

        newScale = MIN(newScale, kMaxScale / currentScale);
        newScale = MAX(newScale, kMinScale / currentScale);  
        CGAffineTransform scaleTransform = CGAffineTransformScale([[gestureRecognizer view] transform], newScale, newScale);

        //随着移动要调整一下 view 的 center point 位置
        [gestureRecognizer view].transform = scaleTransform;
        NSLog(@"self.iconView.height = %@ ,width = %@",@(self.iconView.width),@(self.iconView.height));
        mLastScale = [gestureRecognizer scale];  // Store the previous scale factor for the next pinch gesture call

```


 聪明的小伙伴应该一下就知道这个是怎么处理的，唯一需要注意的是 当前的缩放的 scale，最初查的资料是通过  `CGFloat currentScale = [[[gestureRecognizer view].layer valueForKeyPath:@"transform.scale"] floatValue];`来得到的，但是不知道为什么 layer的 transform 的 scale 和 view 的当前的缩放 scale 不一致，我通过 debug，得到 view的 transform 的 a的值和当前的缩放值是一样的，因为工作缘故不能凡事挖的特别透，所以大家想知道为什么 a 就是 scale 值可以自行查阅分享给我。

4.最后再分享给大家一个缩放时，缩放的中心点的问题，绝大部分我们缩放都是以 view 的中心点来缩放的，但是某些情况下我们需要以下面的边不动。譬如下面图像这种
![image](http://m1.img.srcdd.com/farm4/d/2015/0423/22/857565D816CDADAC1B3E9A0DEAC6F9B1_ORIG_1178_650.gif)

这种方式，我最早是希望通过缩放的时候同时平移就可以处理了，根据缩放的尺寸，缩放到上面多少就平移下来多少，保持下边不动，但是发现特别麻烦。后来使用 layer 的 anchorPoint 来出来，发现特别简单，唯一需要填的坑就是改变 anchorPoint 的时候，它的 frame 会发生瞬移的变化，天啊噜，还好我完美解决!

```

    -(void)setAnchorPoint:(CGPoint)anchorPoint forView:(UIView *)view
       {
    CGPoint newPoint = CGPointMake(view.bounds.size.width * anchorPoint.x,
                                   view.bounds.size.height * anchorPoint.y);
    CGPoint oldPoint = CGPointMake(view.bounds.size.width *      view.layer.anchorPoint.x,
                                   view.bounds.size.height * view.layer.anchorPoint.y);

    newPoint = CGPointApplyAffineTransform(newPoint, view.transform);
    oldPoint = CGPointApplyAffineTransform(oldPoint, view.transform);

    CGPoint position = view.layer.position;

    position.x -= oldPoint.x;
    position.x += newPoint.x;

    position.y -= oldPoint.y;
    position.y += newPoint.y;

    view.layer.position = position;
    view.layer.anchorPoint = anchorPoint;
    }

```

通过这种方式设置 anchorPoint，如果后续你做平移前 速度把 AnchorPoint设置到（0.5，0.5）的位置，就没有问题了。

5,最后一个就是分享下大家团队如果使用 git 来开发，可以快速定位你现在看不懂的代码是哪个2货写的,然后即使把锅甩到他身上（ps:如果是自己写的，你就默不作声，别让别人知道这个技巧哈），其实就是 show blame for line.
![image](http://m3.img.srcdd.com/farm5/d/2015/0423/22/2EFC3CEC0FBE0FB95B89EFD5237E622D_ORIG_1178_650.gif)

当然这种操作是可以使用快捷操作的，熟悉我的人应该都知道我的风格，我把 show blame for line 改成快捷键⌥+c 的操作了（ps:怎么改看我前面的博客）。好了以后有问题及时找到你的同事，质问他你这写的什么垃圾代码，我这种大神都不看不懂。
## 结束语
原来没有用 swift 写演示，最近也是在学，但是没有时间写，本来要写的 storyBoard和 AutoLayout 技巧篇，也是因为最近人写这个太多的，感觉有不少雷同，但是只写出来不同的也写不出来太多，原酿我（后续我会学着写动画，到时给你们带来酷炫的动画哦）！

