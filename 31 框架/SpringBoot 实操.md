module project maven父工程

@Component
@ConfigurationProperties
配置类

@RestController
是 @Controller 与@ResponseBody 的组合注解

@RequestMapping_(_value = "/aaa", method = RequestMethod._GET)_

restful
@PathVariable

序列化？？？？

SpringBoot 项目热部署
# 核心内容
## 新建项目
![image.png](https://cdn.nlark.com/yuque/0/2021/png/12407496/1637483563082-61c25329-7f1d-4d30-961f-03812f6d913d.png#clientId=ua8a70808-566d-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=629&id=u84ce542b&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1258&originWidth=1604&originalType=binary&ratio=1&rotation=0&showTitle=false&size=202447&status=done&style=none&taskId=u40374ef4-9215-4ece-a1d1-fadca67d0d1&title=&width=802)
![image.png](https://cdn.nlark.com/yuque/0/2021/png/12407496/1637483642621-0bb3c9ff-8dc6-4bbb-a5b7-4383c38fc862.png#clientId=ua8a70808-566d-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=738&id=ucb32656b&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1476&originWidth=1604&originalType=binary&ratio=1&rotation=0&showTitle=false&size=190142&status=done&style=none&taskId=ud9e94f25-7ea8-48b6-81be-ba7186695b1&title=&width=802)
## 骨架（mybatis plus）
```
spring.mvc.static-path-pattern=/**
spring.web.resources.static-locations=classpath:/static/
```
### Controller
```java
@Controller
@RequestMapping("admin")
public class AdminController {
    @Resource
    AdminService adminService;

    @RequestMapping("demo")
    public void demo() {
        adminService.domo();
    }
}
```
### Service
```java
public interface AdminService {
    void domo();
}
```

```java
@Service
public class AdminServiceImpl implements AdminService {
    @Resource
    AdminMapper adminMapper;

    @Override
    public void domo() {
        adminMapper.demo();
    }
}
```
### Dao
pom.xml
```xml
<!-- mybatis-plus -->
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.4.3.4</version>
</dependency>

<!-- mybatis -->
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.2.0</version>
</dependency>

```
AdminMapper
```java
public interface AdminMapper {
    void demo();
}
```
```java
入口类
@SpringBootApplication
@MapperScan("com.xxx.xxx.dao")
```
### SQL


[
](https://www.giant-bicycles.com/us/contend-ar-1-2022)



```xml
mybatis.mapper-locations=classpath:mapper/*.xml
```

## 配置文件




# 其他
### 日志
[https://www.jianshu.com/p/546e9aace657](https://www.jianshu.com/p/546e9aace657)
OFF		关闭：最高级别，不输出日志。
FATAL	致命：输出非常严重的可能会导致应用程序终止的错误。
**ERROR	错误：输出错误，但应用还能继续运行。**
**WARN	警告：输出可能潜在的危险状况。**
**INFO	信息：输出应用运行过程的详细信息。**
**DEBUG	调试：输出更细致的对调试应用有用的信息。**
TRACE	跟踪：输出更细致的程序运行轨迹。
ALL		所有：输出所有级别信息。
```java
Logger logger = LoggerFactory.getLogger(getClass());

int i = 1;
String s = "hello";

logger.trace("trace");
logger.debug("debug");
logger.info("info{}and{}", i, s);
logger.info("info[{}]and[{}]", i, s);
logger.warn("warn");
logger.error("error");
```
应用场景

- **编程语言提示异常**：如今各类主流的编程语言都包括异常机制，业务相关的流行框架有完整的异常模块。这类捕获的异常是系统告知开发人员需要加以关注的，是质量非常高的报错。应当适当记录日志，根据实际结合业务的情况使用warn或者error级别。
- **业务流程预期不符**：除开平台以及编程语言异常之外，项目代码中结果与期望不符时也是日志场景之一，简单来说所有流程分支都可以加入考虑。取决于开发人员判断能否容忍情形发生。常见的合适场景包括外部参数不正确，数据处理问题导致返回码不在合理范围内等等。
- **系统核心角色，组件关键动作**：系统中核心角色触发的业务动作是需要多加关注的，是衡量系统正常运行的重要指标，建议记录INFO级别日志，比如电商系统用户从登录到下单的整个流程；微服务各服务节点交互；核心数据表增删改；核心组件运行等等，如果日志频度高或者打印量特别大，可以提炼关键点INFO记录，其余酌情考虑DEBUG级别。
- **系统初始化**：系统或者服务的启动参数。核心模块或者组件初始化过程中往往依赖一些关键配置，根据参数不同会提供不一样的服务。务必在这里记录INFO日志，打印出参数以及启动完成态服务表述。

INFO和DEBUG的选择
DEBUG级别比INFO低，包含调试时更详细的了解系统运行状态的东西，比如变量的值等等，都可以输出到DEBUG日志里。 INFO是在线日志默认的输出级别，反馈系统的当前状态给最终用户看的。输出的信息，应该对最终用户具有实际意义的。从功能角度上说，Info输出的信息可以看作是软件产品的一部分，所以需要谨慎对待，不可随便输出。尝试记录INFO日志时不妨在头脑中模拟线上运行，如果这条日志会被频繁打印或者大部分时间对于纠错起不到作用，就应当考虑下调为DEBUG级别。

由于info及debug日志打印量远大于ERROR，出于前文日志性能的考虑，如果代码为核心代码，执行频率非常高，务必推敲日志设计是否合理，是否需要下调为DEBUG级别日志。
注意日志的可读性，不妨在写完代码review这条日志是否通顺，能否提供真正有意义的信息。
日志输出是多线程公用的，如果有另外一个线程正在输出日志，上面的记录就会被打断，最终显示输出和预想的就会不一致。
作者：GeekerLou
链接：https://www.jianshu.com/p/546e9aace657
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。


修改输出级别（控制台）
_<_root level="DEBUG" includeLocation="true"_>_
_<_appender-ref ref="Console" _/>_
_<_appender-ref ref="RollingFile" _/>_
_</_root_>_
### 
### 
### Excel表
```java
wb = new SXSSFWorkbook();
Sheet sheet = wb.createSheet();
Row r = sheet.createRow(i + 1); 第几行
Cell cell = r.createCell(j); 某行的第几个
cell.setCellValue("aaa");
```
### 
### 配置文件

```
school.name=bjpowernode
school.websit=http://www.bjpowernode.com
```

```
@Component//将此类将给spring容器进行管理
@ConfigurationProperties(prefix = "school")
public class School {
    private String name;
    private String websit;
    //set get 
}
```

```
    @Autowired
    private School school;
    @RequestMapping(value = "/say")
    public @ResponseBody String say() {
        return "school.name="+school.getName()+"-----school.websit="+school.getWebsit();
    }
```

### 异常处理

```
@ControllerAdvice(assignableTypes = {ExceptionController.class})
@ResponseBody
public class GlobalExceptionHandler {

    ErrorResponse illegalArgumentResponse = new ErrorResponse(new IllegalArgumentException("参数错误!"));
    ErrorResponse resourseNotFoundResponse = new ErrorResponse(new ResourceNotFoundException("Sorry, the resourse not found!"));

    @ExceptionHandler(value = Exception.class)// 拦截所有异常, 这里只是为了演示，一般情况下一个方法特定处理一种异常
    public ResponseEntity<ErrorResponse> exceptionHandler(Exception e) {

        if (e instanceof IllegalArgumentException) {
            return ResponseEntity.status(400).body(illegalArgumentResponse);
        } else if (e instanceof ResourceNotFoundException) {
            return ResponseEntity.status(404).body(resourseNotFoundResponse);
        }
        return null;
    }
}
```

```
@ControllerAdvice
public class GlobalExceptionHandler {
    @ExceptionHandler(value = NameException.class)
    public ModelAndView doNameException(Exception exception){
        //处理NameException的异常。
        /*
           异常发生处理逻辑：
           1.需要把异常记录下来， 记录到数据库，日志文件。
             记录日志发生的时间，哪个方法发生的，异常错误内容。
           2.发送通知，把异常的信息通过邮件，短信，微信发送给相关人员。
           3.给用户友好的提示。
         */
        ModelAndView mv = new ModelAndView();
        mv.addObject("msg","姓名必须是zs，其它用户不能访问");
        mv.addObject("ex",exception);
        mv.setViewName("nameError");
        return mv;
    }
}
```

### Filter 过滤器

注解

```
@WebFilter(urlPatterns = "/user/*")
public class MyFilter implements Filter {
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        System.out.println("-------------------您已进入过滤器---------------------");
        filterChain.doFilter(servletRequest, servletResponse);
    }
}
```

```
@SpringBootApplication
@ServletComponentScan(basePackages = "com.bjpowernode.springboot.filter")
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

配置类（ Filter 类上不需要注解）

```
@Configuration  //定义此类为配置类
public class FilterConfig {
    @Bean
    public FilterRegistrationBean myFilterRegistrationBean() {
        //注册过滤器
        FilterRegistrationBean filterRegistrationBean = new FilterRegistrationBean(new MyFilter());
        //添加过滤路径
        filterRegistrationBean.addUrlPatterns("/user/*");
        return filterRegistrationBean;
    }
}
```

### Interceptor 拦截器

```
public class UserInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        //编写业务拦截的规则
        //从session中获取用户的信息
        User user = (User) request.getSession().getAttribute("user");
        //判断用户是否登录
        if (null == user) {
            //未登录
            response.sendRedirect(request.getContextPath() + "/user/error");
            return false;
        }
        return true;
    }
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
    }
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
    }
}
```

```
@Configuration  //定义此类为配置文件(即相当于之前的xml配置文件)
public class InterceptorConfig implements WebMvcConfigurer {
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        String[] addPathPatterns = {
            "/user/**"
        };
        String[] excludePathPatterns = {
            "/user/out", "/user/error","/user/login"
        };
        registry.addInterceptor(new UserInterceptor()).addPathPatterns(addPathPatterns).excludePathPatterns(excludePathPatterns);
    }
}
```

### Mybatis

```
<!--MySQL驱动-->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
</dependency>
<!--MyBatis整合SpringBoot框架的起步依赖-->
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.1.4</version>
</dependency>
```

```
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/sms?useUnicode=true&characterEncoding=UTF-8&useJDBCCompliantTimezoneShift=true&useLegacyDatetimeCode=false&serverTimezone=UTC
spring.datasource.username=root
spring.datasource.password=3333
```

### Mybatis 逆向

插件

```xml
<plugin>
  <groupId>org.mybatis.generator</groupId>
  <artifactId>mybatis-generator-maven-plugin</artifactId>
  <version>1.3.2</version>
  <configuration>
    <configurationFile>src/main/resources/GeneratorMapper.xml</configurationFile>
    <verbose>true</verbose>
    <overwrite>true</overwrite>
  </configuration>
</plugin>
```

GeneratorMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
<generatorConfiguration>
    <!-- 指定连接数据库的 JDBC 驱动包所在位置，指定到你本机的完整路径 -->
    <classPathEntry location="C:\Program F\mysql-connector-java-5.1.49\mysql-connector-java-5.1.49.jar"/>
    <!-- 配置 table 表信息内容体，targetRuntime 指定采用 MyBatis3 的版本 -->
    <context id="tables" targetRuntime="MyBatis3">
        <!-- 抑制生成注释，由于生成的注释都是英文的，可以不让它生成 -->
        <commentGenerator>
            <property name="suppressAllComments" value="true"/>
        </commentGenerator>
        <!-- 配置数据库连接信息 -->
        <!--&nullCatalogMeansCurrent=true
        serverTimezone=UTC
        -->
        <jdbcConnection driverClass="com.mysql.jdbc.Driver"
                        connectionURL="jdbc:mysql://localhost:3306/sms?nullCatalogMeansCurrent=true"
                        userId="root"
                        password="3333">
        </jdbcConnection>
        <!-- 生成 model 类-->
        <javaModelGenerator targetPackage="com.jasonhe.sms.model"
                            targetProject="src/main/java">
            <property name="enableSubPackages" value="false"/>
            <property name="trimStrings" value="false"/>
        </javaModelGenerator>
        <!-- 生成 MyBatis 的 Mapper.xml 文件-->
        <sqlMapGenerator targetPackage="mapper"
                         targetProject="src/main/resources">
            <property name="enableSubPackages" value="false"/>
        </sqlMapGenerator>
        <!-- 生成 MyBatis 的 Mapper 接口类文件-->
        <javaClientGenerator type="XMLMAPPER"
                             targetPackage="com.jasonhe.sms.dao"
                             targetProject="src/main/java">
            <property name="enableSubPackages" value="false"/>
        </javaClientGenerator>

        <!-- 数据库表名及对应的 Java 模型类名 几张表添加几段-->
        <table tableName="t_student" domainObjectName="Student"
               enableCountByExample="false"
               enableUpdateByExample="false"
               enableDeleteByExample="false"
               enableSelectByExample="false"
               selectByExampleQueryId="false"/>
        <table tableName="t_teacher" domainObjectName="Teacher"
               enableCountByExample="false"
               enableUpdateByExample="false"
               enableDeleteByExample="false"
               enableSelectByExample="false"
               selectByExampleQueryId="false"/>
        <table tableName="t_subject" domainObjectName="Subject"
               enableCountByExample="false"
               enableUpdateByExample="false"
               enableDeleteByExample="false"
               enableSelectByExample="false"
               selectByExampleQueryId="false"/>
        <table tableName="t_course" domainObjectName="Course"
               enableCountByExample="false"
               enableUpdateByExample="false"
               enableDeleteByExample="false"
               enableSelectByExample="false"
               selectByExampleQueryId="false"/>
        <table tableName="t_course_selection" domainObjectName="CourseSelection"
               enableCountByExample="false"
               enableUpdateByExample="false"
               enableDeleteByExample="false"
               enableSelectByExample="false"
               selectByExampleQueryId="false"/>
        <table tableName="t_admin" domainObjectName="Admin"
               enableCountByExample="false"
               enableUpdateByExample="false"
               enableDeleteByExample="false"
               enableSelectByExample="false"
               selectByExampleQueryId="false"/>
    </context>
</generatorConfiguration>
```

insert：不可为空
insertSelective：可为空

```
mybatis.mapper-locations=classpath:mapper/*.xml
```

### Dao

```
入口类
@SpringBootApplication
@MapperScan("com.abc.springboot.mapper")
```

```
Dao接口和Mapper.xml分离

mybatis.mapper-locations=classpath:mapper/*.xml
```

![image.png](https://cdn.nlark.com/yuque/0/2021/png/12407496/1636038868457-4bf498b8-a368-4e4b-9edb-e6a480362eab.png#clientId=ue66175e2-90ea-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=352&id=u70f49e10&margin=%5Bobject%20Object%5D&name=image.png&originHeight=704&originWidth=982&originalType=binary&ratio=1&rotation=0&showTitle=false&size=83071&status=done&style=none&taskId=u9a1384d5-1c87-47a2-bbdd-ee9120843cd&title=&width=491)

### 事务

```
入口类
@EnableTransactionManagement
service方法
@Transactional
```

### SpringMVC

[@RestController ](/RestController ) = [@Controller ](/Controller ) +  [@ResponseBody ](/ResponseBody ) 

### 字符编码

### 打包

### Thymeleaf

```
<html xmlns:th="http://www.thymeleaf.org">
```

```
spring.thymeleaf.cache=false

#thymeleaf模版前后缀,默认可以不写
spring.thymeleaf.prefix=classpath:/templates/
spring.thymeleaf.suffix=.html
```

表达式

```
$()
@{}
```

属性

```
<div th:each="user,userStat:${userList}">
    <span th:text="${userStat.index}"></span>
    <span th:text="${userStat.count}"></span>
    <span th:text="${user.id}"></span>
    <span th:text="${user.nick}"></span>
</div>

if unless
switch case
inline="text" inline="javascript"

<body th:inline="text">
<div th:inline="text">
    数据:[[${data}]]
</div>
数据outside:[[${data}]]

<h1>内敛脚本 th:inline="javascript"</h1>
<script type="text/javascript"  th:inline="javascript">
    function showData() {
        alert([[${data}]]);
    }
</script>
<button th:onclick="showData()">展示数据</button>
</body>
```

字面量

字符串拼接

```
<span th:text="'共'+${totalRows}+'条'+${totalPage}+'页，当前第'+${currentPage}+'页'"></span>
<span th:text="|共${totalRows}条${totalPage}页,当前第${currentPage}页|"></span>
```

# 理论
@SpringBootApplication
	@ComponentScan：自动扫描并加载符合条件的组件或者bean
	@SpringBootConfiguration：表示这是一个SpringBoot的配置类
		@Configuration：说明这是一个配置类
		@Component：说明启动类本身也是Spring中的一个组件
	@EnableAutoConfiguration：开启自动配置功能

