title: 图片压缩指定体积Data
date: 2019-03-24 12:51:19
tags: 技巧
---

参考文章来自stackoverflow:[压缩图片计算](https://stackoverflow.com/questions/20403805/how-to-downscale-a-uiimage-in-ios-by-the-data-size)

### 起因
****在开发微信分享的过程中发现分享文件的体积大小限制到128k,但是在下载的图片经常大小超过128k,这里我们需要将图片大小压缩到指定大小
在开发过程中一般只有将图片压缩到指定的长宽，但是很少压缩到指定的体积大小，这里使用递归的方式反复压缩
<!-- more -->

```objc
// Check if the image size is too large
if ((imageData.length/1024) >= 1024) {

    while ((imageData.length/1024) >= 1024) {
        NSLog(@"While start - The imagedata size is currently: %f KB",roundf((imageData.length/1024)));

        // While the imageData is too large scale down the image
        // Get the current image size
          CGSize currentSize = CGSizeMake(image.size.width, image.size.height);

        // Resize the image
        image = [image resizedImage:CGSizeMake(roundf(((currentSize.width/100)*80)), roundf(((currentSize.height/100)*80))) interpolationQuality:kMESImageQuality];

        // Pass the NSData out again
        imageData = UIImageJPEGRepresentation(image, kMESImageQuality);

    }
}

```
