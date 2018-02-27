---
title: vue非父子之间的通信
date: 2017-08-24 10:01:38
categories: Vue
tags:
     - Vue
---

## 非父子组件通信

如果2个组件不是父子组件那么如何通信呢？这时可以通过eventHub来实现通信. 

所谓eventHub就是创建一个事件中心，相当于中转站，可以用它来传递事件和接收事件.

```javascript 1.8
// 1.在跟实例的data里定义一个基于vue的实例
    // 根组件（this.$root)
    new Vue({
     el: '#app',
     router,
     render: h => h(App),
     data: {
      // 空的实例放到根组件下，所有的子组件都能调用
      Bus: new Vue()
     }
    })
//2.在某个组件中绑定方法
    // 当前实例创建完成就监听这个事件
    created(){
     this.$root.Bus.$on('eventName', value => {
      this.print(value)
     })
    },
    methods: {
     print(value) {
      console.log(value)
     }
    },
    // 在组件销毁时别忘了解除事件绑定
    beforeDestroy() {
      this.$root.Bus.$off('eventName')
    },
// 3.在另外一个组件里调取跟实例绑定的事件
    <button @click="submit">提交<button>
     
    methods: {
      submit() {
       // 事件名字自定义，用不同的名字区别事件
       this.$root.Bus.$emit('eventName', 123)
      }
     }

```

## 父组件传递数据给子组件

直接通过props传递数据给子组件,然后子组件定义props获取传过来的属性

```javascript 1.8
// 父组件
    <Child :msg="msg"/>
// 子组件
    // 1.直接获取
    props:["msg"]
    // 2.指定传入的类型
    props: {
        msg: String //这样可以指定传入的类型，如果类型不对，会警告
    }
    // 3.指定默认值
    props: {
        msg: {
            type: String,
            default: "hello world" //这样可以指定默认的值
        }
    }
```

## 子组件与父组件通信

如果子组件想要改变数据呢？这在vue中是不允许的，因为vue只允许单向数据传递，这时候我们可以通过触发事件来通知父组件改变数据，
从而达到改变子组件数据的目的.

```javascript 1.8
// 父组件
    <div>
        <child @upup="change" :msg="msg"></child> //监听子组件触发的upup事件,然后调用change方法
    </div>
    methods: {
        change(msg) {
            this.msg = msg;
        }
    }
// 子组件
    <template>
        <div @click="up"></div>
    </template>
    
    methods: {
        up() {
            this.$emit('upup','hehe'); //主动触发upup方法，'hehe'为向父组件传递的数据
        }
    }
```
