---
title: JSON数据绑定（一）
date: 2026-03-01 21:42:39
categories: Java
tags: 
  - JSON
  - 数据绑定
  - Spring MVC
  - Java EE
password: 335t3
abstract: 私密笔记
---
# JSON数据绑定（一）

【案例】下面通过一个案例来演示如何进行JSON数据交互，具体步骤如下。

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
│   │   │       └───WEB-INF
│   │   │       │    └───web.xml
│   │   │       └───index.jsp
│   └───test
│       └───java
└───pom.xml
```

{% asset_img img01.png%}

## 1.创建应用并导入相关JAR包

pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
         http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>
    <artifactId>JSON01</artifactId>
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
        <!--Jackson转换核心包依赖-->
        <!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-core -->
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-core</artifactId>
            <version>2.9.2</version>
        </dependency>
        <!--Jackson转换的数据绑定包依赖-->
        <!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-databind -->
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.9.2</version>
        </dependency>
        <!--Jackson JSON转换注解包-->
        <!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-annotations -->
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-annotations</artifactId>
            <version>2.9.0</version>
        </dependency>
    </dependencies>
</project>
```

## 2.配置web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
         http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc-servlet.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
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

## 3.配置Spring MVC的核心配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       https://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/mvc
       https://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <context:component-scan base-package="com.zjm.controller"/>
    <!--annotation-driven用于简化开发的配置，
    注解DefaultAnnotationHandlerMapping和AnnotationMethodHandlerAdapter-->
    <mvc:annotation-driven/>
    <!--
    使用resources过滤掉不需要dispatcher servlet的资源。
    在使用resources时必须使用annotation-driven，
    否则resources元素会阻止任意控制器被使用。
    -->
    <!--配置静态资源，允许js目录下的所有文件可见-->
    <mvc:resources mapping="/js/**" location="/js/"/>

    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/pages/"/>
        <property name="suffix" value=".jsp"/>
    </bean>
</beans>
```

## 4.创建POJO类

```java
package com.zjm.pojo;

public class Person {
    private String pname;
    private String password;
    private Integer page;

    public String getPname() {
        return pname;
    }

    public void setPname(String pname) {
        this.pname = pname;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public Integer getPage() {
        return page;
    }

    public void setPage(Integer page) {
        this.page = page;
    }
}
```

## 5.创建JSP页面测试JSON数据交互

```js
<%@ page contentType="text/html;charset=UTF-8" language="java" pageEncoding="UTF-8" %>
<html>
<head>
    <meta http-equiv="Content-Type" content="text/html;charset=UTF-8">
    <title>Title</title>
    <script type="text/javascript"
            src="${pageContext.request.contextPath}/js/jquery-3.7.1.min.js"></script>
    <script type="text/javascript">
        function testJson() {
            // 获取输入的值pname为id
            var pname = $("#pname").val();
            var password = $("#password").val();
            var page = $("#page").val();
            $.ajax({
                // 请求路径
                url: "${pageContext.request.contextPath}/testJson",
                // 请求类型
                type: "post",
                // data表示发送的数据
                data: JSON.stringify({pname: pname, password: password, page: page}),
                // 定义发送请求的数据格式为JSON字符串
                contentType: "application/json;charset=utf-8",
                // 定义回调响应的数据格式为JSON字符串，该属性可以省略
                dataType: "json",
                // 成功响应的结果
                success: function (data) {
                    if (data != null) {
                        alert("输入的用户名：" + data.pname +
                            "，密码：" + data.password + "，年龄：" + data.page);
                    }
                }
            });
        }
    </script>
</head>
<body>
<form action="">
    用户名：<input type="text" name="pname" id="pname"/><br/>
    密&nbsp;&nbsp;&nbsp;码：<input type="password" name="password" id="password"/><br/>
    年&nbsp;&nbsp;&nbsp;龄：<input type="text" name="page" id="page"/><br/>
    <input type="button" value="测试" onclick="testJson()"/>
</form>
</body>
</html>
```

&emsp;&emsp;在index.jsp页面中编写了一个测试JSON交互的表单，当单击“测试”按钮时执行页面中的testJson()函数。在该函数中使用了jQuery的AJAX方式将JSON格式的数据传递给“/testJson”结尾的请求中。

&emsp;&emsp;因为在index.jsp中使用的是jQuery的AJAX进行JSON数据提交和响应，所以还需要引入jquery.js文件。

## 6.创建控制器类

```java
package com.zjm.controller;

import com.zjm.pojo.Person;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class TestController {
    /**
     * 接收页面请求的JSON数据，并返回JSON格式的结果
     */
    @RequestMapping("/testJson")
    @ResponseBody
    public Person testJson(@RequestBody Person user) {
        // 打印接收的JSON格式数据
        System.out.println("pname=" + user.getPname() +
                ",password=" + user.getPassword() + ",page=" + user.getPage());
        // 返回JSON格式的响应
        return user;
    }
}
```

&emsp;&emsp;在上述控制器类中编写了接收和响应JSON格式数据的testJson方法，方法中的@RequestBody注解用于将前端请求体中的JSON格式数据绑定到形参user上，@ResponseBody注解用于直接返回Person对象（当返回POJO对象时默认转换为JSON格式数据进行响应。）

## 7.运行index.jsp页面，测试程序

{% asset_img img02.png%}

控制台输出：

```
pname=歌颂,password=123456,page=23
```

&emsp;&emsp;从上图所示的结果可以看出，编写的代码可以将JSON格式的请求转换为方法中的Java对象，也可以将Java对象转换为JSON格式的响应数据。