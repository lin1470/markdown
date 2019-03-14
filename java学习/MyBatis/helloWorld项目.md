## 首先创建一个maven项目

pom.xml文件如下

```xml
 <dependencies>
        <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.4.1</version>
        </dependency>

        <!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.47</version>
        </dependency>


        <!-- https://mvnrepository.com/artifact/log4j/log4j -->
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.17</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>

    </dependencies>
```

>   其实这部分的话,完全就是可以按照maven上面的地址填写,当你需要什么的时候,就去添加什么.

## 接着编写mybatis-config.xml

```xml
<configuration>
	<environments default="development">
		<environment id="development">
			<transactionManager type="JDBC" />
			<dataSource type="POOLED">
				<property name="driver" value="com.mysql.jdbc.Driver" />
				<property name="url" value="jdbc:mysql://localhost:3307/mybatis" />
				<property name="username" value="root" />
				<property name="password" value="123456" />
			</dataSource>
		</environment>
	</environments>
	<!-- 将我们写好的sql映射文件（EmployeeMapper.xml）一定要注册到全局配置文件（mybatis-config.xml）中 -->
	<mappers>
		<mapper resource="EmployeeMapper.xml" />
	</mappers>
```

>   很明显,这部分我们要使用的是jdbc的配置问题.而mapper就是对应,我们这个bean类所对应的映射文件.

## mapper.xml映射文件

```xml
<mapper namespace="com.atguigu.mybatis.Employee">
    <!--
    namespace:名称空间
    id: 唯一标识
    resultType:返回值类型
    #{id}: 从传递过来的参数中取出id值
    -->
	<!--注意这个sql语句是写在这里的.-->
    <select id="selectEmp" resultType="com.atguigu.mybatis.Employee">
     select id,last_name lastName,email,gender from table_name where id = #{id}
  </select>
</mapper>
```

## 测试程序编写

```java

    /**
     * 1. 根据xml配置文件(全局配置文件)创建一个SqlSessionFactory对象
     *      有数据源一些运行环境信息
     * 2. sql映射文件,配置了每一个sql,以及sql的封装规则等
     * 3. 将sql映射文件注册在全局配置文件中
     * 4. 写代码:
     *      1). 根据全局配置文件得到sqlSeesionFactory;
     *      2). 使用sqlSessionFactory,获取sqlSession对象使用他来执行sql语句
     *          一个sqlsession就是代表和数据库的一次会话,用完关闭
     *      3). 使用sql唯一标识来告诉Mybatis执行那个sql,sql都是保存在每一个映射文件中.
     *
     * @throws IOException
     */
    @Test
    public void test() throws IOException {
        String resource = "mybatis-config.xml";
        InputStream inputStream = Resources.getResourceAsStream(resource);
        SqlSessionFactory sqlSessionFactory =
                new SqlSessionFactoryBuilder().build(inputStream);

        /**
         * Retrieve a single row mapped from the statement key and parameter.
         * @param <T> the returned object type
         * @param statement Unique identifier matching the statement to use.
         * @param parameter A parameter object to pass to the statement.
         * @return Mapped object
         */
        SqlSession openSession = sqlSessionFactory.openSession();

       try {
           Employee employee = openSession.selectOne("com.atguigu.mybatis.Employee.selectEmp", 1);
           System.out.println(employee);
       }finally {
           openSession.close();
       }

    }
```

## 输出

```java
DEBUG 03-13 14:57:08,637 ==>  Preparing: select id,last_name lastName,email,gender from table_name where id = ?   (BaseJdbcLogger.java:145) 
DEBUG 03-13 14:57:08,785 ==> Parameters: 1(Integer)  (BaseJdbcLogger.java:145) 
DEBUG 03-13 14:57:08,848 <==      Total: 1  (BaseJdbcLogger.java:145) 
Employee{id=1, lastName='tom', email='tom@at.guigu.com', gender='0'}
```



## 基于注解的方式helloworld项目



```java
/**
 * 1. 接口式编程
 * 原生： Dao ====> DaoImpl
 * mbatis: Mapper ===> xxMapper.xml
 *
 * 2. SqlSession代表和数据库的一次对话.用完必须关闭;
 * 3. SqlSession 和connection一样他是非线程安全.每次使用都应该去获取新的对象.
 * 4. mapper接口没有实现类,但是mybatis会为这个接口生成一个代理对象
 *      (将接口和xml进行绑定)
 *      sqlSession.getMapper(EmployeeMapper.class)
 * 5. 两个重要的配置文件:
 *      mabatis的全局配置文件:包含数据库连接池信息,事务管理器信息
 *      sql映射文件:保存了每一个sql语句的映射信息:
 *              将sql抽取出来
 *
 */
```

### 编写一个接口类

```java
package com.atguigu.mybatis.dao;

import com.atguigu.mybatis.bean.Employee;

public interface EmployeeMapper {

    public Employee getEmpById(Integer id);
}

```

// 这个类需要绑定



### 然后测试类代码

```Java
 @Test
    public void test01() throws IOException {
        // 1. 获取sqlSessionFactory对象
        SqlSessionFactory sqlSessionFactory = getSessionFactory();

        // 2. 获取sqlSession对象
        SqlSession openSession = sqlSessionFactory.openSession();

        try {
            // 3. 获取接口的实现类对象
            // 会为接口自动的创建一个代理对象,代理对象去执行增删改查.
            EmployeeMapper mapper = openSession.getMapper(EmployeeMapper.class);

            Employee employee = mapper.getEmpById(2);
            System.out.println(mapper.getClass());
            System.out.println(employee);
        }finally {
            openSession.close();
        }
```

注意:

>   ```java
>   DEBUG 03-14 13:17:44,002 ==>  Preparing: select id,last_name lastName,email,gender from table_name where id = ?   (BaseJdbcLogger.java:145) 
>   DEBUG 03-14 13:17:44,071 ==> Parameters: 1(Integer)  (BaseJdbcLogger.java:145) 
>   DEBUG 03-14 13:17:44,120 <==      Total: 1  (BaseJdbcLogger.java:145) 
>   class com.sun.proxy.$Proxy4
>   Employee{id=1, lastName='tom', email='jerry@atguigu.com', gender='0'}
>   ```
>
>   我们查看一下这个控制台的输出语句,我们可知会为这个mapper创建一个代理对象,然后用代理对象去执行sql语句.



### 总结

>   ```Java
>   /**
>    * 1. 根据xml配置文件(全局配置文件)创建一个SqlSessionFactory对象
>    *      有数据源一些运行环境信息
>    * 2. sql映射文件,配置了每一个sql,以及sql的封装规则等
>    * 3. 将sql映射文件注册在全局配置文件中
>    * 4. 写代码:
>    *      1). 根据全局配置文件得到sqlSeesionFactory;
>    *      2). 使用sqlSessionFactory,获取sqlSession对象使用他来执行sql语句
>    *          一个sqlsession就是代表和数据库的一次会话,用完关闭
>    *      3). 使用sql唯一标识来告诉Mybatis执行那个sql,sql都是保存在每一个映射文件中.
>    *
>    */
>   ```

