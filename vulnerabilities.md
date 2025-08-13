---
layout: page
title: Vulnerabilities & POCs
permalink: /vulnerabilities/
---

<div class="collection-header">
  <h1 class="collection-title">🔓 Vulnerabilities & POCs</h1>
  <p class="collection-description">
    Proof-of-concept exploits, vulnerability research, and security findings from real-world applications.
    Responsible disclosure and educational content for the cybersecurity community.
  </p>
  <div class="collection-stats">
    <div class="stat-item">
      <span class="stat-number">{{ site.vulnerabilities | size }}</span>
      <div class="stat-label">Vulnerabilities</div>
    </div>
    <div class="stat-item">
      <span class="stat-number">{{ site.vulnerabilities | map: 'severity' | uniq | size | default: 'Various' }}</span>
      <div class="stat-label">Severity Levels</div>
    </div>
    <div class="stat-item">
      <span class="stat-number">{{ site.vulnerabilities | map: 'vuln_type' | uniq | size | default: 'Multiple' }}</span>
      <div class="stat-label">Vuln Types</div>
    </div>
  </div>
</div>

{% if site.vulnerabilities.size > 0 %}
  <ul class="post-list">
    {% for vuln in site.vulnerabilities reversed %}
      <li class="post-item">
        <div class="post-category vulnerability">VULNERABILITY</div>
        <div class="post-meta">
          {{ vuln.date | date: site.date_format }}
          {% if vuln.severity %} • {{ vuln.severity }} Severity{% endif %}
          {% if vuln.vuln_type %} • {{ vuln.vuln_type }}{% endif %}
        </div>
        <h2><a class="post-link" href="{{ vuln.url | relative_url }}">{{ vuln.title | escape }}</a></h2>
        {% if vuln.excerpt %}
          <div class="post-excerpt">{{ vuln.excerpt | strip_html | truncatewords: 30 }}</div>
        {% endif %}
        {% if vuln.tags %}
          <div class="post-tags">
            {% for tag in vuln.tags %}
              <span class="tag">{{ tag }}</span>
            {% endfor %}
          </div>
        {% endif %}
      </li>
    {% endfor %}
  </ul>
{% else %}
  <div class="empty-state">
    <p>🛡️ No vulnerability reports yet. Stay tuned for security research and POCs!</p>
  </div>
{% endif %}