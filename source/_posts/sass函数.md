---
title: sass函数
date: 2017-07-01 17:52:54
tags:
    - Sass
categories: Sass
---
## scss 函数

### 字符串函数

> * unquote()函数 删除字符串中的引号,没有引号，返回原字符串

```css
.test1 {
    content:  unquote('Hello Sass!') ;
}
.test2 {
    content: unquote("'Hello Sass!");
/*编译结果*/
.test1 {
  content: Hello Sass!; }
.test2 {
  content: 'Hello Sass!; }

```

> * quote()函数 主要用来给字符串添加引号。如果字符串，自身带有引号会统一换成双引号 ""

```css
.test2 {
    content: quote("Hello Sass!");
}
.test3 {
    content: quote(ImWebDesigner);
}
/*编译结果*/
.test2 {
  content: "Hello Sass!";
}
.test3 {
  content: "ImWebDesigner";
}
```

> * To-upper-case() 函数将字符串小写字母转换成大写字母

```css
.test {
  text: to-upper-case(aaaaa);
  text: to-upper-case(aA-aAAA-aaa);
}
/*编译结果*/
//CSS
.test {
  text: AAAAA;
  text: AA-AAAA-AAA;
}
```

> * To-lower-case() 将字符串转换成小写字母

```css
//SCSS
.test {
  text: to-lower-case(AAAAA);
  text: to-lower-case(aA-aAAA-aaa);
}
//编译出来的 css 代码
.test {
  text: aaaaa;
  text: aa-aaaa-aaa;
}
```

### 数字函数

> * percentage() 要是将一个不带单位的数字转换成百分比形式

```css 
.footer{
    width : percentage(.2)
}
// 编译结果
.footer{
    width : 20%
}
```

> * round() 函数可以将一个数四舍五入为一个最接近的整数

```css
.footer {
   width:round(12.3px)
}
//编译结果
.footer {
  width: 12px;
}
```

> * ceil() 函数将一个数转换成最接近于自己的整数

```css
.footer {
   width:ceil(12.3px);
}
//编译结果
.footer {
  width: 13px;
}
```

> * floor() 函数将一个数去除其小数部分

```css
.footer {
   width:floor(12.3px);
}
//编译结果
.footer {
  width: 12px;
}
```

> *  abs( ) 函数会返回一个数的绝对值

```css
.footer {
   width:abs(-12.3px);
}
//编译结果
.footer {
  width: 12.3px;
}
```

> * min() 函数功能主要是在多个数之中找到最小的一个

```css
body {
    width:min(1,2,1%,3,300%)
}
// 编译结果
body {
  width: 1%; }
```

> * max() 函数用来获取一系列数中的最大那个值

```css
body {
    width:max(1,2,1%,3,300%)
}
//编译结果
body {
  width: 300%; }
```

> * random() 函数用来获取一个随机数

```css
body{
    width: ceil(random()*100)px
}
//编译结果
body {
  width: 35 px; }
```

### 列表函数

> * length()函数 返回一个列表中的长度

```css
length(10px,20px,(border 1px solid),2em)
=> 4 
```

> * nth($list,$n) 函数用来指定列表中某个位置的值。不过在 Sass 中，nth() 函数和其他语言不同，1 是指列表中的第一个标签值，2 是指列给中的第二个标签值

```css
body{
    width: nth(10px 20px 30px,1);
}
//编译结果
body{
    width: 10px;
}
```

> * join() 函数是将两个列表连接合并成一个列表,最多两个列表

```css
join((blue,red),(#abc,#def))
=> (#0000ff, #ff0000, #aabbcc, #ddeeff)
```

> * append() 函数是用来将某个值插入到列表中，并且处于最末位

```css
append(10px 20px ,30px)
=> (10px 20px 30px)
```

> * zip()函数将多个列表值转成一个多维的列表

```css
zip(1px 2px 3px,solid dashed dotted,green blue red)
=> ((1px "solid" #008000), (2px "dashed" #0000ff), (3px "dotted" #ff0000))
```

> * index() 函数类似于索引一样，主要让你找到某个值在列表中所处的位置

```css 
index(1px solid red, solid)
=> 2
```
