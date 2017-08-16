---
title: javascript数组遍历之间的区别
date: 2017-08-16 11:35:09
categories: Javascript
tags:
     - Javascript
---

# for..of vs. for..in 语句

> for..of和for..in均可迭代一个列表,但是用于迭代的值却不同

> for..in迭代的是对象的 键 的列表

> for..of则迭代对象的键对应的值


```javascript 1.8
let list = [4, 5, 6];

for (let i in list) {
    console.log(i); // "0", "1", "2",
}

for (let i of list) {
    console.log(i); // "4", "5", "6"
}
```


> for..in可以操作任何对象；它提供了查看对象属性的一种方法

> for..of关注于迭代对象的值

```javascript 1.8
let pets = new Set(["Cat", "Dog", "Hamster"]);
pets["species"] = "mammals";

for (let pet in pets) {
    console.log(pet); // "species"
}

for (let pet of pets) {
    console.log(pet); // "Cat", "Dog", "Hamster"
}
```

# 数组map方法

map() 方法创建一个新数组，其结果是该数组中的每个元素都调用一个提供的函数后返回的结果,必须有返回值

```javascript
let numbers = [1, 5, 10, 15];
let doubles = numbers.map((x) => {
   return x * 2;
});
// numbers is still [1, 5, 10, 15]
// doubles is now [2, 10, 20, 30]
```

使用 map 重新格式化数组中的对象

```javascript 1.8
var kvArray = [{key: 1, value: 10}, 
               {key: 2, value: 20}, 
               {key: 3, value: 30}];

var reformattedArray = kvArray.map(function(obj) { 
   var rObj = {};
   rObj[obj.key] = obj.value;
   return rObj;
});

// reformattedArray is now [{1: 10}, {2: 20}, {3: 30}], 

// kvArray is still: 
// [{key: 1, value: 10}, 
//  {key: 2, value: 20}, 
//  {key: 3, value: 30}]
```

反转字符串

```javascript 1.8
var str = '12345';
Array.prototype.map.call(str, function(x) {
  return x;
}).reverse().join(''); 

//'54321'
```

## find()

find() 方法返回数组中满足提供的测试函数的第一个元素的值。否则返回 undefined,返回符合条件的数组的值

```javascript 1.8
function isBigEnough(element) {
  return element >= 15;
}

[12, 5, 8, 130, 44].find(isBigEnough); // 130
```

## entries()

entries() 方法返回一个新的Array Iterator对象，该对象包含数组中每个索引的键/值对。

```javascript 1.8
var arr = ["a", "b", "c"];
var iterator = arr.entries();
// undefined

console.log(iterator);
// Array Iterator {}

console.log(iterator.next().value); 
// [0, "a"]
console.log(iterator.next().value); 
// [1, "b"]
console.log(iterator.next().value); 
// [2, "c"]
```

##  keys()

keys() 方法返回一个新的Array迭代器，它包含数组中每个索引的键

```javascript 1.8
let arr = ["a", "b", "c"];
let iterator = arr.keys();
// undefined
console.log(iterator);
// Array Iterator {}
console.log(iterator.next()); 
// Object {value: 0, done: false}
console.log(iterator.next()); 
// Object {value: 1, done: false}
console.log(iterator.next()); 
// Object {value: 2, done: false}
console.log(iterator.next()); 
// Object {value: undefined, done: true}
```

## values()

values() 方法返回一个新的 Array Iterator 对象，该对象包含数组每个索引的值。

```javascript 1.8
let arr = ['w', 'y', 'k', 'o', 'p'];
let eArr = arr.values();
console.log(eArr.next().value); // w
console.log(eArr.next().value); // y
console.log(eArr.next().value); // k
console.log(eArr.next().value); // o
console.log(eArr.next().value); // p
```

> 数组的方法entries()、keys()、values()都会返回Array Iterator 对象,可以使用for...of...直接遍历,也可以调取迭代期的next()方法

## every() 

返回值为boolean,检测数组的每一项是否可以通过callback函数,如果遇到false,循环就结束,否则返回true

```javascript 1.8
function isBigEnough(element, index, array) {
  return (element >= 10);
}
var passed = [12, 5, 8, 130, 44].every(isBigEnough);
// passed is false
passed = [12, 54, 18, 130, 44].every(isBigEnough);
// passed is true
```

## filter()

创建一个新数组, 通过回调函数返回结果为true的集合

```javascript 1.8
function isBigEnough(element) {
  return element >= 10;
}
var filtered = [12, 5, 8, 130, 44].filter(isBigEnough);
// filtered is [12, 130, 44]
```

## forEach()

forEach() 方法对数组的每个元素执行一次提供的函数。

```javascript 1.8
let a = ['a', 'b', 'c'];

a.forEach(function(element) {
    console.log(element);
});

// a
// b
// c
```

## reduce()  返回值为数组累计的结果

reduce() 方法对累加器和数组中的每个元素 (从左到右)应用一个函数，将其减少为单个值。

array.reduce(function(accumulator, currentValue, currentIndex, array), initialValue)

accumulator 上一次调用回调返回的值，或者是提供的初始值（initialValue）

initialValue 其值用于第一次调用 callback 的第一个参数。如果没有设置初始值，则将数组中的第一个元素作为初始值。空数组调用reduce时没有设置初始值将会报错


```javascript 1.8
var total = [0, 1, 2, 3].reduce(function(sum, value) {
  return sum + value;
}, 0);
// total is 6
```

计算数组中每个元素出现的次数

```javascript 1.8
var names = ['Alice', 'Bob', 'Tiff', 'Bruce', 'Alice'];

var countedNames = names.reduce(function (allNames, name) { 
  if (name in allNames) {
    allNames[name]++;
  }
  else {
    allNames[name] = 1;
  }
  return allNames;
}, {});
// countedNames is:
// { 'Alice': 2, 'Bob': 1, 'Tiff': 1, 'Bruce': 1 }
```

对于一个数组中的元素全是对象，每个对象又包含一个特定数组的情况，我们可以使用扩展运算符外加一个初始值的方式，来连接所有的特定数组

```javascript 1.8
var friends = [{
  name: 'Anna',
  books: ['Bible', 'Harry Potter'],
  age: 21
}, {
  name: 'Bob',
  books: ['War and peace', 'Romeo and Juliet'],
  age: 26
}, {
  name: 'Alice',
  books: ['The Lord of the Rings', 'The Shining'],
  age: 18
}];
var allbooks = friends.reduce(function(prev, curr) {
  return [...prev, ...curr.books];
}, ['Alphabet']);

// allbooks = [
//   'Alphabet', 'Bible', 'Harry Potter', 'War and peace', 
//   'Romeo and Juliet', 'The Lord of the Rings',
//   'The Shining'
// ]
```

## reduceRight() 

reduceRight() 方法接受一个函数作为累加器（accumulator）和数组的每个值（从右到左）将其减少为单个值。

```javascript 1.8
let flattened = [
    [0, 1], 
    [2, 3], 
    [4, 5]
].reduceRight((a, b) => {
    return a.concat(b);
}, []);

// flattened is [4, 5, 2, 3, 0, 1]
```

## some()

some() 方法测试数组中的某些元素是否通过由提供的函数实现的测试。有一个结果为真返回即为true,返回boolean

与every()方法相反,every()遇到false就会循环结束,而some遇到true就会结束

> 测试数组元素的值

```javascript 1.8
function isBigEnough(element, index, array) {
  return (element >= 10);
}
var passed = [2, 5, 8, 1, 4].some(isBigEnough);
// passed is false
passed = [12, 5, 8, 1, 4].some(isBigEnough);
// passed is true
```