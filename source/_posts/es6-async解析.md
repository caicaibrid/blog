---
title: es6 async解析
date: 2017-08-03 14:06:40
categories: ES6
tags:
    - ES6
---

# async 函数

async函数就是将 Generator 函数的星号（*）替换成async，将yield替换成await，仅此而已

## Generator 函数读取文件例子

```javascript 1.8
    var fs = require('fs');
    var readFile = function (fileName) {
      return new Promise(function (resolve, reject) {
        fs.readFile(fileName, function(error, data) {
          if (error) return reject(error);
          resolve(data);
        });
      });
    };
    
    var gen = function* () {
      var f1 = yield readFile('/etc/fstab');
      var f2 = yield readFile('/etc/shells');
      console.log(f1.toString());
      console.log(f2.toString());
    };
```

## async 函数改写

```javascript 1.8
    var asyncReadFile = async function () {
      var f1 = await readFile('/etc/fstab');
      var f2 = await readFile('/etc/shells');
      console.log(f1.toString());
      console.log(f2.toString());
    };
```

## async 函数优点

1. 内置执行器,不用像Generator函数一样调用next()方法

2. 更好的语义

3. 更广的适用性
   yield命令后面只能是 Thunk 函数或 Promise 对象，而async函数的await命令后面，可以是Promise 对象和原始类型的值（数值、字符串和布尔值，但这时等同于同步操作(相当于直接返回Promise.resolve("成功了"))

4. 返回值是 Promise 可以进行链式调用(.then(function(res){}))

## async 实例

```javascript 1.8
    function a(){
      return new Promise(function(resolve,reject){
         setTimeout(function(){
           console.log("-------")
            //resolve({name:"caicai",age:12})
            //reject("错误")
         },2000)
      })
    }
    async function getName(){
      try {
          let name = await a()
          console.log(name)
      }catch (e){
          console.log(e)
      }
      let age = await "20";
      return age
    }
    
    getName().then(function(res){
      console.log(res) // 若有return则返回return的结果,没有则为undefined
    })
    
    //运行结果:
    //resolve的结果
        //-------
        //Object {name: "caicai", age: 12}
        //20
    //reject的结果
        //-------
        //错误
        //20
```

## 结论 

1. await后面的代码,需要等待await的结果

2. await后面的表达式,可以为任意类型

3. 如果使用return语句会返回return的结果,在.then回调中可以取到,若不使用return,则返回undefined

4. 如果await的结果返回为reject(),则不会执行后面的代码

5. 在写await操作的时候,判断是否可能会失败,如果可能失败,则用try{ }catch (e){ }块捕获异常,可以让下面的代码正常执行