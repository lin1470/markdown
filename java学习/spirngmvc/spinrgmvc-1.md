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

### 总结

```
/**
     * 1. @RequestMapping 除了修饰方法, 还可来修饰类
     * 2.
     *  1). 类定义处: 提供初步的请求映射信息。相对于 WEB 应用的根目录
     *  2). 方法处: 提供进一步的细分映射信息。 相对于类定义处的 URL。若类定义处未标注 @RequestMapping，则方法处标记的 URL
     * 相对于 WEB 应用的根目录
     */
```

@RequestMapping
– 类定义处：提供初步的请求映射信息。相对于 WEB 应用的根目录
– 方法处：提供进一步的细分映射信息。相对于类定义处的 URL。若
类定义处未标注 @RequestMapping，则方法处标记的 URL 相对于
WEB 应用的根目录



## RequestMapping

@RequestMapping
– 类定义处：提供初步的请求映射信息。相对于 WEB 应用的根目录
– 方法处：提供进一步的细分映射信息。相对于类定义处的 URL。若
类定义处未标注 @RequestMapping，则方法处标记的 URL 相对于
WEB 应用的根目录

例如

```Java
@RequestMapping("/springmvc") 注意这个是修饰类上面的.
@Controller
public class SpringmvcTest {
    private static final String SUCCESS="success";

    /**
     * 1. @RequestMapping 除了修饰方法, 还可来修饰类
     * 2.
     *  1). 类定义处: 提供初步的请求映射信息。相对于 WEB 应用的根目录
     *  2). 方法处: 提供进一步的细分映射信息。 相对于类定义处的 URL。若类定义处未标注 @RequestMapping，则方法处标记的 URL
     * 相对于 WEB 应用的根目录
     */
    @RequestMapping("/testResquetsMapping")
    public String testResquetsMapping(){
        System.out.println("testRequestMapping");
        return SUCCESS;
    }
```

>   开始学习的时候可以会有疑问就是RequestMapping的地址究竟要不要加斜杠:
>
>   规范的话,每一个mapping就是一个映射的地址都是应该加斜杠的,类如果加了斜杠就是从项目路径开始,而方法则是相对类的地址的.要不然,这个地址就是一个绝对地址.

### method指定方法

```java
/**
     * 常用: 使用 method 属性来指定请求方式
     */
    @RequestMapping(value = "/testMethod", method = RequestMethod.POST)
    public String testMethod() {
        System.out.println("testMethod");
        return SUCCESS;
    }
```

```html
<form action="springmvc/testMethod" method="post">
      <input type="submit" value="submitaaa">
    </form>
    <br><br>
```

### params和headers

```java
/**
     * 了解: 可以使用 params 和 headers 来更加精确的映射请求. params 和 headers 支持简单的表达式.
     *
     * @return
     */
    @RequestMapping(value = "/testParamsAndHeaders", params = { "username",
            "age!=10" }, headers = { "Accept-Language=zh-CN,zh;q=0.9,en;q=0.8" })
    public String testParamsAndHeaders() {
        System.out.println("testParamsAndHeaders");
        return SUCCESS;
    }
```

```html
<a href="springmvc/testParamsAndHeaders?username=atguigu&age=11">Test ParamsAndHeaders</a>
    <br><br>
```

这里请求参数如果是符合的话,那么才会进来,否则不能映射进来.

### ant风格的通配符

>   Ant 风格资源地址支持 3 种匹配符：
>
>   -   ?：匹配文件名中的一个字符
>   -   \*：匹配文件名中的任意字符
>   -    \**：** 匹 配 多层路径

例如:

>   @RequestMapping 还支持 Ant 风格的 URL：
>   – /user/*/createUser: 匹配
>   /user/aaa/createUser、/user/bbb/createUser 等 URL
>   – /user/**/createUser: 匹配
>   /user/createUser、/user/aaa/bbb/createUser 等 URL
>   – /user/createUser??: 匹配
>   /user/createUseraa、/user/createUserbb 等 URL

```java
@RequestMapping("/testAntPath/*/abc")
public String testAntPath() {
    System.out.println("testAntPath");
    return SUCCESS;
}
```

### pathVariable

```java
/**
 * 这个方法加了个id的占位符,这个占位符可以放到id中去.
 * @param id
 * @return
 */
@RequestMapping("/testPathVariable/{id}")
public String testPathViriable(@PathVariable("id") Integer id){
    System.out.println("testPathVariable"+id);
    return SUCCESS;
}
```

输出如下

>    testPathVariable1

## REST

>   HTTP 协议里面，四个表示操作方式的动
>   词：GET、POST、PUT、DELETE。它们分别对应四种基本操作：GET 用来获
>   取资源，POST 用来新建资源，PUT 用来更新资源，DELETE 用来删除资源。

```java
/**
 * Rest 风格的 URL. 以 CRUD 为例:
 * 新增: /order POST
 * 修改: /order/1 PUT update?id=1
 * 获取: /order/1 GET get?id=1
 * 删除: /order/1 DELETE delete?id=1
 *
 * 如何发送 PUT 请求和 DELETE 请求呢 ?
 * 1. 需要配置 HiddenHttpMethodFilter
 * 2. 需要发送 POST 请求
 * 3. 需要在发送 POST 请求时携带一个 name="_method" 的隐藏域, value为 DELETE 或 PUT
 *
 * 在 SpringMVC 的目标方法中如何得到 id 呢? 使用 @PathVariable 注解
 *
 */
```

首先我们得创建一个过滤器,能够把POST请求转换为PUT和DELETE请求

在web.xml中添加如下:

```xml
<!-- 
	配置 org.springframework.web.filter.HiddenHttpMethodFilter: 可以把 POST 请求转为 DELETE 或 PUT 请求
	-->
    <filter>
        <filter-name>HiddenHttpMethodFilter</filter-name>
        <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
    </filter>
    <!--所有网址的请求都要经过过滤-->
    <filter-mapping>
        <filter-name>HiddenHttpMethodFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```

```java
@RequestMapping("/testRest/{id}")
public String testRestGet(@PathVariable("id") Integer id){
    System.out.println("testRestGet"+ id);
    return SUCCESS;
}

@RequestMapping(value = "/testRest",method = RequestMethod.POST)
public String testRestPost(){
    System.out.println("testRestPost");
    return SUCCESS;
}

@RequestMapping(value =  "/testRest/{id}",method = RequestMethod.PUT)
public String testRestPut(@PathVariable("id") Integer id){
    System.out.println("testRestPut"+ id);
    return SUCCESS;
}

@RequestMapping(value =  "/testRest/{id}",method = RequestMethod.DELETE)
public String testRestDelete(@PathVariable("id") Integer id){
    System.out.println("testRestDelete"+ id);
    return SUCCESS;
}
```

```html
<form action="springmvc/testRest/1" method="post">
  <input  type="hidden" name="_method" value="DELETE">
  <input type="submit" value="test rest delete"/>
</form>
<br><br>

<form action="springmvc/testRest/1" method="post">
  <input  type="hidden" name="_method" value="PUT">
  <input type="submit" value="test rest put"/>
</form>
<br><br>

<form action="springmvc/testRest" method="post">
  <input type="submit" value="test rest post"/>
</form>
<br><br>

<a href="springmvc/testRest/1">test rest get</a>
<br><br>
```



## 请求参数

>   在处理方法入参处使用 @RequestParam 可以把请求参
>   数传递给请求方法
>   – value：参数名
>   – required：是否必须。默认为 true, 表示请求参数中必须包含对应
>   的参数，若不存在，将抛出异常



### RequestParam

```java
/**
 * @RequestParam 来映射请求参数.默认如果没有指定的话,那么就会报错
 * value 值即请求参数的参数名
 * required 该参数是否必须. 默认为 true
 * defaultValue 请求参数的默认值
 */
@RequestMapping(value = "/testRequestParam")
public String testRequestParam(
        @RequestParam(value = "username") String username,
        @RequestParam(value = "age",required = false)Integer age){
    System.out.println("testRequestParam: "+ username+"age "+age);
    return SUCCESS;
}
```

```html
<a href="springmvc/testRequestParam?username=hhh">testRequestParam</a>
<br><br>
```



### RequestHeader

>    请求头包含了若干个属性，服务器可据此获知客户端的信息，通过 @RequestHeader 即可将请求头中的属性值绑定到处理方法的入参中

```java
/**
 * 了解: 映射请求头信息 用法同 @RequestParam
 */
@RequestMapping("/testRequestHeader")
public String testRequestHeader(
        @RequestHeader(value = "Accept-Language") String al) {
    System.out.println("testRequestHeader, Accept-Language: " + al);
    return SUCCESS;
}
```

```html
<a href="springmvc/testRequestHeader">Test RequestHeader</a>
<br><br>
```



### CookieValue

@CookieValue 可让处理方法入参绑定某个 Cookie 值

```java
/**
 * 了解:
 *
 * @CookieValue: 映射一个 Cookie 值. 属性同 @RequestParam
 */
@RequestMapping("/testCookieValue")
public String testCookieValue(@CookieValue("Phpstorm-9f734712") String sessionId) {
    System.out.println("testCookieValue: sessionId: " + sessionId);
    return SUCCESS;
}
```



### 使用 POJO 对象绑定请求参数值

>   Spring MVC 会按请求参数名和 POJO 属性名进行自动匹
>   配，自动为该对象填充属性值。支持级联属性。
>   如：dept.deptId、dept.address.tel 等

```java
/**
 * Spring MVC 会按请求参数名和 POJO 属性名进行自动匹配，
 * 自动为该对象填充属性值。支持级联属性。
 * 如：dept.deptId、dept.address.tel 等
 */
@RequestMapping("/testPojo")
public String testPojo(User user){
    System.out.println("testPojo"+user);
    return SUCCESS;
}
```

```html
form action="springmvc/testPojo" method="post">
  username: <input type="text" name="username"/>
  <br>
  password: <input type="password" name="password"/>
  <br>
  email: <input type="text" name="email"/>
  <br>
  age: <input type="text" name="age"/>
  <br>
  city: <input type="text" name="address.city"/>
  <br>
  province: <input type="text" name="address.province"/>
  <br>
  <input type="submit" value="Submit"/>
</form>
<br><br>
```

### 使用 Servlet API 作为入参

```java
/**
 * 可以使用 Serlvet 原生的 API 作为目标方法的参数 具体支持以下类型
 *
 * HttpServletRequest
 * HttpServletResponse
 * HttpSession
 * java.security.Principal
 * Locale InputStream
 * OutputStream
 * Reader
 * Writer
 */
```

```java
@RequestMapping("/testServletAPI")
    public void testServletAPI(HttpServletRequest request,
                                 HttpServletResponse response,
                                 Writer out) throws IOException {
        System.out.println("testServletAPI ,"+request+response);
        out.write("hello spring mvc");
//        return SUCCESS;
    }
```



## 处理数据模型

### ModelAndView

>   • 控制器处理方法的返回值如果为 ModelAndView, 则其既
>   包含视图信息，也包含模型数据信息。
>   • 添加模型数据:
>   – MoelAndView addObject(String attributeName, Object
>   attributeValue)
>   – ModelAndView addAllObject(Map<String, ?> modelMap)
>   • 设置视图:
>   – void setView(View view)
>   – void setViewName(String viewName)

```java
/**
 * 目标方法的返回值可以是 ModelAndView 类型。
 * 其中可以包含视图和模型信息
 * SpringMVC 会把 ModelAndView 的 model 中数据放入到 request 域对象中.
 * 注意这个类是org.springframework.web.servlet.ModelAndView
 * @return
 */
@RequestMapping("/testModelAndView")
public ModelAndView testModelAndView(){
    String viewName = SUCCESS;
    ModelAndView modelAndView = new ModelAndView(viewName);

    //添加模型数据到 ModelAndView 中.
    modelAndView.addObject("time", new Date());

    return modelAndView;
}
```

在网页上显示是

```html
time: ${requestScope.time}
```



### Map 及 Model

>    – Spring MVC 在调用方法前会创建一个隐
>   含的模型对象作为模型数据的存储容器。
>   – 如果方法的入参为 Map 或 Model 类
>   型，Spring MVC 会将隐含模型的引用传
>   递给这些入参。在方法体内，开发者可以
>   通过这个入参对象访问到模型中的所有数
>   据，也可以向模型中添加新的属性数据

```java
/**
 * 目标方法可以添加 Map 类型(实际上也可以是 Model 类型或 ModelMap 类型)的参数.
 * @param map
 * @return
 */
@RequestMapping("/testMap")
public String testMap(Map<String,Object> map){
    System.out.println(map.getClass().getName());
    map.put("names", Arrays.asList("Tom","Jerry","Mike"));
    return SUCCESS;
}
```

显示如下

```html
names: ${requestScope.names}
names: [Tom, Jerry, Mike]
```

### SessionAttributes

>   • 若希望在多个请求之间共用某个模型属性数据，则可以在
>   控制器类上标注一个 @SessionAttributes, Spring MVC
>   将在模型中对应的属性暂存到 HttpSession 中。
>   • @SessionAttributes 除了可以通过属性名指定需要放到会
>   话中的属性外，还可以通过模型属性的对象类型指定哪些
>   模型属性需要放到会话中
>   – @SessionAttributes(types=User.class) 会将隐含模型中所有类型
>   为 User.class 的属性添加到会话中。
>   – @SessionAttributes(value={“user1”, “user2”})
>   – @SessionAttributes(types={User.class, Dept.class})
>   – @SessionAttributes(value={“user1”, “user2”},
>   types={Dept.class}

```java
/**
 * @SessionAttributes 除了可以通过属性名指定需要放到会话中的属性外(实际上使用的是 value 属性值),
 * 还可以通过模型属性的对象类型指定哪些模型属性需要放到会话中(实际上使用的是 types 属性值)
 *
 * 注意: 该注解只能放在类的上面. 而不能修饰放方法.
 */
@RequestMapping("/testSessionAttributes")
public String testSessionAttributes(Map<String,Object> map){
    User user = new User("tom","123456","tom@atgugu",15);
    map.put("user",user);
    map.put("school", "atguigu");
    return SUCCESS;
}

类头还有个修饰
@SessionAttributes(value = {"user"},types = {String.class})
```