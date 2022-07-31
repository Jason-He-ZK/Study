## 概述

一种脚本语言（脚本语言：“目标程序（能够直接执行的程序）”以普通文本形式保存），简称 JS

JSP : JavaServer Pages（隶属于 Java 语言的，运行在 JVM 当中）

运行在浏览器的内存当中

主要用来操作 HTML 中的节点，产生动态效果，让浏览器更加的生动了，不再是单纯的静态页面了。页面更具有交互性。

不需要我们程序员手动编译，编写完源代码之后，浏览器直接打开解释执行

JS 是一种基于事件驱动型的编程语言，当触发某个事件之后，执行一段代码

要求会使用 F12：查看器，控制台，网络。（会调错，会定位并查看 HTML 页面元素）

## 嵌入 JS 三种方式

```html
<body>
	<input type="button" value="hello1"  onclick="window.alert('hello world')"/>
</body>
```

```html
<script type="text/javascript">
alert("先加载我");
 </script>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<!-- 先加载这个按钮 -->
		<input type="button"  value="按钮1" />
		<!-- 脚本块 -->
		<script type="text/javascript">
			alert("123");
			alert("456");
		</script>
		<!-- 最后加载这个按钮 -->
		<input type="button" name="" id="" value="按钮2" />
	</body>
</html>
```

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<!-- 引入外部的js文件 -->
		<script src="第三种方式.js" type="text/javascript" charset="utf-8">
			//alert("111")写在这里的代码不会被执行
		</script>
		<!-- 单独的脚本块   这里的可以执行-->
		<script type="text/javascript">
			alert("222")
		</script>
	</body>
</html>
```

## ECMAScript，DOM、BOM

ECMAScript：核心语法

DOM：Document Object Model（文档对象模型）

顶级对象是 document

对网页当中的节点进行增删改的过程，HTML 文档被当做一棵 DOM 树看待

DOM 是有规范的，DOM 规范是 W3C 制定的。

BOM：Browser Object Model（浏览器对象模型）

顶级对象是 window，代表当前浏览器窗口

（window 对象有一个 alert()函数，该函数可以在浏览器上弹出消息框。window.可以省略）

包括 DOM（window.document）

是对浏览器本身操作，例如：前进、后退、地址栏、关闭窗口、弹窗等。由于浏览器有不同的厂家制造，所以 BOM 缺少规范，一般只是有一个默认的行业规范。

## ECMAScript

### 杂项

标识符：命名规则和规范按照 java 执行

语句结束后可以使用  ;  结尾，也可以不写

注释同 java

==(等同运算符：只判断值是否相等)

===(全等运算符：既判断值是否相等，又判断数据类型是否相等)

void 运算符：防止页面跳转

```html
<a href="javascript:void(任意表达式)" onclick="window.alert("xxx")">
  javascript:作用是告诉浏览器后面是一段JS代码
  保留住超链接的样式，点击超链接执行一段JS代码，但页面还不能跳转。
</a>

<a href="javascript:void(100)" onclick="window.alert('test code')">
  保留住超链接的样式，点击超链接执行一段JS代码，但页面还不能跳转。
</a>
```

### 变量

声明与赋值

```javascript
var a, b;
// 变量如未赋值，系统默认赋值undefined

// 弱类型编程语言，一个变量可以接收任何类型的数据
a = 1;
a = 1.11;
a = "asdf";
a = true;
```

局部变量和全局变量

全局变量：在函数体外声明的变量、不使用 var 声明的变量，浏览器打开时声明，浏览器关闭时销毁

局部变量：函数的形参、使用 var 在函数体中声明的变量，函数执行结束之后，局部变量的内存就释放了

### 函数

一段可以完成某个功能的可以被重复利用的代码片段（等同于 java 语言中的方法）

回调函数：自己把函数代码写出来，但是这个函数是由其他程序负责调用

定义函数的两种语法

1. 普通函数定义

```javascript
function 函数名(形式参数列表){
    函数体;
}

/*
  调用时，实参可以随意
  sum()，a和b都是undefined，
  sum(1)，a是1，b是undefined，
  sum(1,2)，a是1，b是2
  sum(1,2,3)，a是1，b是2，c被忽略了
  
  同名函数会被后面的覆盖
 */
function sum(a, b){
    return a + b;
}
```

```javascript
函数名 = function(形式参数列表){
    函数体;
}

sum = function(a, b){
    return a + b;
}
```

JS 中的函数定义在脚本块中，页面在打开的时候，函数并不会自动执行，函数是需要手动调用才能执行的。

由于 JS 是一种弱类型编程语言，所以函数不能同名，没有重载机制

这样的代码顺序是可以的，页面打开的时候会先进行所有函数的声明，函数声明优先级较高。

```javascript
<script type="text/javascript">
    sayHello();
    function sayHello(){
        alert("Hello JS");
    }
</script>
```

用户点击按钮，调用函数

```javascript
<script type="text/javascript">
    function sayHello(){
        alert("hello js");
    }
</script>
<input type="button" value="hello" onclick="sayHello();"/>
```

### 数据类型

数据类型有 6 种（ES6 版本之前）（ES6 版本及之后，有 7 种）

原始类型：Undefined、Number、String、Boolean、Null

引用类型：Object 以及 Object 的子类

parseFloat()函数  ：字符串 → 浮点数

```
Math.ceil()函数：向上取整

    isNaN()函数（is not a number）

  1. String
```

```javascript
// String 小
var s = "asdf";
var ss = 'asdf';
// Object 大
var sss = new String("abc");
var ssss = new String('abc');
```

```
可以使用单引号，也可以用双引号

    JS中的字符串包括小String（原始类型），大String（Object类型，是JS的内置对象）

    大小String的属性和方法都是通用的

    比较字符串用  ==
```

```javascript
常用属性：
  length 获取字符串长度
常用函数：
  indexOf        获取指定字符串在当前字符串中第一次出现处的索引
  lastIndexOf    获取指定字符串在当前字符串中最后一次出现处的索引
  replace        替换
  substr（2，5）       截取子字符串，从下标2开始，截取5个
  substring（2，5）    截取子字符串，从下标2开始，截取到下标4（5-1）
  toLowerCase    转换小写
  toUpperCase    转换大写
  split          拆分字符串
```

```javascript
// 第一种方式
// JS中的一个函数，既是函数声明，又是类的定义，同时函数名也可以看做构造方法名。
function 类名(形参){
}

// 第二种方式
类名 = function(形参){
}
Product = function(pno,pname,price){
     // 属性
     this.pno = pno;
     this.pname = pname;
     this.price = price;
     // 函数
     this.getPrice = function(){
       return this.price;
     }
}
```

```
创建对象的语法
```

```javascript
new 构造方法名(实参); // 构造方法名和类名一致。
```

```
JS中如何访问对象属性，调用对象的方法。
```

```javascript
// 访问对象的属性
var u1 = new User(111, "zhangsan", 30);
// 方法一
alert(u1.sno);
// 方法二
alert(u1["sno"]);

// 调用方法
var xigua = new Product(111, "西瓜", 4.0);
var pri = xigua.getPrice();
```

```
使用prototype属性动态地给对象扩展属性以及方法
```

```javascript
Product.prototype.getPname = function(){
     return this.pname;
}
```

```javascript
// 作用：运行时动态获取数据类型
// 格式：typeof 变量名
function sum(a, b){
  if("number" === typeof a && "number" === typeof b){
    return a + b;
  }
  alert("数据格式不合法");
  return 0;
}

// 运算结果都是全部小写的字符串，6种
"undefined"
"number"
"string"
"boolean"
"object"
"function"
```

null，NaN，undefined 区别

```javascript
// null NaN undefined 数据类型不一致.
alert(typeof null); // "object"
alert(typeof NaN); // "number"
alert(typeof undefined); // "undefined"

// null和undefined可以等同.
alert(null == NaN); // false
alert(null == undefined); // true
alert(undefined == NaN); // false
```

```
  （2）null NaN undefined三者类型不同，null和undefined的值可以等同
```

### 事件

```javascript
<script>
  window.onload = sayHello;
</script>

<script>
  window.onload = function(){
  }
</script>
```

JS 中的任何一个事件都对应一个事件句柄

on + 事件 = 事件句柄（例如 click，onclick）

事件句柄都是以标签的属性方式存在。在事件句柄后面可以编写 JS 代码，当触发这个事件之后，这段 JS 代码则执行了。

常用事件

```
blur       失去焦点
focus      获得焦点

click      鼠标单击
dblclick   鼠标双击

keydown    键盘按下
keyup      键盘弹起

mousedown  鼠标按下
mouseup    鼠标弹起

mouseover  鼠标经过
mousemove  鼠标移动
mouseout   鼠标离开

submit     表单提交
reset      表单重置

change     下拉列表选中项改变，或文本框内容改变

select     文本被选定

load       页面加载完毕
```

注册事件的两种方式

1. 在标签中使用事件句柄的方式注册事件

```html
<body onload="sayHello()"></body>
<input type="button" value="hello" onclick="sayHello()"/>
```

1. 在页面加载完毕后使用 JS 代码给元素绑定事件

```javascript
<input type="butten" value="aaa" id="mybtn"/>

<script type="text/javascript">
  function dosome(){
    alert("dosome");
  }
  
  // 第一步，获取对象
  var btnObj = document.getElementById("mybtn"); 
  
  // 第二步，给对象的属性赋值
  // btnObj.onclick = dosome(); 错误，不能加小括号
  btnObj.onclick = dosome;
  // 第二步的另一种方法，匿名函数
  btnObj.onclick = function(){
    alert("dosome");
  }
  
  // 整合第一步和第二步
  document.getElementById("mybtn").onclick = function(){
    alert("dosome");
  }
  
 </script>
```

代码的执行顺序

```html
这是一种错误的写法：（要先有对应Id的元素才能通过getElementById获取）
<script type="text/javascript">
  var elt = document.getElementById("btn");
</script>

<input type="button" id="btn" value="mybtn"/>


这样写：
<input type="button" id="btn" value="mybtn"/>

<script type="text/javascript">
  var elt = document.getElementById("btn");
</script>


或者这样写：（通过onload，等所有元素加载完再获取）（常用）
<script type="text/javascript">
  window.onload = function(){
    var elt = document.getElementById("btn");
  }
</script>

<input type="button" id="btn" value="mybtn"/>
```

keydown（回车提交数据）

```javascript
<script type="text/javascript">
  window.onload = function(){
    var usernameElt = document.getElementById("username")；
    usernameElt.onkeydowm = function(event){
      // 判断键值
      // 键盘事件有keycode属性，回车键13，ESC键27
      if(event.keycode === 13){
      }
    }
  }
</script>

<input type="text" id="username" />
```

### 控制语句

for  in（了解）

```javascript
var arr = [false,true,1,2,"abc",3.14]; 
for(var i in arr){
     alert(arr[i]);
}

var u = new User("zhangsan", "123");
for(var shuXingMingin u){ 
  // shuXingMing为字符串，原理为：alert(u1["sno"]);
  alert(u[shuXingMing]);
}
```

遍历数组

遍历一个对象的属性

with（容易混乱）（了解）

```javascript
alert(u.username);
alert(u.password);

with(u){
  alert(username);
  alert(password);
}
```

### 内置对象

Array

创建数组

```javascript
// JS中数组中元素的类型随意.元素的个数随意.
var arr = [false, 1, "asdf"];

var arr1 = new Array(); // 默认数组长度为0
var arr2 = new Array(4); // 数组长度为4
var arr3 = new Array(4,5,6); // 4 5 6 三个值
```

特点

随意扩容（没有越界的概念）

默认 undefined

常用方法（了解）

```javascript
 // push：在数组的末尾添加一个元素(数组长度+1)
 a.push(10);
 
 // pop：将数组末尾的元素弹出(数组长度-1)
 var endElt = a.pop();
 alert(endElt);
 
 // join：连接数组变成一个字符串
var a = [1,2,3,9];
var str = a.join("-");
alert(str); // "1-2-3-9"

// reverse：反转数组
a.reverse();
```

遍历

```javascript
for(var i = 0; i < arr.length; i++){
  alert(arr[i]);
} 

for(var i in arr){ //i为下标，Java中是元素
  alert(arr[i]);
}
```

Date

```javascript
<script type="text/javascript">
  // 获取系统当前时间
  var nowTime = new Date();
  // 转换成具有本地语言环境的日期格式.
  nowTime = nowTime.toLocaleString();
  document.write(nowTime);
  
  // 当以上格式不是自己想要的,可以通过日期获取年月日等信息,自定制日期格式.
  var t = new Date();
  var year = t.getFullYear(); // getFullYear返回2021，getYear返回21
  var month = t.getMonth(); // 返回0-11
  // var dayOfWeek = t.getDay(); // 获取的一周的第几天(0-6)
  var day = t.getDate(); // 获取日信息.
  document.write(year + "年" + (month+1) + "月" + day + "日");
  
  // 获取毫秒数(从1970年1月1日 00:00:00 000到当前系统时间的总毫秒数)（一般会使用毫秒数当做时间戳）
  // var times = t.getTime();
  document.write(new Date().getTime());
</script>
```

## 正则表达式(Regular Expression)

是一门独立的学科，不仅仅用在 JS 中

主要用来做字符串格式匹配

要求

认识，会写简单的正则，看懂他人写的正则

会创建对象，会使用对象方法

会抄会改

常用的正则表达式符号

```
.  匹配除换行符以外的任意字符 
\w 匹配字母或数字或下划线或汉字
\s 匹配任意的空白符
\d 匹配数字
\b 匹配单词的开始或结束

^  匹配字符串的开始
$  匹配字符串的结束
-------------------------------------------
* 重复零次或更多次 
+ 重复一次或更多次
? 重复零次或一次 

{n}   重复n次 
{n,}  重复n次或更多次 
{n,m} 重复n到m次
-------------------------------------------
\W 匹配任意不是字母，数字，下划线，汉字的字符 
\S 匹配任意不是空白符的字符 
\D 匹配任意非数字的字符 
\B 匹配不是单词开头或结束的位置 

[^x] 匹配除了x以外的任意字符 
-------------------------------------------
正则表达式当中的小括号()优先级较高。

[^aeiou] 匹配除了aeiou这几个字母以外的任意字符 
[1-9] 表示1到9的任意1个数字（次数是1次。）
[A-Za-z0-9] 表示A-Z、a-z、0-9中的任意1个字符
[A-Za-z0-9-] 表示A-Z、a-z、0-9、- 中以上所有字符中的任意1个字符。


| 表示或者
```

简单常用的正则表达式

```
QQ号的正则表达式：^[1-9][0-9]{4,}$

email正则：^\w+([-+.]\w+)*@\w+([-.]\w+)*.\w+([-.]\w+)*$
```

创建正则表达式对象

```javascript
/* 
  flags（几个字母或其组合）
    g：全局匹配
    i：忽略大小写
    m：多行搜索（ES规范制定之后才支持m）
    当前面是正则表达式的时候，m不能用
    只有前面是普通字符串的时候，m才可以使用（用来精确查找字符串）
 */

// 方法一
var regExp = /正则表达式/flags;

// 方法二：使用内置支持类RegExp
var regExp = new RegExp("正则表达式","flags");
```

正则对象常用方法

判断是否匹配：test()

```javascript
// 实例：邮箱验证

window.onload = function(){
 document.getElementById("btn").onclick = function(){
   var email = document.getElementById("email").value;
   var emailRegExp = /^\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*$/;
   
   // true匹配成功，false匹配失败
   var ok = emailRegExp.test(email);
   if(ok){
    document.getElementById("emailError").innerText = "邮箱地址合法";
   }else{
     document.getElementById("emailError").innerText = "邮箱地址不合法";
   }
 }
 
 // 给文本框绑定focus，获得焦点后清空提示信息
 document.getElementById("email").onfocus = function(){
   document.getElementById("emailError").innerText = "";
 }
}
</script>

<input type="text" id="email" />
<span id="emailError" style="color: red; font-size: 12px;"></span>
<br>
<input type="button" value="验证邮箱" id="btn" />
```

去除前后空格：trim()

```javascript
<script type="text/javascript">
  window.onload = function(){
    document.getElementById("btn").onclick = function(){
      var username = document.getElementById("username").value;
  
      // 去除前后空白
      username = username.trim();
      // 测试
      alert("--->" + username + "<----");
    }
  }
</script>
<input type="text" id="username" />
<input type="button" value="获取用户名" id="btn" />
```

## DOM 编程案例

### 操作文本框的 value

```javascript
window.onload = function(){
    var btnElt = document.getElementById("btn");
    btnElt.onclick = function(){

     // 获取username节点的value
     alert(document.getElementById("username").value);
   
     // 修改username节点的value
     document.getElementById("username").value = "zhangsan";
    }
}
```

### 操作 div 和 span 的 innerHTML 和 innerText

```
innerHTML：内容当做HTML代码解释执行

innerText：内容仅当做文本显示
```

### 表单验证实例

用户名不能为空，要在 6-14 位之间，只能由数字和字母组成，不能含有其它符号（正则表达式）

密码和确认密码一致

邮箱地址合法

统一失去焦点验证

错误提示信息统一在 span 标签中提示，并且要求字体 12 号，红色

文本框再次获得焦点后，清空错误提示信息，如果文本框中数据不合法要求清空文本框的 value

最终表单中所有项均合法方可提交

```html
<head>
  <meta charset="utf-8">
  <title>表单验证</title>
  <style type="text/css">
    span {
      color: red;
      font-size: 12px;
    }
  </style>
</head>
<body>
  <script type="text/javascript">
    // js代码，下一个代码块
  </script>
  
  <!--这个表单提交应该使用post，这里为了检测，所以使用get。-->
  <!-- <form id="userForm" action="http://localhost:8080/jd/save" method="get"> -->
  <form id="userForm" method="get">
    用户名<input type="text" name="username" id="username"/><span id="usernameError"></span><br>
    密码<input type="text" name="userpwd" id="userpwd"/><br>
    确认密码<input type="text" id="userpwd2" /><span id="pwdError"></span><br>
    邮箱<input type="text" name="email" id="email" /><span id="emailError"></span><br>
    <!-- <input type="submit" value="注册" /> -->
    <input type="button" value="注册" id="submitBtn"/>
    <input type="reset" value="重置" />
  </form>
  
</body>
```

```javascript
<script type="text/javascript">
   window.onload = function(){
     var usernameErrorSpan = document.getElementById("usernameError");
     var usernameElt = document.getElementById("username");

     // 给用户名文本框绑定blur事件
     usernameElt.onblur = function(){
       // 获取用户名
       var username = usernameElt.value;
       // 去除前后空白
       username = username.trim();
       // 判断用户名是否为空
       /*
       方法1
       if(username){
       }else{
       }
       方法2
       if(username.length == 0){}
       */
       // 方法3
       if(username === ""){
         // 用户名为空
         usernameErrorSpan.innerText = "用户名不能为空";
       }else{
         // 用户名不为空
         // 继续判断长度[6-14]
         if(username.length < 6 || username.length > 14){
           // 用户名长度非法
           usernameErrorSpan.innerText = "用户名长度必须在[6-14]之间";
         }else{
           // 用户名长度合法
           // 继续判断是否含有特殊符号
           var regExp = /^[A-Za-z0-9]+$/;
           var ok = regExp.test(username);
           if(ok){
             // 用户名最终合法
           }else{
             // 用户名中含有特殊符号
             usernameErrorSpan.innerText = "用户名只能由数字和字母组成";
           }
         }
       }
     }
   
     // 给username这个文本框绑定focus事件
     usernameElt.onfocus = function(){
       // 清空非法的value
       if(usernameErrorSpan.innerText != ""){
         usernameElt.value = "";   
       }
       // 清空span
       usernameErrorSpan.innerText = "";
     }
   
   
   
     var pwdErrorSpan = document.getElementById("pwdError");
     var userpwdElt = document.getElementById("userpwd");
     var userpwd2Elt = document.getElementById("userpwd2");
   
     // 绑定blur事件
     userpwd2Elt.onblur = function(){
       // 获取密码和确认密码
       var userpwd = userpwdElt.value;
       var userpwd2 = userpwd2Elt.value;
       if(userpwd != userpwd2){
         // 密码不一致
         pwdErrorSpan.innerText = "密码不一致";
       }else{
         // 密码一致
       }
     }
   
     // 绑定focus事件
     userpwd2Elt.onfocus = function(){
       if(pwdErrorSpan.innerText != ""){
         userpwd2Elt.value = "";
       }
       pwdErrorSpan.innerText = "";
     }
   
   

     var emailSpan = document.getElementById("emailError");
     var emailElt = document.getElementById("email");
   
     // 给emailElt绑定blur
     emailElt.onblur = function(){
       // 获取email
       var email = emailElt.value;
       // 编写email的正则
       var emailRegExp = /^\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*$/;
       var ok = emailRegExp.test(email);
       if(ok){
         // 合法
       }else{
         // 不合法
         emailSpan.innerText = "邮箱地址不合法";
       }
     }
   
     // 给emailElt绑定focus
     emailElt.onfocus = function(){
       if(emailSpan.innerText != ""){
         emailElt.value = "";
       }
       emailSpan.innerText = "";
     }
   
   
   
     var submitBtnElt = document.getElementById("submitBtn");
   
     // 给提交按钮绑定鼠标单击事件
     submitBtn.onclick = function(){
       // 触发各元素的blur，不需要人工操作,使用纯JS代码触发事件.
       usernameElt.focus();
       usernameElt.blur();
       userpwd2Elt.focus();
       userpwd2Elt.blur();
       emailElt.focus();
       emailElt.blur();
   
       // 当所有表单项都是合法的时候,提交表单
       if(usernameErrorSpan.innerText == "" && pwdErrorSpan.innerText == "" && emailSpan.innerText == ""){
         // 获取表单对象
         var userFormElt = document.getElementById("userForm");
         // 可以在这里设置action,也可以不在这里.
         userFormElt.action = "http://localhost:8080/jd/save";
         // 提交表单
         userFormElt.submit();
       }
     }
   }
</script>
```

### 复选框全选和取消全选

```
document.getElementById()

document.getElementsByName() 获取一组元素

document.getElementsByTagName()
```

```javascript
<script type="text/javascript">
  /*
  window.onload = function(){
    var firstChk = document.getElementById("firstChk");
    firstChk.onclick = function(){
      // 根据name获取所有元素
      var aihaos = document.getElementsByName("aihao");
      // 判断所有元素的选中状态
      if(firstChk.checked){
        // 全选
        for(var i = 0; i < aihaos.length; i++){
          aihaos[i].checked = true;
        }
      }else{
        // 取消全选
        for(var i = 0; i < aihaos.length; i++){
          aihaos[i].checked = false;
        }
      }
    }
  }
  */
   
  window.onload = function(){
    // 上面代码优化后
    var aihaos = document.getElementsByName("aihao");
    var firstChk = document.getElementById("firstChk");
    firstChk.onclick = function(){
      for(var i = 0; i < aihaos.length; i++){
        aihaos[i].checked = firstChk.checked;
      }
    }
  
    // 对数组进行遍历
    var all = aihaos.length;
    for(var i = 0; i < aihaos.length; i++){
      aihaos[i].onclick = function(){
        var checkedCount = 0;
        // 总数量和选中的数量相等的时候,第一个复选框选中.
        for(var i = 0; i < aihaos.length; i++){
          if(aihaos[i].checked){
            checkedCount++;
          }
        }
  
        /*
        if(all == checkedCount){
          firstChk.checked = true;
        }else{
          firstChk.checked = false;
        }
        */
        // 上面代码优化
        firstChk.checked = (all == checkedCount);
      }
    }
  }
</script>
<input type="checkbox" id="firstChk"/><Br>
<input type="checkbox" name="aihao" value="smoke" />抽烟<Br>
<input type="checkbox" name="aihao" value="drink" />喝酒<Br>
<input type="checkbox" name="aihao" value="tt" />烫头<Br>
```

### 获取下拉列表选中项的 value

change 事件

```javascript
<script type="text/javascript">
  window.onload = function(){
    var provinceListElt = document.getElementById("provinceList");
    provinceListElt.onchange = function(){
      // 获取选中项的value
      alert(provinceListElt.value);
    }
  }
</script>

<select id="provinceList">
  <option value="">--请选择省份--</option>
  <option value="001">河北省</option>
  <option value="002">河南省</option>
  <option value="003">山东省</option>
  <option value="004">山西省</option>
</select>
```

### 显示网页时钟

```javascript
<script type="text/javascript">
  function displayTime(){
    var time = new Date();
    var strTime = time.toLocaleString();
    document.getElementById("timeDiv").innerHTML = strTime;
  }
  
  // 每隔1秒调用displayTime()函数
  function start(){
    // 从这行代码执行结束开始,则会不间断的,每隔1000毫秒调用一次displayTime()函数.
    v = window.setInterval("displayTime()", 1000);  
  }
  
  function stop(){
    window.clearInterval(v);
  }
</script>
<br><br>
<input type="button" value="显示系统时间" onclick="start();"/>
<input type="button" value="系统时间停止" onclick="stop();" />
<div id="timeDiv"></div>
```

### 拼接 html 的方式，设置 table 的 tbody

```javascript
<script type="text/javascript">
  // 有这些json数据
  var data = {
    "total" : 4,
    "emps" : [
      {"empno":7369,"ename":"SMITH","sal":800.0},
      {"empno":7361,"ename":"SMITH2","sal":1800.0},
      {"empno":7360,"ename":"SMITH3","sal":2800.0},
      {"empno":7362,"ename":"SMITH4","sal":3800.0}
    ]
  };
  
  // 希望把数据展示到table当中.
  window.onload = function(){
    var displayBtnElt = document.getElementById("displayBtn");
    displayBtnElt.onclick = function(){
      var emps = data.emps;
      var html = "";
      for(var i = 0; i < emps.length; i++){
        var emp = emps[i];
        html += "<tr>";
        html += "<td>"+emp.empno+"</td>";
        html += "<td>"+emp.ename+"</td>";
        html += "<td>"+emp.sal+"</td>";
        html += "</tr>";
      }
      document.getElementById("emptbody").innerHTML = html;
      document.getElementById("count").innerHTML = data.total;
    }
  }
</script>

<input type="button" value="显示员工信息列表" id="displayBtn" />
<table border="1px" width="50%">
  <tr>
    <th>员工编号</th>
    <th>员工名字</th>
    <th>员工薪资</th>
  </tr>
  <tbody id="emptbody">
    <!--
    <tr>
      <td>7369</td>
      <td>SMITH</td>
      <td>800</td>
    </tr>
    <tr>
      <td>7369</td>
      <td>SMITH</td>
      <td>800</td>
    </tr>
    <tr>
      <td>7369</td>
      <td>SMITH</td>
      <td>800</td>
    </tr>
    -->
  </tbody>
</table>
总共<span id="count">0</span>条数
```

## BOM 编程案例

#### 窗口打开、关闭

```
window.open()，window.close()
```

```html
<input type="button" value="开启百度(新窗口)" onclick="window.open('http://www.baidu.com');" />
<input type="button" value="开启百度(当前窗口)" onclick="window.open('http://www.baidu.com', '_self');" />
<input type="button" value="开启百度(新窗口)" onclick="window.open('http://www.baidu.com', '_blank');" />
<input type="button" value="开启百度(父窗口)" onclick="window.open('http://www.baidu.com', '_parent');" />
<input type="button" value="开启百度(顶级窗口)" onclick="window.open('http://www.baidu.com', '_top');" />

<input type="button" value="关闭当前窗口" onclick="window.close();" /> 

<input type="button" value="打开002-open.html"  onclick="window.open('002-open.html')"/>
```

#### 消息框、确认框

```
window.alert()，window.confirm()
```

```javascript
<script type="text/javascript">
  function del(){
    /*
    var ok = window.confirm("亲，确认删除数据吗？");
    if(ok){
      alert("delete data ....");
    }
    */
    if(window.confirm("亲，确认删除数据吗？")){
      alert("delete data ....");
    }
  }
</script>

<input type="button" value="弹出消息框" onclick="window.alert('消息框!')" />


<input type="button" value="弹出确认框(删除)" onclick="del();" />
```

#### 当前窗口设为顶级窗口

```javascript
if(window.top != window.self){
  window.top.location = window.self.location;
}
```

#### 历史记录（前进，后退）

```javascript
window.history.back(); 
window.history.go(-1); 

window.history.go(1);
```

#### 地址栏

```
window.location.href或document.location.href

href都可以省略
```

```javascript
<script type="text/javascript">
  function goBaidu(){
    // window.location.href = "http://www.jd.com";
    // window.location = "http://www.126.com";
  
    // document.location.href = "http://www.sina.com.cn";
    // document.location = "http://www.tmall.com";
  }
</script>

<input type="button" value="百度" onclick="goBaidu();"/>
```

## JSON 对象

JSON：JavaScript Object Notation（JavaScript 对象标记）（是一种数据交换格式）

作用：数据交换（目前非常流行，90% 以上的系统交换数据的话，都是采用 JSON）

JSON 特点

```
轻量级的数据交换格式

体积小，易解析
```

XML 特点

```
体积较大，解析麻烦

优点：语法严谨（银行）
```

创建及访问对象（json 也被称为无类型对象）

```javascript
//格式
var jsonObj = {
  "属性名" : 属性值,
  "属性名" : 属性值,
  ....
};

var studentObj = {
  "sno" : "110",
  "sname" : "张三",
  "sex" : "男"
};


alert(studentObj.sno);

alert(studentObj["sno"]);
```

数组及遍历

```javascript
// JSON数组
var students = [
  {"sno":"110","sname":"张三","sex":"男"},
  {"sno":"120","sname":"李四","sex":"男"},
  {"sno":"130","sname":"王五","sex":"男"}
];

// 遍历
for(var i = 0; i < students.length; i++){
  var stuObj = students[i];
  alert(stuObj.sno + "," + stuObj.sname + "," + stuObj.sex);
}
```

嵌套

```javascript
var user = {
  "usercode" : 110,
  "username" : "张三",
  "sex" : true,
  "address" : {
    "city" : "北京",
    "street" : "大兴区",
    "zipcode" : "12212121",
  },
  "aihao" : ["smoke","drink","tt"]
};

// 访问人名以及居住的城市
alert(user.username + "居住在" + user.address.city);
```

写一个这样的 JSON，描述一个班级的总人数，另外包括描述班级中每个学生的信息

```javascript
var jsonData = {
  "total" : 3,
  "students" : [
    {"name":"zhangsan","birth":"1980-10-20"},
    {"name":"lisi","birth":"1981-10-20"},
    {"name":"wangwu","birth":"1982-10-20"}
  ]
};
```

eval 函数：将字符串当做一段 JS 代码解释并执行

```javascript
window.eval("var i = 100;");
alert("i = " + i); // i = 100

// java连接数据库,查询数据之后,将数据在java程序中拼接成JSON格式的“字符串”,可以使用eval函数,将json格式的字符串转换成json对象. 

//这是java程序给发过来的json格式的"字符串"
var fromJava = "{\"name\":\"zhangsan\",\"password\":\"123\"}"; // \转义
// 将以上的json格式的字符串转换成json对象
window.eval("var jsonObj = " + fromJava);
// 访问json对象
alert(jsonObj.name + "," + jsonObj.password); // 在前端取数据.
```

JS 中[]和{}的区别

```
[] 是数组，{} 是JSON

  JS中的数组：var arr = [1,2,3,4,5];

  JSON：var jsonObj = {"email" : "[zhangsan@123.com](mailto:zhangsan@123.com)","age":25};
```

java 中的数组：int[] arr = {1,2,3,4,5};

工具库： gson（google）； fastjson（阿里），jackson， json-lib

```javascript
String sjson = {}; // 保证一定是json格式

// 使用jackson的writeValueAsString方法，把java对象p转为json格式的字符串  
ObjectMapper om = new ObjectMapper();  
sjson = om.writeValueAsString(p);

// 转换回来（有更新的方法）
var json = eval("(" + sjson + ")")
```

## 总结浏览器向服务器发送请求的常见方式

（动态）表单 form 的提交

（动态）直接在浏览器地址栏上输入 URL，然后回车（这个也可以手动输入，提交数据也可以成为动态的。）

超链接

document.location 和 window.location

window.open("url")
