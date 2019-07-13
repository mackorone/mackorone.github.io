---
layout: default
title: Home
---
### Posts

<!-- Custom HTML because markdown doesn't format properly -->
<ul>
  {% for post in site.posts %}
    <li>
      ({{ post.date | date_to_string }}) <a href="{{ post.url}}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
