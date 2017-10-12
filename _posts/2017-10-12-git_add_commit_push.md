---
layout: post
title: Git 提交小技巧
categories: [技术]
description: Git 提交小技巧
keywords: git, git push
---

在我们新增一个文件后，然后将更新提交至远程仓库，我们需要如下几步
```
git add .
git commit -m "add new file"
git push
```

这是最常见的操作，在提交前我们可能还需要进行 diff 操作

```
git diff
```

目前看上去一切都是那么的美好，但是当我们在频繁的新增或者修改文件时，如此的重复操作是令人讨论的，程序员都是很懒的，那么有没有办法一条命令就能搞定呢？

```
git add . && git commit -am add && git push
```




