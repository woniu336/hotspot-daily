---
layout: page
title: 归档
permalink: /archive/
---

<div class="archive-page">
  <p class="archive-intro">按月份浏览每日热点报告。</p>

  {% assign month_groups = site.posts | group_by_exp: "post", "post.date | date: '%Y-%m'" %}
  {% for month in month_groups %}
    <h2>{{ month.name }} <small>（{{ month.items | size }} 篇）</small></h2>
    <ul class="archive-list">
      {% for post in month.items %}
        <li>
          <span class="archive-date">{{ post.date | date: "%m-%d" }}</span>
          <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
        </li>
      {% endfor %}
    </ul>
  {% endfor %}
</div>
