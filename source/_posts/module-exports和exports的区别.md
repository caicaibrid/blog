---
title: module.exports和exports的区别
date: 2016-08-15 20:48:46
categories: Javascript
tags:
    - Javascript
---

module.exports和exports的区别一直很困扰我,看过几次也一直忘记，今天在学习nodeJs时，又看到这两个玩意,看到这里又困惑了，于是专门花了一点时间又研究了下这两个玩意，顿时茅塞顿开，以此笔记来见证我的理解，废话不多说了，也不会说。。。看下面的代码吧！！！
# module.exports和exports的区别

## node 控制台直接打印console.log(module)

```javascript
{
  id: '.',
  exports: {},
  parent: null,
  filename: '/home/sa/Desktop/node_study/module/module.js',
  loaded: false,
  children: [],
  paths:[ '/home/sa/Desktop/node_study/module/node_modules',
     '/home/sa/Desktop/node_study/node_modules',
     '/home/sa/Desktop/node_modules',
     '/home/sa/node_modules',
     '/home/node_modules']
 }
```
## node 控制台直接打印console.log(exports)

```javascript
{}
```
## 在来看个例子

```javascript
var a = { name : 1 };
var b = a;
console.log(a) //{name:1}
console.log(b) //{name:1}

b.name = 2;
console.log(a) //{name:2}
console.log(b) //{name:2}

var b = {name:3};
console.log(a) //{name:2}
console.log(b) //{name:3}
```

解释：a 是一个对象，b 是对 a 的引用，即 a 和 b 指向同一块内存，所以前两个输出一样。当对 b 作修改时，即 a 和 b 指向同一块内存地址的内容发生了改变，所以 a 也会体现出来，所以第三四个输出一样。当 b 被覆盖时，b 指向了一块新的内存，a 还是指向原来的内存，所以最后两个输出不一样。
<br />
明白了上述例子后，我们只需知道三点就知道 exports 和 module.exports 的区别了：

> * module.exports 初始值为一个空对象 {}
> * exports 是指向的 module.exports 的引用
> * require() 返回的是 module.exports 而不是 export

# 结论

> * exports = module.exports = { }
> * exports = { } 将会指向一块新的内存，不能用这种方式导出模块
> * exports.xxx = { } 将会指向同一块内存，可以用这种方式导出模块
> * 如果用第二种方式导出模块，最后加上 module.exports = exports 重新赋值，一样可以达到导出模块的效果
