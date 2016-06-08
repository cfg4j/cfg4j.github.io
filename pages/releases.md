---
layout: page
#
# Content
#
title: "Released »cfg4j« versions"
#
# Styling
#
show_meta: false
header:
   image_fullwidth: "header_unsplash_5.jpg"
   
permalink: "/releases/"
breadcrumb: true
---

{% assign latest_release = site.categories.releases | first %}
The latest stable release is: <a href="{{ site.url "}}{{ latest_release.url }}"><b>{{ latest_release.release.version }}</b></a>

<h3>Release announcements</h3>
<ul>
    {% for post in site.categories.releases %}
    <li><a href="{{ site.url }}{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
</ul>