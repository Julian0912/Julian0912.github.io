---
layout: page
title: About
description: 想要每天开心到冒泡
keywords: JulianFan, 何冥, 木子枫
comments: false
menu: 关于
permalink: /about/
---

喜欢搞技术，是因为觉得技术单纯，不像人情事故，总有那么多变故，那么多猜测。

技术很简单，它就在哪里，只要你用心学，只要有个好的老师指导，总是能学会的。

总是能学会的。

## 联系

{% for website in site.data.social %}
* {{ website.sitename }}：[@{{ website.name }}]({{ website.url }})
{% endfor %}

## 当前状态

{% for category in site.data.skills %}
### {{ category.name }}
<div class="btn-inline">
{% for keyword in category.keywords %}
<button class="btn btn-outline" type="button">{{ keyword }}</button>
{% endfor %}
</div>
{% endfor %}

