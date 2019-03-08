## 开始学习Spring

### 新建helloWorld项目

这个步骤就省略了,不过研究了一下,发现在idea中的很多东西都很方便具体

1. idea中的module相当于是一个project,而project则是对应的workspace.

2. 选择新建一个module

   ![image.png](https://upload-images.jianshu.io/upload_images/6836439-56bace0377bd0043.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

   2. 其次编写一般的HelloWorld类

      ```java
      public class HelloWorld {
          private String name;
      
          public String getName() {
              return name;
          }
      
          public void setName2(String name) {
              System.out.println("setName2:"+ name);
              this.name = name;
          }
      
          public void hello(){
              System.out.println("hello"+ this.name);
          }
      
          public HelloWorld(){
              System.out.println("HelloWorld's Constructor");
          }
      }
      
      ```

      再来个Main.java

      ```java
      import org.springframework.context.ApplicationContext;
      import org.springframework.context.support.ClassPathXmlApplicationContext;
      
      public class Main {
          public static void main(String[] args) {
      //        HelloWorld helloWorld = new HelloWorld();
      //        helloWorld.setName("atguigu");
      //        helloWorld.hello();
      
              // 1. 创建Spring的IOC容器对象
              ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
      
              // 2. 从IOC容器中获取bean实例
              HelloWorld helloWorld = (HelloWorld)ctx.getBean("helloWorld");
      
              //3. 调用hello方法
              helloWorld.hello();
      //        System.out.println("hello world");
          }
      }
      
      ```

      > 在这里我们可以看到,如果不用Spring 的话,那么这个就是我们平时用的java的形式
      >
      > 接下来我们配置Spring的配置文件,就是配置bean

   3. 创建一个配置文件applicationContext.xml文件

      ```
      new -> xml configefile -> spring config
      ```

      内容如下

      ```xml
      <?xml version="1.0" encoding="UTF-8"?>
      <beans xmlns="http://www.springframework.org/schema/beans"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
      <!--bean配置-->
          <bean id="helloWorld" class="HelloWorld">
              <property name="name2" value="Spring"></property>
          </bean>
      </beans>
      ```

      > 看上面这个首先是声明一下beans的属性,这个是自动创建的,不用例会.
      >
      > **重点**:bean的配置
      >
      > id可以理解为一个对象的名称.class就是这个对象的类名称
      >
      > property 就是引用的对应的方法,这个就是setName2的方法,并且把值赋予的是Spring.

      4. 接着

         ```
         // 1. 创建Spring的IOC容器对象
                 ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
         
                 // 2. 从IOC容器中获取bean实例
                 HelloWorld helloWorld = (HelloWorld)ctx.getBean("helloWorld");
         
                 //3. 调用hello方法
                 helloWorld.hello();
         //        System.out.println("hello world");
         ```

         > - 创建ioc对象,这个就是要背下这个名称就行
         >
         > - 获取bean实例
         >
         >   后去这个实例的过程中,实际上就是通过配置文件来创建一个对象,然后调用对象的方法,最后返回这个类名称,最后强转类型即可.(其实这个步骤就是平时创建,和调用方法的步骤都交给了配置文件实现了而已.)
         >
         > - 然后调用方法