---
title: 微信小程序-JS调用组件
date: 2020-10-16 15:45:27
categories: 微信小程序
---

# Js调用微信小程序组件

### 1. getCurrentPages

场景：
- 获取当前页面栈。数组中第一个元素为首页，最后一个元素为当前页面。
- 可以在某个页面修改另一个页面的data或者调用另一个页面的方法。
注意：
- 不要尝试修改页面栈，会导致路由以及页面状态错误。
- 不要在 App.onLaunch 的时候调用 getCurrentPages()，此时 page 还没有生成。
var pages = getCurrentPages();
var prevPage = pages[pages.length - 2];//当前页面的上一个页面
prevPage.setData({})

// 测试页面例子
getCurrentPages()[0].showToast()

### 2. selectComponent

返回组件的实例
let toast = this.selectComponent() // 返回这个组件
toast.setData({}) 
toast.xxx() // 调用组件方法

### 3. 小程序的生命周期

- 小程序初始化完成后，页面首次加载触发onLoad，只会触发一次。
- 当小程序进入到后台，先执行页面onHide方法再执行应用onHide方法。
- 当小程序从后台进入到前台，先执行应用onShow方法再执行页面onShow方法。
- App Page Component生命周期：https://www.jianshu.com/p/1e3b5b507771

### 4. 微信小程序遇到的坑

textarea 
问题： ios默认有padding间距问题
解决方案：获取系统信息getSystemInfoSync(),给ios不加padding去处理

textarea 
问题：显示在最顶层的问题
解决方案：自定义组件，利用view代替textarea

Input
问题：设置靠右展示，当input获得焦点的时候，某些机型光标在placeholder左边的问题，正常是最右边
解决方案：自定义组件，placeholder利用view代替，判断input是否有值输入去控制placeholder的显示隐藏

picker选择器
问题：当设置mode="date"时，ios，start 和 end 两个属性必须为YYYY-MM-DD，Android是正常显示
解决方案：start和end 都传YYYY-MM-DD，因为没有ios手机测试，爬坑爬出来的

三个点的bug
问题：某些机型设置text-overflow: ellipsis;三个点尽然显示在中间
解决方案：利用substring截取字符串处理，开始不知道，后面修改有点麻烦。。

防连击处理
解决方案：因为小程序的ajax封装为异步的，Toast是渐入渐出，穿透了，必须放在callback里才不会连击

getDate()
问题：安卓可以正常显示，ios不行，因为当时想在wxml进行转换日期，因此引用了wxs函数，从而导致ios出现bug
解决方案：用moment函数进行转换，微信小程序提供的getDate有点鸡肋

页面跳转和参数传递
问题：因为我们从一个列表跳转详情，假如没有提供详情接口，需要把列表的这条参数携带过去，这样我们就会用到JSON.stringify()去序列化，但是，当这个json对象的value中包括&&这种字符串的时候，在详情页JSON.parse()去解析就会报错
解决方案：把序列化后的参数进行加密处理，encodeURIComponent()，然后在详情页在进行decodeURIComponent()转换
// 列表页处理
var obj = {a:1}
var obj1 = JSON.stringify(obj)
var encode = encodeURIComponent(obj1)

// 详情页处理
var decode = decodeURIComponent(encode);
var obj2 = JSON.parse(decode)

全局变量
问题：初始化的时候把app.globalData的数据绑定到data上了，当某个页面把全局数据改变，导致data数据没有实时更新
解决方案：在生命周期里重新设置这个data

组件间的通讯
在一个page里有两个相同的组件，但是处理逻辑不一样，他们各自有自己的data和生命周期，不能复用

Input type="number" "digit" 
问题：某些机型无法出来数字键盘
解决方案：正则控制输入

Checkbox radio 
问题：选中状态无法用checked属性清除
解决方案：重新渲染checkbox radio 或者自定义

穿透
问题：因为之前用的view overflow-y:auto去滚动的，这样的话即使加了catchtouchmove也会穿透页面
解决方案：在弹出层中用scroll-view去控制区块滚动，然后再最外层加catchtouchmove函数为空函数
