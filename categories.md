---
layout: page
title: 分类
permalink: /categories/
---

<div class="category-page">
  <p class="category-intro">按报告中出现的分析类别筛选文章（例如：国际、科技、财经）。</p>

  <div class="category-filters" id="category-filters">
    {% assign filter_cats = "全部,国际,科技,财经,社会,体育,娱乐,其他" | split: "," %}
    {% for c in filter_cats %}
      <button class="category-chip{% if c == '全部' %} active{% endif %}" data-cat="{{ c }}">{{ c }}</button>
    {% endfor %}
  </div>

  <ul class="category-post-list" id="category-post-list">
    {% for post in site.posts %}
      {% if post.report_categories %}
        {% assign cats = post.report_categories %}
      {% else %}
        {% assign cats = "其他" | split: "," %}
      {% endif %}
      <li class="category-post-item" data-cats="{{ cats | join: ',' }}">
        <div class="category-post-head">
          <span class="category-post-date">{{ post.date | date: "%Y-%m-%d" }}</span>
          <a class="category-post-title" href="{{ post.url | relative_url }}">{{ post.title }}</a>
        </div>
        <div class="category-post-tags">
          {% for cat in cats %}
            <span class="category-tag">{{ cat }}</span>
          {% endfor %}
        </div>
      </li>
    {% endfor %}
  </ul>

  <p class="category-empty" id="category-empty" hidden>当前分类下暂无文章。</p>
</div>

<script>
  (function () {
    const filters = document.getElementById('category-filters');
    const list = document.getElementById('category-post-list');
    const empty = document.getElementById('category-empty');
    if (!filters || !list) return;

    const chips = Array.from(filters.querySelectorAll('.category-chip'));
    const items = Array.from(list.querySelectorAll('.category-post-item'));

    function applyFilter(cat) {
      let visible = 0;
      items.forEach((item) => {
        const cats = (item.dataset.cats || '').split(',').map((s) => s.trim()).filter(Boolean);
        const show = cat === '全部' || cats.includes(cat);
        item.hidden = !show;
        if (show) visible += 1;
      });
      if (empty) empty.hidden = visible !== 0;
    }

    chips.forEach((chip) => {
      chip.addEventListener('click', () => {
        chips.forEach((c) => c.classList.remove('active'));
        chip.classList.add('active');
        applyFilter(chip.dataset.cat || '全部');
      });
    });

    applyFilter('全部');
  })();
</script>
