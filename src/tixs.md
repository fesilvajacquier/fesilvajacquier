---
layout: page
title: TIX
---

TIXs are:

- TIL (Today I Learned)
- TIR (Today I Read)
- TIG (Today I Watched)
- TIC (Today I Listened)
- TIE (Today I Experienced)
- TIA (Today I Achieved)
- TID (Today I Discovered)
- etc.

It is a way to keep track of the things I wished I had known earlier, the things that demanded me more than ~30 minutes to discover / learn and that I suspect that eventually I will need to get back to them.

<ul>
  {% collections.tixs.resources.each do |tix| %}
    <li>
      <a href="{{ tix.relative_url }}">{{ tix.data.title }}</a>
    </li>
  {% end %}
</ul>
