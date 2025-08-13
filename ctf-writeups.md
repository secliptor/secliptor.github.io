---
layout: page
title: CTF Writeups
permalink: /ctf-writeups/
---

<div class="collection-header">
  <h1 class="collection-title">🚩 CTF Writeups</h1>
  <p class="collection-description">
    Detailed solutions and methodologies for Capture The Flag challenges I've conquered.
    From web exploitation to reverse engineering, cryptography to forensics.
  </p>
  <div class="collection-stats">
    <div class="stat-item">
      <span class="stat-number">{{ site.ctf-writeups | size }}</span>
      <div class="stat-label">Writeups</div>
    </div>
    <div class="stat-item">
      <span class="stat-number">{{ site.ctf-writeups | map: 'difficulty' | uniq | size | default: 'Various' }}</span>
      <div class="stat-label">Difficulty Levels</div>
    </div>
    <div class="stat-item">
      <span class="stat-number">{{ site.ctf-writeups | map: 'ctf_name' | uniq | size | default: 'Multiple' }}</span>
      <div class="stat-label">CTF Events</div>
    </div>
  </div>
</div>

{% if site.ctf-writeups.size > 0 %}
  <ul class="post-list">
    {% for writeup in site.ctf-writeups reversed %}
      <li class="post-item">
        <div class="post-category ctf">CTF</div>
        <div class="post-meta">
          {{ writeup.date | date: site.date_format }}
          {% if writeup.ctf_name %} • {{ writeup.ctf_name }}{% endif %}
          {% if writeup.difficulty %} • {{ writeup.difficulty }}{% endif %}
        </div>
        <h2><a class="post-link" href="{{ writeup.url | relative_url }}">{{ writeup.title | escape }}</a></h2>
        {% if writeup.excerpt %}
          <div class="post-excerpt">{{ writeup.excerpt | strip_html | truncatewords: 30 }}</div>
        {% endif %}
        {% if writeup.tags %}
          <div class="post-tags">
            {% for tag in writeup.tags %}
              <span class="tag">{{ tag }}</span>
            {% endfor %}
          </div>
        {% endif %}
      </li>
    {% endfor %}
  </ul>
{% else %}
  <div class="empty-state">
    <p>🔍 No CTF writeups yet. Check back soon for detailed challenge solutions!</p>
  </div>
{% endif %}