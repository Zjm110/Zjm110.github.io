---
title: JSON数据交互
date: 2026-03-01 22:10:39
categories: Java
tags: 
  - JSON
  - Spring MVC
  - Java EE
password: 335t3
abstract: 私密笔记
---
# JSON数据交互

&emsp;&emsp;Spring MVC在数据绑定的过程中需要对传递数据的格式和类型进行转换，它既可以转换String等类型的数据，也可以转换JSON等其他类型的数据。

## JSON概述

&emsp;&emsp;JSON（JavaScript Object Notation，JS对象标记）是一种轻量级的数据交换格式。与XML一样，JSON也是基于纯文本的数据格式。它有对象结构和数组结构两种数据结构。

### 对象结构

&emsp;&emsp;对象结构以“{”开始、以“}”结束，中间部分由0个或多个以英文“,”分隔的key/value对构成，key和value之间以英文“:”分隔。对象结构的语法结构如下：

```
{
	key1:value1,
	key1:value1,
	…
}
```

&emsp;&emsp;其中，key必须为String类型，value可以是String、Number、Array等数据类型。例如，一个person对象包含姓名、密码、年龄等信息，使用JSON的表示形式如下：

```
{
	"pname":"陈恒",
	"password":"123456",
	"page":40
}
```

### 数组结构

&emsp;&emsp;数组结构以“[” 开始、以“]”结束，中间部分由0个或多个以英文“,”分隔的值的列表组成。数组结构的语法结构如下：

```
[
    value1,
    value2,
    …
]
```

&emsp;&emsp;上述两种（对象、数组）数据结构也可以分别组合构成更加复杂的数据结构。例如，一个student对象包含sno、sname、hobby和college对象，其JSON的表示形式如下：

```
{
	"sno":"201802228888",
	"sname":"陈恒",
	"hobby":["篮球","足球"],
	"college":{
	"cname":"清华大学",
	"city":"北京"
    }
}
```

## JSON数据转换

&emsp;&emsp;为实现浏览器与控制器类之间的JSON数据交互，Spring MVC提供了MappingJackson2HttpMessageConverter实现类默认处理JSON格式请求响应。该实现类利用Jackson开源包读写JSON数据，将Java对象转换为JSON对象和XML文档，同时也可以将JSON对象和XML文档转换为Java对象。

Jackson开源包及其描述如下。
+ jackson-core-2.9.2.jar：JSON转换核心包
+ jackson-databind-2.9.2.jar：JSON转换的数据绑定包
+ jackson-annotations-2.9.2.jar：JSON转换注解包

&emsp;&emsp;在使用注解开发时需要用到两个重要的JSON格式转换注解，分别是@RequestBody和@ResponseBody。
+ @RequestBody：用于将请求体中的数据绑定到方法的形参中，该注解应用在方法的形参上。
+ @ResponseBody：用于直接返回return对象，该注解应用在方法上。