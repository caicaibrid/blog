---
title: Git
date: 2019-06-13 16:29:06
categories: Git
tags:
     - Git
---

## Git

### 版本控制

版本控制是一种记录一个或若干文件内容变化，以便将来查阅特定版本修订情况的系统。 

- 本地的版本控制系统

  相当于拷贝文件夹,以命名不同区分

- 集中化的版本控制系统

  多人开发一个项目,公用一个资源(SVN),必须联网工作,比如提交 还原 对比什么的(SVN本地有log记录吗?)

- 分布式的版本控制系统

  每一个个体都是一台服务器,可以随时看到提交历史,进行提交 还原 对比 什么的

### Git配置

- 生成SSH公钥

  1. 存放目录

     ```
     cd ~/.ssh
     ```

     .pub文件即为公钥文件

  2. 生成公钥

     ```
     ssh-keygen
     ```

- 配置信息

  ```
  git config --global user.name "xxx"
  git config --global user.email xxx@example.com
  git config --global alias.co checkout
  git config --global alias.br branch
  git config --global alias.ci commit
  git config --global alias.st status
  git config --list 查看配置信息
  ```

### Git常用命令

```
git init 初始化仓库
git clone [url] 克隆仓库
git clone [url] xxx 克隆仓库(自定义仓库名字)
git status 检查文件状态 (未修改，已修改或已放入暂存区)
git remote 远程仓库
git log 查看提交历史
gitk --all & (git log 图形化界面)
git tag 打标签
git reflog 查看所有分支的所有操作记录（包括已经被删除的 commit 记录和 reset 的操作）
git diff 文件对比
git branch 分支
git add 添加文件到暂存区
git commit 提交文件
git merge 合并分支
git cherry-pick 获取某个分支的单次提交,作为一个新的提交到当前分支上
git pull 从远端获取代码到本地,并自动合并
git fetch 从远端获取代码到本地,不会自动合并
git push 将本地分支的更新,推送到远端
git reset 修改HEAD的位置,撤销代码
git revert 撤销代码,不会丢失commit信息,会重新生成一个commitId
git rebase 变基
git stash 存储,保存当前的工作状态
git rm 
git mv
```

### 常用命令解析

- git remote 

  远程仓库

  git remote add [shortname] [url]

  添加一个远端仓库

  git remte -v 

  显示对应的克隆地址

  git remote show origin 

  查看远端仓库的详细信息

  git remote rename xx xxx

  修改某个远端仓库的本地简称

  git remote rm xx

  删除远端仓库

  ```
  git clone 所有分支,并push到一个新仓库
  
  git remote rename origin old_origin
  git remote add origin https://github.com/your_name/yyy.git
  git push -u origin --all
  git push -u origin --tags
  ```

- git tag

  打标签 显示已有的标签

  标签分为两类: 轻量级和含附注.

  轻量级标签就像是个不会变化的分支,实际上它就是个指向特定提交对象的引用

  含附注实际上是存储在仓库中的一个独立对象,它有自身的独立信息,包含着标签名,电子邮件和日期,标签说明

  git tag -a v1.0 -m "标签说明"

  -a 表示这是一个含附注类型的标签  -m表示对应的标签说明

  git show v1.0 查看相应标签的版本信息

  git push origin v1.0 提交标签到远端仓库

  git push origin --tags 提交所有的标签到仓库

  git tag -d v1.0 删除tag

  git tag push origin :refs/tags/v1.0 删除远端tag

- git diff

  git diff xxx 工作区与暂存区比较

  git diff HEAD xxx 工作区与HEAD比较

  git diff --cached xxx 暂存区与HEAD比较

  git diff branchName xxx 当前分支和branchName分支比较

- git branch 

  git branch -d xxx  ||  git branch -D xxx 删除本地分支 (-D表示删除的分支在当前分支没有合并过,所以需要强制删除)

  git push origin -d xxx  ||  git push origin :xxx 删除远端分支

  git branch -a 查看远程分支和本地分支  

  git branch -v 查看各个分支的最后一次提交信息

- git merge

  git merge --abort 取消合并

  git merge --containue 继续合并(解决完一个冲突后,执行git add .,执行git merge --containue会提示下一个冲突文件)

- git cherry-pick

  git cherry-pick --abort 取消合并

  git cherry-pick --containue 继续合并

- git pull <远程主机名> <远程分支名>:<本地分支名>

  git fetch和git pull区别

  git fetch 获取远端最新代码,不会自动合并

  git pull 相当于 git fetch 然后 git merge

- git push <远程主机名> <本地分支名>:<远程分支名>

  git push origin :master === git push origin -d master 删除远端分支

  git push origin --all 将本地的所有分支推送到远端

  git push origin --force 强制推送

  git push origin --tags 因为git push默认不会推送tag,所以可以指定推送--tags

- git reset

  git reset --soft commitId 撤销commit,修改的代码还都在,三个里改动最小的一个

  git reset --hard commitId 回退到当前commitId的状态,会丢失工作内容

  git reset --mixed commitId === git reset commitId (git reset默认模式)  撤销到当前的commitId,不会丢失之前的提交,会把之前的提交撤销为未执行add的操作,可以用来**合并多次提交为一个提交**

- git revert

  ```
  git revert --continue
  git revert --quit
  git revert --abort
  ```

- git rebase 

  - rebase操作可以把本地未push的分叉提交历史整理成直线；
  - rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。

  ```
  git rebase --containue 
  git rebase --abort 
  git rebase --skip  // 跳过当前冲突
  ```

- git stash

  ```
  git stash save "save message" 存储时,增加存储信息(不加save不方便查找当前的存储是什么)
  git stash list 查看存储了哪些
  git stash pop stash@{$num} 恢复之前的存储
  git stash show -p stash@{$num} 查看当前的存储,只能看到修改的文件(-p可以查看修改的内容)
  git stash apply stash@{$num} 应用某个存储,不会在存储列表删除
  git stash drop stash@{$num} 丢弃stash@{$num} 从列表中删除这个存储
  git stash clear 删除所有的stash
  ```

  

  
  
  
  
  
  
  
  
  