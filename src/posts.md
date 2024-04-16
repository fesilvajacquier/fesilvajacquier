---
layout: default
paginate:
  collection: posts
---

<div style="gap: 1rem; display: flex; flex-direction: column;">
  {% for post in paginator.resources %}
    <sl-card class="card-header" style="width: 100%;">
      <div slot="header">
        <a href="{{ post.relative_url }}">
          {{ post.data.title }}
        </a>
      </div>
      <small>{{ post.data.date }}</small>
    </sl-card>
  {% end %}
</div>
