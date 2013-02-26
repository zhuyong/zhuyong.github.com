---
layout: post
title: "搭建Android JNI开发环境"
date: 2013-02-26 17:03
comments: true
categories: Develop Envirement
---
每次出差都要在借过来的笔记本搭个环境，甚是麻烦~我懒......  
##### 内容:
- 下载软件:(Eclipse,JDK,Android SDK,Android NDK,Cygwin)
- 安装配置
<!--More-->
##### Eclipse/JDK/Android SDK
略.本地已有,Copy.

##### NDK
[下载Android NDK r8](http://dl.google.com/android/ndk/android-ndk-r8-windows.zip)  

##### Cygwin
[Cygwin官方安装说明](http://cygwin.com/install.html)  
[下载Cygwin安装文件](http://cygwin.com/setup.exe)  

##### 搭建步骤  
1. 下载所需要的软件
2. 安装Cygwin
采用离线安装方式，下载后，以后可以重复使用。
根据需要安装组件,必选Devel category:  
binutils   
gcc   
gcc-mingw   
gdb  
make  
为了省事，也可以全选"Devel"、"Editors"、"Shells"这个三项，安装。     
3. 解压NDK到指定目录
4. 配置Cygwin,找到Cygwin目录下,打开home/用户/.bash_profile文件，添加"NDK=/cygdrive/<你的盘符>/<android ndk 目录>", 例如：
```
NDK_ROOT=/cygdrive/d/android-ndk-r8  
export NDK_ROOT  
```



