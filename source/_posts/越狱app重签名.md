title: 越狱app重签名
date: 2020-09-15 13:34:19
tags: 越狱 
---

![berakphone](https://raw.githubusercontent.com/WymanY/PicBed/master/img/breakjailPhone.jpg)

我们越狱后，想要对app二次进行开发，就正常需要对app进行重签操作，这篇文章列举了我在重签过程中遇到的一些问题，和我找到的解决方案。

<!-- more -->


#### MonkeyDev 重签

我们可以使用越狱集成开发工具[MonkeyDev](https://github.com/AloneMonkey/MonkeyDev)进行App的重签名，我们使用monkeyDev新建一个项目.

在`monkeyDev`中我们可以直接使用Xcode自带的签名系统进行重签名，通常情况下我们都会使用个人免费的账号，但是对于个人免费账号的Capablity 是有限的，导致在签名的时候提示权限不足。

 MonkeyDev中的签名脚本如下:
 ```shell
# ./quick-resign.sh [insert] "origin.ipa path"  "resign.ipa path"
INSERT_DYLIB=NO
INPUT_PATH=$1
OUTPUT_PATH=$2

if [[ $1 == "insert" ]];then
	INSERT_DYLIB=YES
	INPUT_PATH=$2
	OUTPUT_PATH=$3
fi

if [[ ! $OUTPUT_PATH ]];then
	OUTPUT_PATH=$PWD
fi

cp -rf $INPUT_PATH ../TargetApp/
cd ../../
xcodebuild MONKEYDEV_INSERT_DYLIB=$INSERT_DYLIB | xcpretty
cd LatestBuild
./createIPA.command
cp -rf Target.ipa $OUTPUT_PATH
exit 0

 ```