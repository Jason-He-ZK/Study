## 概述

jsp 规范：制定了如何开发 JSP 文件代替响应对象将处理结果写入到响应体的开发流程，规定了 Http 服务器应该如何调用管理 JSP 文件

响应对象与 JSP 文件

- 响应对象缺点：仅适合将数据量较少的处理结果写入到响应体
- JSP 文件优点：在 JSP 文件开发时，可以直接将处理结果写入到 JSP 文件，在 Http 服务器调用 JSP 文件时，根据 JSP 规范要求自动的将 JSP 文件书写的所有内容通过输出流写入到响应体

Servlet 与 JSP

- 分工：

   - Servlet：负责处理业务并得到【处理结果】
   - JSP：将 Servlet 中【处理结果】写入到响应体
- 调用关系：Servlet 工作完毕后，一般通过请求转发方式 向 Tomcat 申请调用 JSP
- 数据共享：

   - Servlet 将处理结果添加到【请求作用域对象】
   - JSP 文件在运行时从【请求作用域对象】得到处理结果

HTML 文件与 JSP 文件区别

- 资源文件类型不同

   - HTML 文件属于静态资源文件，JSP 文件属于动态资源文件
- 调用形式不同

   - 浏览器访问 HTML 文件，Http 服务器直接通过一个输出流将 HTML 文件中所有的内容写入到响应体
   - 浏览器访问 JSP 文件，Http 服务器根据 JSP 规范来操作 JSP 文件编辑----> 编译-----> 调用

Http 服务器【编辑】与【编译】JSP 文件位置：

- 标准答案：我在【work】下看到这个证据

C:\Users[登录 windows 系统用户角色名].IntelliJIdea2018.3\system\tomcat[网站工作空间]\work\Catalina\localhost\【网站别名】\org\apache\jsp

## JSP 中的 Java 命令

执行标记：通知 Http 服务器将 JSP 文件中 Java 命令与其他普通执行结果进行区分

输出标记：通知 Tomcat 将输出标记中【变量的值】或则输出标记中【表达式运算结果】写入到响应体

```javascript
<!--导包-->
<%@ page import="com.bjpowernode.entity.Student" %>
<%@ page import="java.util.ArrayList" %>

<%
  //在jsp文件中，只有书写在执行标记中内容才会被当做Java命令
   int num1 = 100;
   int num2 = 200;
%>

<!--在JSP文件，通过输出标记，通知JSP将Java变量的值写入到响应体-->
变量num1的值:<%=num1%><br/>

<!--执行标记还可以通知Jsp将运算结果写入到响应体-->
num1 + num2 = <%=num1+num2%>
```

## JSP 内置对象

request（类型：HttpServletRequest 请求作用域对象）

- 作用 1：在 JSP 文件运行时读取请求包信息```javascript
<%
   //在JSP文件执行时，借助于内置request对象读取请求包参数信息
    String userName = request.getParameter("userName");
%>
```


- 作用 2：与 Servlet 在请求转发过程中实现数据共享

session（类型：HttpSession）

- 作用：JSP 文件在运行时，可以 session 指向当前用户私人储物柜，添加或读取共享数据```javascript
<%
   // HttpSession session = request.getSession();
   session.setAttribute("key1", 200);
%>

<%
   // 另一jsp文件（但是是同一个用户）
   Integer value = (Integer)session.getAttribute("key1");
%>
```



application（类型：ServletContext application 全局作用域对象）

- 同一网站的 Servlet 与 JSP，都可以通过当前网站的全局作用域对象实现数据共享```javascript
<%
    application.setAttribute("key1", "hello world");
%>
```



## 调用 JSP 文件步骤【2019 年北京地区常考面试题】

1. Http 服务器将 JSP 文件内容【编辑】为一个 Servlet **接口实现类** （.java）
2. Http 服务器将 Servlet 接口实现类【编译】为 **class 文件** (.class)
3. Http 服务器负责创建这个 class 的**实例对象** ，这个实例对象就是 Servlet 实例对象
4. Http 服务器通过 Servlet 实例对象**调用_jspService 方法** ，将 jsp 文件内容写入到响应体

## —EL 表达式

## 概述

是 EL 工具包提供一种特殊命令格式【表达式命令格式】

使用位置：EL 表达式在 JSP 文件上使用

作用：负责在 JSP 文件上从作用域对象读取指定的共享数据并输出到响应体

缺点：EL 表达式没有提供遍历集合方法，因此无法从作用域对象读取集合内容输出

常见异常：javax.el.PropertyNotFoundException:在对象中没有找到指定属性

## 使用方法

```
格式：
${作用域对象别名.共享数据}
${作用域对象别名.共享数据名.属性名}
```

```javascript
  ServletContext      全局作用域对象    application  ${applicationScope.共享数据名}
  HttpSession         会话作用域对象    session      ${sessionScope.共享数据名}
  HttpServletRequest  请求作用域对象    request      ${requestScope.共享数据名}
  PageContext         当前页作用域对象  pageContext  ${pageScope.共享数据名}
  
  PageContext
  这是JSP文件独有的作用域对象。Servlet中不存在
  当前页作用域对象存放的共享数据仅能在当前JSP文件中使用，不能共享给其他Servlet或则其他JSP文件
  主要用于JSTL标签与JSP文件之间数据共享数据（JSTL--->pageContext--->JSP）
```

## 使用方法简化版

格式：${共享数据名}

作用：开发时省略作用域对象别名

工作原理：（由于没有指定作用域对象，所以在执行时采用【猜】算法）

- 先到【pageContext】定位共享数据，如果存在直接读取输出并结束执行，如不存在

再到【request】定位共享数据，如果存在直接读取输出并结束执行，如不存在

再到【session】定位共享数据，如果存在直接读取输出并结束执行，如不存在

再到【application】定位共享数据，如果存在直接读取输出并结束执行，如不存在

返回 null
- pageContext--->request--->session--->application

存在隐患

- 容易降低程序执行速度
- 容易导致数据定位错误

应用场景

- 设计目的，就是简化从 pageContext 读取共享数据并输出难度

尽管存在很多隐患，但是在实际开发过程中，开发人员为了节省时间，一般都使用简化版

## 支持运算表达式（一般只做输出，不做复杂运算）

数学运算

关系运算


> > =   ==    <   <=   !=




gt    ge    eq     lt    le     !=

逻辑运算：  &&   ||    ！

## 内置对象

（实际开发不这么用，了解就行）

格式：${param.请求参数名}

作用： 通过请求对象读取当前请求包中【请求参数内容】，并将请求参数内容写入到【响应体】

```javascript
发送的请求：[Http://localhost:8080/myWeb/index.jsp?userName=mike&password=123]

userName：${param.userName}<br/>
password：${param.password}
```

格式：${paramValues.请求参数名[下标]}  （数组版 param）

作用：如果浏览器发送的请求参数是[一个请求参数关联多个值]，此时可以通过 paramVaues 读取请求参数下指定位置的值并写入到响应体

```javascript
[http://localhost:8080/myWeb/index_2.jsp?pageNo=1&pageNo=2&pageNo=3]
此时pageNo请求参数在请求包以数组形式存在，pageNo:[1,2,3]

第一个值:${paramValues.pageNo[0]}
第二个值:${paramValues.pageNo[1]}
第三个值:${paramValues.pageNo[2]}
```
