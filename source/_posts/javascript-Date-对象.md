---
title: javascript Date 对象
date: 2019-09-03 11:33:09
categories: Javascript
tags:
     - Javascript
---

### JS Date对象API

#### getDate()

返回指定日期对象为一个月的那一天

```javascript 1.8
let date = new Date("2019-09-09")
console.log(date.getDate()) // 9

```

#### getDay()

返回指定日期一周的第几天，即为星期几 （0~6） 0 表示星期日

```javascript 1.8
let date = new Date("2019-09-09");
console.log(date.getDay()) // 1

```

#### 

### JS 获取当前月的最后一天

```javascript 1.8
let date = new Date();
date.setDate(28); // 将日期设置为最小月的天数，防止跨月
date.setMonth(date.getMonth()+1); // 当前月的下个月
date.setDate(0); // date值为1~31，设置0即为上个月
console.log(date.toLocaleDateString());
```
