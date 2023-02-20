---
title: "etc"
layout: archive
permalink: /etc
author_profile: true
sidebar_main: true
sidebar:
    nav: "sidebar-category" #navigation.yml에 있는 docs를 뜻한다.
---

{% assign posts = site.categories.etc %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}