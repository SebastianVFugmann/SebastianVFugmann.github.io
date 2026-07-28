---
title: "Sebastian Vestergaard Fugmann"
subtitle: "IT Consultant & Software Developer"
layout: single
permalink: /
author_profile: true
open_to_opportunities: true # Toggle this to false to hide the "open to opportunities" badge
---

<div class="about-hero">
  <p class="about-tagline">
    Software Developer specializing in the <strong>Salesforce ecosystem</strong> — including
    <strong>Sales Cloud</strong> and <strong>Marketing Cloud</strong> — with <strong>3 years</strong> of experience
    building and configuring solutions on the platform for a variety of business areas.
  </p>
  <p class="about-quickfacts">
    📍 Denmark &nbsp;·&nbsp;
    💼 3 years experience &nbsp;·&nbsp;
    🛠️ Salesforce · Sales Cloud · Marketing Cloud
    {% if page.open_to_opportunities %}
    &nbsp;·&nbsp;<span class="about-badge">✅ Open to new opportunities</span>
    {% endif %}
  </p>
</div>

## Summary

I'm an IT consultant and software developer with 3 years of experience working in the **Salesforce ecosystem**,
including **Sales Cloud** and **Marketing Cloud**. I focus on configuring and building solutions that fit how
a business actually works, and on keeping things maintainable as requirements evolve. I'm still early in my
career and growing my scope with every project — currently looking for roles where I can keep deepening my
Salesforce expertise and take on more ownership. Open to **[target role/domain]**, ideally
**[remote/hybrid/onsite, Denmark/EU/etc.]**.

## Core Competencies

**Salesforce**
`Sales Cloud` `Marketing Cloud` `Apex` `Flow` `SOQL` `[...]`

**Languages**
`Python` `JavaScript` `[...]`

**Frameworks & Tools**
`React` `Node.js` `Jekyll` `[...]`

**Infrastructure & Cloud**
`AWS` `[...]`

**Domain Knowledge**
`[e.g. B2B sales operations, marketing automation — remove if not applicable]`

## Highlighted Work

{% assign highlights = site.projects | concat: site.positions | sort: 'date' | reverse | slice: 0, 3 %}
{% for item in highlights %}
- **[{{ item.title }}]({{ item.url | relative_url }})**{% if item.company %} — {{ item.company }}{% elsif item.organization %} — {{ item.organization }}{% endif %} ({{ item.date | date: "%b %Y" }})
  {{ item.content | strip_html | truncatewords: 28 }}
{% endfor %}

## Certifications

{% for cert in site.certifications %}
- **[{{ cert.title }}]({{ cert.url | relative_url }})** — {{ cert.organization }} ({{ cert.date | date: "%Y" }})
{% endfor %}

## Explore More

- **[Career Timeline](/career/)** — full history of every role, project, and certification
- **[Projects](/projects/)** - Detailed overview of all current and past projects alongside insights into the development.
- **[GitHub](https://github.com/SebastianVFugmann)** — code and open-source contributions

## Let's Connect

- 🔗 **GitHub:** [github.com/SebastianVFugmann](https://github.com/SebastianVFugmann)
- 💼 **LinkedIn:** [linkedin.com/in/sebastian-fugmann](https://www.linkedin.com/in/sebastian-fugmann-ab224a226/)
- ✉️ **Email:** [sf15006@gmail.com](mailto:sf15006@gmail.com)
- 📄 **[Download CV (PDF)](/assets/files/cv-sebastian-fugmann.pdf)** <!-- add the file, or remove this line if not available yet -->