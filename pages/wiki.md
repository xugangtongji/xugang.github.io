---
layout: page
title: 项目展示
description: 人越学越觉得自己无知
keywords: 维基, Wiki
comments: false
menu: ShowTime
permalink: /wiki/
---
> 如果不做一些很炫酷的东西？

<ul class="listing">
{% for wiki in site.wiki %}
{% if wiki.title != "Wiki Template" %}
<li class="listing-item"><a href="{{ site.url }}{{ wiki.url }}">{{ wiki.title }}</a></li>
{% endif %}
{% endfor %}
</ul>
