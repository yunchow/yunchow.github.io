---
layout: post
title: Git 提交小技巧
categories: [技术]
description: Git 提交小技巧
keywords: git, git push
---

在我们新增一个文件后，然后将更新提交至远程仓库，我们需要如下几步
```
git status
git add .
git commit -m "add new file"
git push
```
这是最常见的操作，在提交前我们可能还需要进行 diff 操作
```
git diff
```
目前看上去一切都是那么的美好，但是当我们在频繁的新增或者修改文件时，如此的重复操作是令人崩溃的，程序员都是很懒的，那么有没有办法一条命令就能搞定呢？
```
git add . && git commit -am add && git push
```
这样我们可以将文件提交至远程仓库。但是还是感觉不完美，因为每次要打的字太多。有一个办法就是将上面这一行命令起一个别名即可
```
gitpush() {
  git status
  git add . && git commit -m "$*" && git push
}
alias gp=gitpush
```
将上面这段代码添加至 ～/.bash_profile 文件后就可以一个命令完成所有操作，完美！
```
gp add new file
```
加好后别忘了执行
```
. ./.bash_profile
```
开始懒人的幸福生活，哈哈！！！

> 参考

stackoverflow: [git add, commit and push commands in one](https://stackoverflow.com/questions/19595067/git-add-commit-and-push-commands-in-one)

