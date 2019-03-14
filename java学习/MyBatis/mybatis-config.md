## configuration标签

configuration 配置
• properties 属性
• settings 设置
• typeAliases 类型命名
• typeHandlers 类型处理器
• objectFactory 对象工厂
• plugins 插件
• environments 环境
• environment 环境变量
• transactionManager 事务管理器
• dataSource 数据源
• databaseIdProvider 数据库厂商标识
• mappers 映射器

>   注意这个配置的标签都是有顺序的,具体(文档中说明):
>
>   ```
>   <!ELEMENT configuration (properties?, settings?, typeAliases?, typeHandlers?, objectFactory?, objectWrapperFactory?, reflectorFactory?, plugins?, environments?, databaseIdProvider?, mappers?)>
>   ```

### propertiest标签

```xml
<!--注意下面的标签编写都是得有顺序的.-->
	<!--
		1. mybatis可以使用properties来引入外部properties配置文件的内容:
			resource: 引入类路径下的资源
			url: 引入网络路径或这磁盘路径的资源
	-->
	<properties resource="dbconfig.properties"/>
```

dbconfig.properties

```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3307/mybatis
jdbc.username=root
jdbc.password=123456

```

### setttings

```xml
<!--
	 	2. settings包含很多重要的设置项
	 		setting:用来设置每个设置项
	 			name: 设置名
	 			value: 取值
	 -->
	<settings>
		<setting name="mapUnderscoreToCamelCase" value="true"/> 这个是进行驼峰命名法的设置.
	</settings>
```

### typeAliases

```xml
<!--
		3. typeAliases:别名处理器:可以为我们的Java类型起别名
			注意别名不区分大小写
			1. typeAlias: 为某个Java类型起别名
				type: 指定起别名的全类名,默认别名是类名小写,employee
				alias: 指定新的别名
	-->
	<typeAliases>
		<!--<typeAlias type="com.atguigu.mybatis.bean.Employee" alias="emp"/>-->
		<!-- 2. package: 为某个包下的所有类批量起别名(可能存在别名冲突)
				name: 指定包名(为当前包以及下面的所有的后代包的每一个类都起一个默认别名,(默认小写)

			 3. 批量起别名的情况下,使用@Alias注解为某个类型起别名
		-->
		<package name="com.atguigu.mybatis.bean"/>
	</typeAliases>
```

>   别名尽管是好用,但是有时候这个别名会产生冲突的情况,所以我们一般命名都是用全类名的,这样也有助于我们阅读xml文档.

### environments

```xml
<!--4. environments:环境,mybatis可以配置多种环境信息,default指定某种环境,可以快速切换
			environment: 配置一个具体环境信息,必须有以下两个标签,id 代表当前环境的唯一标识
				transactionManager:事务管理器
						Configuration(org.apache.ibatis.session.Configuration)里面定义的别名而已
					type :管理器类型 [JDBC(JdbcTransactionFactory)|MANAGED(ManagedTransactionFactory)]
						自定义事务管理器类型:实现TransactionFactory接口就行,type指定就行.

				dataSource: 数据源
					type: 数据源类型;[UNPOOLED|POOLED|JNDI]
					自定义数据源:实现DataSourceFactory接口,type是全类名.
	-->
```

例如

```xml
<environments default="dev_mysql">
		<environment id="dev_mysql">
			<transactionManager type="JDBC"/>
			<dataSource type="POOLED">
				<property name="driver" value="${jdbc.driver}" /> #其中这里地方用的就是我们在上面用propertiest标签指定的文件内容.
				<property name="url" value="${jdbc.url}" />
				<property name="username" value="${jdbc.username}" />
				<property name="password" value="${jdbc.password}" />
			</dataSource>
		</environment>

    这里根据不同的环境设置来提取不同的值,下面就是orcl的相应数据源的值.
		<environment id="dev_oracle">
			<transactionManager type="JDBC" />
			<dataSource type="POOLED">
				<property name="driver" value="${orcl.driver}" />
				<property name="url" value="${orcl.url}" />
				<property name="username" value="${orcl.username}" />
				<property name="password" value="${orcl.password}" />
			</dataSource>
		</environment>
	</environments>
```



### databaseIdProvider

```xml
<!-- 5、databaseIdProvider：支持多数据库厂商的；
		 type="DB_VENDOR"：VendorDatabaseIdProvider
		 	作用就是得到数据库厂商的标识(驱动getDatabaseProductName())，mybatis就能根据数据库厂商标识来执行不同的sql;
		 	MySQL，Oracle，SQL Server,xxxx
	  -->
	<databaseIdProvider type="DB_VENDOR">
		<!-- 为不同的数据库厂商起别名 -->
		<property name="MySQL" value="mysql"/>
		<property name="Oracle" value="oracle"/>
		<property name="SQL Server" value="sqlserver"/>
	</databaseIdProvider>

```

我们指定的厂商名字之后,这样我们就可以在相应的映射xml文件中写上多个厂商的对应sql语句,而且会自动匹配出来

例如

```xml
<!--
        这里我们要看环境设置是哪一个,如果是有匹配就加载进去,如果没有环境设置,就加载默认的.
    -->
    <select id="getEmpById" resultType="com.atguigu.mybatis.bean.Employee">
		select * from table_name where id = #{id}
	</select> 这个select语句是默认没有提供厂商的
    <select id="getEmpById" resultType="com.atguigu.mybatis.bean.Employee"
            databaseId="mysql">  这里我们指定databaseId的话就是指定厂商的.
		select * from table_name where id = #{id}
	</select>
    <select id="getEmpById" resultType="com.atguigu.mybatis.bean.Employee"
            databaseId="oracle">
		select EMPLOYEE_ID id,LAST_NAME	lastName,EMAIL email
		from employees where EMPLOYEE_ID=#{id}
	</select>
```

### mappers

```xml
<!-- 将我们写好的sql映射文件（EmployeeMapper.xml）一定要注册到全局配置文件（mybatis-config.xml）中 -->
	<!-- 6、mappers：将sql映射注册到全局配置中 -->
	<mappers>
		<!--
			mapper:注册一个sql映射
				注册配置文件
				resource：引用类路径下的sql映射文件
					mybatis/mapper/EmployeeMapper.xml
				url：引用网路路径或者磁盘路径下的sql映射文件
					file:///var/mappers/AuthorMapper.xml

				注册接口
				class：引用（注册）接口，
					1、有sql映射文件，映射文件名必须和接口同名，并且放在与接口同一目录下；
					2、没有sql映射文件，所有的sql都是利用注解写在接口上;
					推荐：
						比较重要的，复杂的Dao接口我们来写sql映射文件
						不重要，简单的Dao接口为了开发快速可以使用注解；
		-->
		 <!--<mapper resource="EmployeeMapper.xml"/>-->
		 <!--<mapper class="com.atguigu.mybatis.dao.EmployeeMapperAnnotation"/>-->
		<!--<mapper class="com.atguigu.mybatis.dao.EmployeeMapper"/>-->

		<!-- 批量注册： -->
		<package name="com.atguigu.mybatis.dao"/>
	</mappers>
```

这里如果是注册文件的方法就不多说了直接代码

```xml
	 <mapper resource="EmployeeMapper.xml"/>
```

这里指出.xml文件和类接口对应.



其次就是注册接口的方法,有两种

1.  注解版的接口

    -   首先我们写好注解版的dao

    ```java
    package com.atguigu.mybatis.dao;
    
    import com.atguigu.mybatis.bean.Employee;
    import org.apache.ibatis.annotations.Select;
    
    public interface EmployeeMapperAnnotation {
    
        @Select("select * from table_name where id=#{id}")
        public Employee getEmpById(Integer id);
    }
    
    ```

    这里无非就是把映射文件的sql语句直接通过注解的方式写在代码上面而已.

    -   然后我们注册mapper的时候

    ```xml
    -<mapper class="com.atguigu.mybatis.dao.EmployeeMapperAnnotation"/>
    ```

    直接注册这接口的引用路径即可.

2.  .xml文件和接口.java放在同一个目录下.

    ![image.png](https://upload-images.jianshu.io/upload_images/6836439-ee860652b35b31d9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

    这个就是把他们放在同一个目录的具体方法.

    -   然后我们也是直接注册这接口引用即可

        ```xml
        <mapper class="com.atguigu.mybatis.dao.EmployeeMapper"/>
        ```

3.  接着是注册类包的方法

    ```xml
    <!-- 批量注册： -->
    		<package name="com.atguigu.mybatis.dao"/>
    ```


批量注册,这个包下所有的dao都会和同名.xml文件自动匹配.





