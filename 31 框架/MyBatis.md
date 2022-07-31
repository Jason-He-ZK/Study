## 概述

三层架构

```xml
三层中类的交互：用户使用界面层--> 业务逻辑层--->数据访问层（持久层）-->数据库（mysql）

界面层：    和用户打交道的， 接收用户的请求参数， 显示处理结果的。（jsp ，html ，servlet）
业务逻辑层： 接收了界面层传递的数据，计算逻辑，调用数据库，获取数据
数据访问层（持久层）： 就是访问数据库， 执行对数据的查询，修改，删除等等的。

三层对应的包
界面层：    controller包 （servlet）
业务逻辑层： service 包（XXXService类）
数据访问层： dao包（XXXDao类）

三层对应的处理框架
界面层---servlet---springmvc（框架）
业务逻辑层---service类--spring（框架）
数据访问层---dao类--mybatis（框架）
```

框架

- 框架是一个模块

   - 定义好了一些功能。这些功能是可用的。
   - 可以加入项目中自己的功能， 这些功能可以利用框架中写好的功能。
- 框架是一个半成品的软件，定义好了一些基础功能， 需要加入你的功能就是完整的。基础功能是可重复使用的，可升级的。
- 框架特点：

   - 框架一般不是全能的， 不能做所有事情
   - 框架是针对某一个领域有效。 特长在某一个方面，比如 mybatis 做数据库操作强，但是他不能做其它的。
   - 框架是一个软件

## mybatis 框架

```xml
MyBatis：SQL Mapper Framework for Java （sql映射框架）

1）sql mapper :sql映射
  可以把数据库表中的一行数据  映射为一个java对象。
  一行数据可以看做是一个java对象。操作这个对象，就相当于操作表中的数据
2）Data Access Objects（DAOs） : 数据访问 ， 对数据库执行增删改查。
```

```xml
// mybatis提供的功能
1. 提供了创建Connection ,Statement, ResultSet的能力 ，不用开发人员创建这些对象了
2. 提供了执行sql语句的能力， 不用你执行sql
3. 提供了循环sql， 把sql的结果转为java对象， List集合的能力
4. 提供了关闭资源的能力，不用你关闭Connection, Statement, ResultSet

// 开发人员做的是
提供sql语句

最后：开发人员提供sql语句--mybatis处理sql---开发人员得到List集合或java对象（表中的数据）
```

## 框架入门

步骤

1. 
建表，建实体类

2. 
pom.xml 中加入 mybatis 坐标，mysql 驱动坐标

3. 
创建 Dao 接口，定义数据库操作方法

4. 
创建 mybatis 的配置文件（SQL 映射文件）

写 SQL 语句的，一般一个表一个 SQL 映射文件，文件名同 Dao 接口名，扩展名为 xml
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!--约束文件，mybatis-3-mapper.dtd是约束文件的名称，扩展名是dtd的
    作用：限制，检查在当前文件中出现的标签，属性必须符合mybatis的要求-->
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--mapper是根标签，必须的
    namespace：叫做命名空间，唯一值的， 可以是自定义的字符串，要求使用dao接口的全限定名称-->  
<mapper namespace="com.bjpowernode.dao.StudentDao">

    <!-- <select>:表示执行，<update>:表示更新，<insert>:表示插入，<delete>:表示删除 -->
    <!--
       select:表示查询操作。
       id: 你要执行的sql语法的唯一标识，可以自定义，但是要求你使用接口中的方法名称。
       resultType:表示结果类型的，是sql语句执行后得到ResultSet，值写的类型的全限定名称 -->

    <select id="selectStudents" resultType="com.bjpowernode.domain.Student" >
        select id,name,email,age from student order by id
    </select>

    <insert id="insertStudent">
        insert into student values(#{id},#{name},#{email},#{age})
    </insert>
</mapper>
```


5. 
创建 mybatis 的主配置文件

提供数据库连接信息和 SQL 映射文件位置信息
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!--约束文件，mybatis-3-config.dtd是约束文件的名称-->
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<!--根标签-->
<configuration>
    <!--环境配置： 数据库的连接信息
        default:必须和其中某个environment的id值一样。告诉mybatis使用哪个数据库的连接信息-->
    <environments default="mydev">
        <!-- environment : 一个数据库信息的配置，环境
             id:一个唯一值，自定义，表示环境的名称。
        -->
        <environment id="mydev">
            <!--transactionManager ：mybatis的事务类型
                   type: JDBC(表示使用jdbc中的Connection对象的commit，rollback做事务处理)
            -->
            <transactionManager type="JDBC"/>
            <!--dataSource:表示数据源，连接数据库的
                  type：表示数据源的类型， POOLED表示使用连接池
            -->
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/springdb"/>
                <property name="username" value="root"/>
                <property name="password" value="123456"/>
            </dataSource>
        </environment>

        <!--表示线上的数据库，是项目真实使用的库-->
        <environment id="online">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/onlinedb"/>
                <property name="username" value="root"/>
                <property name="password" value="fhwertwr"/>
            </dataSource>
        </environment>
    </environments>

    <!-- sql mapper(sql映射文件)的位置-->
    <mappers>
        <mapper resource="com/bjpowernode/dao/StudentDao.xml"/>
        <mapper resource="com/bjpowernode/dao/SchoolDao.xml" />
        <!--多个mapper文件，使用package指定路径-->
        <!--<mapper resource="com/bjpowernode/dao"/>-->
    </mappers>
</configuration>
```


6. 
创建、使用 mybatis 类
```java
//访问mybatis读取student数据

//1.定义mybatis主配置文件的名称, 从类路径的根开始（target/clasess）
String config="mybatis.xml";
//2.读取这个config表示的文件
InputStream in = Resources.getResourceAsStream(config);
//3.创建了SqlSessionFactoryBuilder对象
SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
//4.创建SqlSessionFactory对象
SqlSessionFactory factory = builder.build(in);
//5.获取SqlSession对象，从SqlSessionFactory中获取SqlSession
SqlSession sqlSession = factory.openSession();
//6.【重要】指定要执行的sql语句的标识。  sql映射文件中的namespace + "." + 标签的id值
//String sqlId = "com.bjpowernode.dao.StudentDao" + "." + "selectStudents";
String sqlId = "com.bjpowernode.dao.StudentDao.selectStudents";
//7. 重要】执行sql语句，通过sqlId找到语句
List<Student> studentList = sqlSession.selectList(sqlId);
//8.输出结果
//studentList.forEach( stu -> System.out.println(stu));
for(Student stu : studentList){
    System.out.println("查询的学生="+stu);
}
//9.关闭SqlSession对象
sqlSession.close();
```

```java
//插入student数据
String config="mybatis.xml";
InputStream in = Resources.getResourceAsStream(config);
SqlSessionFactoryBuilder builder  = new SqlSessionFactoryBuilder();
SqlSessionFactory factory = builder.build(in);
SqlSession sqlSession = factory.openSession(true);
String sqlId = "com.bjpowernode.dao.StudentDao.insertStudent";
Student student  = new Student();
student.setId(1006);
student.setName("关羽");
student.setEmail("guanyu@163.com");
student.setAge(20);
int nums = sqlSession.insert(sqlId,student);
//mybatis默认不是自动提交事务的， 所以在insert ，update ，delete后要手工提交事务
//sqlSession.commit();
System.out.println("执行insert的结果="+nums);
sqlSession.close();
```



主要类的介绍

```java
Resources： mybatis中的一个类， 负责读取主配置文件（2）
  InputStream in = Resources.getResourceAsStream("mybatis.xml");
SqlSessionFactoryBuilder : 创建SqlSessionFactory对象（3）
  SqlSessionFactoryBuilder builder  = new SqlSessionFactoryBuilder();
  SqlSessionFactory factory = builder.build(in);
*SqlSessionFactory ： 重量级对象， 程序创建一个对象耗时比较长，使用资源比较多（在整个项目中，有一个就够用了）
  SqlSessionFactory:接口  ， 接口实现类： DefaultSqlSessionFactory
  SqlSessionFactory作用： 获取SqlSession对象。SqlSession sqlSession = factory.openSession();
  openSession()方法说明：
    openSession() ：无参数的， 获取是非自动提交事务的SqlSession对象
    openSession(boolean)：true获取自动提交事务的SqlSession，false获取非自动提交事务的SqlSession
SqlSession:
  SqlSession接口 ：定义了操作数据的方法 例如 selectOne() ,selectList() ,insert(),update(), delete(), commit(), rollback()
  SqlSession接口的实现类DefaultSqlSession。
  使用要求： SqlSession对象不是线程安全的，需要在方法内部使用， 在执行sql语句之前，使用openSession()获取SqlSession对象。
  在执行完sql语句后，需要关闭它，执行SqlSession.close(). 这样能保证他的使用是线程安全的。
```

工具类简化以上操作

```java
public class MyBatisUtils {
    private  static  SqlSessionFactory factory = null;
    static {
        String config="mybatis.xml"; // 需要和你的项目中的文件名一样
        try {
            InputStream in = Resources.getResourceAsStream(config);
            //创建SqlSessionFactory对象，使用SqlSessionFactoryBuild
            factory = new SqlSessionFactoryBuilder().build(in);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    //获取SqlSession的方法
    public static SqlSession getSqlSession() {
        SqlSession sqlSession  = null;
        if( factory != null){
            sqlSession = factory.openSession();// 非自动提交事务
        }
        return sqlSession;
    }
}
```

```java
public static void main(String[] args) throws IOException {
    //获取SqlSession对象，从SqlSessionFactory中获取SqlSession
    SqlSession sqlSession = MyBatisUtils.getSqlSession();
    //【重要】指定要执行的sql语句的标识。  sql映射文件中的namespace + "." + 标签的id值
    String sqlId = "com.bjpowernode.dao.StudentDao.selectStudents";
    //【重要】执行sql语句，通过sqlId找到语句
    List<Student> studentList = sqlSession.selectList(sqlId);
    //输出结果
    studentList.forEach( stu -> System.out.println(stu));
    //关闭SqlSession对象
    sqlSession.close();
}
```

## ** 框架 Dao 代理

动态代理： 使用 SqlSession.getMapper(dao 接口.class) 获取这个 dao 接口的对象

### 传入参数

从 java 代码中把数据传入到 mapper 文件的 sql 语句中。

parameterType ： 写在 mapper 文件中的 一个属性。 表示 dao 接口中方法的参数的数据类型。

不是强制的，mybatis 通过反射机制能够发现接口参数的数类型（一般不写）

获取参数方法

1. 
获取一个简单类型（java 基本类型 +string）的参数： #{任意字符}#

2. 
获取多个参数：使用 [@Param ](/Param ) 来命名参数 
```java
格式：@Param("参数名")  String name 

public List<Student> selectMulitParam(@Param("myname") String name, @Param("myage") Integer age)
```

```xml
<!--使用：#{参数名}-->

<select>
  select * from student where name=#{myname} or age=#{myage}
</select>
```


3. 
获取多个参数，使用 java 对象的属性值，作为参数实际值
```xml
<!--多个参数， 使用java对象的属性值，作为参数实际值
    使用对象语法： #{属性名,javaType=类型名称,jdbcType=数据类型} 很少用。
                javaType:指java中的属性数据类型。
                jdbcType:在数据库中的数据类型。
                例如： #{paramName,javaType=java.lang.String,jdbcType=VARCHAR}
    我们使用的简化方式： #{属性名}  ，javaType, jdbcType的值mybatis反射能获取。不用提供
-->
```


4. 
获取多个参数，通过位置获取（了解）

5. 
获取多个参数，使用 map（了解）


#和 $#

```
# 和 $区别
1. #使用 ？在sql语句中做站位的， 使用PreparedStatement执行sql，效率高
2. #能够避免sql注入，更安全。
3. $不使用占位符，是字符串连接方式，使用Statement对象执行sql，效率低
4. $有sql注入的风险，缺乏安全性。（能确定数据是安全的。可以使用$）

#{studentId}：# 的结果：select id,name, email,age from student where id=? 

${studentId}：$ 的结果：select id,name, email,age from student where id=1001
String sql="select id,name, email,age from student where id=" + "1001";
使用的Statement对象执行sql， 效率比PreparedStatement低。

$:可以替换表名或者列名，比如可用于排序时的列名
```

### 输出结果

mybatis 执行了 sql 语句，得到 java 对象。

resultMap 和 resultType 不要一起用，二选一

resultType：结果类型

- 指 sql 语句执行完毕后，数据转为的 java 对象， java 类型是任意的。（不能是 list）

resultType 结果类型的值 1. 类型的全限定名称   2. 类型的别名， 例如 java.lang.Integer 别名是 int
- 处理方式：

   - mybatis 执行 sql 完语句， 然后调用类的无参数构造方法创建对象。再把 ResultSet 指定列值付给同名的属性。```java
<select id="selectMultiPosition" resultType="com.bjpowernode.domain.Student">
  select id,name, email,age from student
</select>
```


- 定义自定义类型的别名（更建议用全限定名称而不是别名）

在 mybatis 主配置文件中定义，使  定义别名

可以在 resultType 中使用自定义别名```xml
<typeAliases>
    <!--
        第一种方式：
        可以指定一个类型一个自定义别名
        type:自定义类型的全限定名称，alias:别名（短小，容易记忆的）
    -->
    <typeAlias type="com.bjpowernode.domain.Student" alias="stu" />
    <typeAlias type="com.bjpowernode.vo.ViewStudent" alias="vstu" />
</typeAliases>

<typeAliases>
    <!--
      第二种方式（使用较多）
      <package> name是包名， 这个包中的所有类，类名就是别名（类名不区分大小写）
    -->
    <package name="com.bjpowernode.domain"/>
    <package name="com.bjpowernode.vo"/>
</typeAliases>
```


- map（使用较少，推荐用对象）```xml
<!--返回Map
    1）列名是map的key， 列值是map的value
    2)只能最多返回一行记录。多余一行是错误
-->
<select id="selectMapById" resultType="java.util.HashMap">
    select id,name,email from student where id=#{stuid}
</select>
```



resultMap：结果映射

- 指定列名和 java 对象的属性对应关系
- 使用条件：

   - 你想自定义列值赋值给哪个属性
   - 当你的列名和属性名不一样时，推荐使用 resultMap（也有别的方法，“as”）

```xml
<!--使用resultMap
    1)先定义resultMap
    2)在select标签，使用resultMap来引用1定义的。
-->
<!--定义resultMap
    id:自定义名称，表示你定义的这个resultMap， type：java类型的全限定名称
-->
<resultMap id="studentMap" type="com.bjpowernode.domain.Student">
    <!--列名和java属性的关系-->
  
    <!--主键列，使用id标签， column :列名， property:java类型的属性名-->
    <id column="id" property="id" />
    <!--非主键列，使用result-->
    <result column="name" property="name" />
    <result column="email" property="email" />
    <result column="age" property="age" />
</resultMap>

<select id="selectAllStudents" resultMap="studentMap">
    select id,name, email , age from student
</select>
```

```xml
<!--列名和属性名不一样：第二种方式
   resultType的默认原则是 同名的列值赋值给同名的属性， 使用列别名(java对象的属性名)
-->
<select id="selectDiffColProperty" resultType="com.bjpowernode.domain.MyStudent">
    select id as stuid ,name as stuname, email as stuemail , age stuage from student
</select>
```

### 模糊查询

方法一：java 代码指定 like 后的内容，再传入（最简单方便，推荐）

```
String name = "%李%";
```

方法二：在 mapper 文件中拼接 like 的内容

```xml
<!--#{name}左右要有空格-->
<select id="selectLikeTwo" resultType="com.bjpowernode.domain.Student">
    select id,name,email,age from student where name  like "%" #{name} "%"
</select>
```

## ** 动态 sql

sql 的内容是变化的，可以根据条件获取到不同的 sql 语句（主要是 where 部分发生变化）

动态 sql 的实现，使用的是 mybatis 提供的标签 < if> ,< where>,< foreach>

1. < if> 是判断条件的，

```xml
<!-- if
     <if:test="使用参数java对象的属性值作为判断条件，语法 属性=XXX值">
-->
<select id="selectStudentIf" resultType="com.bjpowernode.domain.Student">
    select select id,name, age, email from student
    where id > 0
    <if test="name !=null and name !='' ">
       and name = #{name}
    </if>
    <if test="age > 0">
        or age > #{age}
    </if>
</select>
```

1. < where> 用来包含多个  的

当多个 if 有一个成立的， 会自动增加一个 where 关键字，并去掉 if 中多余的 and，or 等

```xml
<select id="selectStudentWhere" resultType="com.bjpowernode.domain.Student">
    select id,name, age, email from student
    <where>
        <if test="name !=null and name !='' ">
            name = #{name}
        </if>
        <if test="age > 0">
            or age > #{age}
        </if>
    </where>
</select>
```

1. < foreach> 循环 java 中的数组，list 集合的

主要用在 sql 的 in 语句中（select * from student where id in (1001,1002,1003)）

```
collection:表示接口中的方法参数的类型， 如果是数组使用array , 如果是list集合使用list
item:自定义的，表示数组和集合成员的变量
open:循环开始是的字符
close:循环结束时的字符
separator:集合成员之间的分隔符
```

```xml
<select id="selectForeachOne" resultType="com.bjpowernode.domain.Student">
    select * from student where id in
    <foreach collection="list" item="myid" open="(" close=")" separator=",">
              #{myid}
    </foreach>
</select>
```

1. sql 代码片段

就是复用一些语法

```xml
<!--定义sql片段-->
<!-- <sql id="自定义名称唯一">  sql语句， 表名，字段等 </sql> -->
<sql id="studentSql">
    select id,name, age, email from student
</sql>

<sql id="studentSqlOne">
     id,name, age, email
</sql> 

<!--使用-->
<!-- <include refid="id的值" /> -->
select <include refid="studentSqlOne" /> from student

<include refid="studentSql" />
```

## 配置文件（了解）

数据库的属性配置文件：

把数据库连接信息放到一个单独的文件中。 和 mybatis 主配置文件分开。

目的是便于修改，保存，处理多个数据库的信息。

步骤

1. 在 resources 目录中定义一个属性配置文件， xxxx.properties ,例如 jdbc.properties

在属性配置文件中， 定义数据，格式是 key=value```
key中一般使用 . 做多级目录的。例如 jdbc.mysql.driver , jdbc.driver , mydriver

jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql//.....
jdbc.username=root
jdbc.password=123456
```


2. 在 mybatis 的主配置文件，使用  指定文件的位置，使用文件中的数据： ${key}```xml
<!--指定properties文件的位置，从类路径根开始找文件-->
<properties resource="jdbc.properties" />

<property name="driver" value="${jdbc.driver}"/>
<property name="url" value="${jdbc.url}"/>
<property name="username" value="${jdbc.user}"/>
<property name="password" value="${jdbc.passwd}"/>
```



多个 mapper 文件，使用 package 指定路径

```xml
<mappers>
    <!--第一种方式：指定多个mapper文件-->
    <mapper resource="com/bjpowernode/dao/StudentDao.xml"/>
    <mapper resource="com/bjpowernode/dao/OrderDao.xml" />
</mappers>

<mappers>
    <!--第二种方式： 使用包名
        name: xml文件（mapper文件）所在的包名, 这个包中所有xml文件一次都能加载给mybatis
        使用package的要求：
         1. mapper文件名称需要和接口名称一样， 区分大小写的一样
         2. mapper文件和dao接口需要在同一目录
    -->
    <package name="com.bjpowernode.dao"/>
</mappers>
```

## 扩展

PageHelper，做数据分页的

1. maven 坐标```xml
<!--PageHelper依赖-->
<dependency>
  <groupId>com.github.pagehelper</groupId>
  <artifactId>pagehelper</artifactId>
  <version>5.1.10</version>
</dependency>
```


2. 配置插件```xml
<plugins>
    <plugin interceptor="com.github.pagehelper.PageInterceptor" />
</plugins>
加在环境前面
<environments default="mydev">
</environments>
```


3. 使用 PageHelper 对象```java
public void testSelectAllPageHelper(){
    SqlSession sqlSession = MyBatisUtils.getSqlSession();
    StudentDao dao  =  sqlSession.getMapper(StudentDao.class);
    // 加入PageHelper的方法，PageHelper.startPage(pageNum,pageSize)
    // pageNum: 第几页，从1开始，  pageSize: 一页中有多少行数据
    PageHelper.startPage(1,3);
    List<Student> students = dao.selectAll();
    for(Student stu:students){
        System.out.println("foreach--one ==="+stu);
    }
}
```


