---
layout: single
title: "Career Timeline"
permalink: /career/
author_profile: true
classes: wide
---

<div class="gitlog">

  <div class="gitlog-header">
    <p class="gitlog-branch"><span class="prompt">$</span> git log --graph --all</p>
    <h1>Career Timeline</h1>
    <div class="gitlog-legend">
      <span class="legend-item"><span class="legend-dot dot-position"></span>position <span class="legend-sub">(main)</span></span>
      <span class="legend-item"><span class="legend-dot dot-certification"></span>certification</span>
      <span class="legend-item"><span class="legend-dot dot-project"></span>project</span>
    </div>
  </div>

  {% assign all_items = site.positions | concat: site.certifications | concat: site.projects | sort: 'date' | reverse %}

  {% if all_items.size == 0 %}
    <p class="gitlog-empty">// nothing committed yet — check back soon</p>
  {% else %}
    {% assign total = all_items.size %}
    <ol class="gitlog-rail">
      {% for item in all_items %}
        {% assign commit_no = total | minus: forloop.index0 %}
        {% assign commit_id = commit_no | plus: 1000 %}

        {% if item.type == 'position' %}
          <li class="gitlog-row position-row">
            <div class="gitlog-node node-position" aria-hidden="true"></div>
            <a class="gitlog-card position-card" href="{{ item.url | relative_url }}">
              <div class="card-meta">
                <span class="commit-id">#{{ commit_id }}</span>
                <span class="card-badge badge-position">position</span>
                <span class="card-date">{{ item.date | date: "%b %Y" }}{% if item.end_date %} – {{ item.end_date | date: "%b %Y" }}{% else %} – present{% endif %}</span>
              </div>
              <h3 class="card-title">{{ item.title }}</h3>
              <h4 class="card-org">{{ item.company }}{% if item.location %} <span class="card-location">· {{ item.location }}</span>{% endif %}</h4>
              <p class="card-preview">{{ item.content | strip_html | truncatewords: 22 }}</p>
              {% if item.tags %}
                <div class="card-tags">
                  {% for tag in item.tags limit: 5 %}
                    <span class="tag tag-position">{{ tag }}</span>
                  {% endfor %}
                  {% if item.tags.size > 5 %}<span class="tag tag-position">+{{ item.tags.size | minus: 5 }}</span>{% endif %}
                </div>
              {% endif %}
            </a>
          </li>
        {% else %}
          <li class="gitlog-row accomplishment-row {{ item.type }}-row">
            <div class="gitlog-branch-line" aria-hidden="true"></div>
            <div class="gitlog-node node-{{ item.type }}" aria-hidden="true">
              <i class="{{ item.icon | default: 'fas fa-star' }}" aria-hidden="true"></i>
            </div>
            <a class="gitlog-card accomplishment-card {{ item.type }}-card" href="{{ item.url | relative_url }}">
              <div class="card-meta">
                <span class="commit-id">#{{ commit_id }}</span>
                <span class="card-badge badge-{{ item.type }}">{{ item.type }}</span>
                <span class="card-date">{{ item.date | date: "%b %Y" }}</span>
              </div>
              <h3 class="card-title">{{ item.title }}</h3>
              {% if item.organization %}<h4 class="card-org">{{ item.organization }}</h4>{% endif %}

              {% if item.type == 'certification' %}
                <div class="card-details">
                  {% if item.credential_id %}<span class="detail-chip">id: {{ item.credential_id }}</span>{% endif %}
                  {% if item.expiry_date %}<span class="detail-chip">expires {{ item.expiry_date | date: "%b %Y" }}</span>{% endif %}
                </div>
              {% endif %}

              {% if item.type == 'project' %}
                <div class="card-details">
                  {% if item.project_value %}<span class="detail-chip">value: {{ item.project_value }}</span>{% endif %}
                  {% if item.team_size %}<span class="detail-chip">team: {{ item.team_size }}</span>{% endif %}
                </div>
              {% endif %}

              <p class="card-preview">{{ item.content | strip_html | truncatewords: 16 }}</p>

              {% if item.skills %}
                <div class="card-tags">
                  {% for skill in item.skills limit: 4 %}
                    <span class="tag tag-{{ item.type }}">{{ skill }}</span>
                  {% endfor %}
                  {% if item.skills.size > 4 %}<span class="tag tag-{{ item.type }}">+{{ item.skills.size | minus: 4 }}</span>{% endif %}
                </div>
              {% endif %}
            </a>
          </li>
        {% endif %}
      {% endfor %}
    </ol>
  {% endif %}
</div>