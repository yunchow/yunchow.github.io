---
layout: page
title: About
description: 打码改变世界
keywords: YUNCHOW
comments: true
menu: 关于
permalink: /about/
---

初心不负，方得始终。

我是一个写代码的

我相信相信的力量

![mywife](imgs/r1.jpg)

## 联系

{% for website in site.data.social %}
* {{ website.sitename }}：[@{{ website.name }}]({{ website.url }})
{% endfor %}

## Skill Keywords

{% for category in site.data.skills %}
### {{ category.name }}
<div class="btn-inline">
{% for keyword in category.keywords %}
<button class="btn btn-outline" type="button">{{ keyword }}</button>
{% endfor %}
</div>
{% endfor %}


