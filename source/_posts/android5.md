---
title: 创建新布局
date: 2026-03-04 18:21:20
categories: Android
tags: 
  - Android开发
  - Java
---
# 创建新布局

如何实现控件在界面上布局呈现？
为什么在一个activity_main.xml上添加控件，界面上才可以显示出来？

&emsp;&emsp;App启动后，在androidManifest.xml中找到入口MainActivity，然后调用onCreate()方法，再执行其中的setContentView(R.layout.activity_main),这个方法就会在资源文件中找到layout/activity_main.xml，加载文件到界面上，此时布局中就可以看到某某控件。

【案例】为Activity替换新的布局。

{% asset_img img01.png %}

{% asset_img img02.png %}

{% asset_img img03.png %}

-------------------------------

{% asset_img img04.png %}