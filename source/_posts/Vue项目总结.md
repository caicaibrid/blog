---
title: Vue项目总结
date: 2020-10-16 15:48:18
categories: Vue
---

## 生命周期

1. beforeCreate

   实例初始化之后，数据检测和watch还没初始化

2. Created

   数据检测和方法已完成

3. beforMount

   实例还没有挂载

4. mounted

   实例挂载完成

5. beforeUpdate

   数据更新时调用

6. updated

   数据更改完成之后

7. activated

   被keep-alive激活的组件

8. deactivated

   被keep-alive激活的组件停用时调用

9. beforDestory

   实例销毁之前调用

10. destoryed

    实例销毁之后调用

11. errorCapture

    捕获子孙组件的错误