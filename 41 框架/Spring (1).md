#### 异常处理
todo
```java
@ResponseBody
@ExceptionHandler({ValidException.class})
public RestResult validExceptionHandle(HttpServletRequest request, ValidException e) {
    RestResult restResult = this.createResultByException(e);
    this.externalHandler(restResult, e);
    logger.error(MessageFormat.format("Global exception information {0}", e.getMessage()), e);
    return restResult;
}

@ControllerAdvice
public class CustomExceptionHandler extends GlobalExceptionHandler {

@ResponseBody
   @ExceptionHandler(ValidException.class)
@Override
   public RestResult validExceptionHandle(HttpServletRequest request, ValidException e) {
、、、
    logger.error(MessageFormat.format("Http url={0} valid params error {1}",request.getRequestURI(), e.getMessage()), e);
   return restResult;
}
```
#### @valid



Spring
	IOC
	AOP
SpringMVC
SpringBoot
	自动配置、开箱即用
	整合Web
	整合数据库（事务问题）
	整合权限
		Shiro
		SpringSecurity
	整合各种中间件
		缓存
		MQ
		RPC框架
		NIO框架
		等。。。


Spring 5
	描述：Java 轻量级应用框架
	IOC
	AOP
	事务
SpringMVC
	描述：Java 轻量级 web 开发框架
	什么是 MVC？
	请求与响应
	Restful API
	拦截器
	配置
	执行过程



#### 多配置文件
application-dev.properties
application-test.properties
application-prod.properties

application-sftp.properties
application-druid.properties

在 application.properties 中指定启用哪一个配置文件
spring.profiles.active = test 启用 application-test.properties

同一个环境有多个配置文件，同时启用
spirng.profiles.include = sftp,druid;

优先级，从高到低
1、-- 项目根目录config文件夹里面
./config/ 
2、--项目根目录 
./ 
3、-- src/main/resources/config/文件夹里面 
classpath:/config 
4、-- src/main/resources/ 
classpath:/ 

