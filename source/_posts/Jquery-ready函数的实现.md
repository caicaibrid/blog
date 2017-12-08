---
title: Jquery ready函数的实现
date: 2017-12-08 09:47:40
categories: Javascript
tags:
     - Javascript
---

## ready和onload函数的区别

- 运行时间

onload函数必须等到页面里所有东西加载完,ready函数则为渲染完dom结构就会执行

- 编写个数

window.onload只能写一个(写多个的话,只会执行一个)$(document).ready()可以写多个 

- 简化写法

window.onload没有简写,$(document).ready()可以简写为$(function(){})

## 怎样用javascript实现jquery的ready函数呢?

标准浏览器可以监听DOMContentLoaded,ie浏览器可以监听onreadystatechange

具体代码实现:

```javascript 1.8
function ready(callback) {
  if(document.addEventListener){
      document.addEventListener("DOMContentLoaded",function() {
        document.removeEventListener("DOMContentLoaded",arguments.callee,false);
        callback();
      })
  }else if(document.attanchEvent){
       document.attanchEvent("onreadystatechange",function() {
         if(document.readyState === "complete"){
             document.detanchEvent("DOMContentLoaded",arguments.callee);
             callback();
         }
       })
  }
}
```