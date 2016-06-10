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
  image: "cfg4-header.png"
  background-color: "#7E7E7E"
   
permalink: "/releases/"
breadcrumb: true
---

{% assign latest_release = site.categories.releases | first %}
The latest stable release is: <a href="{{ site.url "}}{{ latest_release.url }}"><b>{{ latest_release.release.version }}</b></a>

<h3>All releases</h3>
<ul>
    {% for post in site.categories.releases %}
    {% assign release_components = post.release.version | split: '.' %}
    {% assign major_release = release_components[0] | plus: 0 %}
    {% assign minor_release = release_components[1] | plus: 0 %}
    {% assign bugfix_release = release_components[2] | plus: 0 %}
    
    {% if major_release < 4 %}  
    {% assign optional_dot = '' %}
    {% elsif major_release == 4 and minor_release <= 1 %} 
    {% assign optional_dot = '' %}
    {% else %}
    {% assign optional_dot = '.' %}
    {% endif %} 
    
    <li>v.{{ major_release }}.{{ minor_release }}.{{ bugfix_release }}
    | <a href="{{ site.url }}{{ post.url }}">Announcement</a>
    | <a href="{{ site.url }}/releases/{{ major_release}}.{{ minor_release }}.x/">Documentation</a>
    | <a href="http://www.javadoc.io/doc/org.cfg4j/cfg4j-core/{{ major_release }}.{{ minor_release }}.{{ bugfix_release }}">Javadoc</a>
    | <a href="https://github.com/cfg4j/cfg4j/releases/tag/v{{ optional_dot }}{{ major_release }}.{{ minor_release }}.{{ bugfix_release }}">GitHub</a>
    | <a href="http://search.maven.org/#search%7Cga%7C1%7Cg%3A%22org.cfg4j%22%20AND%20v%3A%22{{ major_release }}.{{ minor_release }}.{{ bugfix_release }}%22">Maven Central</a>
    </li>
    {% endfor %}
</ul>