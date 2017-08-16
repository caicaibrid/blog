---
title: TypeScript
date: 2017-08-06 13:15:40
categories: TypeScript
tags:
    - TypeScript
---

# TypeScript

## 原则

TypeScript的核心原则之一是对值所具有的结构进行类型检查

# 数据类型

* ## boolean

```TypeScript
let flag:boolean = true;
```

* ## string

```TypeScript
let str:string = "caicai";
```

* ## number

```TypeScript
let num:number = 10;
```

* ## Array

> 定义类型两种方式: 
>1. 第一种,可以在元素类型后面接上 [],表示由此类型元素组成的一个数组
>2. 第二种,使用数组泛型

```TypeScript
//以下两种等价
let arr:number[] = [1,2,3,4];
let arr:Array<number> = [1,2,3,4];
```

* ## 元组

元组类型允许表示一个已知<span style="color:red">__元素数量和类型的数组__</span>，各元素的类型不必相同,可以直接赋值,但是必须为申明的类型

```TypeScript
let list:[string,number]=["1",2];
list[5] = 100;
console.log(list) //["1",2,undefined,undefined,undefined,100]
```

* ## 枚举

enum类型是对JavaScript标准数据类型的一个补充。 像C#等其它语言一样，使用枚举类型可以为一组数值赋予友好的名字,值为0,1,2,...

```TypeScript
enum Color{
    red,
    green,
    blue
}
let c: Color = Color.red;
let cName:string = Color[1];
console.log(cName) //red
console.log(c) //c
```

* ## any

any为任意类型,既可以赋任何值(可以理解为es5里声明的变量都为any类型,__eg:var sum = 100__)

```typescript
let a:any=1
let a:any="2"
```

* ## null undefined void

>1. void 类型与any类型相反,即为没有任何类型,在函数中没写,默认返回void类型
>2. undefined和null两者各自有自己的类型分别叫做undefined和null

```typescript
function method():void{ //返回值为void类型,return就会报错
    
}
let u:undefined = undefined;
let n:null = null;
```

## interface 

接口的作用就是为这些类型命名和为你的代码或第三方代码定义契约(相当于定义数据类型)

可以用interface进行定义,传入的参数必须全部定义,然后区分是否只读 是否可选

* ## 简单接口

```typescript
function func(name:string,age:number = 10){
    console.log(name,age) //name age为必传字段
}
```

* ## 可选属性

接口里的属性不全都是必需的,有些是只在某些条件下存在，或者根本不存在。即给函数传入的参数对象中只有部分属性赋值了。

> 两种方式
>1. 在可选属性后面加?
>2. 可以给参数设置默认值(自己亲测有效,官网没写,有待继续测试)

```typescript
function func(name:string,age?:number){
    console.log(name,age) //caicai,undefined
}
function func(name:string,age:number = 10){
    console.log(name,age) //caicai,10
}
func("caicai")
```

* ## 只读属性 readonly

一些对象属性只能在对象刚刚创建的时候修改其值。 你可以在属性名前用 readonly来指定只读属性

```typescript
interface Point{
    readonly x:number,
    readonly y:number
}

let p:Point = {x:5,y:10}
p.x=100;//error
```

* ## readonly vs const

最简单判断该用readonly还是const的方法是看要把它做为变量使用还是做为一个属性。 做为变量使用的话用 const，若做为属性则使用readonly。

* ## 额外的属性检查

以上定义接口的时候,约定的参数为必传属性,在生活中一般会存在一些不存在的属性需要传入,因此,我们总不能都写到interface里声明一遍(不声明就会报错),
因此我们需要绕开这些检查,最简单的办法:

1. 使用断言

```typescript
interface Point{
    x:number,
    y: number
}

function p(config: Point) {
    console.log(config) //{x:10,y:20,z:100}
}

p({x:10,y:20,z:100} as Point)
```

2. 添加一个字符串索引签名

```typescript
interface Point{
    x:number,
    y: number,
   [propName: string]: any //字符串签名,这样就可以传入任何参数了
}

function p(config: Point) {
    console.log(config) //{x:10,y:20,z:100}
}

p({x:10,y:20,z:100})
```

* ## 函数类型

接口能够描述JavaScript中对象拥有的各种各样的外形。 除了描述带有属性的普通对象外，接口也可以描述函数类型。

```typescript
interface SearchFunc { //申明一个函数类型(传入参数有两个,返回值的类型为boolean)
  (source: string, subString: string): boolean;
}
let mySearch: SearchFunc;
mySearch = function(source: string, subString: string) {
  let result = source.search(subString);
  return result > -1;
}
```

* ## 可索引的类型

描述那些能够“通过索引得到”的类型 可索引类型具有一个 索引签名，它描述了对象索引的类型，还有相应的索引返回值类型

```typescript
interface StringArray {
  [index: number]: string;
}

let myArray: StringArray;
myArray = ["Bob", "Fred"];

let myStr: string = myArray[0];
```

* ## 类类型

> 实现接口

```typescript
interface ClockInterface {
    currentTime: Date;//属性
    setTime(d: Date);//方法
}

class Clock implements ClockInterface {//在类里实现
    currentTime: Date;
    setTime(d: Date) {
        this.currentTime = d;
    }
    constructor(h: number, m: number) { }
}
```

# 类

传统的JavaScript程序使用函数和基于原型的继承来创建可重用的组件,ECMAScript 6开始，JavaScript程序员将能够使用基于类的面向对象的方式

* ##  简单事例

```typescript
class Greeter {
    greeting: string;
    constructor(message: string) {
        this.greeting = message;
    }
    greet() {
        return "Hello, " + this.greeting;
    }
}

let greeter = new Greeter("world");
```

以上声明了一个 Greeter类。这个类有3个成员：一个叫做greeting的属性，一个构造函数和一个greet方法

在引用任何一个类成员的时候都用了this。 它表示我们访问的是类的成员。

最后一行，我们使用new构造了Greeter类的一个实例。 它会调用之前定义的构造函数，创建一个 Greeter类型的新对象，并执行构造函数初始化它

* ## 继承

```typescript
class Animal {
    name:string;
    constructor(theName: string) { this.name = theName; }
    move(distanceInMeters: number = 0) {
        console.log(`${this.name} moved ${distanceInMeters}m.`);
    }
}

class Snake extends Animal {
    constructor(name: string) { super(name); }
    move(distanceInMeters = 5) {
        console.log("Slithering...");
        super.move(distanceInMeters);
    }
}

class Horse extends Animal {
    constructor(name: string) { super(name); }
    move(distanceInMeters = 45) {
        console.log("Galloping...");
        super.move(distanceInMeters);
    }
}

let sam = new Snake("Sammy the Python");
let tom: Animal = new Horse("Tommy the Palomino");

sam.move();
tom.move(34);
//运行结果:
//Slithering...
//Sammy the Python moved 5m.
//Galloping...
//Tommy the Palomino moved 34m.
```

我们使用 extends关键字来创建子类。你可以看到Horse和Snake类是基类Animal的子类，并且可以访问其属性和方法,并且重写了父类的方法

* ## 公共，私有与受保护的修饰符

* ### 默认为public (可加可不加)

```typescript
class Animal {
    public name: string;
    public constructor(theName: string) { this.name = theName; }
    public move(distanceInMeters: number) {
        console.log(`${this.name} moved ${distanceInMeters}m.`);
    }
}
```

* ### private

当成员被标记成private时，它就不能在声明它的类的外部访问。private成员在派生类中仍然可以访问

```typescript
class Animal {
    private name: string;
    constructor(theName: string) { this.name = theName; }
}

new Animal("Cat").name; // Error: 'name' is private;
```

* ### protected

protected修饰符与private修饰符的行为很相似，但有一点不同，protected成员在派生类中仍然可以访问

```typescript
class Person {
    protected name: string;
    constructor(name: string) { this.name = name; }
}

class Employee extends Person {
    private department: string;

    constructor(name: string, department: string) {
        super(name)
        this.department = department;
    }

    public getElevatorPitch() {
        return `Hello, my name is ${this.name} and I work in ${this.department}.`;
    }
}

let howard = new Employee("Howard", "Sales");
console.log(howard.getElevatorPitch());
console.log(howard.name);
```

# 函数

## 函数类型

* ### 为函数定义类型

```typescript
function add(x: number, y: number): number {
    return x + y;
}

let myAdd = function(x: number, y: number): number { return x+y; };//返回值为number类型
```

* ### 书写完整函数类型

```typescript
let myAdd: (x:number, y:number) => number =
    function(x: number, y: number): number { return x+y; };
```

* ### 推断类型

```typescript
let myAdd = function(x: number, y: number): number { return x + y; };
//=> typescript会自动推断出函数的类型
let myAdd: (baseValue:number, increment:number) => number =
    function(x, y) { return x + y; };
```

## 可选参数

 在TypeScript里我们可以在参数名旁使用 ?实现可选参数的功能

```typescript
function buildName(firstName: string, lastName?: string) {
    if (lastName)
        return firstName + " " + lastName;
    else
        return firstName;
}

let result1 = buildName("Bob");  // works correctly now
let result2 = buildName("Bob", "Adams", "Sr.");  // error, too many parameters
let result3 = buildName("Bob", "Adams");  // ah, just right
```

## 默认参数

```typescript
function buildName(firstName: string, lastName = "Smith") {//有默认值的参数
    return firstName + " " + lastName;
}

let result1 = buildName("Bob");                  // works correctly now, returns "Bob Smith"
let result2 = buildName("Bob", undefined);       // still works, also returns "Bob Smith"
let result3 = buildName("Bob", "Adams", "Sr.");  // error, too many parameters
let result4 = buildName("Bob", "Adams");         // ah, just right
```

## 剩余参数

必要参数，默认参数和可选参数有个共同点：它们表示某一个参数。 有时，你想同时操作多个参数，或者你并不知道会有多少参数传递进来。 在JavaScript里，你可以使用 arguments来访问所有传入的参数。

```typescript
function buildName(firstName: string, ...restOfName: string[]) {
  return firstName + " " + restOfName.join(" ");
}

let employeeName = buildName("Joseph", "Samuel", "Lucas", "MacKinzie");
```

# 泛型

```typescript
function identity<T>(arg: T): T { //假如用户传入的参数T为字符串,则t为string类型,函数返回值也为string类型
    return arg;
}
```

# export import

es6 的导入导出可以用的同时,typeScript新加入了 export = 和 import = require() 进行导出和导入

```typescript
//ZipCodeValidator.ts
let numberRegexp = /^[0-9]+$/;
class ZipCodeValidator {
    isAcceptable(s: string) {
        return s.length === 5 && numberRegexp.test(s);
    }
}
export = ZipCodeValidator;
//Test.ts
import zip = require("./ZipCodeValidator");

// Some samples to try
let strings = ["Hello", "98052", "101"];

// Validators to use
let validator = new zip();

// Show whether each string passed each validator
strings.forEach(s => {
  console.log(`"${ s }" - ${ validator.isAcceptable(s) ? "matches" : "does not match" }`);
});
```



