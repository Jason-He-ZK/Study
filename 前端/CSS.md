## CSS 及其作用

CSS（Cascading Style Sheet）：层叠样式表语言。

作用：修饰 HTML 页面，设置 HTML 页面中的某些元素的样式，让 HTML 页面更好看。

HTML 还是主体，CSS 依赖 HTML。CSS 的存在就是修饰 HTML，所以新建的文件还是 xx.html 文件。

CSS 的注释：/_ 注释内容 _/

## HTML 中嵌入 CSS 样式的三种方式

### 内联定义方式

在标签内部使用 style 属性来设置元素的 CSS 样式

```html
<标签 style="样式名:样式值;样式名:样式值;..."></标签>

<div style="width : 300px; 
            height : 300px; 
            background-color : #CCFFFF; 
            display : block; 
            /* border : 1px solid black; */ 
            border-color : red;
            border-width : 1px;
            border-style : solid;"></div>
```

### 样式块

在  和  标签之间插入样式块

```html
在head标签中使用style块

<head>
  <style type="text/css">
    选择器 {
      样式名 : 样式值;
      样式名 : 样式值;
      .....
    }
    选择器 {
      样式名 : 样式值;
      样式名 : 样式值;
      .....
    }
  </style>
</head>
```

选择器

- 
id 选择器
```css
#id{
  样式名 : 样式值;
  样式名 : 样式值;
  ....
}
```

```css
#usernameErrorMsg {
  color : red;
  font-size : 12px;
} 

<span id="usernameErrorMsg">test</span>
```


- 
标签选择器
```css
标签名 {
  样式名 : 样式值;
  样式名 : 样式值;
  ....
}
```

```css
/* 所有div标签都会受影响 */
div {
  background-color : black;
  border : 1px solid red;
  width : 100px;
  height : 100px;
}
```


- 
class 选择器
```css
.类名{
  样式名 : 样式值;
  样式名 : 样式值;
  ....
}
```

```css
.student {
  border : 1px solid red;
  width : 400px;
  height : 30px;
} 



<input type="text" class="student"/> 

<select class="student">
  <option>专科</option>
  <option>本科</option>
</select>
```



### 引入外部独立的 CSS 样式文件

```html
链入外部样式表文件（最常用，维护成本低）

<link type="text/css" rel="stylesheet" href="css文件的路径" />

<head>
  <meta charset="utf-8">
  <title>在HTML中使用CSS样式的第三种方式：引入外部独立的css文件</title>
  
  <!--引入css-->
  <link rel="stylesheet" type="text/css" href="css/1.css" />
  
</head>
<body>
  <a href="http://www.baidu.com">百度</a>
  <span id="baiduSpan">点击我链接到百度哦！</span>
</body>
```

```css
/* 文件名为 1.css */

/* 标签选择器（超链接） */
a {
  text-decoration : none;
}

#baiduSpan {
  text-decoration: underline;
  cursor: pointer;
}
```

## 常用

```css
/*边框*/
div{
  border : 1px solid red;
}
div{
  border-width : 1px;
  border-style : solid;
  border-color : red;
}

/*隐藏*/
div{
  display : none;
}

/*字体*/
div{
  font-size : 12px;
  color : red;
}

/*文本装饰*/
a{
  text-decoration : none;
}
a{
  text-decoration : underline;
}

/*列表*/
ul{
  list-style-type: none;
}

/*鼠标悬停效果*/
#baiduSpan {
  text-decoration: underline;
  cursor: pointer;
}

/*定位*/
div{
  position : absolute; /*绝对定位*/
  left : 100px;
  top : 100px;
}
```
