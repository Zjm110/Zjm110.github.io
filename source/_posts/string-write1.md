---
title: 普通字符串的回写
date: 2026-03-01 20:38:38
categories: Java
tags: 
  - 数据回写
  - Spring MVC
  - Java EE
password: 335t3
abstract: 私密笔记
---
# 普通字符串的回写（一）

数据回写

&emsp;&emsp;默认情况下，Spring MVC的响应会经过视图解析器完成页面跳转。有时客户端希望服务器端在响应时不要进行页面跳转，只需要回写相关的数据即可。这个时候可以选择在响应时直接将数据写入输出流中，而不经过视图解析器。根据数据格式，可以将回写到输出流的数据分为普通字符串和JSON数据。

普通字符串的回写

&emsp;&emsp;以数据回写的方式响应时，可以使用Spring MVC默认支持的类型完成数据的输出。


【案例】下面通过HttpServlet Response输出数据的案例，演示普通字符串的回写，案例具体实现步骤如下。

```bash
# JSON项目目录
├───src
│   ├───main
│   │   ├───java
│   │   │   ├───com.zjm
│   │   │   │   ├───Controller
│   │   │   │   │   └───TestController.java
│   │   │   │   └───POJO
│   │   │   │       └───Person.java
│   │   ├───resources
│   │   │       └───springmvc-servlet.xml
│   │   ├───webapp
│   │   │       ├───js
│   │   │       │   ├───jquery-3.2.1.min.js
│   │   │       │   └───jquery-3.7.1.min.js
│   │   │       ├───WEB-INF
│   │   │       │    └───web.xml
│   │   │       └───index.jsp
│   └───test
│       └───java
└───pom.xml
```

（1）在项目的com.ssm.controller包下创建一个数据回写类DataController，在DataController类中定义showDataByResponse()方法，用于测试Spring MVC中普通字符串的回写。DataController类的具体代码如下图所示。

```java
package com.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

import javax.servlet.http.HttpServletResponse;
import java.io.IOException;


@Controller
public class DataController {
    /**
     * 普通字符串的回写
     */
    @RequestMapping("showDataByResponse")
    public void showDataByResponse(HttpServletResponse response) {
        try {
            response.getWriter().print("response");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    /*
    HttpServletResponse对象通过形参传入，
    并使用HttpServletResponse对象将字符串response写入输出流中。
     */
}
```

（2）启动项目，在浏览器中访问地址http:\//localhost:8080/showDataByResponse，访问后，浏览器页面不跳转，页面显示效果如下图所示。

{% asset_img img01.png %}

&emsp;&emsp;由上图所示的内容可以得出，访问地址后执行了showDataByResponse()方法，方法执行后将普通字符串通过HttpServletResponse输出到请求页面中，完成了普通字符串的数据回写。

## 运行环境

pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>
    <artifactId>Rewriter01</artifactId>
    <version>1.0-SNAPSHOT</version>
    <dependencies>
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-core -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>5.2.8.RELEASE</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-beans -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-beans</artifactId>
            <version>5.2.8.RELEASE</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-context -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.2.8.RELEASE</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-expression -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-expression</artifactId>
            <version>5.2.8.RELEASE</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.2.8.RELEASE</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api -->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.1.0</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/javax.servlet.jsp/jsp-api -->
        <dependency>
            <groupId>javax.servlet.jsp</groupId>
            <artifactId>jsp-api</artifactId>
            <version>2.1</version>
        </dependency>
    </dependencies>

</project>
```

springmvc-servlet.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       https://www.springframework.org/schema/context/spring-context.xsd">

    <context:component-scan base-package="com.controller"/>
</beans>
```

web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
         http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <servlet>
        <servlet-name>DispatcherServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc-servlet.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>DispatcherServlet</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
    <filter>
        <filter-name>characterEncodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>characterEncodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
</web-app>
```

