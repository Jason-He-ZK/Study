## 概述

解决企业开发的难度，减轻对项目模块之间的管理，类和类之间的管理
帮助开发人员创建对象，管理对象之间的关系。

核心技术： ioc，aop。（实现模块之间，类之间的解耦合）

优点：轻量，针对接口编程解耦合，AOP 编程的支持，方便集成各种优秀框架

框架： 框架是一个软件，其它人写好的软件。

- 1）知道框架功能， mybatis--访问数据库， 对表中的数据执行增删改查。
- 2）框架的语法， 框架要完成一个功能，需要一定的步骤支持的，
- 3）框架的内部实现， 框架内部怎么做。 原理是什么。
- 4）通过学习，可以实现一个框架。

## IoC 控制反转（Inversion of Control）

### 概述

是一个理论，概念，思想： 把对象的创建，赋值，管理工作都交给代码之外的容器实现

```
控制：创建对象，对象的属性赋值，对象之间的关系管理。
反转：把原来的开发人员管理，创建对象的权限转移给代码之外的容器实现。 由容器代替开发人员。
正转：由开发人员在代码中，使用new 构造方法创建对象， 开发人员主动管理对象。
容器：是一个服务器软件， 一个框架（spring）
```

目的：减少对代码的改动， 也能实现不同的功能。 实现解耦合。

java 中创建对象的方式：构造方法，反射，序列化，克隆，ioc，动态代理

ioc 思想的体现：servlet

```
1. 创建类继承HttpServelt 
2. 在web.xml 注册servlet
	<servlet-name> myservlet </servlet-name>
	<servelt-class>com.bjpwernode.controller.MyServlet1</servlet-class>
3. 没有创建 Servlet对象， 没有 MyServlet myservlet = new MyServlet()
4. Servlet 是Tomcat服务器它能你创建的。 Tomcat也称为容器
  Tomcat作为容器：里面存放的有Servlet对象，（还有Listener ， Filter等对象）
```

### 第一个程序

```
ch01-hello-spring： 使用的ioc， 由spring创建对象

实现步骤：
1.创建maven项目
2.加入maven的依赖
  spring的依赖，版本5.2.5版本
  junit依赖
3.创建类（接口和他的实现类）
  和没有使用框架一样， 就是普通的类。
4.创建spring需要使用的配置文件
  声明类的信息，这些类由spring创建和管理
  位置resource，选择xml--Spring config
  一般叫：applicationContext.xml
5.测试spring创建的。
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--spring的配置文件
        1.beans : 是根标签，spring把java对象成为bean。
        2.spring-beans.xsd 是约束文件，和mybatis指定  dtd是一样的。
    -->
  
    <!--如何告诉spring创建对象： 声明bean，就是告诉spring要创建某个类的对象
  
        id:对象的自定义名称，唯一值。 spring通过这个名称找到对象
        class:类的全限定名称（不能是接口，因为spring是反射机制创建对象，必须使用类）

        一个bean标签声明一个对象。
    -->
    <bean id="someService" class="com.bjpowernode.service.impl.SomeServiceImpl" />

    <bean id="someService1" class="com.bjpowernode.service.impl.SomeServiceImpl" scope="prototype"/>

    <!--spring能创建一个非自定义类的对象吗， 创建一个存在的某个类的对象。-->
    <bean id="mydate" class="java.util.Date" />

</beans>
```

```java
public void test02(){
    //使用spring容器创建的对象
    //1.指定spring配置文件的名称
    String config="beans.xml";
    //2.创建 表示spring容器的对象，ApplicationContext，通过其获取对象
    // ClassPathXmlApplicationContext:表示从类路径中加载spring的配置文件，此时对象就已经创建了（无参构造）
    ApplicationContext ac = new ClassPathXmlApplicationContext(config);
    //从容器中获取某个对象，你要调用对象的方法， getBean("配置文件中的bean的id值")
    SomeService service = (SomeService) ac.getBean("someService");
    //使用spring创建好的对象
    service.doSome();
   
    //使用spring提供的方法， 获取容器中定义的对象的数量
    int nums  = ac.getBeanDefinitionCount();
    System.out.println("容器中定义的对象数量："+nums);
  
    //容器中每个定义的对象的名称
    String names [] = ac.getBeanDefinitionNames();
    for(String name:names){
        System.out.println(name);
    }
}
```

### 基于 xml 的 DI

经常修改用 xml

在 spring 的配置文件中， 使用标签和属性完成

#### set 注入（设置注入）

spring 调用类的 set 方法，在 set 方法可以实现属性的赋值（占比 80% 左右）

只是调用 set 方法，有没有对应属性无所谓

```xml
简单类型的set注入
<bean id="myStudent" class="com.bjpowernode.ba01.Student" >
    <property name="name" value="李四lisi" /><!--setName("李四")-->
    <property name="age" value="22" /><!--setAge(21)-->
    <property name="email" value="lisi@qq.com" /><!--setEmail("lisi@qq.com")-->
</bean>

引用类型的set注入
<bean id="myStudent" class="com.bjpowernode.ba02.Student" >
    <property name="name" value="李四" />
    <property name="age" value="26" />
    <!--引用类型-->
    <property name="school" ref="mySchool" /><!--setSchool(mySchool)-->
</bean>
<!--声明School对象-->
<bean id="mySchool" class="com.bjpowernode.ba02.School">
    <property name="name" value="北京大学"/>
    <property name="address" value="北京的海淀区" />
</bean>
```

#### 构造注入

spring 调用类的有参数构造方法，创建对象。在构造方法中完成赋值。

```xml
<!--1 使用name属性实现构造注入-->
<bean id="myStudent" class="com.bjpowernode.ba03.Student" >
    <constructor-arg name="myage" value="20" />
    <constructor-arg name="mySchool" ref="myXueXiao" />
    <constructor-arg name="myname" value="周良"/>
</bean>
<!--2 使用index属性-->
<bean id="myStudent2" class="com.bjpowernode.ba03.Student">
    <constructor-arg index="1" value="22" />
    <constructor-arg index="0" value="李四" />
    <constructor-arg index="2" ref="myXueXiao" />
</bean>
<!--3 省略index（按顺序）-->
<bean id="myStudent3" class="com.bjpowernode.ba03.Student">
    <constructor-arg  value="张强强" />
    <constructor-arg  value="22" />
    <constructor-arg  ref="myXueXiao" />
</bean>
<!--声明School对象-->
<bean id="myXueXiao" class="com.bjpowernode.ba03.School">
    <property name="name" value="清华大学"/>
    <property name="address" value="北京的海淀区" />
</bean>

<!--创建File,使用构造注入-->
<bean id="myfile" class="java.io.File">
    <constructor-arg name="parent" value="D:\course\JavaProjects\spring-course\ch01-hello-spring" />
    <constructor-arg name="child" value="readme.txt" />
</bean>
```

#### 引用类型的自动注入

spring 框架根据某些规则可以给引用类型赋值。不用你再给引用类型赋值了，使用的规则常用的是 byName, byType.

byName(按名称注入)

java 类中引用类型的属性名和 spring 容器中（配置文件） 的 id 名称一样，且数据类型是一致的，这样的容器中的 bean，spring 能够赋值给引用类型。

```xml
<bean id="xx" class="yyy" autowire="byName">
     简单类型属性赋值
</bean>

<!--byName-->
<bean id="myStudent" class="com.bjpowernode.ba04.Student"  autowire="byName">
    <property name="name" value="李四" />
    <property name="age" value="26" />
    <!--引用类型-->
    <!--<property name="school" ref="mySchool" />-->
</bean>
<!--声明School对象-->
<bean id="school" class="com.bjpowernode.ba04.School">
    <property name="name" value="清华大学"/>
    <property name="address" value="北京的海淀区" />
</bean>
```

byType（按类型注入）

java 类中引用类型的数据类型和 spring 容器中（配置文件） 的 class 属性是同源关系的，这样的 bean 能够赋值给引用类型

同源就是一类的意思

```
1.java类中引用类型的数据类型和bean的class的值是一样的。
2.java类中引用类型的数据类型（父）和bean的class的值（子）是父子类关系的。
3.java类中引用类型的数据类型和bean的class的值接口和实现类关系的
```

注意：在 byType 中，在 xml 配置文件中声明 bean 只能有一个符合条件的，多余一个是错误的

```xml
<bean id="xx" class="yyy" autowire="byType">
     简单类型属性赋值
</bean>

<!--byType-->
<bean id="myStudent" class="com.bjpowernode.ba05.Student"  autowire="byType">
    <property name="name" value="张飒" />
    <property name="age" value="26" />
    <!--引用类型-->
    <!--<property name="school" ref="mySchool" />-->
</bean>
<!--声明School对象-->
<bean id="mySchool" class="com.bjpowernode.ba05.School">
    <property name="name" value="人民大学"/>
    <property name="address" value="北京的海淀区" />
</bean>
<!--声明School的子类-->
<!--<bean id="primarySchool" class="com.bjpowernode.ba05.PrimarySchool">
    <property name="name" value="北京小学" />
    <property name="address" value="北京的大兴区" />
</bean>-->
```

```xml
1.byName(按名称注入) ：
java类中引用类型的属性名和spring容器中（配置文件）<bean>的id名称一样，且数据类型是一致的，这样的容器中的bean，spring能够赋值给引用类型。
  语法：
  <bean id="xx" class="yyy" autowire="byName">
     简单类型属性赋值
  </bean>
2. ： 
java类中引用类型的数据类型和spring容器中（配置文件）<bean>的class属性是同源（同源就是一类的意思）关系的，这样的bean能够赋值给引用类型
   1.java类中引用类型的数据类型和bean的class的值是一样的。
   2.java类中引用类型的数据类型和bean的class的值父子类关系的。
   3.java类中引用类型的数据类型和bean的class的值接口和实现类关系的
  语法：
  <bean id="xx" class="yyy" autowire="byType">
     简单类型属性赋值
  </bean>
```

多配置文件

- 优势 
   - 每个文件的大小比一个文件要小很多。效率高
   - 避免多人竞争带来的冲突。
- 分配方式 
   - 按功能模块，一个模块一个配置文件
   - 按类的功能（数据库相关的配置一个文件配置文件， 做事务的功能一个配置文件， 做 service 功能的一个配置文件等）

### 基于注解的 DI

不常修改用注解

基于注解的 di： 通过注解完成 java 对象创建，属性赋值。

使用注解的步骤：

1. 加入 maven 的依赖 spring-context ，在你加入 spring-context 的同时， 间接加入 spring-aop 的依赖（使用注解必须使用 spring-aop 依赖）
2. 创建类，在类上方加入 spring 的注解（多个不同功能的注解）
3. 创建 spring 的配置文件，在文件中加入一个组件扫描器的标签，说明注解在你的项目中的位置 
```xml
<!--声明组件扫描器(component-scan),组件就是java对象
    base-package：指定注解在你的项目中的包名。
    component-scan工作方式： spring会扫描遍历base-package指定的包，
       把包中和子包中的所有类，找到类中的注解，按照注解的功能创建对象，或给属性赋值。
   加入了component-scan标签，配置文件的变化：
    1.加入一个新的约束文件spring-context.xsd
    2.给这个新的约束文件起个命名空间的名称
-->

<!--指定多个包的三种方式-->
<context:component-scan base-package="com.bjpowernode.ba01" />
<context:component-scan base-package="com.bjpowernode.ba02" />

<context:component-scan base-package="com.bjpowernode.ba03，com.bjpowernode.ba04" />
<context:component-scan base-package="com.bjpowernode.ba03；com.bjpowernode.ba04" />

<context:component-scan base-package="com.bjpowernode" />
<!--
 <bean id="myXueXiao" class="com.bjpowernode.ba03.School">
    <property name="name" value="清华大学" />
    <property name="address" value="北京" />
</bean>
-->
<!--加载属性配置文件-->
<context:property-placeholder location="classpath:test.properties" />
```

4. 使用注解创建对象， 创建容器 ApplicationContext

学习的注解

```
@Component（234用法同1）
@Respotory
@Service
@Controller
@Value
@Autowired
@Resource（来自JDK而不是Spring）
```

```java
/**
 * @Component: 创建对象的， 等同于<bean>的功能
 *     属性：value 就是对象的名称，也就是bean的id值，
 *          value的值是唯一的，创建的对象在整个spring容器中就一个
 *     位置：在类的上面
 *
 *  @Component(value = "myStudent")等同于
 *   <bean id="myStudent" class="com.bjpowernode.ba01.Student" />
 *
 *  spring中和@Component功能一致，创建对象的注解还有：
 *  1.@Repository（用在持久层类的上面） : 放在dao的实现类上面，表示创建dao对象，dao对象是能访问数据库的。
 *  2.@Service(用在业务层类的上面)：放在service的实现类上面，创建service对象，service对象是做业务处理，可以有事务等功能的。
 *  3.@Controller(用在控制器的上面)：放在控制器（处理器）类的上面，创建控制器对象的，控制器对象，能够接受用户提交的参数，显示请求的处理结果。
 *  以上三个注解的使用语法和@Component一样的。 都能创建对象，但是这三个注解还有额外的功能。
 *  @Repository，@Service，@Controller是给项目的对象分层的。
 *  其他类用@Component
 */
 
// 1 使用value属性，指定对象名称
//@Component(value = "myStudent")
// 2 省略value（最常用）
@Component("myStudent")
// 3 不指定对象名称，由spring提供默认名称: 类名的首字母小写
//@Component
public class Student {
}
```

```java
/**
 * @Value: 简单类型的属性赋值
 *   属性： value 是String类型的，表示简单类型的属性值
 *   位置： 1.在属性定义的上面，无需set方法，推荐使用。
 *         2.在set方法的上面（很少有）
 */
//@Value("李四" )
@Value("${myname}") //使用属性配置文件中的数据
private String name;

//@Value("30")
public void setAge(Integer age) {
    System.out.println("setAge:"+age);
    this.age = age;
}
```

```java
/**
 * 引用类型
 * @Autowired: spring框架提供的注解，实现引用类型的赋值。
 * spring中通过注解给引用类型赋值，使用的是自动注入原理 ，支持byName, byType
 * @Autowired:默认使用的是byType自动注入。
 *
 *  位置：1）在属性定义的上面，无需set方法， 推荐使用
 *       2）在set方法的上面
 
 *  属性：required ，是一个boolean类型的，默认true，建议true
      required=true：表示引用类型赋值失败，程序报错，并终止执行。
      required=false：引用类型如果赋值失败， 程序正常执行，引用类型是null
      @Autowired(required = false)
 *
 *  如果要使用byName方式，需要做的是：
 *  1.在属性上面加入@Autowired
 *  2.在属性上面加入@Qualifier(value="bean的id") ：表示使用指定名称的bean完成赋值。
 */
@Autowired
private School school;

@Autowired
@Qualifier("mySchool")
private School school; 



/**
 * 引用类型
 * @Resource: 来自jdk中的注解，spring框架提供了对这个注解的功能支持，可以使用它给引用类型赋值
 *            使用的也是自动注入原理，支持byName， byType .默认是byName
 *  位置： 1.在属性定义的上面，无需set方法，推荐使用。
 *        2.在set方法的上面
 
    默认是byName： 先使用byName自动注入，如果byName赋值失败，再使用byType
  
    @Resource只使用byName方式，需要增加一个属性 name
 *  name的值是bean的id（名称）
 */

@Resource
private School school; 

@Resource(name = "mySchool")
private School school;
```

注解中用 $

```java
@value("${配置文件的key}")

<!--加载属性配置文件-->
<context:property-placeholder location="classpath:test.properties" />
```

## AOP 面向切面编程

### 概述

Aop：面向切面编程， 基于动态代理的，可以使用 jdk，cglib 两种代理方式。

Aop 就是动态代理的规范化， 把动态代理的实现步骤，方式都定义好了， 让开发人员用一种统一的方式，使用动态代理。

AOP（Aspect Orient Programming）面向切面编程

```
Aspect: 切面，给你的目标类增加的功能，就是切面。 像上面用的日志，事务都是切面。
  切面的特点： 一般都是非业务方法，独立使用的。
Orient：面向， 对着。
Programming：编程
```

怎么理解面向切面编程 （面试）

- 需要在分析项目功能时，找出切面。
- 合理的安排切面的执行时机（在目标方法前， 还是目标方法后）
- 合理的安全切面执行的位置（在哪个类，哪个方法增加增强功能）

术语

```
1）Aspect:切面，表示增强的功能， 就是一堆代码，完成某个一个功能。非业务功能，
  常见的切面功能有日志， 事务， 统计信息， 参数检查， 权限验证。
2）JoinPoint:连接点 ，连接业务方法和切面的位置。 就某类中的业务方法
3）Pointcut : 切入点 ，指多个连接点方法的集合。多个方法
4）目标对象： 给哪个类的方法增加功能， 这个类就是目标对
5）Advice:通知，通知表示切面功能执行的时间。
```

切面有三个关键的要素

- 切面的功能代码，切面干什么
- 切面的执行位置，使用 Pointcut 表示切面执行的位置
- 切面的执行时间，使用 Advice 表示时间，在目标方法之前，还是目标方法之后。

### aop 的实现

aop 是一个规范，是动态的一个规范化，一个标准

什么时候考虑用 aop：1 增加功能，2 给多个类增加相同的功能，3 增加事务，日志功能

aop 的技术实现框架：

```
1.spring：spring在内部实现了aop规范，能做aop的工作。
  spring主要在事务处理时使用aop。
  我们项目开发中很少使用spring的aop实现。 因为spring的aop比较笨重。

2.aspectJ: 一个开源的专门做aop的框架。spring框架中集成了aspectj框架，通过spring就能使用aspectj的功能。
  aspectJ框架实现aop有两种方式：
    1.使用xml的配置文件 ： 配置全局事务
    2.使用注解，aspectj有5个注解（我们在项目中要做aop功能，一般都使用注解）
```

学习 aspectj 框架

1）切面的执行时间， 这个执行时间在规范中叫做 Advice(通知，增强)

在 aspectj 框架中使用注解表示的。也可以使用 xml 配置文件中的标签

```
1）@Before（掌握）
2）@AfterReturning（掌握）
3）@Around（掌握）
4）@AfterThrowing（了解）
5）@After（了解）
@pointcut（定义切入点）
```

2）表示切面执行的位置，使用的是切入点表达式。

```
AspectJ定义了专门的表达式用于指定切入点。表达式的原型是：
execution(modifiers-pattern? ret-type-pattern declaring-type-pattern?name-pattern(param-pattern) throws-pattern?)

modifiers-pattern  访问权限类型
ret-type-pattern  返回值类型
declaring-type-pattern  包名 类名
name-pattern(param pattern)  方法名 参数 类型和参数个数
throws-pattern  抛出异常类型
？表示可选的部分 
返回值和声明是必须的

以上表达式共4 个部分。
execution（访问权限 方法返回值 方法声明(参数) 异常类型）
```

```
切入点表达式要匹配的对象就是目标方法的方法名。所以execution 表达式中明显就是方法的签名。

*   0至多个任意字符

..  用在方法参数中，表示任意多个参数
    用在包名后，表示当前包及其子包路径
(+了解即可）
+   用在类名后，表示当前类及其子类
    用在接口后，表示当前接口及其实现类
```

![image.png](https://cdn.nlark.com/yuque/0/2021/png/12407496/1636038829703-af1e7812-0e18-41a1-9995-a58738fc0dfa.png#clientId=ub5da5342-5cab-4&from=paste&height=215&id=u1df0cb28&margin=%5Bobject%20Object%5D&name=image.png&originHeight=429&originWidth=959&originalType=binary&ratio=1&size=70717&status=done&style=none&taskId=u9d88a2cf-c0c2-4854-bf6f-fa2889c0548&width=479.5)

### 用 aspectj 框架实现 aop

1.  新建 maven 项目 
2.  加入依赖：spring 依赖，aspectj 依赖，junit 单元测试 
3.  创建目标类：接口和他的实现类（要做的是给类中的方法增加功能） 
4.  ***创建切面类：普通类
在类的上面加入 [@Aspect ](/Aspect ) 
在类中定义方法， 方法就是切面要执行的功能代码
在方法的上面加入 aspectj 中的通知注解，例如 [@Before ](/Before ) 
还需要指定切入点表达式 execution() 
5.  创建 spring 的配置文件：声明对象，把对象交给容器统一管理
声明对象你可以使用注解或者 xml 配置文件
声明目标对象
声明切面类对象
声明 aspectj 框架中的自动代理生成器标签。 
6.  创建测试类，从 spring 容器中获取目标对象（实际就是代理对象），通过代理执行方法，实现 aop 的功能增强      
```xml
<!--把对象交给spring容器，由spring容器统一创建，管理对象-->
<!--声明目标对象-->
<bean id="someService" class="com.bjpowernode.ba08.SomeServiceImpl" />
<!--声明切面类对象-->
<bean id="myAspect" class="com.bjpowernode.ba08.MyAspect" />

<!--声明自动代理生成器：使用aspectj框架内部的功能，创建目标对象的代理对象。
    创建代理对象是在内存中实现的（修改目标对象的内存中的结构，创建为代理对象）
    所以目标对象就是被修改后的代理对象.
    aspectj-autoproxy:会把spring容器中的所有的目标对象，一次性都生成代理对象。
-->
<aop:aspectj-autoproxy />

<!--
   如果你期望目标类有接口，使用cglib代理
   proxy-target-class="true":告诉框架，要使用cglib动态代理
-->
<aop:aspectj-autoproxy proxy-target-class="true"/>
```
```java
/**
 *  @Aspect : 是aspectj框架中的注解。
 *     作用：表示当前类是切面类。
 *     切面类：是用来给业务方法增加功能的类，在这个类中有切面的功能代码
 *     位置：在类定义的上面
 */
@Aspect
public class MyAspect {
    /**
     * 定义方法，方法是实现切面功能的。
     * 方法的定义要求：
     * 1.公共方法 public
     * 2.方法没有返回值
     * 3.方法名称自定义
     * 4.方法可以有参数，也可以没有参数。（如果有参数，参数不是自定义的，有几个参数类型可以使用）
     */

    /**
     * @Before: 前置通知注解
     *   属性：value ，是切入点表达式，表示切面的功能执行的位置。
     *   位置：在方法的上面
     * 特点：
     *  1.在目标方法之前先执行的
     *  2.不会改变目标方法的执行结果
     *  3.不会影响目标方法的执行。
     */
    @Before(value = "execution(public void com.bjpowernode.ba01.SomeServiceImpl.doSome(String,Integer))")
    // @Before(value = "execution(void com.bjpowernode.ba01.SomeServiceImpl.doSome(String,Integer))")
    // @Before(value = "execution(void *..SomeServiceImpl.doSome(String,Integer))")
    // @Before(value = "execution(* *..SomeServiceImpl.*(..))")
    // @Before(value = "execution(* do*(..))")
    // @Before(value = "execution(* com.bjpowernode.ba01.*ServiceImpl.*(..))")
    public void myBefore(){
        //就是你切面要执行的功能代码
        System.out.println("前置通知， 切面功能：在目标方法之前输出执行时间："+ new Date());
    }


    /**
     * 指定通知方法中的参数 ： JoinPoint（连接点）
     * JoinPoint:业务方法，要加入切面功能的业务方法
     *    作用是：可以在通知方法中获取方法执行时的信息， 例如方法名称，方法的实参。
     *    如果你的切面功能中需要用到方法的信息，就加入JoinPoint.
     *    这个JoinPoint参数的值是由框架赋予， 必须是第一个位置的参数
     */
    @Before(value = "execution(void *..SomeServiceImpl.doSome(String,Integer))")
    public void myBefore(JoinPoint jp){
        //获取方法的完整定义
        System.out.println("方法的签名（定义）="+jp.getSignature());
        System.out.println("方法的名称="+jp.getSignature().getName());
        //获取方法的实参
        Object args [] = jp.getArgs();
        for (Object arg:args){
            System.out.println("参数="+arg);
        }
        //就是你切面要执行的功能代码
        System.out.println("2=====前置通知， 切面功能：在目标方法之前输出执行时间："+ new Date());
    }
}
```
```java
public class MyAspect {
    /**
     * 后置通知定义方法，方法是实现切面功能的。
     * 方法的定义要求：
     * 1.公共方法 public
     * 2.方法没有返回值
     * 3.方法名称自定义
     * 4.方法有参数的,推荐是Object ，参数名自定义
     */

    /**
     * @AfterReturning:后置通知
     *    属性：1.value 切入点表达式
     *         2.returning 自定义的变量，表示目标方法的返回值的。
     *          自定义变量名必须和通知方法的形参名一样。
     *    位置：在方法定义的上面
     * 特点：
     *  1。在目标方法之后执行的。
     *  2. 能够获取到目标方法的返回值，可以根据这个返回值做不同的处理功能
     *      Object res = doOther();
     *  3. 可以修改这个返回值
     *
     *  后置通知的执行
     *    Object res = doOther();
     *    参数传递： 传值， 传引用
     *    myAfterReturing(res);
     *    System.out.println("res="+res)
     *
     */
    @AfterReturning(value = "execution(* *..SomeServiceImpl.doOther(..))", returning = "res")
    public void myAfterReturing(  JoinPoint jp  ,Object res ){
        // Object res:是目标方法执行后的返回值，根据返回值做你的切面的功能处理
        System.out.println("后置通知：方法的定义"+ jp.getSignature());
        System.out.println("后置通知：在目标方法之后执行的，获取的返回值是："+res);
        if(res.equals("abcd")){
            //做一些功能
        } else{
            //做其它功能
        }

        //修改目标方法的返回值， 看一下是否会影响 最后的方法调用结果
        if( res != null){
            res = "Hello Aspectj";
        }
    }


    //作业
    @AfterReturning(value = "execution(* *..SomeServiceImpl.doOther2(..))", returning = "res")
    public void myAfterReturing2(Object res){
        // Object res:是目标方法执行后的返回值，根据返回值做你的切面的功能处理
        System.out.println("后置通知：在目标方法之后执行的，获取的返回值是："+res);

        //修改目标方法的返回值， 看一下是否会影响 最后的方法调用结果
        //如果修改了res的内容，属性值等，是不是会影响最后的调用结果呢

    }
}
```
```java
public class MyAspect {
    /**
     * 环绕通知方法的定义格式
     *  1.public
     *  2.必须有一个返回值，推荐使用Object
     *  3.方法名称自定义
     *  4.方法有参数，固定的参数 ProceedingJoinPoint
     */

    /**
     * @Around: 环绕通知
     *    属性：value 切入点表达式
     *    位置：在方法的定义什么
     * 特点：
     *   1.它是功能最强的通知
     *   2.在目标方法的前和后都能增强功能。
     *   3.控制目标方法是否被调用执行
     *   4.修改原来的目标方法的执行结果。 影响最后的调用结果
     *
     *  环绕通知，等同于jdk动态代理的，InvocationHandler接口
     *
     *  参数：  ProceedingJoinPoint 就等同于 Method
     *         作用：执行目标方法的
     *  返回值： 就是目标方法的执行结果，可以被修改。
     *
     *  环绕通知： 经常做事务， 在目标方法之前开启事务，执行目标方法， 在目标方法之后提交事务
     */
    @Around(value = "execution(* *..SomeServiceImpl.doFirst(..))")
    public Object myAround(ProceedingJoinPoint pjp) throws Throwable {

        String name = "";
        //获取第一个参数值
        Object args [] = pjp.getArgs();
        if( args!= null && args.length > 1){
              Object arg=  args[0];
              name =(String)arg;
        }

        //实现环绕通知
        Object result = null;
        System.out.println("环绕通知：在目标方法之前，输出时间："+ new Date());
        //1.目标方法调用
        if( "zhangsan".equals(name)){
            //符合条件，调用目标方法
            result = pjp.proceed(); //method.invoke(); Object result = doFirst();

        }

        System.out.println("环绕通知：在目标方法之后，提交事务");
        //2.在目标方法的前或者后加入功能

        //修改目标方法的执行结果， 影响方法最后的调用结果
        if( result != null){
              result = "Hello AspectJ AOP";
        }

        //返回目标方法的执行结果
        return result;
    }
}
```
```java
public class MyAspect {
    @After(value = "mypt()")
    public  void  myAfter(){
        System.out.println("执行最终通知，总是会被执行的代码");
        //一般做资源清除工作的。
     }
    @Before(value = "mypt()")
    public  void  myBefore(){
        System.out.println("前置通知，在目标方法之前先执行的");
    }

    /**
     * @Pointcut: 定义和管理切入点， 如果你的项目中有多个切入点表达式是重复的，可以复用的。
     *            可以使用@Pointcut
     *    属性：value 切入点表达式
     *    位置：在自定义的方法上面
     * 特点：
     *   当使用@Pointcut定义在一个方法的上面 ，此时这个方法的名称就是切入点表达式的别名。
     *   其它的通知中，value属性就可以使用这个方法名称，代替切入点表达式了
     */
    @Pointcut(value = "execution(* *..SomeServiceImpl.doThird(..))" )
    private void mypt(){
        //无需代码，
    }

}
```

## Spring 集成 MyBatis

### 概述

把 mybatis 框架和 spring 集成在一起，像一个框架一样使用。

- 用的技术是：ioc
- 是因为 ioc 能创建对象。可以把 mybatis 框架中的对象交给 spring 统一创建， 开发人员从 spring 中获取对象。

mybatis 使用步骤

1. 定义 dao 接口 ，StudentDao
2. 定义 mapper 文件 StudentDao.xml
3. 定义 mybatis 的主配置文件 mybatis.xml
4. 创建 dao 的代理对象，调用方法

要使用 dao 对象，需要使用 getMapper()方法，怎么能使用 getMapper()方法，需要哪些条件

1. 获取 SqlSession 对象， 需要使用 SqlSessionFactory 的 openSession()方法。
2. 创建 SqlSessionFactory 对象。 通过读取 mybatis 的主配置文件，能创建 SqlSessionFactory 对象

需要 SqlSessionFactory 对象， 使用 Factory 能获取 SqlSession ，有了 SqlSession 就能有 dao ， 目的就是获取 dao 对象
Factory 创建需要读取主配置文件

我们会使用独立的连接池类替换 mybatis 默认自己带的， 把连接池类也交给 spring 创建。

通过以上的说明，我们需要让 spring 创建以下对象

1. 独立的连接池类的对象， 使用阿里的 druid 连接池
2. SqlSessionFactory 对象
3. 创建出 dao 对象

需要学习就是上面三个对象的创建语法，使用 xml 的 bean 标签。

### 实现步骤

```xml
<configuration>
    <!--settings：控制mybatis全局行为-->
    <settings>
        <!--设置mybatis输出日志-->
        <setting name="logImpl" value="STDOUT_LOGGING"/>
    </settings>

    <!--设置别名-->
    <typeAliases>
        <!--name:实体类所在的包名
            表示com.bjpowernode.domain包中的列名就是别名
            你可以使用Student表示com.bjpowenrode.domain.Student
        -->
        <package name="com.bjpowernode.domain"/>
    </typeAliases>

    <!--sql mapper(sql映射文件)的位置-->
    <mappers>
        <!--name：是包名， 这个包中的所有mapper.xml一次都能加载-->
        <package name="com.bjpowernode.dao"/>
    </mappers>
</configuration>
```

```
1. 创建Service接口和实现类，属性是dao。

2. 创建spring的配置文件：声明mybatis的对象交给spring创建

  1）数据源DataSource

  2）SqlSessionFactory

  3) Dao对象

  4）声明自定义的service
```

```xml
<!--
   把数据库的配置信息，写在一个独立的文件，编译修改数据库的配置内容
   spring知道jdbc.properties文件的位置
-->
<context:property-placeholder location="classpath:jdbc.properties" />
<!--声明数据源DataSource, 作用是连接数据库的-->
<bean id="myDataSource" class="com.alibaba.druid.pool.DruidDataSource"
      init-method="init" destroy-method="close">
    <!--set注入给DruidDataSource提供连接数据库信息 -->
    <!--    使用属性配置文件中的数据，语法 ${key} -->
    <property name="url" value="${jdbc.url}" /><!--setUrl()-->
    <property name="username" value="${jdbc.username}"/>
    <property name="password" value="${jdbc.passwd}" />
    <property name="maxActive" value="${jdbc.max}" />
</bean>

<!--声明的是mybatis中提供的SqlSessionFactoryBean类，这个类内部创建SqlSessionFactory的
    SqlSessionFactory  sqlSessionFactory = new ..
-->
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
    <!--set注入，把数据库连接池付给了dataSource属性-->
    <property name="dataSource" ref="myDataSource" />
    <!--mybatis主配置文件的位置
       configLocation属性是Resource类型，读取配置文件
       它的赋值，使用value，指定文件的路径，使用classpath:表示文件的位置
    -->
    <property name="configLocation" value="classpath:mybatis.xml" />
</bean>

<!--创建dao对象，使用SqlSession的getMapper（StudentDao.class）
    MapperScannerConfigurer:在内部调用getMapper()生成每个dao接口的代理对象。
-->
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
    <!--指定SqlSessionFactory对象的id-->
    <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory" />
    <!--指定包名， 包名是dao接口所在的包名。
        MapperScannerConfigurer会扫描这个包中的所有接口，把每个接口都执行
        一次getMapper()方法，得到每个接口的dao对象。
        创建好的dao对象放入到spring的容器中的。 dao对象的默认名称是 接口名首字母小写
    -->
    <property name="basePackage" value="com.bjpowernode.dao"/>
</bean>

<!--声明service-->
<bean id="studentService" class="com.bjpowernode.service.impl.StudentServiceImpl">
    <property name="studentDao" ref="studentDao" />
</bean>
```

```
1. 获取Service对象，通过service调用dao完成数据库的访问
```

```java
@Test
public void testServiceInsert(){
    String config="applicationContext.xml";
    ApplicationContext ctx = new ClassPathXmlApplicationContext(config);
    //获取spring容器中的dao对象
    StudentService service = (StudentService) ctx.getBean("studentService");
    Student student  = new Student();
    student.setId(1015);
    student.setName("李胜利");
    student.setEmail("zhoufeng@qq.com");
    student.setAge(26);
    int nums = service.addStudent(student);
    //spring和mybatis整合在一起使用，事务是自动提交的。 无需执行SqlSession.commit();
    System.out.println("nums="+nums);
}
@Test
public void testServiceSelect(){
    String config="applicationContext.xml";
    ApplicationContext ctx = new ClassPathXmlApplicationContext(config);
    //获取spring容器中的dao对象
    StudentService service = (StudentService) ctx.getBean("studentService");
    List<Student> students = service.queryStudents();
    for (Student stu:students){
        System.out.println(stu);
    }
}
```

## Spring 事务

### 概述

```
什么是事务

  事务是指一组sql语句的集合， 集合中有多条sql语句

   我们希望这些多个sql语句都能成功，或者都失败， 这些sql语句的执行是一致的，作为一个整体执行。

在什么时候想到使用事务

  当操作涉及到多个表，或者是多个sql语句的insert，update，delete。需要保证这些语句都是成功才能完成我的功能，或者都失败，保证操作是符合要求的。

  在java中控制事务，事务应该放在service类的业务方法上，因为业务方法会调用多个dao方法，执行多个sql语句

通常使用JDBC访问数据库， 还是mybatis访问数据库怎么处理事务

  jdbc访问数据库，处理事务  Connection conn ; conn.commit(); conn.rollback();

  mybatis访问数据库，处理事务， SqlSession.commit();  SqlSession.rollback();

  hibernate访问数据库，处理事务， Session.commit(); Session.rollback();

以上事务的处理方式，有什么不足

  1)不同的数据库访问技术，处理事务的对象，方法不同，需要了解不同数据库访问技术使用事务的原理

  2)需要掌握多种数据库中事务的处理逻辑。什么时候提交事务，什么时候回顾事务

  3)需要了解处理事务的多种方法。

  总结： 就是多种数据库的访问技术，有不同的事务处理的机制，对象，方法。

  怎么解决不足：spring提供一种处理事务的统一模型， 能使用统一步骤，方式完成多种不同数据库访问技术的事务处理。

处理事务，需要怎么做，做什么

  spring处理事务的模型，使用的步骤都是固定的。把事务使用的信息提供给spring就可以了

  1）事务内部提交，回滚事务，使用的事务管理器对象，代替你完成commit，rollback

    事务管理器是一个接口和他的众多实现类。

      接口：PlatformTransactionManager ，定义了事务重要方法 commit ，rollback

      实现类：spring把每一种数据库访问技术对应的事务处理类都创建好了。

        例如：mybatis访问数据库---spring创建好的是DataSourceTransactionManager

      怎么使用：声明数据库访问技术对于的事务管理器实现类， 在spring的配置文件中使用<bean>声明就可以了

  2）你的业务方法需要什么样的事务，说明需要事务的类型。

    说明方法需要的事务：

      1）事务的隔离级别：有4个值

        ISOLATION_DEFAULT：采用 DB 默认的事务隔离级别。MySql 默认 REPEATABLE_READ； Oracle默认READ_COMMITTED。

        ISOLATION_READ_UNCOMMITTED：读未提交。未解决任何并发问题。

        ISOLATION_READ_COMMITTED：读已提交。解决脏读，存在不可重复读与幻读。

        ISOLATION_REPEATABLE_READ：可重复读。解决脏读、不可重复读，存在幻读

        ISOLATION_SERIALIZABLE：串行化。不存在并发问题。

      2) 事务的超时时间： 表示一个方法最长的执行时间，如果方法执行时超过了时间，事务就回滚。

        单位是秒， 整数值， 默认是 -1

      3）事务的传播行为 ： 控制业务方法是不是有事务的， 是什么样的事务的。

        7个传播行为，表示你的业务方法调用时，事务在方法之间是如果使用的。
```

```
PROPAGATION_REQUIRED
  指定的方法必须在事务内执行。若当前存在事务，就加入到当前事务中；若当前没有事务，则创建一个新事务。这种传播行为是最常见的选择（最常见，默认的）
  
PROPAGATION_SUPPORTS
  指定的方法支持当前事务，但若当前没有事务，也可以以非事务方式执行。
PROPAGATION_REQUIRES_NEW
  总是新建一个事务，若当前存在事务，就将当前事务挂起，直到新事务执行完毕。
  
以上三个需要掌握的

PROPAGATION_MANDATORY
PROPAGATION_NESTED
PROPAGATION_NEVER
PROPAGATION_NOT_SUPPORTED
```

```
  3）事务提交事务、回滚事务的时机

    业务方法，执行成功，没有异常抛出，当方法执行完毕，spring在方法执行后提交事务。调用事务管理器的commit

    业务方法抛出运行时异常或ERROR， spring执行回滚，调用事务管理器的rollback

    业务方法抛出非运行时异常， 主要是受查异常时，提交事务
```

### 总结 spring 的事务

```
1.管理事务的是 事务管理和他的实现类

2.spring的事务是一个统一模型

  1）指定要使用的事务管理器实现类，使用<bean>

  2）指定哪些类，哪些方法需要加入事务的功能

  3）指定方法需要的隔离级别，传播行为，超时

  你需要告诉spring，你的项目中类信息，方法的名称，方法的事务传播行为。
```

### spring 的事务处理方案

```
1.适合中小项目使用的， 注解方案。

  spring框架自己用aop实现给业务方法增加事务的功能， 使用@Transactional注解增加事务。

  @Transactional注解是spring框架自己注解，放在public方法的上面，表示当前方法具有事务。

  可以给注解的属性赋值，表示具体的隔离级别，传播行为，异常信息等等

  使用@Transactional的步骤：

  1.需要声明事务管理器对象

    <bean id="xx" class="DataSourceTransactionManager">

  2.开启事务注解驱动， 告诉spring框架，我要使用注解的方式管理事务。

    spring使用aop机制，创建@Transactional所在的类代理对象，给方法加入事务的功能。

    spring给业务方法加入事务：

      在你的业务方法执行之前，先开启事务，在业务方法之后提交或回滚事务，使用aop的环绕通知
```

```java
@Around("你要增加的事务功能的业务方法名称")

Object myAround(){
  开启事务，spring给你开启
  try{
    buy(1001,10);
    spring的事务管理器.commit();
  }catch(Exception e){
    spring的事务管理器.rollback();
  }
}
```

```
  3.在你的方法的上面加入@Trancational

2.适合大型项目，有很多的类，方法，需要大量的配置事务，使用aspectj框架功能，在spring配置文件中

  声明类，方法需要的事务。这种方式业务方法和事务配置完全分离。

  实现步骤： 都是在xml配置文件中实现。 

    1)要使用的是aspectj框架，需要加入依赖

    2）声明事务管理器对象

      <bean id="xx" class="DataSourceTransactionManager">

    3) 声明方法需要的事务类型（配置方法的事务属性【隔离级别，传播行为，超时】）

    4) 配置aop：指定哪些哪类要创建代理。
```

### 实现步骤

```
1. 新建maven项目

2.  加入maven的依赖

  spring依赖，mybatis依赖，mysql驱动，spring的事务的依赖

  mybatis和spring集成的依赖： mybatis官方体用的，用来在spring项目中创建mybatis的SqlSesissonFactory，dao对象的

1. 创建实体类

2. 创建dao接口和mapper文件

3. 创建mybatis主配置文件

4. 创建Service接口和实现类

5. 创建spring的配置文件：声明mybatis的对象交给spring创建

  1）数据源DataSource

  2）SqlSessionFactory

  3）Dao对象

  4）声明自定义的service

1. 创建测试类，获取Service对象，通过service调用dao完成数据库的访问
```

## Spring 与 Web

### 概述

```
1.javase项目是有main方法的，执行代码是执行main方法的，

  在main里面创建的容器对象：ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");

2.web项目是在tomcat服务器上运行的。 tomcat一起动，项目一直运行的。

  需求：web项目中容器对象只需要创建一次，把容器对象放入到全局作用域ServletContext中。

  怎么实现：使用监听器 当全局作用域对象被创建时 创建容器 存入ServletContext

    监听器作用：

      1）创建容器对象，执行 ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");

      2）把容器对象放入到ServletContext， ServletContext.setAttribute(key,ctx)

    监听器可以自己创建，也可以使用框架中提供的ContextLoaderListener

      加入依赖

      注册监听器web.xml
```

```xml
<!--注册监听器ContextLoaderListener
    监听器被创建对象后，会读取/WEB-INF/spring.xml
    为什么要读取文件：因为在监听器中要创建ApplicationContext对象，需要加载配置文件。
    /WEB-INF/applicationContext.xml就是监听器默认读取的spring配置文件路径
    可以修改默认的文件位置，使用context-param重新指定文件的位置
    配置监听器：目的是创建容器对象，创建了容器对象， 就能把spring.xml配置文件中的所有对象都创建好。
    用户发起请求就可以直接使用对象了。
-->
<context-param>
    <!-- contextConfigLocation:表示配置文件的路径  -->
    <param-name>contextConfigLocation</param-name>
    <!--自定义配置文件的路径-->
    <param-value>classpath:spring.xml</param-value>
</context-param>
<listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
```

```
      private WebApplicationContext context;

      public interface WebApplicationContext extends ApplicationContext

      ApplicationContext:javase项目中使用的容器对象

        WebApplicationContext：web项目中的使用的容器对象

        把创建的容器对象，放入到全局作用域

      key： WebApplicationContext.ROOT_WEB_APPLICATION_CONTEXT_ATTRIBUTE

      value：this.context

      servletContext.setAttribute(WebApplicationContext.ROOT_WEB_APPLICATION_CONTEXT_ATTRIBUTE, this.context);
```

### 实现步骤

```
（在web项目中使用spring，完成学生注册功能）

1. 创建maven，web项目

2. 加入依赖

  拷贝ch7-spring-mybatis中依赖，jsp，servlet依赖

1. 拷贝ch7-spring-mybatis的代码和配置文件

2. 创建一个jsp发起请求，有参数id,name ,email,age.

3. 创建Servlet，接收请求参数， 调用 Service ，调用dao完成注册

4. 创建一个jsp作为显示结果页面
```
