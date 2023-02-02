---
title: "기초"
layout: archive
permalink: /categories/oracle/base
author_profile: true
sidebar_main: true
---

{% assign posts = site.categories.oracleBase %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %}
{% endfor %}
