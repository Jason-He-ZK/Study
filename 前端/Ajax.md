## 概述

全局刷新和局部刷新

- 全局刷新： 整个浏览器被新的数据覆盖。

在网络中传输大量的数据。 浏览器需要加载，渲染页面。
- 局部刷新： 在浏览器器的内部，发起请求，获取数据，改变页面中的部分内容。其余的页面无需加载和渲染。

网络中数据传输量少， 给用户的感受好。

```
（Asynchronous JavaScript and XML）
（异步的 JavaScript 和 XML）

Asynchronous: 异步的意思
JavaScript：javascript脚本，在浏览器中执行   
and : 和
xml : 是一种数据格式
```

ajax

- 用来做局部刷新的，是一种局部刷新的方法
- 局部刷新使用的核心对象是异步对象（XMLHttpRequest）
- 异步对象可以代替浏览器发送请求，可以有多个异步对象
- 异步对象是存在浏览器内存中的 ，使用 javascript 语法创建和使用 XMLHttpRequest 对象。
- javascript：负责创建异步对象， 发送请求， 更新页面的 dom 对象。 ajax 请求需要服务器端的数据

xml： 网络中的传输的数据格式。 使用 json 替换了 xml

Json

- 将对象转换为 json 格式的字符串，从而实现多组信息的传递

## XMLHttpRequest 对象

#### 属性

readyState 属性：表示异步对象请求的状态变化

```javascript
0：创建异步对象时，   new XMLHttpRequest();
1：初始异步请求对象， xmlHttp.open()
2：发送请求，        xmlHttp.send()
3：从服务器端获取了数据，（注意3是异步对象内部使用， 获取了原始的数据，一般不用）
4：异步对象把接收的数据处理完成后，（开发人员在4的时候处理数据（更新当前页面））
```

status 属性：表示网络请求的状况

- 200， 404， 500， 需要是当 status==200 时，表示网络请求是成功的

responseText 属性：表示服务器端返回的数据

#### 使用方法

1. 创建异步对象```javascript
 var xmlHttp = new XMLHttpRequest();
```


2. 给异步对象绑定事件（onreadystatechange）

当异步对象发起请求，获取了数据都会触发这个事件，这个事件需要指定一个函数， 在函数中处理状态的变化。

回调：当请求的状态变化时，异步对象会自动调用 onreadystatechange 事件对应的函数```javascript
例如
xmlHttp.onreadystatechange= function(){
   // 处理请求的状态变化
   if(xmlHttp.readyState == 4 && xmlHttp.status== 200 ){
      // 可以处理服务器端的数据，更新当前页面
      /* servlet中代码，输出的数据
        PrintWriter pw = response.getWriter();
        pw.println(msg);
      */
      var data = xmlHttp.responseText;
      document.getElementById("name").value= data;
   }
}
```


3. 初始异步请求对象（ open() ）```javascript
xmlHttp.open(请求方式（get|post）, "服务器端的访问地址", 同步|异步请求（默认是true，异步请求）)
// true :异步处理请求。 使用异步对象发起请求后，不用等待数据处理完毕，就可以执行其它的操作。可多个同时进行，效率高
// false:同步，异步对象必须处理完成请求，从服务器端获取数据后，才能执行send之后的代码。 任意时刻只能执行一个异步请求。

xmlHttp.open("get", "loginServlet?name=zs&pwd=123",true);
```


4. 使用异步对象发送请求```javascript
xmlHttp.send();
```


