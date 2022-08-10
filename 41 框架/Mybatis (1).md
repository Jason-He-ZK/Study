MyBatis
	描述：数据访问框架，操作数据库进行增删改查等操作
	增删改查
	全局配置
	动态 SQL
	缓存
	和其他框架的整合
	逆向工程

### MyBatis Plus
#### 基本步骤
添加依赖
```xml
<dependency>
  <groupId>com.baomidou</groupId>
  <artifactId>mybatis-plus-boot-starter</artifactId>
  <version>3.0.5</version>
</dependency>
```
数据库配置
```
spring:
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://127.0.0.1:3306/springdb?useUnicode=true&characterEncoding=utf-8
    username: root
    password: 123456
```
实体类
```java
// 类名和表名不同时，指定对应关系，asdf_ffff为表名
@TableName(value = "asdf_ffff")
public class User {
    //定义属性： 属性名和表中的列名一样
    /**
     * 指定主键的方式：
     * value:主键字段的名称，如果是id，可以不用写
     * type:指定主键的类型
     * 
     * idType.AUTO 表示自动增长。
     * idType.id_worker，实体类用 Long id ， 表的列用 bigint ，int 类型大小不够
     * idType.id_worker_str 实体类使用 String id, 表的列使用 varchar 50
     */
    @TableId(value = "id", type = IdType.AUTO)
    private Integer id;
    
    // 属性名和字段名不同，指定对应关系
    @TableField(value = "ming_zi")
    private String name;
}
```
Mapper接口类
```java
public interface UserMapper extends BaseMapper<User> {
}
```
启动类加注解，指定Mapper文件位置
```java
@MapperScan(value = "com.wkcto.order")
```

配置日志
```
mybatis-plus:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
```


#### 通用方法和自定义方法
##### insert
插入后，用user.getId()能获取主键值
##### update
user类中，为null的值不更新（int的初始化值为0，会更新，Integer不会）
##### delete
deleteByMap，根据多个字段的条件删除
##### 自定义方法
```java
public interface StudentMapper extends BaseMapper<Student> {
    //自定义方法
    public int insertStudent(Student student);
}

```
指定xml文件位置，xml 文件夹下的 *Mapper.xml 文件
```
mybatis-plus:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
  mapper-locations: classpath*:xml/*Mapper.xml
```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd"> 
<mapper namespace="com.wkcto.plus.mapper.StudentMapper">
    <insert id="insertStudent">
        insert into student(name,age,email,status) values(#{name},#{age},#{email},#{status})
    </insert>
</mapper>
```
#### ActiveRecord（AR）
直接用实体类调用insert等方法
只要实体类继承Model接口即可（Mapper类还是需要有）
#### 查询构造器（条件构造器）
![image.png](https://cdn.nlark.com/yuque/0/2021/png/12407496/1634478415088-6ddb9d09-bd17-4e73-895b-2c505fe05cf1.png#clientId=u1623c14d-a246-4&from=paste&height=215&id=uf77829ec&margin=%5Bobject%20Object%5D&name=image.png&originHeight=429&originWidth=735&originalType=binary&ratio=1&size=52237&status=done&style=none&taskId=u71924fa7-c14c-43ff-a406-ee4bb7dc6ad&width=367.5)
```java
QueryWrapper<Student> qw = new QueryWrapper<>();
// 组成条件，比如eq
qw.eq("name", "李四");
// 进行查询
List<Student> students = studentDao.selectList(qw);
```
![image.png](https://cdn.nlark.com/yuque/0/2021/png/12407496/1634478316031-e19f29dd-5e27-46a2-8821-2aca63c55e58.png#clientId=u1623c14d-a246-4&from=paste&height=266&id=u7a906030&margin=%5Bobject%20Object%5D&name=image.png&originHeight=532&originWidth=718&originalType=binary&ratio=1&size=43245&status=done&style=none&taskId=u9ef271d3-71ed-4717-8477-fe1823df55f&width=359)
![image.png](https://cdn.nlark.com/yuque/0/2021/png/12407496/1634478326513-625fc68a-fcf0-4f79-a389-ab67cdce87ef.png#clientId=u1623c14d-a246-4&from=paste&height=548&id=ub6afbb49&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1096&originWidth=718&originalType=binary&ratio=1&size=127508&status=done&style=none&taskId=uf650b8fb-30ee-47a9-a34c-ff963eea405&width=359)
![image.png](https://cdn.nlark.com/yuque/0/2021/png/12407496/1634478352421-a416968b-69b4-4845-b6c3-3d0df7ac028b.png#clientId=u1623c14d-a246-4&from=paste&height=134&id=u349de082&margin=%5Bobject%20Object%5D&name=image.png&originHeight=268&originWidth=722&originalType=binary&ratio=1&size=40118&status=done&style=none&taskId=u279e43ac-4f46-45f1-b896-3123617abc3&width=361)
#### 分页
通过拼接 limit 语句实现
```java
@Configuration
public class Config {
    /***
     * 定义方法，返回的返回值是java 对象，这个对象是放入到spring容器中
     * 使用@Bean修饰方法
     */
    @Bean
    public PaginationInterceptor paginationInterceptor(){
        return new PaginationInterceptor();
    }
}
```
```java
IPage<Student> page  = new Page<>();
//设置分页的数据
page.setCurrent(1);//第一页
page.setSize(3);// 每页的记录数

IPage<Student> result = studentDao.selectPage(page,qw); 

System.out.println("总页数："+result.getPages());
System.out.println("总记录数："+result.getTotal());
System.out.println("当前页码："+result.getCurrent());
System.out.println("每页的记录数："+result.getSize());
```
#### 代码生成器
模板引擎依赖，二选一，用最新版本
```java
<dependency>
    <groupId>org.apache.velocity</groupId>
    <artifactId>velocity-engine-core</artifactId>
    <version>2.0</version>
</dependency>

<dependency>
    <groupId>org.freemarker</groupId>
    <artifactId>freemarker</artifactId>
    <version>2.0</version>
</dependency>
```
```java
public class AutoMapper {
    public static void main(String[] args) {
        //创建AutoGenerator ,MP中对象
        AutoGenerator ag = new AutoGenerator();

        //设置全局配置
        GlobalConfig gc  = new GlobalConfig();
        //设置代码的生成位置， 磁盘的目录
        String path = System.getProperty("user.dir");
        gc.setOutputDir(path+"/src/main/java");
        //设置生成的类的名称（命名规则）
        gc.setMapperName("%sMapper");//所有的Dao类都是Mapper结尾的，例如DeptMapper
        //设置Service接口的命名
        gc.setServiceName("%sService");//DeptService
        //设置Service实现类的名称
        gc.setServiceImplName("%sServiceImpl");//DeptServiceImpl
        //设置Controller类的命名
        gc.setControllerName("%sController");//DeptController
        //设置作者
        gc.setAuthor("changming");
        //设置主键id的配置
        gc.setIdType(IdType.ID_WORKER);
        ag.setGlobalConfig(gc);

        //设置数据源DataSource
        DataSourceConfig ds  = new DataSourceConfig();
        //驱动
        ds.setDriverName("com.mysql.jdbc.Driver");
        //设置url
        ds.setUrl("jdbc:mysql://localhost:3306/springdb");
        //设置数据库的用户名
        ds.setUsername("root");
        //设置密码
        ds.setPassword("123456");
        //把DataSourceConfig赋值给AutoGenerator
        ag.setDataSource(ds);

        //设置Package信息
        PackageConfig pc  = new PackageConfig();
        //设置模块名称， 相当于包名， 在这个包的下面有 mapper， service， controller。
        pc.setModuleName("order");
        //设置父包名，order就在父包的下面生成
        pc.setParent("com.wkcto"); //com.wkcto.order
        ag.setPackageInfo(pc);

        //设置策略
        StrategyConfig sc  = new StrategyConfig();
        sc.setNaming(NamingStrategy.underline_to_camel);
        //设置支持驼峰的命名规则
        sc.setColumnNaming(NamingStrategy.underline_to_camel);
        ag.setStrategy(sc);

        //执行代码的生成
        ag.execute();
    }
}
```

插件扩展
自定义全局操作
