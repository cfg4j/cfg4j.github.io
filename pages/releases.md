---
layout: page
show_meta: false
title: "»cfg4j« release announcements"
#subheadline: "Layouts of Feeling Responsive"
header:
   image_fullwidth: "header_unsplash_5.jpg"
permalink: "/releases/"
---
<ul>
    {% for post in site.categories.releases %}
    <li><a href="{{ site.url }}{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
</ul>