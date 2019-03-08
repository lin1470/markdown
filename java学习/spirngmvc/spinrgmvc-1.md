[TOC]

## Hello world项目

### 新建一个spring mvc的项目

新建这个项目的话,我们要选择了

![image.png](https://upload-images.jianshu.io/upload_images/6836439-a5a0814a0d5b048a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 注意这几个我们要选择的地方,特别是这个spring的 library,如果我们选择的话,那么就会自动帮我们加载jar进来的.
>
> 如果网络不行的话,那么jar包有可能会加载不进来.

### 在web.xml中配置dispacherServlet

```xml
 <!-- 配置 DispatcherServlet -->
    <servlet>
        <servlet-name>dispatcherServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!-- 配置 DispatcherServlet 的一个初始化参数: 配置 SpringMVC 配置文件的位置和名称 -->
        <!--
            实际上也可以不通过 contextConfigLocation 来配置 SpringMVC 的配置文件, 而使用默认的.
            默认的配置文件为: /WEB-INF/<servlet-name>-servlet.xml
        -->
        <!--
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:dispatcherServlet-servlet.xml</param-value>
        </init-param>
        -->

        <load-on-startup>1</load-on-startup>
    </servlet>

    <servlet-mapping>
        <servlet-name>dispatcherServlet</servlet-name>  <!-- 这里的意思所有的请求都往这个写-->
        <url-pattern>/</url-pattern>
    </servlet-mapping>
```

注意web项目在加载的时候首先就是加载这个web.xml的配置文件先的.

这里他会声明一个vervlet的位置在哪里,其实就是这个响应类的配置文件在哪里.

### servlet配置和代码

配置

```xml
<!--配置自定扫描的包-->
    <context:component-scan base-package="com.atguigu.springmvc"/>

    <!--配置试图解析器: 如何把handler方法返回值解析为实际的物理视图-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/views/"/>
        <property name="suffix" value=".jsp"/>
    </bean>
```

> 这里就是spring的普通配置,开始定义扫描的是那个包



代码的话,我们就直接写的是HelloWorld.java

```java
package com.atguigu.springmvc.handlers;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class HelloWorld {

    /**
     * 1.使用@RequestMapping 注解来映射请求的URL
     * 2.返回值会通过试图解释器解析为实际的物理视图,对于InternalResourceViewResolver视图解析器而言,会做如下的解析:
     * 通过prefix+returnVal+suffix 这样的方式得到实际的物理视图,然后会做转发操作
     *
     * /WEB-INF/views/success.jsp
     * @return
     */
    @RequestMapping("/helloworld")
    public String hello(){
        System.out.println("hello world");
        return "success";
    }
}

```

> 1. 使用@RequestMapping 注解来映射请求的URL,
> 2. 返回值会通过试图解释器解析为实际的物理视图,对于InternalResourceViewResolver视图解析器而言,会做如下的解析:
>
>      * 通过prefix+returnVal+suffix 这样的方式得到实际的物理视图,然后会做转发操作
>      * /WEB-INF/views/success.jsp

因为这个转发了物理的视图,所以我们要创建这个success.jsp文件.

### index.jsp 来请求文件

核心

```html
<body>
  $END$
  <a href="helloworld">Hello World</a>
  </body>
```

> 这里的helloworld就是提出请求,而HelloWorld里面的通过注解来声明响应请求.