---
layout: page
title: 相册
description: 
keywords: album
comments: false
menu: 相册
permalink: /wiki/ 
---
 >你证明我的存在：
 > 如果我不认识你，我没活过；
 > 如果至死不认识你，我没死，因为我没活过。
 
<ul class="listing">
{% for wiki in site.wiki %}
{% if wiki.title != "Wiki Template" %}
<li class="listing-item"><a href="{{ site.url }}{{ wiki.url }}">{{ wiki.title }}</a></li>
{% endif %}
{% endfor %}
</ul>
