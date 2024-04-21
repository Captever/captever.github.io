---
layout: archive
title: 한국어 페이지
---
여기는 한국어 페이지입니다.

<h3 class="archive__subtitle">{{ site.data.ui-text[site.locale].recent_posts | default: "Recent Posts" }}</h3>

{% assign posts = site.posts | where: 'lang', 'ko' %}
<div class="entries-{{ entries_layout }}">
  {% for post in posts %}
    {% include archive-single.html type=entries_layout %}
  {% endfor %}
</div>