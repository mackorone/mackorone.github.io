---
layout: default
title: About
---

![Headshot](/assets/images/headshot.jpg)

### Contact
<!-- Add hidden characters to prevent spam -->
- mackorone<span style="display:none">null</span>@gmail.com

### Links
<ul>
  {% for item in site.data.links %}
  <li>
    <a href="{{ item.link }}" data-proofer-ignore>
      {{ item.link | remove: "https://www."}}
    </a>
  </li>
  {% endfor %}
</ul>
