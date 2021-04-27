---
title: Web Componets
date: 2021-03-06 10:29:21
tags:
---

# Web Componets

## 简介

Web Components 是一套不同的技术，允许您创建可重用的定制元素（它们的功能封装在您的代码之外）并且在您的web应用中使用它们。

## 背景

- 组件化开发已经是前端主流开发方式，因为组件化开发在代码复用提升团队效率方面有着无可比拟的优势，现在流行的React，Vue都是组件框架。
- 谷歌公司一直在推动浏览器的原生组件，即 Web Components API。相比第三方框架，原生组件简单直接，符合直觉，不用加载任何外部模块，代码量小。目前，它还在不断发展，但已经可用于生产环境。

## 组成部分

### Customer Elements（自定义元素）

一组JavaScript API，允许您定义custom elements及其行为，然后可以在您的用户界面中按照需要使用它们。

### Shadow Dom（影子Dom）

一组JavaScript API，用于将封装的“影子”DOM树附加到元素（与主文档DOM分开呈现）并控制其关联的功能。通过这种方式，您可以保持元素的功能私有，这样它们就可以被脚本化和样式化，而不用担心与文档的其他部分发生冲突。

### Html Template (Html模板)

 template 和 slot 元素使您可以编写不在呈现页面中显示的标记模板。然后它们可以作为自定义元素结构的基础被多次重用。

### 实现步骤

1. 创建一个类或函数来指定web组件的功能
2. 使用`customElements.define()`方法注册您的新自定义元素 ，并向其传递要定义的元素名称、指定元素功能的类、以及可选的其所继承自的元素
3. 如果需要的话，使用`Element.attachShadow()`方法将一个shadow DOM附加到自定义元素上。使用通常的DOM方法向shadow DOM中添加子元素、事件监听器等等
4. 如果需要的话，使用`template` 和`slot`定义一个HTML模板。再次使用常规DOM方法克隆模板并将其附加到您的shadow DOM中
5. 在页面任何您喜欢的位置使用自定义元素，就像使用常规HTML元素那样

## 生命周期

1. `connectedCallback`

   当自定义元素第一次被连接到文档DOM时被调用

2. `disconnectedCallback`

   当自定义元素与文档DOM断开连接时被调用

   https://developer.mozilla.org/zh-CN/docs/web/api/document/adoptnode

3. `adoptedCallback`

    当自定义元素被移动到新文档时被调用

4. attributeChangedCallback

   当自定义元素的一个属性被增加、移除或更改时被调用

## 兼容性

![image-20210306104516079](/Users/edz/Library/Application Support/typora-user-images/image-20210306104516079.png)

![image-20210309163509359](/Users/edz/Library/Application Support/typora-user-images/image-20210309163509359.png)

![image-20210306104556606](/Users/edz/Library/Application Support/typora-user-images/image-20210306104556606.png)

![image-20210306104724192](/Users/edz/Library/Application Support/typora-user-images/image-20210306104724192.png)

## 优缺点

### 优点

1. 简单直接，符合直觉，语义化强
2. 不需要编译，浏览器原生支持

### 缺点

1. 资源文件的引用取不到
2. 写法较繁琐，需要频繁操作dom

## 注意事项

1. 自定义元素名称必须包含-，使用单个单次不可以
2. 自定义元素默认为`display: inline`