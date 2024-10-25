---
title: Home
---

This is a personal website. You can engage with me on <a href="https://twitter.com/Mycolaos">Twitter</a>.

<h2 style="margin-bottom: 1rem;">Engineering blog</h2>
<ul style="margin-left: 1rem;">
  {% for post in site.posts %}
    <li style="padding-left: 0; margin-bottom: .5rem;">
      <a href="{{ post.url }}">{{ post.title }}</a> {{ post.date | date_to_string }} 
    </li>
  {% endfor %}
</ul>
