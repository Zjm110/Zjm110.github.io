---
title: 创建首个Android应用
date: 2026-03-04 15:09:44
categories: Android
tags: 
  - Android开发
  - Java
password: 33dr3
abstract: 博主自己看的
---
# 创建首个Android应用

## 1.课程内容——Android应用开发

前置课程：Java编程语言、网页设计；
定位：面向Android平台的应用开发。

&emsp;&emsp;Android原生开发可以直接调取硬件设备，Android可以和c++程序交互。Uni-app通过浏览器来执行程序的。

&emsp;&emsp;这些方向包含：手机端、智能家居端、可穿戴设备端、车机端等等。

相关知识：

移动端应用开发的技术分类：
1. 原生开发：Android、iOS、Flutter（dart）
2. WebApp：前端开发技术、服务器技术作为支撑。前端开发技术：html+css+JavaScript；后端开发技术：JavaWeb、SSM、SpringBoot、Asp\.net core 、PHP。
3. hybridApp:基于浏览器做了定制开发，开放成第三方框架。作用可以调用部分原生api，又可以享受WebApp开发的便捷。例如：uni-app、ionic等等
4. 其他类：微信小程序、支付宝小程序等等。

## 2.Android开发使用的工具

Android开发是给予java编程语言进行开发的。

软件环境：
1. JDK8
1. Android Studio Koala

https\://developer.android.google.cn/studio?hl=zh-cn

硬件环境：

{% asset_img img01.png %}

## 3.移动应用开发领域的职业分类

1. Android工程师
1. 前端开发工程师
1. 后端开发工程师
1. iOS开发工程师

## 4.移动应用开发的编程语言：

1. java、kotlin
1. swift、Objective-C
1. C#
1. html、css、javascript

## 5.Android的知识体系

1. java基础知识：数据类型、表达式、运算符、流程控制、OOP、集合、文件访问、泛型、线程、网络访问
2. android知识：开发工具、开发环境、界面开发、4大组件、数据存储、网络访问、线程。
3. 第三方框架：OkHttp、ButterKnife、Glide、GreenDao、Room、RxJava、Retrofit等等。
4. 项目实战

## 6.安装开发环境

--------------------------

<font color="red">电脑中已有android studio（第一次安装可不看）</font>

### （1）卸载android studio

### （2）删除.gradle

{% asset_img img02.png %}

### （3）删掉默认的项目

{% asset_img img03.png %}

### （4）删除sdk

{% asset_img img04.png %}

<font color="red">若电脑没安装过Android Studio，上面四个步骤可以省略</font>

---------------------------
### （5）下载Android Studio

### （6）双击安装，按向导安装

{% asset_img img05.png %}

{% asset_img img06.png %}

{% asset_img img07.png %}

{% asset_img img08.png %}

Android SDK Location：我的笔记本电脑上安装在D:\android\SDK

{% asset_img img09.png %}

{% asset_img img10.png %}

{% asset_img img11.png %}

{% asset_img img12.png %}

### （7）新建项目

{% asset_img img13.png %}

{% asset_img img14.png %}

{% asset_img img15.png %}

{% asset_img img16.png %}

{% asset_img img17.png %}

{% asset_img img18.png %}

{% asset_img img19.png %}

--------------------------

<font color="red">报错时看</font>

{% asset_img img20.png %}


解决访问
（1）手工下载
（2）放到.gradle目录下

{% asset_img img21.png %}

----------------------------------

### （8）创建模拟器

{% asset_img img22.png %}

{% asset_img img23.png %}

{% asset_img img24.png %}

{% asset_img img25.png %}


-----------------------

<font color="red">模拟器报错</font>

{% asset_img img26.png %}

粘贴到D:\android，删除C:\用户\朱俊梅\ .android

{% asset_img img27.png %}

{% asset_img img28.png %}

配置系统变量：

进入系统变量配置窗口：

{% asset_img img29.png %}

重启即可。

参考：

{% asset_img img30.png %}

--------------------------

### （9）发送程序到模拟器

{% asset_img img31.png %}

{% asset_img img32.png %}