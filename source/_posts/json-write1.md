---
title: JSON数据的回写
date: 2026-03-01 20:48:38
categories: Java
tags: 
  - JSON
  - 数据回写
  - Spring MVC
  - Java EE
password: 335t3
abstract: 私密笔记
---
# JSON数据的回写（一）

&emsp;&emsp;在实际开发中，将数据回写的需求不会是普通字符串那么简单，更多时候需要回写对象和集合等数据。为此，可以将对象和集合数据转换成JSON数据后进行回写。

## 环境配置

pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>
    <artifactId>JSON06</artifactId>
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

web.xml

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
    <mvc:annotation-driven/>
    <mvc:resources mapping="/js/**" location="/js/"/>
</beans>
```

## 对象数据转换成JSON数据后的回写

【案例】项目中已经导入了Jackson的依赖，可以先调用Jackson的JSON转换的相关方法，将对象或集合转换成JSON数据，然后通过HttpServletResponse将JSON数据写入到输出流中完成回写，具体实现步骤如下。

```bash
# JSON项目目录
├───src
│   ├───main
│   │   ├───java
│   │   │   ├───com.zjm
│   │   │   │   ├───Controller
│   │   │   │   │   └───DataController.java
│   │   │   │   └───POJO
│   │   │   │       ├───Order.java
│   │   │   │       └───User.java
│   │   ├───resources
│   │   │       └───springmvc-servlet.xml
│   │   ├───webapp
│   │   │       ├───js
│   │   │       │   ├───jquery-3.2.1.min.js
│   │   │       │   └───jquery-3.7.1.min.js
│   │   │       └───WEB-INF
│   │   │           └───web.xml
│   └───test
│       └───java
└───pom.xml
```

### POJO类

Order.java

```java
package com.zjm.pojo;

public class Order {
    private String orderId;            // 订单id

    public String getOrderId() {
        return orderId;
    }

    public void setOrderId(String orderId) {
        this.orderId = orderId;
    }
}
```

User.java

```java
package com.zjm.pojo;

import java.util.List;

public class User {
    private String username;          // 用户名
    private String password;          // 用户密码
    private List<Order> orders;       // 用户订单
    private List<String> address;     // 订单地址

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public List<Order> getOrders() {
        return orders;
    }

    public void setOrders(List<Order> orders) {
        this.orders = orders;
    }

    public List<String> getAddress() {
        return address;
    }

    public void setAddress(List<String> address) {
        this.address = address;
    }
}
```

### Contrller类

&emsp;&emsp;将对象转换成JSON数据并写入输出流中完成回写。

```java
package com.zjm.controller;

import com.fasterxml.jackson.databind.ObjectMapper;
import com.zjm.pojo.User;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@Controller
public class DataController {
    /**
     * JSON数据的回写
     * 1.对象数据转换成JSON数据后的回写
     */
    @RequestMapping("showDataByJSON")
    public void showDataByJSON(HttpServletResponse response) {
        // 手动设置响应的字符编码
        response.setContentType("text/html;charset=UTF-8");
        try {
            ObjectMapper om = new ObjectMapper();
            User user = new User();
            user.setUsername("塔尔");
            user.setPassword("666");
            String ujson = om.writeValueAsString(user);
            response.getWriter().print(ujson);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    /*
    上述代码的showDataByJSON()方法中调用Jackson的writeValueAsString()方法，
    方法执行后将User对象的数据转换成JSON格式的数据输出到请求页面中。
     */
}
```

（2）启动项目，在浏览器中访问地址。http:\//localhost:8080/showDataByJSON，访问后，页面显示效果如下图所示。

{% asset_img img01.png %}

&emsp;&emsp;由上图所示的内容可以得出，访问地址后，执行了showDataByJSON()方法，方法执行后将User对象的数据转换成JSON格式的数据输出到请求页面中。

> @ResponseBody注解的使用范围
> 
> &emsp;&emsp;如果每次回写对象或者集合等数据都需要手动转换成JSON数据，那么操作将非常烦琐。为此，Spring MVC提供了@ResponseBody注解，@ResponseBody注解的作用是将处理器返回的对象通过适当的转换器转换为指定的格式之后，写入HttpServletResponse对象的body区。@ResponseBody注解的作用是将处理器返回的对象通过适当的转换器转换为指定的格式之后，写入HttpServletResponse对象的body区。@ResponseBody注解通常用于返回JSON数据。
> 
> &emsp;&emsp;@ResponseBody注解可以标注在方法和类上，当标注在类上时，表示该类中的所有方法均应用@ResponseBody注解。如果需要当前类中的所有方法均应用@ResponseBody注解，也可以使用@RestController注解。@RestController注解相当于@Controller和@ResponseBody这两个注解的结合。
> 
> @ResponseBody注解的2个使用要求
> 若想使用@ResponseBody注解，项目至少需要符合以下2个要求。
> 1. 项目中有转换JSON相关的依赖。
> 2. 可以配置转换JSON数据的消息类型转换器。
> 
> &emsp;&emsp;针对上述两个要求，本项目都已经满足，项目的pom.xml文件中引入了Jackson相关的依赖，可以用于转换JSON；Spring MVC的配置文件中配置的&lt;mvc:annotation-driven /&gt;元素默认注册了Java数据转JSON数据的消息转换器。

## 集合数据转换成JSON数据后的回写

【案例】下面通过一个案例演示使用@ResponseBody注解回写JSON格式的对象数据和集合数据，案例具体实现步骤如下。


```bash
# JSON项目目录
├───src
│   ├───main
│   │   ├───java
│   │   │   ├───com.zjm
│   │   │   │   ├───Controller
│   │   │   │   │   └───DataController.java
│   │   │   │   └───POJO
│   │   │   │       ├───Product.java
│   │   │   │       └───User.java
│   │   ├───resources
│   │   │       └───springmvc-servlet.xml
│   │   ├───webapp
│   │   │       ├───js
│   │   │       │   ├───jquery-3.2.1.min.js
│   │   │       │   └───jquery-3.7.1.min.js
│   │   │       ├───WEB-INF
│   │   │       │    └───web.xml  
│   │   │       └───product_add
│   └───test
│       └───java
└───pom.xml
```

```java
package com.zjm.pojo;

public class Product {
    private String proId;         // 商品id
    private String proName;       // 商品名称

    public String getProId() {
        return proId;
    }

    public void setProId(String proId) {
        this.proId = proId;
    }

    public String getProName() {
        return proName;
    }

    public void setProName(String proName) {
        this.proName = proName;
    }
}
```

```java
package com.zjm.pojo;

public class User {
    private String username;        // 用户名
    private String password;         // 用户密码

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }
}
```

```java
package com.zjm.controller;

import com.zjm.pojo.Product;
import com.zjm.pojo.User;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

import java.util.ArrayList;
import java.util.List;

@Controller
public class DataController {
    /**
     * 2.集合数据转换成JSON数据后的回写
     * getUser()方法：用于返回JSON类型的User信息
     * addProducts()方法：用于返回JSON类型的Product列表信息
     */
    @RequestMapping("getUser")
    @ResponseBody
    public User getUser() {
        User user = new User();
        user.setUsername("heima2");
        return user;
    }

    @RequestMapping("addProducts")
    @ResponseBody
    public List<Product> addProducts() {
        Product p1 = new Product();
        p1.setProId("p001");
        p1.setProName("红牛");
        Product p2 = new Product();
        p2.setProId("p002");
        p2.setProName("三文鱼");
        ArrayList<Product> products = new ArrayList<Product>();
        products.add(p1);
        products.add(p2);
        return products;
    }
    /*
    上述的getUser()方法和addProducts()方法
    都使用@ResponseBody注解将返回的对象转换成JSON数据返回到客户端。
     */
}
```

（2）在项目的src/main/webapp目录下，创建一个商品添加页面product_add.jsp，在product_add.jsp中创建一个表格，用于显示用户信息和添加商品信息。product_add.jsp的具体代码如下图所示。

```js
<%@ page contentType="text/html;charset=UTF-8" language="java" pageEncoding="UTF-8" %>
<html>
<head>
    <title>添加商品</title>
    <script type="text/javascript"
            src="${pageContext.request.contextPath}/js/jquery-3.7.1.min.js"></script>
</head>
<body>
<table id="products" border="1" width="60%">
    <tr align="center">
        <td>欢迎您：</td>
        <td id="username"></td>
    </tr>
    <tr align="center">
        <td colspan="2" align="center">
            <input type="button" value="添加多个商品" onclick="addProducts()">
        </td>
    </tr>
    <tr align="center">
        <td>商品id</td>
        <td>商品名称</td>
    </tr>
</table>

<%--定义了2个JavaScript方法--%>
<script type="text/javascript">
    // 显示当前用户名
    window.onload = function () {
        // 处理器的映射路径
        var url = "${pageContext.request.contextPath}/getUser";
        $.get(url, function (response) {
            // 将处理器返回的用户信息中的用户名显示在表格中
            $("#username").text(response.username);
            // 将处理器返回的数据的username属性值显示在id为username的单元格中
        })
    };
    // 该方法在页面加载完毕后，自动发送异步请求到服务器端的处理器

    // 添加商品
    // 定义addProducts()方法，该方法在提交多个商品信息后触发，并发送异步请求到服务器端处理器
    function addProducts() {
        // 处理器的映射路径
        var url = "${pageContext.request.contextPath}/addProducts";
        $.get(url, function (products) {
            // 将处理器返回的商品列表信息添加到表格中
            for (var i = 0; i < products.length; i++) {
                // 将处理器返回的数据拼接成一个新的<tr>，最后将拼接好的<tr>追加到id为products的表格中
                $("#products").append("<tr><td>" + products[i].proId + "</td>" +
                    "<td>" + products[i].proName + "</td></tr>")
            }
        })
    }
</script>
</body>
</html>
```

（3）启动项目，在浏览器中访问商品添加页面product_add.jspproduct_add.jsp页面显示效果如下图所示。

{% asset_img img02.png %}

（4）单击“添加多个商品”按钮，product_add.jsp页面显示效果如下图所示。

{% asset_img img03.png %}

&emsp;&emsp;由上图所示的内容可以得出，单击“添加多个商品”按钮，程序成功回写了List对应的JSON数据。