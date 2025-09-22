---
layout: page
title: Archive
permalink: /archive/
---

# Blog Archive

Browse all blog posts organized by date.

{% assign postsByYear = site.posts | group_by_exp: "post", "post.date | date: '%Y'" %}

{% for year in postsByYear %}
## {{ year.name }}

{% assign postsByMonth = year.items | group_by_exp: "post", "post.date | date: '%B'" %}
{% for month in postsByMonth %}
### {{ month.name }}

<ul class="post-list">
{% for post in month.items %}
  <li>
    <span class="post-date">{{ post.date | date: "%d" }} -</span>
    <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
    {% if post.categories %}
      <span class="post-categories">
        {% for category in post.categories %}
          <span class="category-tag">{{ category }}</span>
        {% endfor %}
      </span>
    {% endif %}
  </li>
{% endfor %}
</ul>
{% endfor %}
{% endfor %}

## Categories

{% assign categories = site.posts | map: 'categories' | join: ',' | split: ',' | uniq | sort %}
{% for category in categories %}
### {{ category }}

<ul class="post-list">
{% for post in site.posts %}
  {% if post.categories contains category %}
    <li>
      <span class="post-date">{{ post.date | date: "%B %d, %Y" }} -</span>
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
    </li>
  {% endif %}
{% endfor %}
</ul>
{% endfor %}

## Tags

{% assign tags = site.posts | map: 'tags' | join: ',' | split: ',' | uniq | sort %}
<div class="tag-cloud">
{% for tag in tags %}
  <span class="tag">{{ tag }}</span>
{% endfor %}
</div>

---

[← Back to Home](/)