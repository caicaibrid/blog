---
title: js高程(第三版)兼容问题和注意事项
date: 2017-08-24 10:01:38
categories: Javascript
tags:
     - Javascript
---

# js高级程序设计(第三版) 兼容bug

## 数组(Array)

### 构造函数创建数组

* new Array(arg) 

1. arg为数字,则创建一个对应数量的数组

2. arg为字符串,则创建包含arg个的一个数组

3. new操作符可以省略

### 对象字面量创建数组

* bug

1. ```let arr = [1,2,]```

在ie中arr的值为1,2,undefined,而在其它浏览器中arr的值为1,2

2. ```let arr = [,,,,]```

在ie8以下```arr.length = 5```,而其它浏览器为```arr.length = 4```

3. ```let arr = [1,2,3,4]```

若改变arr的length长度,则会改变数组对应的数据```arr.length=5;arr[4] = undefined```

### sort 方法

sort方法默认为升序排列,但是比较的每一项的字符串,因此```let arr = [0,1,5,10,15];arr.sort();console.log(arr)//0,1,10,15,5```

可以借助sort方法的参数(比较函数)得到正确的结果

```javascript 1.8
let arr = [0,1,5,10,15];
arr.sort(function(a,b) {
  if(a<b){
      return -1;
  }
  if(a==b){
      return 0;
  }
  if(a>b){
      return 1;
  }
});
console.log(arr)//[0,1,5,10,15]
```

## 对象(Object)

### 对象属性类型

* 数据属性  可以直接定义

数据属性包含一个数据的位置,在这个位置可以读取和写入,数据属性有4个描述其属性的行为特性

* * [[ Configureable ]] 表示能否通过delete直接删除其属性,默认值true

* * [[ Enumerable ]] 表示该属性能否通过```for ... in ...```进行遍历,默认值为true

* * [[ Writable ]] 表示该属性的值能否修改,默认值为true

* * [[ Value ]] 表示该属性的数据值,默认值为undefined

<font color="red">以上为每个对象的属性都具有的特性,要想修改其默认属性,必须同过```Object.defineProperty()```方法</font>

```
Object.defineProperty(属性所在的对象,属性的名字,描述符)

//描述符为以上四个属性,这样就可以修改默认值了

//例子

var person = {}
Object.defineProperty(person,"name",{
    value:"caicai",
    writable:false
})
console.log(person.name) // caicai
person.name="hahaha"; //严格模式下报错,非严格模式下将会忽略
console.log(person.name) // caicai
```

* 访问器属性 不可以直接定义,必须通过`Object.defineProperty()`定义

访问器属性不包含数据值,它们包含一对getter和setter函数

* * [[ Configureable ]] 表示能否通过delete直接删除其属性,默认值true

* * [[ Enumerable ]] 表示该属性能否通过```for ... in ...```进行遍历,默认值为true

* * [[ get ]] 表示读取该属性时的值,默认值为undefined

* * [[ set ]] 表示写入该属性的数据值,默认值为undefined

```
let book={
    __year:2004,
    edition:1
}
Object.defineProperty(book,"year",{
    get:function(){
        return this.__year;
    },
    set:function(newValue){
        if(newValue>this.__year){
            this.edition += newValue - this.__year;
            this.__year = newValue;
        }
    }
})
book.year = 2015;
console.log(book.edition)//12
```

### 创建对象

虽然Objectg构造函数和对象字面量都可以创建对象,但有个缺点,使用同一个接口创建很多对象,会产生大量的重复代码,因此就出现了以下的设计模式

#### *  工厂模式

用函数来封装特定的接口,然后进行创建对象

虽然解决了创建多个对象相似的问题,但是没有解决对象识别的问题

```
//工厂模式
funcction CreatePerson(name,age,job){
    var o = new Object();
    o.name = name;
    o.age = age;
    o.job = job;
    o.sayName=function(){
        return this.name
    }
    return o;
}
var p1 = CreatePerson("caicai1",20,"民工")
var p2 = CreatePerson("caicai2",22,"IT民工")
```

#### * 构造函数模式

自定义构造函数,从而定义自定义对象类型的属性和方法
* * 与工厂模式的区别

* * * 1.没有显示的创建对象

* * * 2.直接将属性和方法赋值给this对象

* * * 3.没有return语句

* * 优点

* * * 1.没有显示的创建对象

* * * 2.直接将属性和方法赋值给this对象

* * * 3.没有return返回值

* * 缺点

* * * 每个方法都要在实例上重新创建一遍

* * * 可以将方法写到全局,然后赋值,这样可以解决两个函数做同一件事的问题,但是这样的话就会出现很多全局函数,没有封装可言,因此出现了原型模式

```
//方式一
function Person(name,age,job){
    this.name = name;
    this.age = age;
    this.job = job;
    this.sayName = function(){
        return this.name
    }
}
var p1 = new Person("caicai1",20,"民工")
var p2 = new Person("caicai2",22,"IT民工")

//方式二
 function sayName(){
    return this.name
}
function Person(name,age,job){
    this.name = name;
    this.age = age;
    this.job = job;
    this.sayName = sayName;
}
var p1 = new Person("caicai1",20,"民工")
var p2 = new Person("caicai2",22,"IT民工")
```

#### * 原型模式

每个函数都有一个prototype属性,这个属性是个指针,指向一个对象,这个对象的用途为让所有实例的属性共享属性和方法

即为不必在构造函数中定义对象的属性和方法,而是将这些属性和方法添加到原型对象上

```
function Person(){}
Person.prototype.name= "caicai"
Person.prototype.age= 20
Person.prototype.job= "It"
Person.prototype.sayName= function(){
    return this.name
}

var p1 = new Person();

p1.sayName()//caicai
p1.hasOwnPropertype("name")//false

p1.name = "hahha"
p1.hasOwnPropertype("name")//true

delete p1.name
p1.hasOwnPropertype("name")//false

var p2 = new Person();
p2.sayName()//caicai
```

!["原型模式"](/img/prototype.jpg)

* 原型对象

只要创建函数,就会根据一组特定的规则为该函数创建一个prototype属性,而这个属性指向函数的原型对象(Function.prototype),默认情况下该原型对象会获得一个construtor属性,这个属性指向*包含一个prototype属性的函数指针*(Function.prototype.constructor),并且我们可以通过prototype对象继续添加属性和方法
<hr>
创建自定义构造函数后,其原型对象默认只会取得construtor属性,其它方法都是继承自Object

当创建一个实例后,该实例内部有一个指针(*._ _proto_ _*),指向构造函数的原型对象

```
function Parent(){}

var p1 = new Parent();

Parent.prototype.constructor === Parent //true

p1.__proto__ === Parent.prototype //true
```

确定一个属性存在于原型对象中 

hasOwnProperty() //判断一个属性是否存在于实例中存在true,不存在false

in 操作符 //判断一个属性是否存在于对象中(在实例和原型上都会去寻找)

isPrototypeOf() //判断一个对象是否为构造函数的实例

```
function hasPrototypeProperty(object,name){
    return !object.hasOwnProperty(name)&&(name in object);
}

function Person(){}
var p = new Person();
Person.prototype.isPrototypeOf(p) //true
```

#### * 组合使用构造函数模式和原型模式

即为两种模式同时使用,构造函数用于定义实例属性,而原型模式用于定义方法和公用的属性

```
function Person(name,age){
    this.name = name;
    this.age = age;
}
Person.prototype = {
    constructor:Person,
    sayName:function(){
        return this.name
    }
}
```

#### * 动态原型模式

因为组合模式看起来比较混乱,因此就出现动态原型模式,动态原型模式即为在构造函数内动态的创建原型对象

```
function Person(name,age){
    this.name = name;
    this.age = age;
    if(typeof this.sayName!=undefined){
        Person.prototype.sayName = function(){
            return this.name    
        }
    }
}

```

### 继承

#### * 原型链继承

原型链为实现继承的主要方法,其基本思想为利用原型让一个引用类型继承另外一个引用类型的属性和方法

* 原型丶构造函数丶实例的关系 

- [x] 每个构造函数都有一个原型对象.每个原型对象都有一个指向构造函数的指针,而实例都包含一个指向原型对象的指针

```javascript 1.8
function Person() {
  this.name = "caicai"; 
}
Person.prototype.getName = function() {
  return this.name
}

function  Student() {
    this.age = 20;
}

Student.prototype = new Person();
Student.prototype.getAge = function() {
  return this.age;
}

```

#### * 借用构造函数

在子类型的内部调用父类型的构造函数

```javascript 1.8
function Person() {
  this.colors = ["red","blue"]
}
function Student() {
  Person.call(this);
}
let s = new Student();
s.colors.push("black");
console.log(s.colors)// red,blue,black

let s1 = new Student();
console.log(s1.colors)//red,blue
```

#### * 组合继承

将借用构造函数继承和原型链继承结合在一起

```javascript 1.8
function  Person( name ) {
   this.name = name;
}
Person.prototype.getName = function() {
  return this.name
}

function Student() {
  Person.call(this,"caicai");
  this.age = 20;
}
Student.prototype = new Person();

let s= new Student();
s.getName() //caicai
```

#### * 原型式继承

#### * 寄生式继承

#### * 寄生组合式继承









