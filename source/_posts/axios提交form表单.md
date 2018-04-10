---
title: axios提交form表单
date: 2018-04-09 17:08:23
categories: Javascript
tags:
     - Javascript
---

# 利用axios第三方庫提交form表单数据

```javascript 1.8
let form = new FormData();

// 例如现在有个上传的表单,利用form表单上传文件

// 取文件
let file = document.getElementById("file");
let files = file.files; // 文件对象 = 类数组的对象 { 0:File对象,1:File对象,length:2}

for(let o of files){
    form.append("file",o)
}

form.append("key",JSON.stringify({name:"caicai"})); // 传其它参数,在form里边

axios({
    url:"xxx",
    method:"post",
    data: form,
    params:{ // 地址栏传参,其它参数
      xx:"xx"  
    },
    headers:{
        "Content-Type":"multipart/form-data"
    }
})
```
