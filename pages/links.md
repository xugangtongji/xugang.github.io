---
layout: page
title: 我的相册
description: 没有链接的博客是孤独的
keywords: 友情链接
comments: false
menu: 相册
permalink: /links/
---

 >你证明我的存在：
 > 
 > 如果我不认识你，我没活过；
 > 
 > 如果至死不认识你，我没死，因为我没活过。
 
{% for link in site.data.links %}
* [{{ link.name }}]({{ link.url }})
{% endfor %}
