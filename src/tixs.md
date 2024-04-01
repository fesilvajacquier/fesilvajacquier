---
layout: page
title: TIX
paginate:
  collection: tixs
---

TIXs are TIL (Today I Learned), TIR (Today I Read), TIW (Today I Watched), etc.
Somehow it is a mixture of Felipe's <a href="https://fpsvogel.com/reading/" target="_blank">Reading</a> and Sergio's <a href="https://sergiodxa.com/bookmarks">Bookmarks</a>.
It is a way to keep track of the things I wished I had known (consumed) earlier, the things that demanded me more than ~30 minutes to discover / learn and that I suspect that eventually I will need to get back to them.

<ul>
  {% for tix in paginator.resources %}
    <li>
      {% if tix.data.external_url %}
        <a href="{{ tix.data.external_url }}" target="_blank">{{ tix.data.title }}</a>
      {% else %}
        <a href="{{ tix.relative_url }}">{{ tix.data.title }}</a>
      {% end %}
      ({%= tix.data.category %})
      {% if tix.data.read_at %}
        - {{ tix.data.read_at | date: "%Y-%m-%d" }}
      {% end %}
    </li>
  {% end %}
</ul>
