### 其他

##### Web

```
server.port=8080
server.servlet.context-path=/
```

##### 拦截器

```
//@Component
public class UserInterceptor implements HandlerInterceptor {
    @Autowired
    private RedisTemplate<String, Object> redisTemplate;

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("拦截器");
        String bigKey = "spring:session:sessions:" + request.getSession().getId();
        String smallKey = "sessionAttr:" + request.getSession().getId();
        Boolean hasKey = redisTemplate.boundHashOps(bigKey).hasKey(smallKey);
        if (!hasKey) {
            response.sendRedirect("/loginPage");
            return false;
        } else {
            return true;
        }
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
@Configuration
public class InterceptorConfig implements WebMvcConfigurer {
    @Bean
    public UserInterceptor userInterceptor(){
        return new UserInterceptor();
    }
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        String[] addPathPatterns = {"/**"};
        String[] excludePathPatterns = {"/", "/loginPage", "/login", "/index", "/student/insert"};
        registry.addInterceptor(userInterceptor()).addPathPatterns(addPathPatterns).excludePathPatterns(excludePathPatterns);
    }
}
```

##### 异常

exception 包：自定义异常类

```
public class TestException extends Exception {
    public TestException() {
    }
    public TestException(String message) {
        super(message);
    }
}
```

controller 包：全局异常处理类

```
@ControllerAdvice
public class GlobalExceptionResolver {
    @ExceptionHandler(value = TestException.class)
    public String testException(Exception exception, Model model){
        model.addAttribute("massage", exception);
        return "massage";
    }

    @ExceptionHandler
    public String other(Exception exception, Model model){
        model.addAttribute("massage", exception);
        return "massage";
    }
}
```

##### Thymeleaf

pom 文件：添加依赖

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

application.properties

```
spring.thymeleaf.cache=false

spring.thymeleaf.prefix=classpath:/templates/
spring.thymeleaf.suffix=.html
```

修改设置

![image.png](https://cdn.nlark.com/yuque/0/2021/png/12407496/1636038511325-f20af9b3-ad10-4e3b-a978-17f7452cb3ba.png#clientId=u348eef84-0239-4&from=paste&height=351&id=u43a48f93&margin=%5Bobject%20Object%5D&name=image.png&originHeight=702&originWidth=1186&originalType=binary&ratio=1&size=73167&status=done&style=none&taskId=uddd272fa-82ac-4e17-a674-8f55faf956c&width=593)；

##### Json

##### 事务

启动类：添加注解

```
@EnableTransactionManagement
```

方法：添加注解

```
@Transactional
public int changeString(String id) throws TestException {
    Student student = studentMapper.selectByPrimaryKey(id);
    student.setPhone(id + "phone");
    if (Integer.parseInt(student.getId()) % 2 == 0) {
        throw new TestException("事务测试产生的异常");
    }
    student.setName(id + "name");
    studentMapper.updateByPrimaryKeySelective(student);
    return 1;
}
```

##### GIt

![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X3BuZy91SkRBVUtyR0M3S3N1OFVsSVR3TWxiWDNrTUd0WjlwMEFJSTZZVm9vVXppYnBpYnpKbm9PSEhYVXNMM2Y5RHFBNGhvclVpYmZjcEVaODhPeWYyZ1FRTlI2dy82NDA?x-oss-process=image/format,png#id=wAbZS&originHeight=337&originWidth=1080&originalType=binary&ratio=1&status=done&style=none)；

下载：git clone url

工作区
add .
暂存区
commit -m "asdf"
本地仓库
push
远程仓库

分支
列出：git branch
切换：git checkout master
创建并切换：git checkout -b feature_x
删除：git branch -d feature_x

Gitee

公钥

```
// C:\Users\123\.ssh
ssh-keygen -t rsa -C"1585398723@qq.com"
一路回车
```

```
1、使用ssh方式克隆 git clone git@gitee.com:Name/project.git
就是说，在项目克隆/下载处，选择ssh方式的下载地址
2、如果你已经用https方式克隆了仓库，不必删除仓库重新克隆，只需将当前项目中的 .git/config文件中的
url = https://gitee.com/Name/project.git
修改为
url = git@gitee.com:Name/project.git
再次提交就不需要密码了！
```
