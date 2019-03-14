## 数据库的增删改查方法实现

```java

    public Employee getEmpById(Integer id);
    // 如果要想有返回值直接在这里写就可以了.不用在xml里面再写返回值类型
    public Long addEmp(Employee employee);

    public boolean updateEmp(Employee employee );

    public void deleteEmpById(Integer id);

	public Employee getEmpByMap(Map<String,Object> map);

    public Employee getEmpByIdAndLastName(@Param("id")Integer id,@Param("lastName") String lastName);

```

### 查询方法

```xml
    <!--public Employee getEmpById(Integer id); -->
<select id="getEmpById" resultType="com.atguigu.mybatis.bean.Employee">
		select * from table_name where id = #{id}
	</select>
    <select id="getEmpById" resultType="com.atguigu.mybatis.bean.Employee"
            databaseId="mysql">
		select * from table_name where id = #{idsss}
	</select>
```

这里存在一个单参数传入的问题,如果是单参数的话,那么id=#{xxx}这个xxx无论你写什么都可以.

### 添加方法

```xml
<!--    public Long addEmp(Employee employee); -->
<insert id="addEmp" parameterType="com.atguigu.mybatis.bean.Employee"
	useGeneratedKeys="true" keyProperty="id" databaseId="mysql">
		insert into table_name(last_name,email,gender)
		values (#{lastName},#{email},#{gender})
	</insert>
```

注意

>   这个标签中间的就直接是sql语句而已.



### 更新方法

```xml
<!--   public void updateEmp(Employee employee );-->
<update id="updateEmp">
		update  table_name
			set last_name=#{lastName},email=#{email},gender=#{gender}
			where id=#{id}
	</update>
```



## 主键生成方式

```xml
<!--   public void addEmp(Employee employee);-->
	<!--parameterType是可以省略的-->
	<!-- parameterType：参数类型，可以省略，
	获取自增主键的值：
		mysql支持自增主键，自增主键值的获取，mybatis也是利用statement.getGenreatedKeys()；
		useGeneratedKeys="true"；使用自增主键获取主键值策略
		keyProperty；指定对应的主键属性，也就是mybatis获取到主键值以后，将这个值封装给javaBean的哪个属性
	-->
```

mapper文件写法

```xml
<!--    public Long addEmp(Employee employee);-->
	<insert id="addEmp" parameterType="com.atguigu.mybatis.bean.Employee"
	useGeneratedKeys="true" keyProperty="id" databaseId="mysql">
		insert into table_name(last_name,email,gender)
		values (#{lastName},#{email},#{gender})
	</insert>
```

测试程序

```java
Employee employee= new Employee(null,"jerry","jerry@atguigu.com","1");
            mapper.addEmp(employee);
//            employee.getEmail();
            System.out.println(employee.getId());
```

这里只要是设置了上面的mapper写法就能获取到主键,并且还是绑定了在pojo类中.这样我们在程序中也能看到输出.



## 参数传递问题

```
单个参数：mybatis不会做特殊处理，
   #{参数名/任意名}：取出参数值。
```



```
多个参数：mybatis会做特殊处理。
   多个参数会被封装成 一个map，
      key：param1...paramN,或者参数的索引也可以
      value：传入的参数值
   #{}就是从map中获取指定的key的值；

   异常：
   org.apache.ibatis.binding.BindingException:
   Parameter 'id' not found.
   Available parameters are [1, 0, param1, param2]
   操作：
      方法：public Employee getEmpByIdAndLastName(Integer id,String lastName);
      取值：#{id},#{lastName}
1. 可以使用命令参数
【命名参数】：明确指定封装参数时map的key；@Param("id")
	多个参数会被封装成 一个map，
		key：使用@Param注解指定的值
		value：参数值
	#{指定的key}取出对应的参数值
	
2. map传参
```

例如:传入语句

```Java
Employee employee = mapper.getEmpByIdAndLastName(1, "tom");
```

mapper语句要这样写

```xml

	<!--        public Employee getEmpByIdAndLastName(@Param("id")Integer id,@Param("lastName") String lastName);-->

	<select id="getEmpByIdAndLastName" resultType="com.atguigu.mybatis.bean.Employee">
		select * from table_name where id = #{id} and last_name=#{lastName}
	</select>
```

注意接口中写法需要用上注解的方式说明.

或者我们直接传入一个map

```xml
	<!--public Employee getEmpByMap(Map<String,Object> map);-->
	<select id="getEmpByMap" resultType="com.atguigu.mybatis.bean.Employee">
		select * from ${tableName} where id = ${id} and last_name=#{lastName}
	</select>
```

这时测试代码是:

```java
Map<String,Object> map =  new HashMap<String, Object>();
            map.put("id",1);
            map.put("lastName","tom");
            map.put("tableName","table_name");
            Employee employee = mapper.getEmpByMap(map);
//            mapper.get
            System.out.println(employee);
```





```
POJO：
如果多个参数正好是我们业务逻辑的数据模型，我们就可以直接传入pojo；
   #{属性名}：取出传入的pojo的属性值
```

这个就比如我们上面添加方法就是直接传入一个bean类.

```Java
Employee employee= new Employee(null,"jerry","jerry@atguigu.com","1");
            mapper.addEmp(employee);
```

而这时我们使用的mapper是

```xml
<insert id="addEmp" parameterType="com.atguigu.mybatis.bean.Employee"
	useGeneratedKeys="true" keyProperty="id" databaseId="mysql">
		insert into table_name(last_name,email,gender)
		values (#{lastName},#{email},#{gender})
	</insert>
```

获取也是直接用#{xxx}来获取的.不用封装成map也可以.





### 重要思考

```
========================思考================================
public Employee getEmp(@Param("id")Integer id,String lastName);
   取值：id==>#{id/param1}   lastName==>#{param2}

public Employee getEmp(Integer id,@Param("e")Employee emp);
   取值：id==>#{param1}    lastName===>#{param2.lastName/e.lastName}

##特别注意：如果是Collection（List、Set）类型或者是数组，
       也会特殊处理。也是把传入的list或者数组封装在map中。
         key：Collection（collection）,如果是List还可以使用这个key(list)
            数组(array)
public Employee getEmpById(List<Integer> ids);
   取值：取出第一个id的值：   #{list[0]}
```

## 参数获取问题

```
===========================参数值的获取======================================
#{}：可以获取map中的值或者pojo对象属性的值；
${}：可以获取map中的值或者pojo对象属性的值；


select * from tbl_employee where id=${id} and last_name=#{lastName}
Preparing: select * from tbl_employee where id=2 and last_name=?
   区别：
      #{}:是以预编译的形式，将参数设置到sql语句中；PreparedStatement；防止sql注入
      ${}:取出的值直接拼装在sql语句中；会有安全问题；
      大多情况下，我们去参数的值都应该去使用#{}；

      原生jdbc不支持占位符的地方我们就可以使用${}进行取值
      比如分表、排序。。。；按照年份分表拆分
         select * from ${year}_salary where xxx;
         select * from tbl_employee order by ${f_name} ${order}
```

例子:

```java
EmployeeMapper mapper = openSession.getMapper(EmployeeMapper.class);
//            Employee employee = mapper.getEmpByIdAndLastName(1, "tom");
            Map<String,Object> map =  new HashMap<String, Object>();
            map.put("id",1);
            map.put("lastName","tom");
            map.put("tableName","table_name");
            Employee employee = mapper.getEmpByMap(map);
```

>   这里我们传入的map的tableName就是用#{xxx}来获取的.



### 最后需要有个知识点.

```
* 1、mybatis允许增删改直接定义以下类型返回值
*     Integer、Long、Boolean、void
* 2、我们需要手动提交数据
*     sqlSessionFactory.openSession();===》手动提交
*     sqlSessionFactory.openSession(true);===》自动提交
```

