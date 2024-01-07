---
layout: page
show_meta: false
title: "Projects"
subheadline: "Technical"
sidebar: right
comments: true
breadcrumb: true
header:
   image_fullwidth: "headers/header_unsplash_4.jpg"
permalink: "/about-me/projects/"
---
<ul>
    {% for post in site.categories.projects %}
        {% include _page_entries.html %}
    {% endfor %}
</ul>