---
type: pages
layout: archive
permalink: /AD/
title: "Active Directory"
excerpt: "Posts on Active Directory exploitation"
author_profile: true
classes: wide
---

Upcoming Posts -

- Responder
- LLMNR poisoning
- SMB Relay
- SMB Signing
- LDAPS
- PowerView
- Bloodhound
- Pass the hash
- Cracking hashes
- Token Impersonation
- Kerberoasting
- Group Policy
- Mimikatz
- Golden Ticket Attacks
- Pivoting


<ul class="taxonomy__index">
  {% for post in site.categories.ActiveDirectory %}
  {% for year in postsInYear %}
    <li>
      <a href="#{{ year.name }}">
        <strong>{{ year.name }}</strong> <span class="taxonomy__count">{{ year.items | size }}</span>
      </a>
    </li>
  {% endfor %}
  {% endfor %}
</ul>

{% assign entries_layout = page.entries_layout | default: 'list' %}
{% assign postsByYear = site.categories.ActiveDirectory  | where_exp: "item", "item.hidden != true" | group_by_exp: 'post', 'post.date | date: "%Y"' %}
{% for year in postsByYear %}
  <section id="{{ year.name }}" class="taxonomy__section">
    <h2 class="archive__subtitle">{{ year.name }}</h2>
    <div class="entries-{{ entries_layout }}">
      {% for post in year.items %}
        {% include archive-single.html type=entries_layout %}
      {% endfor %}
    </div>
    <a href="#page-title" class="back-to-top">{{ site.data.ui-text[site.locale].back_to_top | default: 'Back to Top' }} &uarr;</a>
  </section>
{% endfor %}
