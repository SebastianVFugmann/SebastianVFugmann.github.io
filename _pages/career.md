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

  var MAIN_COLOR = '#4F46E5';
  var COLORS = { position: '#1F8A55', certification: '#C98A2B', project: '#3457D5' };
  var RADIUS = 16;
  var STUB = 40;

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
    mainLine.setAttribute('stroke', MAIN_COLOR);
    mainLine.setAttribute('stroke-width', 3);
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

    function addLine(x1, y1, x2, y2, color) {
      var l = ns('line');
      l.setAttribute('x1', x1); l.setAttribute('y1', y1);
      l.setAttribute('x2', x2); l.setAttribute('y2', y2);
      l.setAttribute('stroke', color);
      l.setAttribute('stroke-width', 2);
      svg.appendChild(l);
    }

    function addDot(x, y, color) {
      var c = ns('circle');
      c.setAttribute('cx', x); c.setAttribute('cy', y); c.setAttribute('r', 5);
      c.setAttribute('fill', color);
      svg.appendChild(c);
    }

    // A "commit": dot on the branch line + a straight stub out to the card.
    // dir: -1 stub points left (toward position cards), +1 points right (toward acc cards).
    function addCommit(laneX, y, color, dir) {
      addDot(laneX, y, color);
      addLine(laneX, y, laneX + dir * STUB, y, color);
    }

    // Positions: short symmetric bump around each row; current role stays open.
    var positions = rows.map(function (r, i) { return { r: r, i: i }; })
      .filter(function (o) { return o.r.dataset.type === 'position'; });

    positions.forEach(function (o, idx) {
      var ownRow = o.i;
      var bottomY = boundaryY(centers, ownRow, 'down');
      var ongoing = o.r.dataset.end === '';
      var open = ongoing && idx === 0;
      var topY = open ? 0 : boundaryY(centers, ownRow, 'up');

      addPath(branchPath(mainX, posLaneX, bottomY, topY, open), COLORS.position, open);
      addCommit(posLaneX, centers[ownRow], COLORS.position, -1);
    });

    // Certifications: no branch — dot on main, straight line out to card.
    var certRows = rows.map(function (r, i) { return { r: r, i: i }; })
      .filter(function (o) { return o.r.dataset.type === 'certification'; });

    certRows.forEach(function (o) {
      var y = centers[o.i];
      addLine(mainX, y, accLaneX, y, COLORS.certification);
      addDot(mainX, y, COLORS.certification);
    });

    // Projects: grouped by consecutive same-month, still branch/merge.
    // Every row in the group still gets its own commit (dot + stub).
    var projRows = rows.map(function (r, i) { return { r: r, i: i }; })
      .filter(function (o) { return o.r.dataset.type === 'project'; });

    var groups = [];
    projRows.forEach(function (o) {
      var month = o.r.dataset.month;
      var last = groups[groups.length - 1];
      if (last && last.month === month) { last.rows.push(o.i); }
      else groups.push({ month: month, rows: [o.i] });
    });

    groups.forEach(function (g) {
      var bottomIdx = g.rows[g.rows.length - 1];
      var topIdx = g.rows[0];
      var bottomY = boundaryY(centers, bottomIdx, 'down');
      var topY = boundaryY(centers, topIdx, 'up');

      addPath(branchPath(mainX, accLaneX, bottomY, topY, false), COLORS.project, false);
      g.rows.forEach(function (rowIdx) { addCommit(accLaneX, centers[rowIdx], COLORS.project, 1); });
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