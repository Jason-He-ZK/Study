## 概述

jQuery 是 js 库，是存放 js 代码的地方，放的用 js 代码写的函数

优点：少写代码，免费开源轻量，兼容性好

dom 对象和 jquery 对象

- dom 对象（js 对象）：使用 javascript 的语法创建的对象
- jquery 对象： 使用 jquery 语法表示对象叫做 jquery 对象
（注意：jquery 表示的对象都是数组，可能只有一个值）
为了区分，建议以“$”开头命名
- dom 对象可以和 jquery 对象相互的转换 
   - 转换目的：使用对象的属性，或者方法
   - dom —> jquery ，语法： $(dom 对象)
   - jquery —> dom， 语法： 从数组中获取的对象就是 dom 对象， 使用 [0] 或者 .get{3)

简单实例

```javascript
// 引入.js文件
<script type="text/javascript" src="/html/js/jquery-3.4.1.js" ></script>
<script type="text/javascript">
    // ready：是jQuery中的函数，相当于js中的onLoad事件
    // function()自定义的表示onLoad后要执行的功能
    $(document).ready(function(){
      alert("Hello jQuery")
    })  
    // 上面方法的简写
    $(function(){
        alert("Hello JQuery 快捷方式")
    }
)
</script>
```

## 选择器

就是一个字符串， 用来定位 dom 对象的。定位了 dom 对象，就可以通过 jquery 的函数操作 dom

基本选择器（  $("*")  选中全部）（可组合，用 , 隔开）

- id 选择器， 语法： $("#id")
- class 选择器， 语法： $(".class 名")
class 表示 css 中的样式， 使用样式的名称定位 dom 对象的。
- 标签选择器， 语法： $("标签名")

表单选择器

```
使用<input>标签的type属性值，定位dom对象的方式（无论是否有form表单都可定位）
语法： $(":type属性值")
例如： $(":text") 选择的是所有的单行文本框，$(":button") 选择的是所有的按钮。
```

## 过滤器

在定位了 dom 对象后，根据一些条件筛选 dom 对象

过滤器不能单独使用， 必须和选择器一起使用。

基本过滤器（数组顺序与页面中声明顺序相同）

- $("选择器:first")：第一个 dom 对象
- $("选择器:last")：数组中的最后一个 dom 对象
- $("选择器:eq(数组的下标)")：获取指定下标的 dom 对象
- $("选择器:lt(下标)")：获取小于下标的所有 dom 对象
- $("选择器:gt(下标)")：获取大于下标的所有 dom 对象

表单对象属性过滤器

- 根据表单中 dom 对象的状态情况，定位 dom 对象 
```javascript
$("选择器:状态")

状态：
启用状态     enabled
不可用状态   disabled
选中状态（单、复选框）  checked（radio， checkbox）

// 获取下拉列表的选中值
$("选择器>option：selected")
$("select>option：selected")
$("#s1>option：selected")
```

## 函数（val，append，each 常用）

```javascript
// 操作value值
val() // 获取数组第一个对象的value值
val(值) // 给所有对象的value赋值

// 操作起始标签与结束标签之间的文本
text() // 获取数组中所有对象的文字，拼成一个字符串返回
text(值) // 统一赋值

// 操作其他属性
attr("属性名") // 获取第一个对象的属性值
attr("属性名", "值") // 统一赋值
```

```javascript
// 删除所有对象及其子对象
remove()

// 删除所有对象的子对象
empty() 

// 给所有对象添加子对象
append("DOM对象语句") 
append("<div>asdf</div>")

// 获取类似于innerHTML中文本内容（拿到的是代码）
html() // 获取第一个对象的内容
html(值) // 统一赋值
```

```javascript
// each：可以对 数组， json ，dom数组循环处理
// 表示使用each循环数组，每个数组成员都会执行“处理函数”一次
// 处理函数：index（循环的索引）, element（数组中的成员）都是自定义的形参， 名称自定义。
$.each(循环的内容/数组， function(index, emelent){} )
jQuery对象.each(循环的内容/数组， function(index, emelent){} )

$("#btn6").click(function(){
  //循环普通数组,非dom数组
  var  arr = [ 11, 12, 13];
  $.each(arr, function(i,n){
    alert("循环变量："+i + "=====数组成员:"+ n);
  })
})

$("#btn7").click(function(){
  //循环json
  var json={"name":"张三","age":20};
  //var  obj = eval("{'name':'张三','age':20}");
  $.each(json,function(i,n){
    alert("i是key="+i+",n是值="+n);
  })
  
})

$("#btn8").click(function(){
  //循环dom数组
  var domArray = $(":text");
  $.each( domArray, function(i,n){
    // n 是数组中的dom对象
    alert("i="+i+"  , n="+n.value);
  })
})

$("#btn9").click(function(){
  //循环jquery对象, jquery对象就是dom数组
  $(":text").each(function(i,n){
    alert("i="+i+"，n="+ n.value);
  })
})
```

## 给 dom 对象绑定事件

$(选择器) . 事件名称(事件的处理函数)

```javascript
$(选择器)：定位dom对象， dom对象可以有多个， 这些dom对象都绑定事件了
事件名称：就是js中事件去掉on的部分， 例如  click  onclick(),
事件的处理函数：就是一个function，当事件发生时，执行这个函数的内容。

$("#btn").click(funtion(){
  alert("btn按钮单击了")
})
```

$(选择器) . on( 事件名称 , 事件的处理函数 )

```javascript
$("#btn").on("click", function() { 处理按钮单击 } )
```

## JQuery 与 AJAX

使用三个函数可以实现 ajax 请求的处理

```
（$.post()和get()内部都是调用的 $.ajax() ）
```

- 1） $.ajax() : jquery 中实现 ajax 的核心函数。
- 2） $.post() : 使用 post 方式做 ajax 请求。
- 3） $.get() : 使用 get 方式发送 ajax 请求。

```
$.ajax()参数是一个json的结构。 $.ajax(  {名称:值， 名称1:值1..... } )
```

```javascript
$.ajax({async:true, 
        contentType:"application/json", 
        data: {name:"lisi",age:20 },
        dataType:"json",
        error:function(){
            // 请求出现错误时，执行的函数
        },
        success:function( data ) {
            // data 就是responseText, 是jquery处理后的数据。
        },
        url:"bmiAjax",
        type:"get"
       }
)
```

```
json结构的参数说明（主要使用的是 url , data ,dataType, success）（顺序不限制）

async：
  一个boolean类型的值， 可以不写，默认是true ，表示异步请求的
  xmlHttp.open(get,url,true),第三个参数一样的意思。
contentType：
  一个字符串，表示从浏览器发送服务器的参数的类型。 可以不写。
  例如你想表示请求的参数是json格式的， 可以写application/json
data：
  可以是字符串，数组，json（常用），表示请求的参数和参数值
dataType：
  表示期望从服务器端返回的数据格式，可选的有： xml ， html ，text ，json
  使用$.ajax()发送请求时， 会把dataType的值发送给服务器， 
  那我们的servlet能够读取到dataType的值，就知道你的浏览器需要的数据格式，
  那么服务器就可以返回你需要的数据格式。
error：
  一个function ，表示当请求发生错误时，执行的函数。
  error:function() {   发生错误时执行  }  
sucess：
  一个function , 请求成功了，从服务器端返回了数据，会执行success指定函数
  相当于之前使用XMLHttpRequest对象， 当readyState==4 && status==200的时候。
url：
  请求的地址
type：
  请求方式，get或者post， 不用区分大小写。 默认是get方式。
```

```
$.post()，$.get()格式相同，$.get(url , data, success ,dataType)
```
