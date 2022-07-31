## 互联网通信流程

### 概述

互联网通信是什么

- 两台计算机通过网络实现文件共享行为

![image.png](https://cdn.nlark.com/yuque/0/2021/png/12407496/1636038680709-022ca84c-830d-4d11-bc30-2938ead20fa6.png#clientId=u777e828a-1dcf-4&from=paste&height=233&id=u29c85902&margin=%5Bobject%20Object%5D&name=image.png&originHeight=465&originWidth=1323&originalType=binary&ratio=1&size=57505&status=done&style=none&taskId=uc25d819f-847c-444c-b30b-79ffed6bd96&width=661.5)

互联网通信过程角色划分

- 客户端计算机：用于发送请求，来索要资源文件的计算机
- 服务端计算机：用于接收请求，并提供对应的资源文件计算机

共享资源文件

- 共享资源文件：可以通过网络进行传输的文件（所有的文件内容都可以通过网络传输，所有文件都是共享资源文件）
- Http 服务器下对于共享资源文件分类 
   - 静态资源文件
文件内容是固定，如文档，图片，视频
文件存放命令，但这些命令只能在浏览器编译与执行，如 .html，.css，.js
   - 动态资源文件
文件存放命令，并且命令不能在浏览器编译与执行，只能在服务端计算机编译执行，如.class
- 静态资源文件与动态资源文件调用区别 
   - 静态文件被索要时
Http 服务器直接通过【输出流】将静态文件中内容或者命令以【二进制形式】推送给发起请求浏览器
   - 动态文件被索要时
Http 服务器需要创建当前 class 文件的实例对象
通过实例对象调用对应的方法处理用户请求
通过【输出流】将运行结果以【二进制形式】推送给发起请求浏览器

任务和要求

- 掌握互联网通信流程
- 本阶段使用命令都是老旧命令，不需要记忆
- 一定要背住互联网通信流程细节

涉及技术老旧

- 控制浏览器行为技术：HTML，CSS，JavaScript（被 jQuery 替代）
- 控制硬盘上数据库行为技术：MySql 数据库服务器管理使用（SQL（重点）），JDBC 规范（被 MyBatis 替代）
- 控制服务端 Java 行为技术：Http 服务器，Servlet（被 SpringMVC 替代），JSP（被模板化技术替代）
- 互联网通信流程开发规则： MVC
- 贯穿项目：在线考试管理系统

### 互联网通信模型

C/S 通信模型

C,client software;客户端软件

- 专门安装在客户端计算机上
- 帮助客户端计算机向指定服务端计算机发送请求，索要资源文件
- 帮助客户端计算机将服务端计算机发送回来【二进制数据】解析为【文字，数字，图片，视频，命令】

S,server software;服务器软件

- 专门安装在服务端计算机上
- 用于接收来自于特定的客户端软件发送请求
- 在接收到请求之后自动的在服务端计算机上定位被访问的资源文件
- 自动的将定位的文件内容解析为【二进制数据】通过网络发送回发起请求的客户端软件上

适用场景

- 普遍用于个人娱乐市场
- 如【微信，淘宝/京东，视频（优酷/B 站），大型网络游戏(魔兽/英雄联盟)】
- 企业办公领域相对应用较少

优点:

- 安全性较高
- 有效降低服务端计算机工作压力

缺点：

- 增加客户获得服务的成本（有性能要求）
- 更新较为繁琐

B/S 通信模型

B:browser,浏览器

- 安装在客户端计算机软件
- 可以向任意服务器发送请求，索要资源文件
- 可以将服务器返回的【二进制数据】解析为【文字，数字，图片，视频，命令】

S: server software 服务器软件

- 专门安装在服务端计算机上
- 可以接收任意浏览器发送请求
- 自动的在服务端计算机上定位被访问的资源文件
- 自动的将定位的资源文件内容以二进制形式发送回发起请求浏览器上

适用场景

- 既适用于个人娱乐市场
- 又广泛适用于企业日常活动

优点：

- 不会增加用户获得服务的成本
- 几乎不需要更新浏览器
- 更新容易

缺点：

- 几乎无法有效对服务端计算机资源文件进行保护
- 服务端计算机工作压力异常巨大
- 速度慢

### 开发人员职责

1. 控制浏览器请求行为（三要素）
请求地址
请求方式
发送请求时所带的参数
2. 控制浏览器接受结果行为
3. 开发动态资源文件来解决用户请求（后端）

![image.png](https://cdn.nlark.com/yuque/0/2021/png/12407496/1636038687811-86ba2fb4-e71e-4fa3-9da0-51f391056f10.png#clientId=u777e828a-1dcf-4&from=paste&height=249&id=u9441dd30&margin=%5Bobject%20Object%5D&name=image.png&originHeight=498&originWidth=1243&originalType=binary&ratio=1&size=198300&status=done&style=none&taskId=u71cd0128-6a6a-448b-9fb6-0ca498f19f5&width=621.5)![image.png](https://cdn.nlark.com/yuque/0/2021/png/12407496/1636038690549-0b00ac76-b0e5-4e8d-b68a-d2d4cbfa50de.png#clientId=u777e828a-1dcf-4&from=paste&height=368&id=u4827c51f&margin=%5Bobject%20Object%5D&name=image.png&originHeight=735&originWidth=1885&originalType=binary&ratio=1&size=1068571&status=done&style=none&taskId=uca39e2a0-5166-4dcc-bb93-718f9e08fd7&width=942.5)



## Http 网络协议与 Tomcat

### 网络协议包

是什么

- 一组有规律二进制数据
- 在这组数据存在固定空间，每一个空间专门存放特定信息，这样接收方在接收网络协议包之后，就可以到固定空间得到对应信息

解决的问题

- 在网络中传递信息都是以【二进制】形式存在的。接收方【浏览器/服务器】在接收信息后，要做第一件事就是将【二进制数据】进行编译【文字，图片，视频，命令】
- 传递信息数据量往往比较巨大，导致接收方很难在一组连续二进制得到对应数据
比如 浏览器发送一个请求： [http://192.168.100.2:8080/index.html](http://192.168.100.2:8080/index.html)，这个请求信息以二进制形式发送 01010101010110101010101101010，Http 服务器很难从二进制数据得到相关信息
- 网络协议包出现极大降低了接收方对接收二进制数据编译难度

常见网络协议：FTP，Http

URL 格式

- 网络协议包：//服务端计算机 ip 地址：服务器端口号/网站名/资源文件名
- [http://ocalhost:8080/myweb/car.ipg](http://ocalhost:8080/myweb/car.ipg)

### Http 网络协议包

在基于 B/S 结构下互联网通信过程中，所有在网络中传递信息都是保存在 Http 网络协议包中

分类

- Http 请求协议包
在浏览器准备发送请求时，负责创建一个 Http 请求协议包
浏览器将请求信息以二进制形式保存在 Http 请求协议包各个空间
由浏览器负责将 Http 请求协议包推送到指定服务端计算机
- Http 响应协议包
Http 服务器在定位到被访问的资源文件之后，负责创建一个 Http 响应协议包
Http 服务器将定位文件内容或者文件命令以二进制形式写入到 Http 响应协议包各个空间
由 Http 服务器负责将 Http 响应协议包推送回发起请求的浏览器上

【背】请求协议包内部空间（自上而下，分成四个空间）

1. 请求行
URL：请求地址（[http://192.168.100.2:8080/index.html）
method：请求方式（POST/GET）
2. 请求头：GET 方式的请求参数信息
3. 空白行：没有任何内容，起到隔离作用
4. 请求体：POST 方式的请求参数信息

【背】响应协议包内部结构（自上而下，分成四个空间）

1. 状态行：Http 状态码
2. 响应头：content-type：指定浏览器采用对应编译器对响应体二进制数据进行解析；location 属性
3. 空白行：没有任何内容，起到隔离作用
4. 响应体：可能是静态资源文件内容、静态资源文件命令、动态资源文件运行结果（都是二进制）

### Http 服务器

Http 服务器是服务器中一种，其行为与 Http 协议相关

工作过程

- 接收来自于浏览器发送的 Http 请求协议包
- 并自动对 Http 请求协议包内容进行解析
- 自动定位被访问的文件
- 将定位的文件内容写入到 Http 响应协议包中
- 将 Http 响应协议包推送回发起请求的浏览器上

Tomcat

环境变量

- java 相关（配一个就可以）
JAVA_HOME：JDK 地址
JRE_HOME：JDK 下的 jre 的地址
- CATALINA_HOME（部分系统需要）（tomcat 的地址）

启动：startup.bat（bin 文件夹下）
关闭：shutdown.bat

内部文件

- bin：管理命令
- conf：核心配置文件
- lib：需要的 jar 包
- logs：日志
- temp：临时文件
- webapps：默认资源文件位置
- work：工作空间

模拟互联网通信

- 在 Tomcat 安装地址/webapp 文件夹创建一个网站【myweb】
不能用中文
网站：网络资源站点（文件夹）
- 将一个静态资源文件添加到网站【car.jpg】
- 启动 tomcat
- 启动浏览器，命令浏览器向 tomcat 索要 car.Jpg
- [http://localhost:8080/myweb/car.Jpg](http://localhost:8080/myweb/car.Jpg)

## Servlet 概述

来自于 JAVAEE 规范中的一种

作用：指定了

- 1 动态资源文件开发步骤
- 2 Http 服务器 调用动态资源文件规则
- 3 Http 服务器 管理动态资源文件实例对象规则

## Servlet 接口实现类

### 概述

来自于 Servlet 规范下的一个接口，这个接口存在于 Http 服务提供 jar 包
Tomcat 服务器下 lib 文件有一个 servlet-api.jar 存放 Servlet 接口

Servlet 规范：Http 服务器能调用的【动态资源文件】必须是一个 Servlet 接口实现类

```java
//不是动态资源文件，Tomcat无权调用
class Student{
}

//合法动态资源文件，Tomcat有权利调用
class Teacher implements Servlet{
}
```

### Servlet 接口实现类开发步骤

1. 创建一个 Java 类继承与 HttpServlet 父类（使之成为一个 Servlet 接口实现类）（把“√”去掉）
2. 重写 HttpServlet 父类两个方法（Ctrl+o）：doGet，doPost
（模板设计模式：子类写具体方法，由父类来选择调用哪一个）
3. 将 Servlet 接口实现类信息【注册】到 Tomcat 服务器（web.xml） 
```xml
 <!--servlet接口实现类类路径地址交给Tomcat-->
 <servlet>
     <servlet-name>oneServlet</servlet-name>
     <servlet-class>com.bjpowernode.controller.OneServlet</servlet-class>
 </servlet>

 <!--为servlet接口实现类提供简短别名-->
 <servlet-mapping>
     <servlet-name>oneServlet</servlet-name>
     <url-pattern>/one</url-pattern>
 </servlet-mapping>
```

### Servlet 对象生命周期

- 创建
网站中所有的 Servlet 接口实现类的实例对象，只能由 Http 服务器负责额创建。开发人员不能手动创建
默认情况下，Http 服务器接收到对于当前 Servlet 接口实现类第一次请求时自动创建这个 Servlet 接口实现类的实例对象
手动配置情况下，可以要求 Http 服务器在启动时自动创建某个 Servlet 接口实现类的实例对象 
```xml
<servlet>
    <servlet-name>mm</servlet-name> 
    <servlet-class>com.bjpowernode.controller.OneServlet</servlet-class>
    <!--填写一个大于0的整数即可，默认是0-->
    <load-on-startup>9<load-on-startup>
</servlet>
```

- 运行：在 Http 服务器运行期间，一个 Servlet 接口实现类只能被创建出一个实例对象
- 销毁：在 Http 服务器关闭时刻，自动将网站中所有的 Servlet 对象进行销毁

## 请求与相应接口

### HttpServletResponse 接口

介绍

- 负责将 doGet/doPost 方法执行结果写入到【响应体】交给浏览器
- 开发人员习惯于将 HttpServletResponse 接口修饰的对象称为【响应对象】

主要功能

1. 将执行结果以二进制形式写入到【响应体】 
```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
     //--------响应对象将结果写入到响应体--------------start

     //1.通过响应对象，向Tomcat索要输出流
     PrintWriter out = response.getWriter();

     //2.通过输出流，将执行结果以二进制形式写入到响应体
     out.write("Hello World");

     // 建议都用print
     out.write(97); // a
     out.print(97); // 97
}
//doGet执行完毕
//Tomcat将响应包推送给浏览器
```

2. 设置 content-type 属性值（控制浏览器使用对应编译器将数据编译为【文字，图片，视频，命令】） 
```java
// 必须在得到输出流之前，设置响应头content-type
response.setContentType("text/html"); // 把内容当成HTML代码编译

// 同时修改字符集
response.setContentType("text/html;charset=utf-8");
```

3. 设置响应头中【location】属性（将一个请求地址赋值给 location，从而控制浏览器向指定服务器发送请求） 
```java
/*
  浏览器在接收到响应包之后，如果发现响应头中存在location属性
  自动通过地址栏向location指定网站发送请求（get）

  sendRedirect方法远程控制浏览器请求行为【请求地址，请求方式，请求参数】
*/
response.sendRedirect("http://www.baidu.com?userName=mike");
```

### HttpServletRequest 接口

介绍

- 负责在 doGet/doPost 方法运行时读取 Http 请求协议包中信息
- 开发人员习惯于将 HttpServletRequest 接口修饰的对象称为【请求对象】

作用

1.  读取【请求行】信息
URI（资源文件精准定位地址）
实际上请求行没有 URI 这个属性，是 URL 中截取的字符串，格式为 "/网站名/资源文件名"
用于让 Http 服务器对被访问的资源文件进行定位  
```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    // 读取url
    String url = request.getRequestURL().toString();
    // 读取uri
    String uri =  request.getRequestURI(); // substring

    // 读取method
    String method = request.getMethod();
}
```

2.  读取【请求头】或【请求体】中请求参数信息（方法一样） 
GET 方式请求参数保存在【请求头】，由 Tomcat 负责解码
Tomcat9.0 默认使用【utf-8】字符集，可以解释一切国家文字
POST 方式请求参数保存在【请求体】，由当前请求对象（request）负责解码
request 默认使用[ISO-8859-1]字符集，一个东欧语系字符集  无法解析中文  
```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    // 1.通过请求对象获得【请求头】中【所有请求参数名】

    String no = request.getParameter("no");

    // 将所有请求参数名称保存到一个枚举对象进行返回
    Enumeration paramNames = request.getParameterNames(); 
    while(paramNames.hasMoreElements()){
          String paramName = (String)paramNames.nextElement();
          //2.通过请求对象读取指定的请求参数的值
          String value = request.getParameter(paramName);
          System.out.println("请求参数名 "+paramName+" 请求参数值 "+value);
    }
}
```
```java
protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    //通知请求对象，使用utf-8字符集对请求体二进制内容进行一次重写解码
    request.setCharacterEncoding("utf-8");
    //通过请求对象，读取【请求体】参数信息
    String value = request.getParameter("userName");
    System.out.println("从请求体得到参数值 "+value);
}
```

3.  代替浏览器向 Http 服务器申请资源文件调用  
```java
response.sendRedirect("/myWeb/index.html");
```

解码方式

问题

- 以 GET 方式发送中文参数内容“老杨是正经人”时，得到正常结果
- 以 POST 方式发送中文参数内容"老崔是个男人"，得到【乱码】“è?????????????·??”

原因

- 浏览器以 GET 方式发送请求,请求参数保存在【请求头】
在 Http 请求协议包到达 Http 服务器之后，第一件事就是进行解码
请求头二进制内容由 Tomcat 负责解码，Tomcat9.0 默认使用【utf-8】字符集，可以解释一切国家文字
- 浏览器以 POST 方式发送请求，请求参数保存在【请求体】
在 Http 请求协议包到达 Http 服务器之后，第一件事就是进行解码
请求体二进制内容由当前请求对象（request）负责解码。request 默认使用[ISO-8859-1]字符集，一个东欧语系字符集，此时如果请求体参数内容是中文，将无法解码只能得到乱码

解决方法

- 在 Post 请求方式下，在读取请求体内容之前，应该通知请求对象使用 utf-8 字符集对请求体内容进行一次重新解码 
```java
protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    //通知请求对象，使用utf-8字符集对请求体二进制内容进行一次重写解码
    request.setCharacterEncoding("utf-8");

    //通过请求对象，读取【请求体】参数信息
    String value = request.getParameter("userName");

}
```

### 请求对象和响应对象生命周期

- 接收 Http 请求协议包之后
自动为当前的【Http 请求协议包】生成一个【请求对象】和一个【响应对象】
- 调用 doGet/doPost 方法时
负责将【请求对象】和【响应对象】作为实参传递到方法，确保 doGet/doPost 正确执行
- 推送 Http 响应协议包之前
还要负责将本次请求关联的【请求对象】和【响应对象】销毁
- 【请求对象】和【响应对象】生命周期贯穿一次请求的处理过程中，相当于用户在服务端的代言人

![image.png](https://cdn.nlark.com/yuque/0/2021/png/12407496/1636038707879-04996cbf-bccb-418c-a293-3210dd33a458.png#clientId=u777e828a-1dcf-4&from=paste&height=266&id=ub65e3077&margin=%5Bobject%20Object%5D&name=image.png&originHeight=531&originWidth=1326&originalType=binary&ratio=1&size=562595&status=done&style=none&taskId=u93b174d4-7338-4457-9666-54b1a294fdf&width=663)![image.png](https://cdn.nlark.com/yuque/0/2021/png/12407496/1636038710265-e8da2bc4-8e63-4563-b9d0-ad11d1acc466.png#clientId=u777e828a-1dcf-4&from=paste&height=258&id=u0f5f21ab&margin=%5Bobject%20Object%5D&name=image.png&originHeight=515&originWidth=1323&originalType=binary&ratio=1&size=143211&status=done&style=none&taskId=ufd8efc7c-19e9-4180-9a86-c99c88263aa&width=661.5)

![image.png](https://cdn.nlark.com/yuque/0/2021/png/12407496/1636038713922-b5c1e519-ce64-4d69-89d3-524378bdacc3.png#clientId=u777e828a-1dcf-4&from=paste&height=266&id=uc50a1139&margin=%5Bobject%20Object%5D&name=image.png&originHeight=531&originWidth=1326&originalType=binary&ratio=1&size=562595&status=done&style=none&taskId=uadcd1c36-c1a4-4fd8-afeb-6c2c1dc6cba&width=663)

## 欢迎资源文件

使用原因：用户可以记住网站名，但是不会记住网站资源文件名

默认欢迎资源文件

- 用户发送了一个针对某个网站的【默认请求】时，由 Http 服务器自动从当前网站返回的资源文件
- 正常请求： [http://localhost:8080/myWeb/index.html](http://localhost:8080/myWeb/index.html)
默认请求：[http://localhost:8080/myWeb/](http://localhost:8080/myWeb/)

Tomcat 对于默认欢迎资源文件定位规则

- 位置：Tomcat 安装位置/conf/web.xml 
```xml
<welcome-file-list>
  <welcome-file>index.html</welcome-file>
  <welcome-file>index.htm</welcome-file>
  <welcome-file>index.jsp</welcome-file>
</welcome-file-list>
```

设置当前网站的默认欢迎资源文件规则

- 位置:  网站/web/WEB-INF/web.xml（设置自定义默认文件定位规则后，Tomcat 自带定位规则将失效） 
```xml
<welcome-file-list>
  <welcome-file>login.html</welcome-file>
  <!--<welcome-file>user/add</welcome-file> 要去掉/ -->
</welcome-file-list>
```

## Http 状态码

介绍

- 由三位数字组成的一个符号。100---599；分为 5 个大类
- Http 服务器在推送响应包之前，根据本次请求处理情况，将 Http 状态码写入到响应包中【状态行】上

作用

- 如果 Http 服务器 返回了对应资源文件，通知浏览器 应该如何处理这个结果
- 如果 Http 服务器 无法返回对应资源文件，向浏览器 解释不能提供服务的原因

分类（5 大类）

- 1XX 
   - 100
本次返回的资源文件并不是一个独立的资源文件
需要浏览器在接收响应包之后，继续向 Http 服务器所要依赖的其他资源文件
- 2XX 
   - 200
本次返回的资源文件是一个完整独立资源文件
浏览器在接收到之后不需要所要其他关联文件
- 3XX 
   - 302
本次返回的不是一个资源文件内容而是一个资源文件地址
需要浏览器根据这个地址自动发起请求来索要这个资源文件
response.sendRedirect("资源文件地址")写入到响应头中
location
这种行为导致 Tomcat 将 302 状态码写入到状态行
- 4XX 
   - 404
在服务端没有定位到被访问的资源文件
因此无法提供帮助
   - 405
在服务端已经定位到被访问的资源文件（Servlet）
但是这个 Servlet 对于浏览器采用的请求方式不能处理
- 5XX 
   - 500
在服务端已经定位到被访问的资源文件（Servlet）
这个 Servlet 可以接收浏览器采用请求方式
但是 Servlet 在处理请求期间，由于 Java 异常导致处理失败

## ** Servlet 之间调用

### 概述

某些来自于浏览器发送请求，往往需要服务端中多个 Servlet 协同处理。
但浏览器一次只能访问一个 Servlet，导致用户需要手动通过浏览器发起多次请求才能得到服务
这样增加用户获得服务难度，导致用户放弃访问当前网站

提高用户使用感受
无论本次请求涉及到多少个 Servlet，用户只需要【手动】通知浏览器发起一次请求即可

### 重定向解决方案

工作原理

- 用户第一次通过【手动方式】通知浏览器访问 OneServlet。
- OneServlet 工作完毕后，将 TwoServlet 地址写入到响应头 location 属性中，导致 Tomcat 将 302 状态码写入到状态行。
- 在浏览器接收到响应包之后，会读取到 302 状态。此时浏览器自动根据响应头中 location 属性地址发起第二次请求，访问 TwoServlet 去完成请求中剩余任务

实现命令（将地址写入到响应包中响应头中 location 属性）

```java
// 请求地址格式：/网站名/资源文件名
response.sendRedirect("请求地址"); 

response.sendRedirect("http://www.baidu.com"); 
response.sendRedirect("/myWeb/two");
```

特征

- 请求地址：可以发送“当前网站内部的资源文件地址”，也可以发送“其他网站资源文件地址”
- 请求次数：浏览器至少两次，用户只要一次
- 请求方式：一定是【GET】（因为是通过地址栏通知浏览器发起下一次请求）

缺点

- 需要在浏览器与服务器之间多次往返，大量时间消耗在往返次数上，增加用户等待服务时间

### 请求转发解决方案

原理

- 用户第一次通过手动方式要求浏览器访问 OneServlet。
- OneServlet 工作完毕后，通过当前的请求对象代替浏览器向 Tomcat 发送请求，申请调用 TwoServlet。
- Tomcat 在接收到这个请求之后，自动调用 TwoServlet 来完成剩余任务

实现命令（请求对象代替浏览器向 Tomcat 发送请求）

```java
// 1.通过当前请求对象生成“资源文件申请报告对象”（资源文件名一定要以"/"为开头）
RequestDispatcher report = request.getRequestDispatcher("/资源文件名");
// 2.将报告对象发送给Tomcat
report.forward(当前请求对象，当前响应对象)

RequestDispatcher report = request.getRequestDispatcher("/two");
report.forward(request, response);
```

特征

- 请求次数：浏览器只发送一次
- 请求地址：只能向 Tomcat 服务器申请调用当前网站下资源文件地址（**不能写网站名**）
- 请求方式：与浏览器发送的请求方式保持一致
浏览器只发送了一个 Http 请求协议包，参与本次请求的所有 Servlet 共享同一个请求协议包

优点

- Servlet 之间调用发生在服务端计算机上，节省服务端与浏览器之间往返次数，增加处理服务速度

## Servlet 之间数据共享

### 概述

例如 OneServlet 工作完毕后，将产生数据交给 TwoServlet 来使用

4 种方法

### 1  ServletContext 接口（全局作用域对象）

介绍

- 使用条件：两个 Servlet 来自于同一个网站
- “垃圾箱”，不可随意添加数据，要放关键数据
- 每一个网站都存在一个全局作用域对象。【相当于】一个 Map，这个网站中的 Servlet 可以将一个数据存入到全局作用域对象，也可以从全局作用域对象得到去除数据

生命周期：贯穿网站整个运行期间

- 在 Http 服务器启动过程中，自动为当前网站在内存中**创建**一个全局作用域对象
- 在 Http 服务器运行期间，一个网站只有**一个**全局作用域对象，且一直处于**存活**状态
- 在 Http 服务器准备关闭时，负责将当前网站中全局作用域对象进行**销毁**处理

命令实现

```java
// OneServlet将数据共享给TwoServlet

OneServlet{
  public void doGet(HttpServletRequest request,HttpServletResponse response){
    //1.通过【请求对象】向Tomcat索要当前网站中【全局作用域对象】
    ServletContext application = request.getServletContext();// 一般起名application 
    //2.将数据添加到全局作用域对象作为【共享数据】
    application.setAttribute("key1",数据值)
  }
}

TwoServlet{
  public void doGet(HttpServletRequest request,HttpServletResponse response){
    //1.通过【请求对象】向Tomcat索要当前网站中【全局作用域对象】
    ServletContext application = request.getServletContext();
    //2.从全局作用域对象得到指定关键字对应数据
    Object 数据对象 =  application.getAttribute("key1");
  }
}
```

### 2  Cookie 类

介绍

- 使用条件：两个 Servlet 来自于同一个网站，并且为同一个浏览器/用户提供服务
- Cookie 存放当前用户的私人数据，在共享数据过程中提高服务质量
- 在现实生活场景中，Cookie 相当于用户在服务端得到【会员卡】
- 一个 cookie 中只能存放一个键值对,的 key 与 value 只能是 String,key 不能是中文

原理

- 用户通过浏览器第一次向 MyWeb 网站发送请求申请 OneServlet。
OneServlet 在运行期间创建一个 Cookie 存储与当前用户相关数据
- OneServlet 工作完毕后，【将 Cookie 写入到响应头】交还给当前浏览器。
浏览器收到响应响应包之后，将 cookie 存储在浏览器的缓存
- 一段时间之后，用户通过【同一个浏览器】再次向【myWeb 网站】发送请求申请 TwoServlet 时
【浏览器需要无条件的将 myWeb 网站之前推送过来的 Cookie，写入到请求头】发送过去
此时 TwoServlet 就可以通过读取请求头中 cookie 中信息，得到 OneServlet 提供的共享数据

存储位置及销毁时机

- 默认情况：Cookie 对象存放在浏览器的缓存中（浏览器关闭，Cookie 对象就被销毁掉）
- 手动设置：要求浏览器将 Cookie 存放在客户端计算机上硬盘上，同时需要指定 Cookie 在硬盘上存活时间
- 在存活时间内，关闭浏览器、客户端计算机、服务器，都不会导致 Cookie 被销毁
在存活时间到达时，Cookie 自动从硬盘上被删除

```java
// 同一个网站 OneServlet 与  TwoServlet 借助于Cookie实现数据共享

OneServlet{
  public void doGet(HttpServletRequest request,HttpServletResponse resp){
    // 1创建一个cookie对象，保存共享数据（当前用户数据）
    Cookie card = new Cookie("key1","abc");
    Cookie card1= new Cookie("key2","efg");
    // 2指定Cookie在硬盘上存活时间（秒）
    card.setMaxAge(60);
    // 3【发卡】将cookie写入到响应头，交给浏览器
    resp.addCookie(card);
    resp.addCookie(card1)
  }
}

TwoServlet{
  public void doGet(HttpServletRequest request,HttpServletResponse resp){
    //1.调用请求对象从请求头得到浏览器返回的Cookie
    Cookie cookieArray[] = request.getCookies();
    //2.循环遍历数据得到每一个cookie的key 与 value
    for(Cookie card : cookieArray){
      String key = card.getName();
      String value = card.getValue();
    }
  }
}
```

### 3  HttpSession 接口（会话作用域对象）

概述

- 使用条件：两个 Servlet 来自于同一个网站，并且为同一个浏览器/用户提供服务
- 如何将用户与 HttpSession 关联起来，如何区分不同用户的 HttpSession 对象
给 HttpSession 对象编号，保存成 cookie 对象发送给浏览器，用户再次访问，通过 cookie 找对应的 HttpSession 对象

HttpSession 与  Cookie 区别：【面试题】

```
存储位置：
  Cookie：存放在客户端计算机（浏览器内存/硬盘）
  HttpSession：存放在服务端计算机内存
数据类型：
  Cookie对象存储共享数据类型只能是String
  HttpSession对象可以存储任意类型的共享数据Object
数据数量:
  一个Cookie对象只能存储一个共享数据
  HttpSession使用map集合存储共享数据，可以存储任意数量共享数据
参照物:
  Cookie相当于客户在服务端【会员卡】
  HttpSession相当于客户在服务端【私人保险柜】
```

getSession()，getSession(false)

- 相同：如果当前用户在服务端 已经拥有 了自己的私人储物柜，要求 tomcat 将这个私人储物柜进行返回
- 不同：如果当前用户在服务端 尚未拥有 自己的私人储物柜 
   - getSession()：要求 tocmat 为当前用户创建一个全新的私人储物柜
   - getSession(false)：此时 Tomcat 将返回 null
- 身份合法用 getSession() ，不合法用 getSession(false)

HttpSession 销毁时机

- 用户与 HttpSession 关联时使用的 Cookie 只能存在浏览器缓存（不能存入硬盘），浏览器关闭，关系切断
- 由于 Tomcat 无法检测浏览器关闭，因此在浏览器关闭时并不会导致 HttpSession 销毁
- 所以 Tomcat 为每一个 HttpSession 对象设置【空闲时间】（默认 30 分钟），超过就销毁 HttpSession
- 手动设置空闲时间（位置：当前网站/web/WEB-INF/web.xml） 
```xml
<session-config>
    <!--当前网站中每一个session最大空闲时间5分钟-->
    <session-timeout>5</session-timeout> 
</session-config>
```

```java
// 同一个网站（myWeb）下OneServlet将数据传递给TwoServlet

OneServlet{
  public void doGet(HttpServletRequest request,HttpServletResponse response){
    //1.调用请求对象向Tomcat索要当前用户在服务端的私人储物柜
    HttpSession session = request.getSession();
    //2.将数据添加到用户私人储物柜
    session.setAttribute("key1",共享数据)
  }
}

TwoServlet{
  public void doGet(HttpServletRequest request,HttpServletResponse response){
    //1.调用请求对象向Tomcat索要当前用户在服务端的私人储物柜
    HttpSession session = request.getSession();
    //2.从会话作用域对象得到OneServlet提供的共享数据
    Object 共享数据 = session.getAttribute("key1");
  }
}
```

### 4  HttpServletRequest 接口（请求作用域对象）

概述

- 使用条件：同一网站，两个 Servlet 通过【请求转发】进行调用，彼此共享同一个请求协议包，进而对应同一个请求对象

```java
//OneServlet通过请求转发申请调用TwoServlet时，需要给TwoServlet提供共享数据

OneServlet{
  public void doGet(HttpServletRequest request,HttpServletResponse response){
    //1.将数据添加到【请求作用域对象】中attribute属性
    request.setAttribute("key1",数据); //数据的类型可以任意类型Object
    //2.向Tomcat申请调用TwoServlet
    request.getRequestDispatcher("/two").forward(request,response)
  }
}

TwoServlet{
  public void doGet(HttpServletRequest request,HttpServletResponse response){
    //从当前请求对象得到OneServlet写入到共享数据
    Object 数据 = request.getAttribute("key1");
  }
}
```

## 监听器接口

概述

- 共有 8 个接口
- 用于监控【作用域对象生命周期变化时刻】以及【作用域对象共享数据变化时刻】
- 监听器接口需要由开发人员亲自实现，Http 服务器提供 jar 包并没有对应的实现类

作用域对象（服务端内存中，可以在某些条件下，为两个 Servlet 之间提供数据共享方案的对象）

- 全局作用域对象：ServletContext
- 会话作用域对象：HttpSession
- 请求作用域对象：HttpServletRequest

监听器接口实现类开发步骤

1. 根据监听的实际情况，选择对应监听器接口进行实现
2. 重写监听器接口声明【监听事件处理方法】
3. 在 web.xml 文件将监听器接口实现类注册到 Http 服务器

ServletContextListener 接口

-  作用：检测“全局作用域对象”被初始化时刻以及被销毁时刻   
```java
public class OneListener implements ServletContextListener {
    // 初始化时被调用
    public void contextInitialized(ServletContextEvent sce) {
        System.out.println("恭喜恭喜，来世走一朝");
    }
    // 毁时被调用
    public void contextDestroyed(ServletContextEvent sce) {
        System.out.println("兄弟不要怕，二十年之后你还是一条好汉");
    }
}
```
```xml
<listener>
    <listener-class>com.bjpoewrnode.listener.OneListener</listener-class>
</listener>
```

ServletContextAttributeListener 接口

-  作用：检测全局作用域对象共享数据变化时刻   
```java
public class OneListener implements ServletContextAttributeListener {
    // 添加共享数据时被调用
    public void attributeAdded(ServletContextAttributeEvent scae) {
        System.out.println("新增共享数据");
    }
    // 更新共享数据时被调用
    public void attributeRemoved(ServletContextAttributeEvent scae) {
        System.out.println("删除共享数据");
    }
    // 删除共享数据时被调用
    public void attributeReplaced(ServletContextAttributeEvent scae) {
        System.out.println("更新共享数据");
    }
}
```
```xml
<listener>
    <listener-class>com.bjpowernode.listener.OneListener</listener-class>
</listener>
```

## 过滤器接口（Filter）

介绍

- Filter 接口实现类由开发人员负责提供，Http 服务器不负责提供
- Filter 接口在 Http 服务器调用资源文件之前，对 Http 服务器进行拦截

Filter 接口实现类开发步骤（三步）

1. 创建一个 Java 类实现 Filter 接口
2. 重写 Filter 接口中 doFilter 方法
3. web.xml 将过滤器接口实现类注册到 Http 服务器

具体作用

-  拦截 Http 服务器，帮助 Http 服务器检测当前请求合法性   
```java
public class OneFilter implements Filter {
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        //1.通过拦截请求对象得到请求包参数信息，从而得到来访用户的真实年龄
        String age= servletRequest.getParameter("age");
        //2.根据年龄，帮助Http服务器判断本次请求合法性
        if(Integer.valueOf(age) < 70){ //合法请求
            //将拦截请求对象和响应对象交还给Tomcat,由Tomcat继续调用资源文件
            filterChain.doFilter(servletRequest, servletResponse);//放行
        }else{
            //过滤器代替Http服务器拒绝本次请求
            servletResponse.setContentType("text/html;charset=utf-8");
            PrintWriter out = servletResponse.getWriter();
            out.print("<center><font style='color:red;font-size:40px'>大爷，珍爱生命啊!</font></center>");
        }
    }
}
```
```xml
<!--将过滤器类文件路径交给Tomcat-->
<filter>
    <filter-name>oneFilter</filter-name>
    <filter-class>com.bjpowernode.filter.OneFilter</filter-class>
</filter>

<!--通知Tomcat在调用何种资源文件时需要被当前过滤器拦截-->
<filter-mapping>
    <filter-name>oneFilter</filter-name>
    <url-pattern>/mm.jpg</url-pattern>
</filter-mapping>



<!--拦截内容格式-->

<!--某一个具体文件-->
<url-pattern>/img/mm.jpg</url-pattern>
<!--某一个文件夹下所有的资源文件-->
<url-pattern>/img/*</url-pattern>
<!--某种类型文件-->
<url-pattern>*.jpg</url-pattern>
<!--所有文件-->
<url-pattern>/*</url-pattern>
```

-  拦截 Http 服务器，对当前请求进行增强操作   
```java
public class OneFilter implements Filter {
    //通知拦截的请求对象，使用UTF-8字符集对当前请求体信息进行一次重新编辑
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        servletRequest.setCharacterEncoding("utf-8");//增强
        filterChain.doFilter(servletRequest, servletResponse);
    }
}
```
```xml
<filter>
    <filter-name>oneFilter</filter-name>
    <filter-class>com.bjpowernode.filter.OneFilter</filter-class>
</filter>

<filter-mapping>
    <filter-name>oneFilter</filter-name>
    <url-pattern>/*</url-pattern><!--通知tomcat在调用所有资源文件之前都需要调用OneFilter进行拦截-->
</filter-mapping>
```
