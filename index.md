---
layout: default
title: Home
---
### Posts

<!-- Custom HTML because markdown doesn't format properly -->
<ul>
  {% for post in site.posts %}
    <li>
      <span class="text-monospace">({{ post.date | date_to_string }})</span>
      <a href="{{ post.url}}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
