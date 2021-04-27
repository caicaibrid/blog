---
title: npm私库
date: 2021-04-14 14:50:04
tags: JavaScript
---

### 什么是私有Npm仓库？

私有Npm仓库是专门给一些公司内部开发者提供的私有Npm包管理仓库，类似还有私有Git仓库

### 为什么要用私有Npm仓库？

公司内部开发的包有可能是不开源共享的，只是给公司内部人员使用的，那么这些包就不可能上传到Npm官网上

虽然Npm官网也支持私有，但是需要收费。

这时候本地私有Npm仓库就解决了这个问题，本地私有仓库具有免费、本地化、私有化、高速下载的特点。

因为部署在本地，所以下载包的速度是非常快的。

### 怎么搭

1. verdaccio
2. sinopia 不更新了

使用文件系统作为存储，仅保存用户需要的包，如果本地仓库没有对应的包，则从指定的registry下载，默认为npmjs.org，可以改成淘宝的镜像。

### 安装步骤

1. 全局安装

   ```javascript
   npm install -g sinopia
   npm install -g verdaccio
   ```

2. 修改配置

   查看npm全局包的位置

   ```bash
   npm root -g
   ```

   进入npm全局安装包的目录，找到对应的npm包

   ``````
   verdaccio/conf/default.yaml修改配置
   ``````

3. 启动

   ``````
   sinopia
   verdaccio
   ``````

### 主要配置文件

- config.yaml 启动后生成的是仓库的配置文件，仓库的配置都写在里面
- htpasswd 当有用户注册后会生成的用户账号信息文件，记录账号和密码以及创建日期
- storage 所有上传的包都保存在这

### 配置项

在配置前请一定注意配置文件内的缩进，不然会启动会报错

打开 config.yaml 配置文件，我们可以看到下面几项

- storage 设置用户上传包的存放目录
- plugins 插件目录
- web 前端页面的配置，设置访问页的标题、图片等
- i18n 设置页面的语言
  - web 设置web的默认语言 zh-CN
- auth 设置账号相关的内容
  - file 存放账号密码的文件目录
  - max_user 最大注册用户，-1 代表禁止注册，但可以手动在账号文件添加
  - htpasswd 设置账号密码相关
- uplinks 设置上游匹配，主要用于包匹配不到时，系统该往哪里去找这个包
- packages 包相关配置，用于设置包的上传、下载、访问的权限控制
  - $all 所有登录、未登录者都可以访问
  - $authenticated 只有登录者可以访问
  - a、b、v等 指定用户名、只有该用户可以访问
  - access 可访问权，能否下载
  - publish 发布权
  - unpublish 取消发布权
  - 权限的控制大致有三种

### nrm

nrm是npm源管理器，允许你快速npm源间切换

1. 全局安装

   ```npm install -g nrm```

2. 常用命令

   ``````
   nrm ls 查看所有源
   nrm use xxx 使用源
   ``````

