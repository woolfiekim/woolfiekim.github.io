---
title: 'Data structure'
layout: archive
permalink: categories/data-structure
author_profile: true
sidebar_main: true
---

{% assign posts = site.categories["Data structure"] %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}