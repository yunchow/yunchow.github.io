---
layout: post
title: Python 怎么打包 Mac APP 应用
categories: [技术]
description: Python 怎么打包 Mac APP 应用
keywords: python, py2applet, py2app
---

## 安装 py2app
有几种方式，任用一种即可

+ pip install py2app
+ sudo easy_install py2app
+ sudo easy_install -U py2app

## 生成 setup.py

py2applet --make-setup tk1.py

如果命令找不到用下面的全路径

`
/System/Library/Frameworks/Python.framework/Versions/2.7/Extras/bin/py2applet --make-setup tk1.py
`

## 生成应用

python setup.py py2app

