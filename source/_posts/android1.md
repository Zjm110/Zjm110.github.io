---
title: Android绪论
date: 2026-03-04 14:49:44
categories: Android
tags: 
  - Android开发
  - Java
password: 33dr3
abstract: 博主自己看的
---
# Android绪论

## Android发展历史

&emsp;&emsp;Android本意指机器人，是第一个真正为手机打造的开放性系统。

&emsp;&emsp;Android操作系统最早来自生产数码相机的公司，是由安迪.罗宾开发出来的，2005年被Google收购，开始Dalvik VM的研究。

{% asset_img img01.png %}

&emsp;&emsp;Google公式在2007年11月向外界展示了这款基于Linux平台的操作系统。2009年4月Google发布的Android1.5，吸引了大量开发者。

{% asset_img img02.png %}

{% asset_img img03.png %}

{% asset_img img04.png %}

## Android体系结构

&emsp;&emsp;Android系统采用分层架构，从高到低分为四层，分别是应用程序层、应用程序框架层、系统库与Android运行层和Linux内核层。层与层间藕合松散，各层分工明确。

### 1.应用程序层

&emsp;&emsp;提供一些核心应用程序包，如短信客户端程序、电话拨号程序、Web浏览器、日历、闹钟等。
+ 电话簿
+ 日历
+ SMS应用程序

&emsp;&emsp;在Android智能终端中，有很多应用软件拍照、联系人管理软件，它们都属于Android的应用程序层。

&emsp;&emsp;应用程序员编写的Android应用程序层，主要调用应用框架层。

### 2.应用程序框架层

&emsp;&emsp;主要提供构建应用程序时用到的各种API。例如，活动管理器、窗体管理器、内容提供者、资源管理器等。

### 3.系统运行库层

#### 系统层

&emsp;&emsp;是应用程序框架的支撑，主要通过C/C++库来为Android系统提供主要的特性支持。例如包含有SQLite嵌入式数据库引擎，WebKit提供浏览器内核的支持、Media Framework多媒体库，支持多种常用的音频、视频格式的录制和回放。

#### Android运行（时）层

&emsp;&emsp;包含核心库和虚拟机，核心库兼容了大多数JAVA语言所需要调用的功能函数，还包含了Android的核心API。

### 4.核心层

&emsp;&emsp;最底层Linux内核层：作为硬件和软件之间的抽象层，为Android设备上的各种硬件提供底层驱动，如显示驱动、音频驱动、蓝牙驱动等。

> Dalvik虚拟机
> &emsp;&emsp;Android程序能够运行，必须有Dalvik虚拟机。
>
> &emsp;&emsp;Google公司因为版权问题重写了虚拟机。
>
> &emsp;&emsp;Android系统基于移动设备，移动设备特点是内存比较小，处理器比较弱，借助于Dalvik VM可以对程序的执行进行进一步的优化。
> + Dalvik虚拟机用来解析、执行程序。
> + Dalvik虚拟机与JAVA虚拟机是互不相容的
>
> Dalvik虚拟机编译文件的过程：
> 
> {% asset_img img05.png %}
>
> Dalvik虚拟机与JAVA虚拟机主要有两大区别：
> 1. 基于不同的结构
> 2. 编译后的文件不同
>
> {% asset_img img06.png %}
> 
> + Java虚拟机：基于栈的结构，运行的是Java字节码。
> + Dalvik虚拟机：基于寄存器结构，运行的是专有文件格式dex。

## Android应用开发技术

### 1.四大组件

{% asset_img img07.png %}

### 2.系统控件

&emsp;&emsp;所有的程序都有界面，界面在Android应用中是很重要的部分。

### 3.SQLite数据库、嵌入式关系型数据库

&emsp;&emsp;Android中还提供有SQLite数据库。

&emsp;&emsp;Android系统自带了这种轻量级、运算速度快的嵌入式关系型数据库，可以通过封装好的API进行操作。

### 4.网络编程

&emsp;&emsp;开发的应用会对网络进行访问。Android的网络编程不但能够实现信息的实时交互，还可以实现移动办公、电子商务等复杂逻辑。

### 5.多媒体

&emsp;&emsp;手机已经成为人们日常娱乐的设备，Android系统提供了多媒体服务。

### 6.地理位置

## Android环境搭建

——软件的下载与安装

&emsp;&emsp;开发Android程序前先要搭建开发环境。最开始Android使用Eclipse作为开发工具，但2005年底Google声明不再对Eclipse提供支持服务，Android Studio将全面取代Eclipse。

### 1.安装JDK

{% asset_img img08.png %}

&emsp;&emsp;配置环境变量：JAVA_HOME指定到JDK目录，JDK的bin目录添加到Path环境变量，新建CLASSPATH将各个jar包的路径添加进去。

### 2.安装Android Studio

{% asset_img img09.png %}

{% asset_img img10.png %}

### 3.启动软件

{% asset_img img11.png %}

{% asset_img img12.png %}

{% asset_img img13.png %}

{% asset_img img14.png %}

{% asset_img img15.png %}

{% asset_img img16.png %}

### 4.新建项目

{% asset_img img17.png %}

域名与应用名称构成当前应用的包名。

{% asset_img img18.png %}

{% asset_img img19.png %}

{% asset_img img20.png %}

{% asset_img img21.png %}

### 5.模拟器创建

&emsp;&emsp;Android程序需要在手机或平板电脑等设备上运行，开发时，可以连接Android手机调试程序，也可以使用开发环境提供的模拟器。

{% asset_img img22.png %}

{% asset_img img23.png %}

{% asset_img img24.png %}

{% asset_img img25.png %}

### 6.程序的运行

{% asset_img img26.png %}

{% asset_img img27.png %}

### 7.在Android Studio中下载SDK

&emsp;&emsp;Android SdK有很多个版本，虽然安装软件时已经附带了SDK，但是Google会不断地更新版本。需要根据需求重新下载。下载SDK需要单击导航栏中的SDKManager图标，进入SDK Manager对话框。

{% asset_img img28.png %}

