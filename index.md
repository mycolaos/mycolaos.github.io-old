---
title: Home
---

This is a personal website. You can engage with me on <a href="https://twitter.com/Mycolaos">Twitter</a>.

<h2>Engineering blog</h2>
<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a> {{ post.date | date_to_string }} 
    </li>
  {% endfor %}
</ul>
