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
        <span>Career Positions</span>
      </div>
      <div class="legend-item">
        <div class="legend-marker accomplishment-marker"></div>
        <span>Accomplishments</span>
      </div>
    </div>
  </div>

  {% assign career_items = site.career | sort: 'start_date' | reverse %}
  {% assign accomplishments = site.accomplishments | sort: 'date' | reverse %}
  
  <!-- Get all unique dates for timeline positioning -->
  {% assign all_dates = '' | split: '' %}
  {% for item in career_items %}
    {% assign all_dates = all_dates | push: item.start_date %}
    {% if item.end_date %}
      {% assign all_dates = all_dates | push: item.end_date %}
    {% endif %}
  {% endfor %}
  {% for item in accomplishments %}
    {% assign all_dates = all_dates | push: item.date %}
  {% endfor %}
  {% assign sorted_dates = all_dates | uniq | sort | reverse %}

  <div class="dual-timeline">
    <!-- Career Lane (Left) -->
    <div class="timeline-lane career-lane">
      <div class="lane-header">
        <h3>Career Positions</h3>
      </div>
      <div class="timeline-line career-line"></div>
      
      {% for date in sorted_dates %}
        {% assign career_on_date = career_items | where: 'start_date', date %}
        {% for position in career_on_date %}
          <div class="timeline-item career-item" data-date="{{ position.start_date }}">
            <div class="timeline-marker career-marker"></div>
            <div class="timeline-content career-content">
              <div class="item-type career-type">Position</div>
              <h3>{{ position.title }}</h3>
              <h4>{{ position.company }}</h4>
              <div class="location">{{ position.location }}</div>
              <p class="timeline-date">
                {{ position.start_date | date: "%b %Y" }}
                {% if position.end_date %} - {{ position.end_date | date: "%b %Y" }}
                {% else %} - Present
                {% endif %}
              </p>
              <div class="content-preview">
                {{ position.content | strip_html | truncatewords: 20 }}
              </div>
              {% if position.skills %}
                <div class="skills">
                  {% for skill in position.skills limit: 4 %}
                    <span class="skill-tag career-skill">{{ skill }}</span>
                  {% endfor %}
                  {% if position.skills.size > 4 %}
                    <span class="skill-tag career-skill">+{{ position.skills.size | minus: 4 }} more</span>
                  {% endif %}
                </div>
              {% endif %}
            </div>
          </div>
        {% endfor %}
      {% endfor %}
    </div>

    <!-- Accomplishments Lane (Right) -->
    <div class="timeline-lane accomplishments-lane">
      <div class="lane-header">
        <h3>Key Accomplishments</h3>
      </div>
      <div class="timeline-line accomplishments-line"></div>
      
      {% for accomplishment in accomplishments %}
        <div class="timeline-item accomplishment-item {{ accomplishment.type }}-item" data-date="{{ accomplishment.date }}">
          <div class="timeline-marker accomplishment-marker {{ accomplishment.type }}-marker">
            {% if accomplishment.icon %}
              <i class="{{ accomplishment.icon }}"></i>
            {% else %}
              <i class="fas fa-star"></i>
            {% endif %}
          </div>
          <div class="timeline-content accomplishment-content">
            <div class="item-type {{ accomplishment.type }}-type">{{ accomplishment.type | capitalize }}</div>
            <h3>{{ accomplishment.title }}</h3>
            {% if accomplishment.organization %}
              <h4>{{ accomplishment.organization }}</h4>
            {% endif %}
            <p class="timeline-date">{{ accomplishment.date | date: "%b %Y" }}</p>
            
            <!-- Type-specific details -->
            {% if accomplishment.type == 'certification' and accomplishment.credential_id %}
              <div class="cert-details">
                <span class="credential">ID: {{ accomplishment.credential_id }}</span>
                {% if accomplishment.expiry_date %}
                  <span class="expiry">Expires: {{ accomplishment.expiry_date | date: "%b %Y" }}</span>
                {% endif %}
              </div>
            {% endif %}
            
            {% if accomplishment.type == 'project' %}
              <div class="project-details">
                {% if accomplishment.project_value %}
                  <span class="project-value">Value: {{ accomplishment.project_value }}</span>
                {% endif %}
                {% if accomplishment.team_size %}
                  <span class="team-size">Team: {{ accomplishment.team_size }} people</span>
                {% endif %}
              </div>
            {% endif %}
            
            <div class="content-preview">
              {{ accomplishment.content | strip_html | truncatewords: 15 }}
            </div>
            
            {% if accomplishment.skills %}
              <div class="skills">
                {% for skill in accomplishment.skills limit: 3 %}
                  <span class="skill-tag accomplishment-skill">{{ skill }}</span>
                {% endfor %}
                {% if accomplishment.skills.size > 3 %}
                  <span class="skill-tag accomplishment-skill">+{{ accomplishment.skills.size | minus: 3 }}</span>
                {% endif %}
              </div>
            {% endif %}
          </div>
        </div>
      {% endfor %}
    </div>
  </div>
</div>