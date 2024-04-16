---
layout: default
paginate:
  collection: tixs
---

TIXs are TIL (Today I Learned), TIR (Today I Read), TIW (Today I Watched), etc.
Somehow it is a mixture of Felipe's <a href="https://fpsvogel.com/reading/" target="_blank">Reading</a> and Sergio's <a href="https://sergiodxa.com/bookmarks">Bookmarks</a>.

<div  style="gap: 1rem; display: flex; flex-direction: column;">
  {% for tix in paginator.resources %}
    <sl-card class="card-header" style="width: 100%;">
      <div slot="header">
        {% if tix.data.external_url %}
          <a href="{{ tix.data.external_url }}" target="_blank">{{ tix.data.title }}</a>
        {% else %}
          {{ tix.data.title }}
        {% end %}
      </div>
      <div style="display: flex; gap: 1rem;">
        {% if tix.data.read_at %}
          <small style="display: flex; gap: 0.5rem; align-items: center;">
              <sl-icon name="calendar-check"></sl-icon> {{ tix.data.read_at | date: "%Y-%m-%d" }}
          </small>
        {% end %}
        {% if tix.data.category %}
          <small style="display: flex; gap: 0.5rem; align-items: center;">
              <sl-icon name="{{ tix.data.icon || 'tag' }}"></sl-icon>
              {{ tix.data.category }}
          </small>
        {% end %}
      </div>
      <div style="display: flex; gap: 1rem;">
        {% if tix.data.tags %}
          {% for tag in tix.data.tags %}
            <span>#{{ tag }}</span>
          {% end %}
        {% end %}
      </div>
    </sl-card>
  {% end %}
</div>
