---
type: pages
layout: archive
permalink: /web_application/
title: "WEB APPLICATION"
excerpt: "A list of all web application posts"
author_profile: true
classes: wide
---

Upcoming posts

1. DNS Recon
2. Amass
3. Assetfinder
4. httprobe
5. OWASP Top 10 -
  1. Injection
  2. Broken Authentication
  3. Sensitive Data Exposure
  4. XML External Entities (XXE)
  5. Broken Access Control
  6. Security Misconfiguration
  7. Cross-Site Scripting (XSS)
  8. Insecure Deserialisation
  9. Using Components with Known Vulnerabilities
  10. Insufficient Logging and Monitoring

<ul class="taxonomy__index">
  {% for post in site.categories.web_application %}
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
{% assign postsByYear = site.categories.web_application  | where_exp: "item", "item.hidden != true" | group_by_exp: 'post', 'post.date | date: "%Y"' %}
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
