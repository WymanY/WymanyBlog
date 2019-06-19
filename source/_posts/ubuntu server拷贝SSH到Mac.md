title: Ubuntu Server拷贝SSH到Mac
date: '2019-04-04 18:35'
tags:
    - Linux
    - 杂记
---
### Ubuntu Server拷贝SSH到Mac
如何从远程Linux服务器拷贝提供给`Git` 服务器的`SSH RSA`公钥到Mac电脑。
<!-- more -->

```
scp -r remoteVM:.ssh .
```
比如 scp -r wymany@192.168.180.128:.ssh .

这一段主要的作用就是使用wymany用户登录Host为192.168.180.128的主机，拷贝.ssh文件到 .（当前文件夹下

另外一种方式，是使用rsync命令从Linux拷贝对应的文件。


参考连接
[https://linuxmoz.com/how-to-copy-ssh-public-key-from-a-mac-to-a-linux-server/](https://linuxmoz.com/how-to-copy-ssh-public-key-from-a-mac-to-a-linux-server/)
