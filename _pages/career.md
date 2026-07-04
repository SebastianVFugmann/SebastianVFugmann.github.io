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

  {% assign career_items = site.positions %}
  {% assign certification_items = site.certifications %}
  {% assign project_items = site.projects %}
  {% assign all_items = career_items | concat: certification_items | concat: project_items | sort: 'date' | reverse %}

  <div class="single-timeline">
    <div class="timeline-line"></div>

    {% for item in all_items %}
      {% if item.type == 'position' %}
        <div class="timeline-item position-item" data-date="{{ item.date }}">
          <div class="timeline-marker career-marker"></div>
          <div class="timeline-content career-content">
            <div class="item-type career-type">Position</div>
            <h3>{{ item.title }}</h3>
            <h4>{{ item.company }}</h4>
            <div class="location">{{ item.location }}</div>
            <p class="timeline-date">
              {{ item.date | date: "%b %Y" }}
              {% if item.end_date %} - {{ item.end_date | date: "%b %Y" }}
              {% else %} - Present
              {% endif %}
            </p>
            <div class="content-preview">
              {{ item.content | strip_html | truncatewords: 20 }}
            </div>
            {% if item.skills %}
              <div class="skills">
                {% for skill in item.skills limit: 4 %}
                  <span class="skill-tag career-skill">{{ skill }}</span>
                {% endfor %}
                {% if item.skills.size > 4 %}
                  <span class="skill-tag career-skill">+{{ item.skills.size | minus: 4 }} more</span>
                {% endif %}
              </div>
            {% endif %}
          </div>
        </div>
      {% else %}
        {% comment %} This is an accomplishment item {% endcomment %}
        <div class="timeline-item accomplishment-item {{ item.type }}-item" data-date="{{ item.date }}">
          <div class="timeline-marker accomplishment-marker {{ item.type }}-marker">
            {% if item.icon %}
              <i class="{{ item.icon }}"></i>
            {% else %}
              <i class="fas fa-star"></i>
            {% endif %}
          </div>
          <div class="timeline-content accomplishment-content">
            <div class="item-type {{ item.type }}-type">{{ item.type | capitalize }}</div>
            <h3>{{ item.title }}</h3>
            {% if item.organization %}
              <h4>{{ item.organization }}</h4>
            {% endif %}
            <p class="timeline-date">{{ item.accomplishment_date | date: "%b %Y" }}</p>

            {% if item.type == 'certification' and item.credential_id %}
              <div class="cert-details">
                <span class="credential">ID: {{ item.credential_id }}</span>
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
          </div>
        </div>
      {% endif %}
    {% endfor %}
  </div>
</div>