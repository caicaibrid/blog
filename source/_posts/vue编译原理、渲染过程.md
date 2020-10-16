---
title: vue编译原理、渲染过程
date: 2020-10-16 15:17:58
categories: Vue
---

## Vue 编译原理

  关于 Vue 编译原理这块的整体逻辑主要分三个部分，也可以说是分三步，这三个部分是有前后关系的：

* 第一步是将 模板字符串 转换成 element ASTs（解析器）

* 第二步是对 AST 进行静态节点标记，主要用来做虚拟DOM的渲染优化（优化器）

* 第三步是 使用 element ASTs 生成 render 函数代码字符串（代码生成器）

![ss](/img/vue.jpeg)

[Vue 编译原理](https://www.cnblogs.com/dhsz/p/8462227.html)

## Vue 渲染过程

* 第一步：解析模板成 render 函数

* 第二步：响应式开始监听

* 第三步：首次渲染，显示页面，且绑定依赖

* 第四步：data 属性变化，触发 rerender

[Vue 渲染过程](https://www.cnblogs.com/chrislinlin/p/12587723.html)


