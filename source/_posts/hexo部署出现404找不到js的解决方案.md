---
title: hexo部署出现404找不到js的解决方案
date: 2016-11-06 20:24:55
categories: hexo
tags:
    - hexo
---

马拉个币，好不容易写了几篇文章，在部署的时候本地没问题，部署到github却出现各种404，原以为是自己配置哪里出现的问题，没想到是用的hexo的next主题出现了问题，找啊找啊找，终于找到了解决方案，蛋痛了！

<div>
    <img src="/img/fuck.jpg" width="100%"/>
</div>

这是NexT主题的问题，解决方法：
1.更新最新版本即可修复。(不建议更新，配置繁琐，伤不起)
2.根据作者的提示，首先修改source/vendors为source/lib，然后修改_config.yml， 将 _internal: vendors修改为_internal:lib 然后修改next底下所有引用source/vendors路径为source/lib。这些地方可以通过文件查找找出来。
主要集中在这几个文件中。
 - Hexo\themes\next.bowerrc
 - Hexo\themes\next.gitignore
 - Hexo\themes\next.javascript_ignore
 - Hexo\themes\next\bower.json

。修改完毕后，刷新重新g一遍就ok啦。

##### [hexo部署出现404找不到js的解决方案](https://github.com/hexojs/hexo/issues/2238)

<div>
    <img src="/img/fuck.jpg" width="100%"/>
</div>