---
layout: single
title: "Career Timeline"
permalink: /career/
author_profile: true
classes: wide
---

<div class="timeline-container">
  <div class="timeline-header">
    <h1>My Professional Journey</h1>
    <div class="timeline-legend">
      <div class="legend-item">
        <div class="legend-marker career-marker"></div>
        <span>Job Positions</span>
      </div>
      <div class="legend-item">
        <div class="legend-marker accomplishment-marker"></div>
        <span>Accomplishments</span>
      </div>
    </div>
  </div>

  {% assign all_items = site.positions | concat: site.certifications | concat: site.projects | sort: 'date' | reverse %}

  {% if all_items.size == 0 %}
    <p class="timeline-empty">Nothing to show yet — check back soon.</p>
  {% else %}
    <ol class="single-timeline">
      <div class="timeline-line" aria-hidden="true"></div>

      {% for item in all_items %}
        {% assign side = 'side-left' %}
        {% if forloop.index0 | modulo: 2 == 1 %}
          {% assign side = 'side-right' %}
        {% endif %}

        {% if item.type == 'position' %}
          <li class="timeline-item position-item {{ side }}" data-date="{{ item.date }}">
            <div class="timeline-marker career-marker" aria-hidden="true"></div>
            <a class="timeline-content career-content" href="{{ item.url | relative_url }}">
              <div class="item-type career-type">Position</div>
              <h3>{{ item.title }}</h3>
              <h4>{{ item.company }}</h4>
              {% if item.location %}
                <div class="location">{{ item.location }}</div>
              {% endif %}
              <p class="timeline-date">
                {{ item.date | date: "%b %Y" }}
                {% if item.end_date %} - {{ item.end_date | date: "%b %Y" }}
                {% else %} - Present
                {% endif %}
              </p>
              <div class="content-preview">
                {{ item.content | strip_html | truncatewords: 20 }}
              </div>
              {% if item.tags %}
                <div class="skills">
                  {% for tag in item.tags limit: 4 %}
                    <span class="skill-tag career-skill">{{ tag }}</span>
                  {% endfor %}
                  {% if item.tags.size > 4 %}
                    <span class="skill-tag career-skill">+{{ item.tags.size | minus: 4 }} more</span>
                  {% endif %}
                </div>
              {% endif %}
            </a>
          </li>
        {% else %}
          {% comment %} This is an accomplishment item: certification or project {% endcomment %}
          <li class="timeline-item accomplishment-item {{ item.type }}-item {{ side }}" data-date="{{ item.date }}">
            <div class="timeline-marker accomplishment-marker {{ item.type }}-marker" aria-hidden="true">
              {% if item.icon %}
                <i class="{{ item.icon }}" aria-hidden="true"></i>
              {% else %}
                <i class="fas fa-star" aria-hidden="true"></i>
              {% endif %}
            </div>
            <a class="timeline-content accomplishment-content" href="{{ item.url | relative_url }}">
              <div class="item-type {{ item.type }}-type">{{ item.type | capitalize }}</div>
              <h3>{{ item.title }}</h3>
              {% if item.organization %}
                <h4>{{ item.organization }}</h4>
              {% endif %}
              <p class="timeline-date">{{ item.date | date: "%b %Y" }}</p>

              {% if item.type == 'certification' %}
                <div class="cert-details">
                  {% if item.credential_id %}
                    <span class="credential">ID: {{ item.credential_id }}</span>
                  {% endif %}
                  {% if item.expiry_date %}
                    <span class="expiry">Expires: {{ item.expiry_date | date: "%b %Y" }}</span>
                  {% endif %}
                </div>
              {% endif %}

              {% if item.type == 'project' %}
                <div class="project-details">
                  {% if item.project_value %}
                    <span class="project-value">Value: {{ item.project_value }}</span>
                  {% endif %}
                  {% if item.team_size %}
                    <span class="team-size">Team: {{ item.team_size }} people</span>
                  {% endif %}
                </div>
              {% endif %}

              <div class="content-preview">
                {{ item.content | strip_html | truncatewords: 15 }}
              </div>

              {% if item.skills %}
                <div class="skills">
                  {% for skill in item.skills limit: 3 %}
                    <span class="skill-tag accomplishment-skill">{{ skill }}</span>
                  {% endfor %}
                  {% if item.skills.size > 3 %}
                    <span class="skill-tag accomplishment-skill">+{{ item.skills.size | minus: 3 }}</span>
                  {% endif %}
                </div>
              {% endif %}
            </a>
          </li>
        {% endif %}
      {% endfor %}
    </ol>
  {% endif %}
</div>