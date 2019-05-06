---
layout: post
title: iOS依赖库管理工具CocoaPods和Carthage总结
date: 2019-05-06
excerpt: iOS依赖库管理工具总结
project: true
tags: [iOS, CocoaPods, Carthage]
comments: false
---

# iOS依赖库管理工具CocoaPods和Carthage总结

目前有很多依赖库管理工具,例如:

Java EE----------Maven,

Android----------Gradle,

Node.js----------npm,

Python----------Pip,

OC/Swift----------CocoaPods,Carthage,

等等.

iOS中主要用的两个依赖库管理工具就是CocoaPods和Carthage,下面就是这两个工具的使用,原理和注意事项.



## 一.CocoaPods

### 1.介绍

CocoaPods使用Ruby开发的Xcode项目管理依赖库的工具.

```ruby
CocoaPods is a dependency manager for Swift and Objective-C Cocoa projects. It has over 61 thousand libraries and is used in over 3 million apps. CocoaPods can help you scale your projects elegantly.
```

官网: https://cocoapods.org

开源仓库地址: https://github.com/CocoaPods/CocoaPods

开发iOS项目使用CocoaPods管理第三方开源的框架,可以节省设置,下载,删除,更新的时间,提高开发效率.

### 2.安装和使用

#### 2.1.安装

CocoaPods是在Ruby环境下开发的,Mac系统本身自带Ruby,可以通过`ruby -v`命令查看当前系统Ruby的版本,使用Ruby的gem命令即可下载安装CocoaPods.

```shell
sudo gem install cocoapods
pod setup
```

(1)如果提示gem版本太老,需要更新gem: `sudo gem update --system`

(2)如果安装过程很慢,可以将官方的Ruby源替换成下面的这些:

```shell
删除源: gem source -remove https://rubygems.org/
添加源: gem source -a https://gems.ruby-china.org
查看源: gem sources -l
当前源:
*** CURRENT SOURCES ***
https://gems.ruby-china.org
https://rubygems.org/
```

(3)在执行`pod setup`命令时,会输出Setting up CocoaPods master repo,这个过程会等待比较长的时间,这个过程是同步远程的CocoaPods信息,将信息下载到本地`~/.cocoapods`目录中.

`open ~/.cocoapods`可以看到repos文件夹,包含了CocoaPods的所有项目镜像索引,通过项目的镜像索引去远程仓库下载相应的第三方库.

(4)安装完成后可以通过`pod —version`命令查看CocoaPods版本信息,显示版本号说明安装成功.

#### 2.2.使用

(1)查找第三方库

使用`pod search AFNetworking`命令可以查找第三方库,属于模糊搜索,会搜索AFNetworking相关的库信息.

(2)使用CocoaPods管理第三方库

首先在项目的根目录下创建Podfile文件,按照下面的格式编辑即可:

```text
target '项目名称XXXX' do
    platform :ios, '8.0'
    
    pod '第三方库名称XXXX'
    
end
```

使用终端工具cd到当前工程的根目录路径下,执行`pod install`命令或者`pod update`命令,完成之后会在根目录下出现两个文件(*.xcworkspace, Podfile.lock)和一个文件夹(Pods).

①*.xcworkspace是项目启动文件.

②Podfile.lock是锁定当前各个依赖库的版本.(`pod install`命令不会更改依赖库的版本,也就是不会修改Podfile.loack文件; `pod update`命令会修改)

③Pods文件夹包含了下载的第三方依赖库,并且编译成lib库.

(3)工作原理:将所有依赖库都放到另一个名为Pods的项目中,然后让主项目依赖Pods项目,这样源码管理工作从主项目转移到了Pods项目中. Pods项目会编译成一个libPods.a文件,主项目只需要添加这个.a文件.



## 二.Carthage

### 1.介绍

Carthage是用Swift开发的项目管理依赖库的工具,Carthage的目的是用最简单的方式添加依赖库到Cocoa项目中.

Carthage为用户管理第三方框架和依赖库,但不会自动修改项目文件和生成配置.

```ruby
Carthage is intended to be the simplest way to add frameworks to your Cocoa application.Carthage builds your dependencies and provides you with binary frameworks, but you retain full control over your project structure and setup. Carthage does not automatically modify your project files or your build settings.
```

开源仓库地址: https://github.com/Carthage/Carthage

### 2.Carthage与CocoaPods对比

Carthage优点:

(1)Carthage为用户管理第三方框架和依赖库,不会自动修改项目文件和生成配置.

(2)Carthage是去中心化的依赖库管理工具,安装依赖库时不需要去中心仓库获取CocoaPods所有依赖库的索引,节省了时间.

(3)对项目无侵入性,Carthage设计上也比较简单,利用的都是Xcode自身的功能,开发者在创建依赖库时,相比CocoaPods也简单许多.

(4)Carthage管理的依赖库只需编译一次,项目干净编译时,不会再去重新编译依赖库.

(5)自动将第三方矿建编译为Dynamic framework(动态库).

(6)Carthage与CocoaPods无缝集成,一个项目能同时拥有CocoaPods和Carthage.

Carthage缺点:

(1)仅支持iOS8+.

(2)支持的Carthage安装的第三方框架和依赖库不如CocoaPods丰富.

(3)无法在Xcode定位到源码.

(4)安装包的大小比用CocoaPods的大.

### 3.安装

(1)下载安装包安装.

(2)使用Homebrew安装: 

`brew update`命令更新brew.

`brew install carthage`命令安装Carthage.

`brew upgrade carthage`命令升级Carthage.

`carthage version`命令查看Carthage版本.

```shell
localhost:~ chengshuangshuang$ brew install carthage
==> Downloading https://homebrew.bintray.com/bottles/carthage-0.32.0.mojave.bott
Already downloaded: /Users/chengshuangshuang/Library/Caches/Homebrew/downloads/8b63911fc554a38a05e3e4210b86fe980e630ecc6830e72aaa8c22da808763d6--carthage-0.32.0.mojave.bottle.tar.gz
==> Pouring carthage-0.32.0.mojave.bottle.tar.gz
==> Caveats
Bash completion has been installed to:
/usr/local/etc/bash_completion.d
 
zshcompletions have been installed to:
/usr/local/share/zsh/site-functions
==> Summary
 /usr/local/Cellar/carthage/0.32.0: 69files, 25.2MB
localhost:~ chengshuangshuang$ carthage version
0.32.0
localhost:~ chengshuangshuang$ 
```

(3)使用源码安装: clone项目,进入项目根目录,执行`make install`命令即可.

### 4.使用

(1)cd到项目所在文件夹`cd TestCarthage`,创建Cartfile文件`touch Cartfile`.

(2)编辑Cartfile文件.

```text
git "https://github.com/AFNetworking/AFNetworking.git"

git "https://github.com/zhao0/ReactiveCocoa.git" == 2.5.2
```

(3)执行更新命令`Carthage update --platform iOS`. 需要加上platform,不加上的话就会生成iOS,macOS,tvOS,watchOS等的XXXX.xcworkspace.

(4)更新成功后,项目中会出现一个文件(Cartfile.resolved)和一个文件夹(Carthage).

Carthage/Checkouts: 从github获取的源代码.

Carthage/Build: 编译出来的framework二进制代码库.

(5)配置项目

<1>打开项目,点击Target->Build Phases->Link Library with Libraries,选择Carthage/Build目录中要导入的framework.

<2>添加编译的脚本,该脚本文件保证再提交归档时会对相关文件和dSYMs进行复制

点击Build Phases,点击+ —> New Run Script Phase

①添加脚本 `/usr/local/bin/carthage copy-frameworks`

②添加"Input Files"

```shell
$(SRCROOT)/Carthage/Build/iOS/AFNetworking.framework
$(SRCROOT)/Carthage/Build/iOS/ReactiveCocoa.framework
......
```

(6)调用

```objective-c
// 正确写法
#import <AFNetworking/AFNetworking.h>
//#import <ReactiveCocoa/ReactiveCocoa.h>
#import "ReactiveCocoa/ReactiveCocoa.h"
 
// 错误写法
//#import "AFNetworking.h"
//#import "ReactiveCocoa.h"
```

(7)卸载Carthage `brew uninstall carthage`

(8)更新第三方框架:

<1>更新多个框架 `carthage update --platform iOS`

```shell
localhost:TestPods chengshuangshuang$ carthage update --platform iOS
*** Fetching ReactiveCocoa
*** Fetching AFNetworking
*** Checking out AFNetworking at "3.2.1"
*** Checking out ReactiveCocoa at "2.5.2"
*** xcodebuild output can be found in/var/folders/xd/nxn2kvn532b2rq7phvlzxm2m0000gn/T/carthage-xcodebuild.MpQRfp.log
*** Building scheme "AFNetworking iOS"inAFNetworking.xcworkspace
*** Building scheme "ReactiveCocoa iOS"inReactiveCocoa.xcworkspace
localhost:TestPods chengshuangshuang$
```

<2>更新某个框架 `carthage update ReactiveCocoa --platform iOS`

```shell
localhost:TestPods chengshuangshuang$ carthage update ReactiveCocoa --platform iOS
*** Fetching AFNetworking
*** Fetching ReactiveCocoa
*** Checking out ReactiveCocoa at "2.5.2"
*** xcodebuild output can be found in/var/folders/xd/nxn2kvn532b2rq7phvlzxm2m0000gn/T/carthage-xcodebuild.FXPWVI.log
*** Building scheme "ReactiveCocoa iOS"inReactiveCocoa.xcworkspace
localhost:TestPods chengshuangshuang$
```

