---
title: URL到展现页面的全过程
date: 2016-12-28 22:28:47
categories: Javascript
tags:
     - Javascript
---

---------------------

## url到页面展现的过程   

[原文地址](http://www.cnblogs.com/strick/p/5494869.html)

1.域名解析 	-> 		DNS（Domain Name System）位于应用层，提供域名和IP地址之间的解析服务。
2.建立tcp连接 		传输控制协议
3.发起http请求
4.服务器响应http请求
5.浏览器渲染页面

## TCP/IP分为4层:

应用层（HTTP客户端）、传输层（tcp）、网络层（ip）、链路层（网络）

HTTP（Hyper Text Transfer Protocol），超文本传输协议，由请求和响应构成。

## 3次握手

1）.发送端发送一个带SYN标志的数据包给对方
2）.接收端回传一个带SYN（握手信号）和ACK（确认字符）标志的数据包以示传达确认信息
3）.发送端再回传一个带ACK标志的数据包，代表握手结束

## 状态码:

1xx 信息性状态码，接收的请求正在处理
2xx 成功状态码，请求正常处理完毕
3xx 重定向状态码，需要进行附加操作以完成请求
4xx 客户端错误状态码，服务器无法处理请求
5xx 服务端错误状态码，服务器处理请求出错

## html解析过程

解析HTML以构建DOM树 -> 构建Render（渲染）树 -> 布局Render树 -> 绘制Render树

## 网页优化

[毫秒必争，前端网页性能最佳实践](http://www.cnblogs.com/developersupport/p/3248695.html)
