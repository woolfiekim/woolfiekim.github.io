---
title: 'Project'
layout: archive
permalink: /project
author_profile: true
sidebar_main: true
sidebar:
    nav: "sidebar-category" #navigation.yml에 있는 docs를 뜻한다.
---

{% assign posts = site.categories.project %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}