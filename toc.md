---
layout: home
title: 目次
description: チュートリアルの目次
permalink: /toc/
---

## 目次

{% for chapter in site.data.chapters %}

### 第{{ chapter.id }}章　{{ chapter.title }}

{% assign secs = site.sections | where: "chap", chapter.id | sort: "sec" %}

{% for doc in secs %}
- [{{doc.sec}}　{{ doc.title | escape }}]({{ doc.url | relative_url }})
{% endfor %}

{% endfor %}