---
title: javascript设计模式
date: 2018-07-20 16:29:06
categories: Javascript
tags:
     - Javascript
---

# JavaScript设计模式

## 对象创建的方式

```javascript
var obi = {};

// or

var obj = Object.create(null);

// or 

var obj = new Object();
```

### 对象的赋值 

```javascript
// 直接用.
obj.someKey = "hello world";
// 方括号
obj["someKey"] = "hello world";
// Object.defineProperty()
Object.defineProperty(obj,"someKey",{
    value:"hello world",
    writable:true,
    enumerable: true,
    configurable: true
})
// Object.defineProperties() 
Object.defineProperties(obj,{
    "someKey":{
        value:"hello world"
    },
    "anotherKey":{
        value:"hello"
    }
})
```

## 构造器模式

### 基础构造器

把一个函数当作构造器来用(使用new关键字),它可以用自己定义的成员来初始化一个对象

```javascript
function Car(name,yeal) {
  this.name = name;
  this.year = yeal;
  this.toString = function() {
    return this.name+"-"+this.year;
  }
}
```

### 使用原型的构造器

在javascript中函数有一个prototype属性,当调用一个构造器创造对象时,其prototype上的属性和方法都对这个方法是可见的

```javascript
function Car(name,year) {
  this.name = name;
  this.year = year;
}
Car.prototype.toString = function() {
  return this.name + "-" + this.year;
}
```

## 模块化模式

### 模块

模块是一个程序不可缺少的一部分,特点是有助于保持应用的代码单元既能清晰地分析又能分离

## 单例模式

是因为他限制一个类智能有一个实例对象

经典的实现方式,创建一个类，这个类包含一个方法，这个方法在没有对象存在的情况下，将会创建一个新的实例对象。如果对象存在，这个方法只是返回这个对象的引用

```javascript
var mySingleton = (function() {
  var instance;
  function init() {
    // 单例
    //私有方法和变量
    function privateMethod() {
      console.log( "I am private" );
    }
    var privateVariable = "Im also private";
    var privateRandomNumber = Math.random();
    
    return {
      // 共有方法和变量
      publicMethod: function () {
        console.log( "The public can see me!" );
      },
      publicProperty: "I am also public",
      getRandomNumber: function() {
        return privateRandomNumber;
      }
    }
  }
   return {
      // 如果存在获取此单例实例，如果不存在创建一个单例实例
      getInstance: function () {
        if ( !instance ) {
          instance = init();
        }
        return instance;
      }
    };
})();
```