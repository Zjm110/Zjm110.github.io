---
title: JSON数据绑定(三)
date: 2026-03-01 21:55:26
categories: Java
tags: 
  - JSON
  - 数据绑定
  - Spring MVC
  - Java EE
password: 335t3
abstract: 私密笔记
---
# 【案例】JSON数据绑定（三）

&emsp;&emsp;下面通过一个异步提交商品案例，演示Spring MVC中的JSON数据绑定，案例具体实现步骤如下。

项目目录：

```bash
# JSON项目目录
├───src
│   ├───main
│   │   ├───java
│   │   │   ├───com.zjm
│   │   │   │   ├───Controller
│   │   │   │   │   └───ProductController.java
│   │   │   │   └───POJO
│   │   │   │       └───Product.java
│   │   ├───resources
│   │   │       └───springmvc-servlet.xml
│   │   ├───webapp
│   │   │       ├───js
│   │   │       │   ├───jquery-3.2.1.min.js
│   │   │       │   └───jquery-3.7.1.min.js
│   │   │       ├───WEB-INF
│   │   │       │    └───web.xml
│   │   │       └───product.jsp
│   └───test
│       └───java
└───pom.xml
```

## 1.创建应用并导入相关JAR包

pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>
    <artifactId>JSON02</artifactId>
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

## 3.配置Spring MVC的核心配置文件

### JSON转换器一：使用&lt;bean&gt;元素配置JSON转换器

&emsp;&emsp;在配置JSON转换器时，可以使用&lt;bean&gt;元素进行显示的配置JSON。

springmvc-servlet.xml
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

    <!--<bean>元素配置注解方式的处理器映射器和处理器适配器必须配对使用-->
    <!--<bean>元素配置注解方式的处理器映射器-->
    <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping"/>
    <!--<bean>元素配置注解方式的处理器适配器-->
    <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter">
        <property name="messageConverters">
            <list>
                <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter"/>
            </list>
        </property>
    </bean>
    <mvc:resources mapping="/js/**" location="/js/"/>
    <!--从上述示例代码可以看出，使用<bean>元素配置JSON转换器时，需要同时配置处理器映射器和处理器适配器，
    并且JSON转换器应配置在适配器中。-->
</beans>
```

### 静态资源访问的配置方式一：使用&lt;mvc:resources&gt;配置静态资源

&emsp;&emsp;在配置JSON转换器时，常用&lt;mvc:annotation-driven&gt;元素。

springmvc-servlet.xml
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
    <!-- 访问静态资源 -->
    <mvc:resources mapping="/js/**" location="/js/"/>
</beans>
```



### 静态资源访问的配置方式二：使用&lt;mvc:default-servlet-handler&gt;配置静态资源

&emsp;&emsp;在Spring MVC的配置文件中，使用&lt;mvc:default-servlet-handler&gt;元素配置静态资源，也可以实现对静态资源的访问。

springmvc-servlet.xml
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
    <!-- 使用<mvc:default-servlet-handler>元素配置静态资源 
    该方法本质上是使用Web服务器默认的Servlet来处理静态资源的访问。-->
    <mvc:default-servlet-handler/>
</beans>
```

### 静态资源访问的配置方式三：激活Tomcat默认的Servlet来处理静态资源访问

&emsp;&emsp;在web.xml文件中激活Tomcat默认的Servlet去处理对应的静态资源。

springmvc-servlet.xml
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
</beans>
```

修改web.xml

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

    <!--激活Tomcat默认的Servlet，添加需要处理的静态资源-->
    <servlet-mapping>
        <servlet-name>default</servlet-name>
        <url-pattern>*.js</url-pattern>
    </servlet-mapping>
    <!-- 在上述代码中，<servlet-mapping>元素可以激活Tomcat默认的Servlet来处理静态文件。
    在配置时，可以根据需要继续追加<servlet-mapping>。
    该方法本质上是使用Web服务器默认的Servlet来处理静态资源的访问。
    -->
</web-app>
```

## 4.创建POJO类

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

## 5.创建JSP页面测试JSON数据交互

&emsp;&emsp;在项目的src/main/webapp目录下创建一个商品信息页面product.jsp，在product.jsp中创建一个表单用于填写商品信息，表单提交时，表单发送异步请求将表单的商品信息发送到处理器。product.jsp的部分代码如下图所示。

```js
<%@ page contentType="text/html;charset=UTF-8"
         language="java" pageEncoding="UTF-8" %>
<html>
<head>
    <title>异步提交商品</title>
    <%--引入js文件夹下的jquery-3.7.1.min.js--%>
    <script type="text/javascript"
            src="${pageContext.request.contextPath}/js/jquery-3.7.1.min.js"></script>
</head>
<body>
<%--创建了一个包含2个商品信息的表单--%>
<form id="products">
    <table border="1">
        <tr>
            <th>商品id</th>
            <th>商品名称</th>
            <th>提交</th>
        </tr>
        <tr>
            <td>
                <input name="proId" value="1" id="proId" type="text">
            </td>
            <td>
                <input name="proName" value="三文鱼" id="proName" type="text">
            </td>
            <td>
                <input type="submit" value="提交单个商品" onclick="submitProduct()">
            </td>
        </tr>
        <tr>
            <td>
                <input name="proId" value="2" id="proId2" type="text">
            </td>
            <td>
                <input name="proName" value="红牛" id="proName2" type="text">
            </td>
            <td>
                <input type="submit" value="提交多个商品" onclick="submitProducts()">
            </td>
        </tr>
    </table>
</form>
<script type="text/javascript">
    /*定义了JavaScript方法submitProduct()，
    该方法被触发时，将单个商品的信息以JSON格式异步发送到处理器*/
    function submitProduct() {
        var proId = $("#proId").val();
        var proName = $("#proName").val();
        $.ajax({
            url: "${pageContext.request.contextPath}/getProduct",
            type: "post",
            data: JSON.stringify({proId: proId, proName: proName}),
            contentType: "application/json;charset=UTF-8",
            DataType: "json",
            success: function (response) {
                alert(response);
            }
        });
    }

    /*定义了JavaScript方法submitProducts(),
    该方法将表单中2个商品的信息以JSON格式异步发送到处理器*/
    function submitProducts() {
        var pro1 = {proId: $("#proId").val(), proName: $("#proName").val()};
        var pro2 = {proId: $("#proId2").val(), proName: $("#proName2").val()};
        $.ajax({
            url: "${pageContext.request.contextPath}/getProductList",
            type: "post",
            data: JSON.stringify([pro1, pro2]),
            contentType: "application/json;charset=UTF-8",
            DataType: "json",
            success: function (response) {
                alert(response);
            }
        });
    }
</script>
</body>
</html>
```

## 6.创建控制器类

&emsp;&emsp;在ProductController类中创建getProduct()方法和getProductList()方法，分别用于获取客户端提交的单个商品信息和多个商品信息。由于客户端发送的是JSON格式的数据，此时，在处理器中无法直接使用方法形参接收数据，以及完成数据的自动绑定。对此，可以使用Spring MVC提供的@RequestBody注解。@RequestBody注解结合Jackson提供的JSON格式转换器，即可将JSON格式数据绑定到方法形参中。在添加@RequestBody注解时，需要将@RequestBody注解书写在方法的形参前。

```java
package com.zjm.controller;

import com.zjm.pojo.Product;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;

import java.util.List;

@Controller
public class ProductController {
    /**
     * JSON数据绑定
     * 获取单个商品信息
     */
    @RequestMapping("/getProduct")
    public void getProduct(@RequestBody Product product) {
        String proId = product.getProId();
        String proName = product.getProName();
        System.out.println("获取到了Id为" + proId + "名称为" + proName + "的商品");
    }


    /**
     * JSON数据绑定
     * 获取多个商品信息
     */
    @RequestMapping("/getProductList")
    public void getProductList(@RequestBody List<Product> products) {
        for (Product product : products) {
            String proId = product.getProId();
            String proName = product.getProName();
            System.out.println("获取到了Id为" + proId + "名称为" + proName + "的商品");
        }
    }
}
```

## 7.运行product.jsp页面，测试程序

在上图所示的页面中，单击右侧的“提交单个商品”按钮，product.jsp表单中的单个商品信息以JSON格式异步发送到服务器端getProduct()方法中。提交单个商品时控制台输出信息如下图所示。

{% asset_img img01.png %}

&emsp;&emsp;从上图所示的输出信息可以得出，客户端异步提交的JSON数据按照形参product属性的格式进行关联映射，并赋值给product对应的属性，完成了JSON数据的绑定。


&emsp;&emsp;在上图所示的页面中，单击“提交多个商品”按钮，product.jsp表单中的2个商品信息以JSON格式异步发送到服务器端getProductList()方法中。提交多个商品时控制台输出信息如下图所示。

{% asset_img img02.png %}

&emsp;&emsp;从上图所示的输出信息可以得出，客户端异步提交的JSON数据按照形参products的存储结构进行关联映射，并赋值给products中对象的对应属性，完成了JSON数据的绑定。

{% asset_img img03.png %}

