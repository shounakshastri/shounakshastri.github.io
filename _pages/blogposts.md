---
layout: archive
title: "Blog Posts"
permalink: /blogposts/
read_time: true
author_profile: true
---

{% include base_path %}

{% for post in site.blogposts reversed %}
  {% include archive-single.html %}
{% endfor %}
