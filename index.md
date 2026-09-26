---
title: "Sebastian Vestergaard Fugmann"
subtitle: "IT Consultant & Software Developer"
layout: single
permalink: /
author_profile: true
open_to_opportunities: true # Toggle this to false to hide the "open to opportunities" badge
---
{% assign start_seconds = site.career_start_date | date: "%s" %}
{% assign now_seconds = site.time | date: "%s" %}
{% assign seconds_of_experience = now_seconds | minus: start_seconds %}
{% assign years_of_experience = seconds_of_experience | divided_by: 31556952 %}

<div class="about-hero">
  <p class="about-tagline">
    Full-stack, software developer with <strong>{{ years_of_experience }} years</strong> professional experience working as a consultant within the <strong>Salesforce</strong> and <strong>.NET</strong> ecosystems.
  </p>
  <p class="about-quickfacts">
    📍 Denmark &nbsp;·&nbsp;
    💼 {{ years_of_experience }} years experience &nbsp;·&nbsp;
    🛠️ .NET · Salesforce · Mobile App Development
    {% if page.open_to_opportunities %}
    &nbsp;·&nbsp;<span class="about-badge">✅ Open to new opportunities</span>
    {% endif %}
  </p>
</div>

## Summary

I focus on configuring and building solutions that are scalable and maintainable. In my years as a consultant I have grown fond of working alongside the end-users to figure out what solutions fit their needs the best. I have experience across both frontend and backend, which lets me have deep discussions with end-users about usability. This helps me avoid over-complicating functionality.

I have worked with a range of businesses across different business areas, making me adept at quickly understanding business requirements and constraints.

I'm still early in my career and growing my competencies with every project. I enjoy tackling new tech stacks or challenges, which improves my overall abilities as a developer and as a deliverer of systems.

{% if page.open_to_opportunities %}
I am open to both generalist and specialist roles, both internally and as a consultant, ideally within the larger Copenhagen area.
{% endif %}

## Core Competencies

**Languages**
`C#` `Java` `Kotlin` `JavaScript` `SQL`

**Frameworks & Tools**
`.NET` `React` `Node.js` `Jekyll`

**Databases & Data Stores**
`PostgreSQL` `Microsoft SQL Server`

**Infrastructure & Cloud**
`Docker`

**CI/CD & DevOps**
`Azure Pipelines`

**Testing & Quality Assurance**
`TDD`

**Salesforce**
`Sales Cloud` `Marketing Cloud` `Apex` `Flow` `SOQL` `Trigger` `LWC`

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
- 📄 **[Download CV (PDF)](/assets/files/cv-sebastian-fugmann.pdf)** <!-- TODO: Add CV as PDF -->