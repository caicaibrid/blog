---
title: nodeJS开发指南
date: 2017-12-11 14:43:54
categories: NodeJS
tags:
     - NodeJS
---

# nodeJS创建服务器

```javascript 1.8
var http = require("http");
http.createServer(function(req, res) {
res.writeHead(200, {'Content-Type': 'text/html'});
res.write('<h1>Node.js</h1>');
res.end('<p>Hello World</p>');
}).listen(3000);
console.log("HTTP server is listening at port 3000.");
```

可以利用supervisor进行实时监控代码的修改 

# 异步式I/O与事件编程

## 阻塞

线程在执行中如果遇到磁盘的读写和网络通信通常需要耗费较长时间,这时操作系统会剥夺该线程的CPU控制权,使其暂停执行,同时,将资源让给其它的工作线程,
这种线程调度即为阻塞.

## 同步式I/O和阻塞式I/O

在阻塞的基础上,当I/O操作完毕时,操作系统将这个线程的阻塞解除,恢复其对CPU的控制权,另其继续执行

## 非阻塞式的I/O和异步式的I/O

针对所有的I/O操作不采用阻塞的策略,当线程遇到I/O操作时,不会以阻塞的方式等待I/O操作的完成或数据的返回,只是将I/O操作请求发送给操作系统,继续执行
下一条语句,当操作系统完成I/O操作时,以事件的形式通知I/O操作的线程,线程会在特定的时间处理这个事件。
为了处理异步 I/O，线程必须有事件循环，不断地检查有没有未处理的事件，依次予以处理。

## 为什么 Node.js 使用了单线程、非阻塞的事件编程模式

阻塞模式下，一个线程只能处理一项任务，要想提高吞吐量必须通过多线程。而非阻塞模式下，一个线程永远在执行计算操作，这个线程所使用的 CPU 核心利用率永远是 100%，
I/O 以事件的方式通知。在阻塞模式下，多线程往往能提高系统吞吐量，因为一个线程阻塞时还有其他线程在工作，多线程可以让 CPU 资源不被阻塞中的线程浪费。而在非阻塞模式
下，线程不会被 I/O 阻塞，永远在利用 CPU。多线程带来的好处仅仅是在多核 CPU 的情况下利用更多的核，而Node.js的单线程也能带来同样的好处

## 事件

NodeJS中的异步I/O操作在完成时都会发送一个事件到事件队列(都是通过一个事件去触发的回调函数)

## NodeJS的事件循环机制

Node.js 程序由事件循环开始，到事件循环结束，所有的逻辑都是事件的回调函数，所以 Node.js 始终在事件循环中，程序入口就是
事件循环第一个事件的回调函数。事件的回调函数在执行的过程中，可能会发出 I/O 请求或直接发射（emit）事件，执行完毕后再返回事件循环，事件循环会检查事件队列中有没有未
处理的事件，直到程序结束。

## 包和模块

模块即为平常自己写的js代码,然后通过exports或者module.export导出的

包在模块的基础上更抽象一点,可以理解为实现了某个功能模块的集合

### 创建包

NodeJS的包是一个目录,其中包含一个JSON格式的包说明文件(package.json),严格符合CommonJS规范的包应该包括以下内容

- package.json  必须在包的根目录下

- bin目录        二进制文件

- lib目录        JavaScript文件

- DOC目录        文档目录

- Test目录        测试目录

-[ ]  简单的包 

创建一个目录,在该目录下创建一个js文件,这种方法就将一个目录封装为一个模块,就是所谓的包(因为就是一个目录)

```javascript 1.8
//创建一个somepackage 目录

//在该目录下创建一个index.js文件 

// somepackage/index.js
exports.hello = function() {
console.log('Hello.');
};

// 然后在包外建立一个js文件 getsomepackage.js文件 ,引入刚才建立的包

var hello = require("./somepackage").hello;
hello() // Hello.
```

-[ ]  package.json

在前面例子中的somepackage目录下创建一个package.json文件,然后将index.js改名为interface.js

```json
{
    "main":"./lib/interface.js"
}
```

继续调用这个包,依然可以访问,因为Node.js 在调用某个包时，会首先检查包中 package.json 文件的 main 字段，将其作为
包的接口模块，如果 package.json 或 main 字段不存在，会尝试寻找 index.js 或 index.node 作为包的接口。

### NodeJS包管理器

1. 获取包 
    
    npm install [packageName]

2. 全局包和本地包 

    全局包  npm install -g [packageName]
    
    本地包  npm install 安装到跟目录下的node_modules
    
3. 创建全局链接
    
    全局--->本地
    
    安装一个包为全局的  npm install -g [packageName]
    
    如果想在项目下使用这个包,即可使用 npm link [packageName]
    
    本地--->全局
    
    在项目的根目录下(package.json所在的目录),运行npm link [packageName] 即可
    
4. 包的发布

    1) 通过npm init 命令根据交互式回答完成package.json文件,创建一个入口文件,这样就生成了一个包
    
    2) 创建一个帐号,用于维护自己的包 npm adduser 根据提示输入自己的用户名 密码 邮箱 
    
    3) 通过npm whoami 检测是否拿到了帐号
    
    4) 然后在根目录下运行npm publish 然后就发布成功了
    
    5) 如果你的包将来有更新,请修改version字段,然后重新npm publish就可以了
    
    6) 卸载包 npm unpublish

### NodeJS代码调试

    1) 利用node debug  xxx.js 进行调试 
    
    2) node --debug-drk[=port] xxx.js进行调试  //这个会暂停js代码执行
    
    2) 利用node-inspector 调试NodeJS
    
    全局安装 npm install node-inspector -g 
    
    然后在终端中通过 node --debug-brk=5858 xxx.js 
    
    命令行启动 node-inspector 
    
    在窗口中输入 localhost:8080/debug?port=5858

<img src="http://caicaibrid.github.io/img/debug.png" width="100%"/>





