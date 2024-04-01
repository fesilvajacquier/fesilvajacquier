---
layout: page
title: Posts
paginate:
  collection: posts
---

<ul>
  {% for post in paginator.resources %}
    <li>
      <a href="{{ post.relative_url }}">{{ post.data.title }}</a>
    </li>
  {% end %}
</ul>
