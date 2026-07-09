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
      <span class="legend-item"><span class="legend-dot dot-position"></span>position</span>
      <span class="legend-item"><span class="legend-dot dot-certification"></span>certification</span>
      <span class="legend-item"><span class="legend-dot dot-project"></span>project</span>
    </div>
  </div>

  {% assign all_items = site.positions | concat: site.certifications | concat: site.projects | sort: 'date' | reverse %}

  {% if all_items.size == 0 %}
    <p class="gitlog-empty">// nothing committed yet — check back soon</p>
  {% else %}
    <div class="gitlog-body" id="gitlog-body">
      <svg class="gitlog-svg" id="gitlog-svg" aria-hidden="true"></svg>
      <ol class="gitlog-rail" id="gitlog-rail">
        {% for item in all_items %}
          {% assign start_epoch = item.date | date: '%s' %}
          {% assign month_key = item.date | date: '%Y-%m' %}

          {% if item.type == 'position' %}
            {% assign end_epoch = "" %}
            {% if item.end_date %}{% assign end_epoch = item.end_date | date: '%s' %}{% endif %}
            <li class="gl-row" data-type="position" data-start="{{ start_epoch }}" data-end="{{ end_epoch }}" data-month="{{ month_key }}">
              <div class="gl-card gl-pos">
                <div class="gl-meta">
                  <span class="gl-badge badge-position">position</span>
                  <span class="gl-date">{{ item.date | date: "%b %Y" }}{% if item.end_date %} – {{ item.end_date | date: "%b %Y" }}{% else %} – present{% endif %}</span>
                </div>
                <a class="gl-link" href="{{ item.url | relative_url }}">
                  <h3 class="gl-title">{{ item.title }}</h3>
                  <h4 class="gl-org">{{ item.company }}{% if item.location %} <span class="gl-location">· {{ item.location }}</span>{% endif %}</h4>
                  <p class="gl-preview">{{ item.content | strip_html | truncatewords: 20 }}</p>
                  {% if item.tags %}
                    <div class="gl-tags">
                      {% for tag in item.tags limit: 4 %}<span class="gl-tag tag-position">{{ tag }}</span>{% endfor %}
                      {% if item.tags.size > 4 %}<span class="gl-tag tag-position">+{{ item.tags.size | minus: 4 }}</span>{% endif %}
                    </div>
                  {% endif %}
                </a>
              </div>
            </li>
          {% else %}
            <li class="gl-row" data-type="{{ item.type }}" data-start="{{ start_epoch }}" data-month="{{ month_key }}">
              <div class="gl-card gl-acc gl-{{ item.type }}">
                <div class="gl-meta">
                  <span class="gl-badge badge-{{ item.type }}">{{ item.type }}</span>
                  <span class="gl-date">{{ item.date | date: "%b %Y" }}</span>
                </div>
                <a class="gl-link" href="{{ item.url | relative_url }}">
                  <h3 class="gl-title">{{ item.title }}</h3>
                  {% if item.organization %}<h4 class="gl-org">{{ item.organization }}</h4>{% endif %}

                  {% if item.type == 'certification' %}
                    <div class="gl-chips">
                      {% if item.credential_id %}<span class="gl-chip">id: {{ item.credential_id }}</span>{% endif %}
                      {% if item.expiry_date %}<span class="gl-chip">expires {{ item.expiry_date | date: "%b %Y" }}</span>{% endif %}
                    </div>
                  {% endif %}

                  {% if item.type == 'project' %}
                    <div class="gl-chips">
                      {% if item.project_value %}<span class="gl-chip">value: {{ item.project_value }}</span>{% endif %}
                      {% if item.team_size %}<span class="gl-chip">team: {{ item.team_size }}</span>{% endif %}
                    </div>
                  {% endif %}

                  <p class="gl-preview">{{ item.content | strip_html | truncatewords: 14 }}</p>

                  {% if item.skills %}
                    <div class="gl-tags">
                      {% for skill in item.skills limit: 3 %}<span class="gl-tag tag-{{ item.type }}">{{ skill }}</span>{% endfor %}
                      {% if item.skills.size > 3 %}<span class="gl-tag tag-{{ item.type }}">+{{ item.skills.size | minus: 3 }}</span>{% endif %}
                    </div>
                  {% endif %}
                </a>
              </div>
            </li>
          {% endif %}
        {% endfor %}
      </ol>
    </div>
  {% endif %}
</div>

<script>
document.addEventListener('DOMContentLoaded', function () {
  var body = document.getElementById('gitlog-body');
  var rail = document.getElementById('gitlog-rail');
  var svg = document.getElementById('gitlog-svg');
  if (!rail || !svg) return;

  var COLORS = { position: '#1F8A55', certification: '#C98A2B', project: '#3457D5' };
  var RADIUS = 18;

  function ns(tag) { return document.createElementNS('http://www.w3.org/2000/svg', tag); }

  function boundaryY(centers, idx, dir) {
    if (dir === 'up') {
      return idx > 0 ? (centers[idx] + centers[idx - 1]) / 2 : centers[idx] - 30;
    }
    return idx < centers.length - 1 ? (centers[idx] + centers[idx + 1]) / 2 : centers[idx] + 30;
  }

  function branchPath(mainX, laneX, bottomY, topY, open) {
    var r = RADIUS;
    var d = 'M ' + mainX + ' ' + bottomY +
      ' C ' + mainX + ' ' + (bottomY - r) + ', ' + laneX + ' ' + (bottomY - r) + ', ' + laneX + ' ' + (bottomY - 2 * r) +
      ' L ' + laneX + ' ' + (open ? 0 : topY + 2 * r);
    if (!open) {
      d += ' C ' + laneX + ' ' + (topY + r) + ', ' + mainX + ' ' + (topY + r) + ', ' + mainX + ' ' + topY;
    }
    return d;
  }

  function draw() {
    var rows = Array.prototype.slice.call(rail.querySelectorAll('.gl-row'));
    if (!rows.length) return;

    var railRect = rail.getBoundingClientRect();
    var height = rail.offsetHeight;
    var width = body.clientWidth;
    svg.setAttribute('width', width);
    svg.setAttribute('height', height);
    svg.setAttribute('viewBox', '0 0 ' + width + ' ' + height);
    while (svg.firstChild) svg.removeChild(svg.firstChild);

    var centers = rows.map(function (r) {
      var rect = r.getBoundingClientRect();
      return (rect.top - railRect.top) + rect.height / 2;
    });

    var mainX = width / 2;
    var posLaneX = mainX - 130;
    var accLaneX = mainX + 130;

    var mainLine = ns('line');
    mainLine.setAttribute('x1', mainX); mainLine.setAttribute('x2', mainX);
    mainLine.setAttribute('y1', 0); mainLine.setAttribute('y2', height);
    mainLine.setAttribute('stroke', '#E3E4E0');
    mainLine.setAttribute('stroke-width', 2);
    svg.appendChild(mainLine);

    function addPath(d, color, dashed) {
      var p = ns('path');
      p.setAttribute('d', d);
      p.setAttribute('stroke', color);
      p.setAttribute('stroke-width', 3);
      p.setAttribute('fill', 'none');
      if (dashed) p.setAttribute('stroke-dasharray', '4,4');
      svg.appendChild(p);
    }

    function addDot(x, y, color) {
      var c = ns('circle');
      c.setAttribute('cx', x); c.setAttribute('cy', y); c.setAttribute('r', 6);
      c.setAttribute('fill', color);
      svg.appendChild(c);
    }

    var positions = rows.map(function (r, i) { return { r: r, i: i }; })
      .filter(function (o) { return o.r.dataset.type === 'position'; });

    positions.forEach(function (o, idx) {
      var ownRow = o.i;
      var bottomY = centers[ownRow];
      var nextNewer = positions[idx - 1];
      var ongoing = o.r.dataset.end === '';

      if (ongoing && idx === 0) {
        addPath(branchPath(mainX, posLaneX, bottomY, 0, true), COLORS.position, true);
      } else if (nextNewer) {
        var topY = boundaryY(centers, nextNewer.i, 'down');
        addPath(branchPath(mainX, posLaneX, bottomY, topY, false), COLORS.position, false);
      } else {
        addPath(branchPath(mainX, posLaneX, bottomY, boundaryY(centers, ownRow, 'up'), false), COLORS.position, false);
      }
      addDot(mainX, bottomY, COLORS.position);
    });

    var accRows = rows.map(function (r, i) { return { r: r, i: i }; })
      .filter(function (o) { return o.r.dataset.type !== 'position'; });

    var groups = [];
    accRows.forEach(function (o) {
      var month = o.r.dataset.month;
      var last = groups[groups.length - 1];
      if (last && last.month === month) { last.rows.push(o.i); }
      else groups.push({ month: month, type: o.r.dataset.type, rows: [o.i] });
    });

    groups.forEach(function (g) {
      var bottomIdx = g.rows[g.rows.length - 1];
      var topIdx = g.rows[0];
      var bottomY = centers[bottomIdx];
      var topY = boundaryY(centers, topIdx, 'up');
      var color = COLORS[g.type] || COLORS.certification;
      addPath(branchPath(mainX, accLaneX, bottomY, topY, false), color, false);
      g.rows.forEach(function (rowIdx) { addDot(mainX, centers[rowIdx], color); });
    });
  }

  draw();
  var resizeTimer;
  window.addEventListener('resize', function () {
    clearTimeout(resizeTimer);
    resizeTimer = setTimeout(draw, 150);
  });
  window.addEventListener('load', draw);
});
</script>