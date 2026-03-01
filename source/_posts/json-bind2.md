---
title: JSON数据绑定(二)
date: 2026-03-01 21:52:39
categories: Java
tags: 
  - JSON
  - 数据绑定
  - Spring MVC
  - Java EE
password: 335t3
abstract: 私密笔记
---
# JSON数据绑定

&emsp;&emsp;JSON是一种轻量级的数据交换格式，它与XML非常相似，都可以用于存储数据，但相对于XML来说，JSON解析速度更快，占用空间更小。因此在实际开发中，客户端请求中发送的数据通常为JSON格式。

## 消息转换器—HttpMessageConverter接口

&emsp;&emsp;针对客户端不同的请求，HttpServletRequest中数据的MediaType也会不同，如果想将HttpServletRequest中的数据转换成指定对象，或者将对象转换成指定格式的数据，就需要使用对应的消息转换器来实现。Spring中提供了一个HttpMessageConverter接口作为消息转换器。因为数据的类型有多种，所以Spring中提供了多个HttpMessageConverter接口的实现类，其中，MappingJackson2HttpMessageConverter是HttpMessageConverter接口的实现类之一，在处理请求时，可以将请求的JSON报文绑定到处理器的形参对象，在响应请求时，将处理器的返回值转换成JSON报文。

### HttpMessageConverter与Converter类型转换器的区别

&emsp;&emsp;需要注意的是，HttpMessageConverter消息转换器和之前所学习的Converter类型转换器是有区别的。HttpMessageConverter消息转换器用于将请求消息中的报文数据转换成指定对象，或者将对象转换成指定格式的报文进行响应；Converter类型转换器用于对象之间的类型转换。


## 步骤

### （1）在项目的pom.xml文件中导入Jackson的依赖.

&emsp;&emsp;使用MappingJackson2HttpMessageConverter对JSON数据进行转换和绑定，需要导入Jackson JSON转换核心包、JSON转换的数据绑定包和JSON转换注解包的相关依赖，具体如下所示。

```xml
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
```

&emsp;&emsp;在pom.xml文件中依次导入这3个依赖

### （2）在项目中导入jQuery文件。

&emsp;&emsp;由于本次演示的是异步数据提交，需要使用jQuery，所以需要将jQuery文件导入项目中，以便发送ajax请求。在项目的/webapp文件夹下创建名称为js的文件夹，在js文件夹中导入jQuery文件。 

&emsp;&emsp;在webapp目录下新建文件夹js，将jquery-3.7.1.min.js复制到js目录下。

{% asset_img img01.png%}

### （3）在项目的src/main/webapp目录下创建jsp页面

### （4）创建Controller类

### （5）创建核心配置文件

&emsp;&emsp;在项目的web.xml文件中配置的DispatcherServlet会拦截所有URL，导致项目中的静态资源（如css、jsp、js等）也被DispatcherServlet拦截。如果想放行静态资源，可以在Spring MVC的配置文件（springmvc-servlet.xml）中进行静态资源配置。Spring MVC配置文件的部分配置代码如下所示。

```xml
<mvc:resources mapping="/js/**" location="/js/"/>
```

&emsp;&emsp;说明：&lt;mvc:resources&gt;元素用于配置静态资源的访问路径，配置了静态资源的访问映射后，程序会自动加载配置路径下的静态资源。

### （6）启动项目。

## &lt;mvc:resources …/&gt;的两个重要属性

&lt;mvc:resources …/&gt;有两个重要属性location和mapping，这两个属性的说明如下表所示。


|属性	|说明|
|--------|-----|
|location|	用于定位需要访问的本地静态资源文件路径，具体到某个文件夹|
|mapping	|匹配静态资源全路径，其中“/**”表示文件夹及其子文件夹下的某个具体文件|

## JSON转换器配置和静态资源访问配置

&emsp;&emsp;JSON转换器配置和静态资源访问配置，除了之前讲解的配置方案外，还可以通过其他方式完成，下面讲解两种配置方式，使用&lt;bean&gt;元素配置JSON转换器和静态资源访问的配置方式。具体如下所示。

### 使用&lt;bean&gt;元素配置JSON转换器

&emsp;&emsp;在配置JSON转换器时，除了常用的&lt;mvc:annotation-driven /&gt;元素，还可以使用&lt;bean&gt;元素进行显示的配置，&lt;bean&gt;元素配置JSON转换器方式具体如下所示。 

```xml
<!--<bean>元素配置注解方式的处理器映射器-->
<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping"/>
<!--<bean>元素配置注解方式的处理器适配器-->
<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter">
    <property name="messageConverters">
        <list> <!--配置JSON转换器-->
            <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter"/>
        </list>
    </property>
</beans>
```

&emsp;&emsp;说明：从上述示例代码可以看出，使用&lt;bean&gt;元素配置JSON转换器外，需要同时配置处理器映射器和处理器适配器，并且JSON转换器应配置在适配器中。


### 静态资源访问的配置方式

&emsp;&emsp;除了使用&lt;mvc:resources&gt;元素实现对静态资源的访问外，Spring MVC还提供了另外2种静态资源访问的配置方式。下面分别进行介绍。

#### （1）使用&lt;mvc:default-servlet-handler&gt;配置静态资源

&emsp;&emsp;在Spring MVC的配置文件中，使用&lt;mvc:default-servlet-handler&gt;元素配置静态资源，也可以实现对静态资源的访问。配置静态资源的具体代码如下：
 
&emsp;&emsp;在Spring MVC的配置文件中配置&lt;mvc:default-servlet-handler /&gt;后，Spring MVC会在Spring MVC上下文中定义一个默认的Servlet请求处理器
org.springframework.web.servlet.resource.DefaultServletHttpRequestHandler，该处理器像一个检查员，会对进入DispatcherServlet的URL进行筛查，如果发现是静态资源的请求，就将该请求转由Web服务器默认的Servlet处理，默认的Servlet会对这些静态资源放行；如果不是静态资源的请求，就由DispatcherServlet继续处理。

#### （2）激活Tomcat默认的Servlet来处理静态资源访问

&emsp;&emsp;在web.xml文件中激活Tomcat默认的Servlet去处理对应的静态资源，web.xml配置代码如下所示：

```xml
<!--激活Tomcat默认的Servlet，添加需要处理的静态资源-->
<servlet-mapping>
    <servlet-name>default</servlet-name>
    <url-pattern>*.js</url-pattern>
</servlet-mapping>
<servlet-mapping>
    <servlet-name>default</servlet-name>
    <url-pattern>*.css</url-pattern>
</servlet-mapping>
```
 
&emsp;&emsp;在上述代码中， &lt;servlet-mapping&gt;元素可以激活Tomcat默认的Servlet来处理静态文件。在配置时，可以根据需要继续追加&lt;servlet-mapping&gt;。此种配置方式和第（1）种方式本质上是一样的，都是使用Web服务器默认的Servlet来处理静态资源文件的访问。
