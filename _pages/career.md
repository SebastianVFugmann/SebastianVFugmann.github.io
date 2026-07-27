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
    <div class="gitlog-legend">
      <span class="legend-item">— branch = position or project</span>
      <span class="legend-item">● on main = certification</span>
      <span class="legend-item">● colored = a distinct branch</span>
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
            <li class="gl-row" data-type="position" data-start="{{ start_epoch }}" data-end="{{ end_epoch }}" data-month="{{ month_key }}" data-company="{{ item.company }}">
              <div class="gl-card">
                <a class="gl-link" href="{{ item.url | relative_url }}">
                  <div class="gl-meta">
                    <span class="gl-badge">position</span>
                    <span class="gl-date">{{ item.date | date: "%b %Y" }}{% if item.end_date %} – {{ item.end_date | date: "%b %Y" }}{% else %} – present{% endif %}</span>
                  </div>
                  <p class="gl-title">{{ item.title }}</p>
                  <p class="gl-org">{{ item.company }}</p>
                  {% if item.tags %}
                    <div class="gl-tags-compact">
                      {% for tag in item.tags limit: 3 %}<span class="gl-tag">{{ tag }}</span>{% endfor %}
                    </div>
                  {% endif %}
                  <div class="gl-hover-detail">
                    {% if item.location %}<p class="gl-detail-location">{{ item.location }}</p>{% endif %}
                    <p class="gl-preview">{{ item.content | strip_html | truncatewords: 24 }}</p>
                    {% if item.tags.size > 3 %}
                      <div class="gl-tags">
                        {% for tag in item.tags offset: 3 %}<span class="gl-tag">{{ tag }}</span>{% endfor %}
                      </div>
                    {% endif %}
                    <span class="gl-more">Click for full details →</span>
                  </div>
                </a>
              </div>
            </li>
          {% else %}
            <li class="gl-row" data-type="{{ item.type }}" data-start="{{ start_epoch }}" data-month="{{ month_key }}">
              <div class="gl-card">
                <a class="gl-link" href="{{ item.url | relative_url }}">
                  <div class="gl-meta">
                    <span class="gl-badge">{{ item.type }}</span>
                    <span class="gl-date">{{ item.date | date: "%b %Y" }}</span>
                  </div>
                  <p class="gl-title">{{ item.title }}</p>
                  {% if item.organization %}<p class="gl-org">{{ item.organization }}</p>{% endif %}
                  {% if item.skills %}
                    <div class="gl-tags-compact">
                      {% for skill in item.skills limit: 3 %}<span class="gl-tag">{{ skill }}</span>{% endfor %}
                    </div>
                  {% endif %}
                  <div class="gl-hover-detail">
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
                    <p class="gl-preview">{{ item.content | strip_html | truncatewords: 20 }}</p>
                    {% if item.skills.size > 3 %}
                      <div class="gl-tags">
                        {% for skill in item.skills offset: 3 %}<span class="gl-tag">{{ skill }}</span>{% endfor %}
                      </div>
                    {% endif %}
                    <span class="gl-more">Click for full details →</span>
                  </div>
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

  var INK = '#14171A';
  var PALETTE = ['#059669', '#D97706', '#0891B2', '#C026D3', '#65A30D'];
  // Add more hex values here any time — everything below reads from this
  // one array, so nothing else needs to change to make use of a new color.

  function nextColor(i) { return PALETTE[i % PALETTE.length]; }

  var RADIUS = 16;
  var DOT_OFFSET = 26;
  var DOT_RADIUS = 9;
  var DOT_CLEARANCE = DOT_OFFSET + DOT_RADIUS + RADIUS * 2 + 12;
  var CARD_FRAC = 0.36;  // must match $gl-card width in main.scss
  var GAP_FRAC = 0.05;

  function ns(tag) { return document.createElementNS('http://www.w3.org/2000/svg', tag); }

  // Boundary = midpoint between a row and its immediate neighbor (quick
  // branch-out / merge points). For rows with no neighbor (first/last in
  // the whole list), clear past that row's own rendered height plus the
  // commit dot's footprint, so the curve never cuts through the card or
  // leaves the bottommost dot floating off the line.
  function boundaryY(centers, idx, dir, rows) {
    if (dir === 'up') {
      if (idx > 0) {
        var halfHeight = rows[idx].getBoundingClientRect().height / 2;
        return centers[idx] - halfHeight - DOT_OFFSET;  // ← extend below card
      }
      var halfUp = rows[idx].getBoundingClientRect().height / 2;
      return centers[idx] - halfUp - DOT_CLEARANCE;
    }
    if (idx < centers.length - 1) {
      var halfHeight = rows[idx].getBoundingClientRect().height / 2;
      return centers[idx] + halfHeight + DOT_OFFSET;  // ← extend below card
    }
    var halfDown = rows[idx].getBoundingClientRect().height / 2;
    return centers[idx] + halfDown + DOT_CLEARANCE;
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

    // Reserve enough space above/below the rail so the very first and last
    // card's branch curves — and their commit dots — have room to render
    // before hitting the SVG edge.
    var firstH = rows[0].getBoundingClientRect().height;
    var lastH = rows[rows.length - 1].getBoundingClientRect().height;
    var topPad = Math.ceil(firstH / 2 + DOT_CLEARANCE + 8);
    var bottomPad = Math.ceil(lastH / 2 + DOT_CLEARANCE + 8);
    rail.style.paddingTop = topPad + 'px';
    rail.style.paddingBottom = bottomPad + 'px';

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

    // Lane x targets each card's OWN horizontal center, so the branch line
    // runs straight through the card visually — entering from below,
    // merging out the top — rather than beside it with a separate stub.
    var posLaneX = (width * CARD_FRAC) / 2;
    var accLaneX = width - (width * CARD_FRAC) / 2;
    var certConnectorX = width * (1 - CARD_FRAC) - (width * GAP_FRAC);

    var mainLine = ns('line');
    mainLine.setAttribute('x1', mainX); mainLine.setAttribute('x2', mainX);
    mainLine.setAttribute('y1', 0); mainLine.setAttribute('y2', height);
    mainLine.setAttribute('stroke', INK);
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
      c.setAttribute('cx', x); c.setAttribute('cy', y); c.setAttribute('r', DOT_RADIUS);
      c.setAttribute('fill', color);
      svg.appendChild(c);
    }

    // Places a larger commit dot below a card, still on the given lane's
    // x — so it reads as sitting on that item's branch.
    function addCommitDot(laneX, rowIdx, color) {
      var cardHeight = rows[rowIdx].getBoundingClientRect().height;
      var y = centers[rowIdx] + cardHeight / 2 + DOT_OFFSET;
      var c = ns('circle');
      c.setAttribute('cx', laneX); c.setAttribute('cy', y); c.setAttribute('r', DOT_RADIUS);
      c.setAttribute('fill', color);
      svg.appendChild(c);
    }

    // Certifications: a single point in time, not a branch — a neutral dot
    // directly on the main line with a straight connector out to the card.
    var certRows = rows.map(function (r, i) { return { r: r, i: i }; })
      .filter(function (o) { return o.r.dataset.type === 'certification'; });

    certRows.forEach(function (o) {
      var y = centers[o.i];
      addLine(mainX, y, certConnectorX, y, INK);
      addDot(mainX, y, INK);
    });

    // Positions: consecutive same-company positions (ignoring non-position
    // rows in between) merge into one branch with multiple commits inside
    // it. The current role, if it has no end date, stays open with a
    // dashed line to the top (HEAD).
    var positions = rows.map(function (r, i) { return { r: r, i: i }; })
      .filter(function (o) { return o.r.dataset.type === 'position'; });

    var posGroups = [];
    positions.forEach(function (o) {
      var company = o.r.dataset.company;
      var last = posGroups[posGroups.length - 1];
      if (last && last.company === company) { last.rows.push(o.i); }
      else posGroups.push({ kind: 'position', company: company, rows: [o.i] });
    });

    // Projects: grouped by consecutive same-month, same branch/merge
    // treatment as positions.
    var projRows = rows.map(function (r, i) { return { r: r, i: i }; })
      .filter(function (o) { return o.r.dataset.type === 'project'; });

    var projGroups = [];
    projRows.forEach(function (o) {
      var month = o.r.dataset.month;
      var last = projGroups[projGroups.length - 1];
      if (last && last.month === month) { last.rows.push(o.i); }
      else projGroups.push({ kind: 'project', rows: [o.i] });
    });

    // One combined list, sorted by earliest row (= chronological, newest
    // first), so palette assignment reflects true encounter order across
    // both lanes rather than two independent counters.
    var allGroups = posGroups.concat(projGroups).sort(function (a, b) {
      return a.rows[0] - b.rows[0];
    });

    var newestPositionIdx = allGroups.findIndex(function (g) { return g.kind === 'position'; });

    allGroups.forEach(function (g, idx) {
      var color = nextColor(idx);
      var bottomIdx = g.rows[g.rows.length - 1];
      var topIdx = g.rows[0];
      var bottomY = boundaryY(centers, bottomIdx, 'down', rows);

      var laneX = g.kind === 'position' ? posLaneX : accLaneX;
      var topRow = rows[topIdx];
      var ongoing = g.kind === 'position' && idx === newestPositionIdx && topRow.dataset.end === '';
      var topY = ongoing ? 0 : boundaryY(centers, topIdx, 'up', rows);

      addPath(branchPath(mainX, laneX, bottomY, topY, ongoing), INK, ongoing);
      g.rows.forEach(function (rowIdx) { addCommitDot(laneX, rowIdx, color); });
    });
  }

  draw();

  // Flip the hover-detail panel above the card instead of below when there
  // isn't enough viewport space beneath it — otherwise the last row (and
  // any row near the bottom of the screen) gets clipped or pushes in a
  // scrollbar.
  function setupHoverFlip() {
    var cards = rail.querySelectorAll('.gl-card');
    cards.forEach(function (card) {
      var detail = card.querySelector('.gl-hover-detail');
      if (!detail) return;

      function check() {
        card.classList.remove('gl-flip-up');
        var rect = card.getBoundingClientRect();
        var neededSpace = detail.scrollHeight + 16;
        var spaceBelow = window.innerHeight - rect.bottom;
        if (spaceBelow < neededSpace) {
          card.classList.add('gl-flip-up');
        }
      }

      card.addEventListener('mouseenter', check);
      card.addEventListener('focusin', check);
    });
  }
  setupHoverFlip();

  var resizeTimer;
  window.addEventListener('resize', function () {
    clearTimeout(resizeTimer);
    resizeTimer = setTimeout(draw, 150);
  });
  window.addEventListener('load', draw);
});
</script>