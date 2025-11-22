---
layout: page
title: projects
permalink: /projects/
description: Research projects and work in progress.
nav: true
nav_order: 2
---

## Work in Progress

{% assign sorted_projects = site.projects | where: "category", "research" | sort: "importance" %}
{% for project in sorted_projects %}
- **[{{ project.title }}]({{ project.url | relative_url }})** {% if project.coauthors %}(with {{ project.coauthors }}){% endif %}
{% endfor %}
