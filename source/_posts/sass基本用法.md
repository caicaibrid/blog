---
title: sass基本用法
date: 2017-07-01 15:06:31
tags:
    - Sass
categories: Sass
---
## 选择器嵌套

假设我们有一段这样的结构：

```css
<header>
    <nav>
        <a href="#">Home</a>
        <a href="#">About</a>
        <a href="#">Blog</a>
    </nav>
<header>
/*css写法*/
nav a {
  color:red;
}
header nav a {
  color:green;
}
/*scss写法*/
nav {
  a {
    color: red;
    header & {
      color:green;
    }
  }  
}
```
## 属性嵌套
```css
/*css写法*/
.box {
    border-top: 1px solid red;
    border-bottom: 1px solid green;
}
/*scss写法*/
.box{
    border:{
        top: 1px solid red;
        bottom:1px solid green;
    }
}
```
## 伪类嵌套
```css
/*css写法*/
.clearfix:before, .clearfix:after {
  content: "";
  display: table;
}
.clearfix:after {
  clear: both;
  overflow: hidden;
}
/*scss写法*/
.clearfix {
    &:before,&:after{
        content: "";
        display: table;
    }
    &:after{
        overflow: hidden;
        clear: both;
    }
}
```
## 混合宏  @mixin
* 优点:将共用的代码块定义为宏，直接引用
* 缺点:会生成冗余的代码块，不会合并在一起
```css
/*声明混合宏*/
@mixin border-radius($raidus:5px){ 
    //单个参数加默认值($radius:5px) 
    //多个参数用逗号隔开($radius,$width,$height)
    //多个参数还可以用[...]表示($shadows...)
    border-radius: $radius;
}
/*调用混合宏*/
div{
    @include border-radius(10px);
}
/*多个参数使用...为参数*/
@mixin box-shadow($shadows...){
  @if length($shadows) >= 1 {
    -webkit-box-shadow: $shadows;
    box-shadow: $shadows;
  } @else {
    $shadows: 0 0 2px rgba(#000,.25); // 默认参数  如果没有传 就用这个  传了就用传过来的参数
    -webkit-box-shadow: $shadow;
    box-shadow: $shadow;
  }
}
/*调用宏*/
.box {
  @include box-shadow(0 0 1px rgba(#000,.5),0 0 2px rgba(#000,.2));
}
/*编译结果*/
.box {
  -webkit-box-shadow: 0 0 1px rgba(0, 0, 0, 0.5), 0 0 2px rgba(0, 0, 0, 0.2);
  box-shadow: 0 0 1px rgba(0, 0, 0, 0.5), 0 0 2px rgba(0, 0, 0, 0.2);
}
```
## 继承 @extend 可以将公用的代码合并在一起
```css
/*scss写法*/
.btn {
  border: 1px solid #ccc;
  padding: 6px 10px;
  font-size: 14px;
}

.btn-primary {
  background-color: #f36;
  color: #fff;
  @extend .btn;
}

.btn-second {
  background-color: orange;
  color: #fff;
  @extend .btn;
}
/*编译结果*/
.btn, .btn-primary, .btn-second {
  border: 1px solid #ccc;
  padding: 6px 10px;
  font-size: 14px;
}

.btn-primary {
  background-color: #f36;
  color: #fff;
}

.btn-second {
  background-clor: orange;
  color: #fff;
}
```
## 占位符 %placeholder 编译出来的代码会将相同的代码合并在一起
* %placeholder 功能是一个很强大，很实用的一个功能，
他可以取代以前 CSS 中的基类造成的代码冗余的情形,
因为 %placeholder 声明的代码，如果不被 @extend 调用的话，
不会产生任何代码。因此，需要配合@extend使用
```css
/*占位符代码*/
%mt5 {
  margin-top: 5px;
}
%pt5{
  padding-top: 5px;
}
/*继承占位符*/
.btn {
  @extend %mt5;
  @extend %pt5;
}

.block {
  @extend %mt5;

  span {
    @extend %pt5;
  }
}
/*编译结果*/
.btn, .block {
  margin-top: 5px;
}

.btn, .block span {
  padding-top: 5px;
}
```
## 混合宏 VS 继承 VS 占位符

![](/img/scss.jpg)

## 插值#{} 为了使让变量和属性工作的很完美

```css
/*scss*/
$properties: (margin, padding);
@mixin set-value($side, $value) {
    @each $prop in $properties {
        #{$prop}-#{$side}: $value;
    }
}
.login-box {
    @include set-value(top, 14px);
}

@mixin generate-sizes($class, $small, $medium, $big) {
    .#{$class}-small { font-size: $small; }
    .#{$class}-medium { font-size: $medium; }
    .#{$class}-big { font-size: $big; }
}
@include generate-sizes("header-text", 12px, 20px, 40px);


/*编译结果*/
.login-box {
    margin-top: 14px;
    padding-top: 14px;
}
.header-text-small { font-size: 12px; }
.header-text-medium { font-size: 20px; }
.header-text-big { font-size: 40px; }

```
## scss中的注释

1、类似 CSS 的注释方式，使用 "/* "开头，结属使用 "*/ "

2、类似 JavaScript 的注释方式，使用"//"

<p style="color:red">区别</p>

前者会在编译出来的 CSS 显示，后者在编译出来的 CSS 中不会显示，来看一个示例：

## Sass运算

* 加法运算 单位必须相同
```css
body{
    width: 100px + 100px;
}
```
* 减法运算 单位必须相同
```css
body{
    width: 100px - 10px;
}
```
* 乘法运算 单位必须相同且数值只能有一个单位
```css
/*scss写法*/
body{
     width: 10px * 3;
}
$list: twitter,facebook,github,weibo;

@for $i from 1 through length($list){
  .icon-#{nth($list,$i)}{
    background-postion: 0 - 20px * $i;
  }
}
/*编译结果*/
body{
    width: 30px;
}
.icon-twitter {
  background-postion: -20px; }
.icon-facebook {
  background-postion: -40px; }
.icon-github {
  background-postion: -60px; }
.icon-weibo {
  background-postion: -80px; }
```
* 除法运算

    <p style="color:red">"/" 符号被当作除法运算符时有以下几种情况：</p>
    
    •    如果数值或它的任意部分是存储在一个变量中或是函数的返回值。
    
    •    如果数值被圆括号包围。
    
    •    如果数值是另一个数学表达式的一部分。
```css
body{
  font: 10px/8px;             // 纯 CSS，不是除法运算
  $width: 1000px;
  width: $width/2;            // 使用了变量，是除法运算
  width: round(1.5)/2;        // 使用了函数，是除法运算
  height: (500px/2);          // 使用了圆括号，是除法运算
  margin-left: 5px + 8px/2px; // 使用了加（+）号，是除法运算
}
```
