title: iOS越狱环境搭建
date: 2019/10/29
tags: 
	- iOS
	- 越狱
---
越狱工具推荐MonkeyDev安装[uncover3.6.2](https://github.com/pwn20wndstuff/Undecimus/releases),目前越狱系统最高支持12.4版本。
![iphoneX](https://raw.githubusercontent.com/WymanY/PicBed/master/img/iphoneX.jpg)

<!-- more -->


1. 网络修复助手，连个锤子或者NetControl。（越狱后的cydia甚至安装的软件都没网络。
2. 越狱后电脑端装的软件：ifunbox访问系统文件，系统需要安装AF2
3. 安装手机远程登录工具openSSH，和本地文件修改工具Vim.,然后修改手机密码。（强烈要求修改root账户密码。

### 常见的越狱源使用：
1.蚂蚁源。
2.BigBoss源

### 开发工具安装：
1. 内存调试工具，Cyrun，目前直接cycript不好用，推荐一个写好的工具mjcript
2. 界面调试工具 Reveal
3. 其中编写tweak，目前推荐使用。Monkeydev
4. 砸壳工具.手机端直接砸壳CrackXI，电脑端iclutch，或者decripted
5. 手机远程调试连接脚本工具[issh](https://github.com/4ch12dy/issh)。其中lldb就推荐使用issh的方式来安装。其中issh中依赖的iproxy是通过homebrew的`brew install usbmuxd`来安装。‘cfgutil’可装可不装把，安装“cfgutil”是通过appSotre下载apple configurator 2，然后在这个的菜单上选择下载cfgutil。其中手机安装adv-command
6. 逆向分析二进制文件工具推荐**IDA**和**Hopper**
7. TheOS（主要用来开发tweak越狱插件)的搭建，也是使用Homebrew


### 推荐安装插件：
1. AppAdmin ，appStore降级下载工具。
2. respring，便捷重启springboard工具，每次下载完应用都会
3. BackupAZ3，备份越狱环境工具。
4. CrackXI,可以在手机上进行砸壳的工具。
5. Filza FIle工具，手机端的文件管理工具，类似ifunbox.电脑端的查看工具。
6. 手机端的终端工具推荐NewIterm2。
7. 可以安装破解版本的soloVPN工具。
8. 微信助手，可自动抢红包和防止消息撤回和修改步数，修改界面（推荐安装个微信屏蔽助手插件。
