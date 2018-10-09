---
layout: page
title: 我的相册
description: 没有链接的博客是孤独的
keywords: 友情链接
comments: false
menu: 相册
permalink: /links/
---

> 人在日光之下的一切劳碌有什么益处呢？一代过去，一代又来，大地却永远长存。太阳升起，太阳落下，匆忙回到升起之地。风吹向南，又转向北，循环不息，周而复始。江河涌流入海，海却不会满溢；江河从何处流出，又返回何处。万事令人厌烦，人述说不尽。眼看，看不饱；耳听，听不够。以往发生的事，将来还会发生；先前做过的事，将来也必再做。日光之下，根本没有新事。


<ul class="listing">
{% for links in site.links %}
{% if links.title != "links Template" %}
<li class="listing-item"><a href="{{ site.url }}{{ links.url }}">{{ links.title }}</a></li>
{% endif %}
{% endfor %}
</ul>

