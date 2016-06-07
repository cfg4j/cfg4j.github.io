---
#
# Use the widgets beneath and the content will be
# inserted automagically in the webpage. To make
# this work, you have to use › layout: frontpage
#
layout: frontpage
header:
  image_fullwidth: header_unsplash_12.jpg
widget1:
  title: "Announcements & Blog"
  url: 'http://phlow.github.io/feeling-responsive/blog/'
  image: widget-1-302x182.jpg
  text: 'Here you can learn about latest »<em>cfg4j</em>« releases and articles that cover different ways of using the library.<br/>1. <a href="">Releases</a><br/>2. <a href="">Blog</a><br/>3. <a href="">Twitter</a><br/>'
widget2:
  title: "Documentation"
  url: 'http://phlow.github.io/feeling-responsive/info/'
  image: widget-1-302x182.jpg
  text: 'Here you can find all sorts of documentation for »<em>cfg4j</em>«.<br/>0. Quick start<br/>1. <a href="">User&#39;s guide</a><br/>2. <a href="">Developer&#39;s guide</a><br/>3. <a href="">Javadoc</a><br/>4. Stack overflow<br/>5. Plugins </br>6. Customers<br/>'
widget3:
  title: "Source code & Examples"
  url: 'https://github.com/Phlow/feeling-responsive'
  image: widget-github-303x182.jpg
  text: 'Here you can find all sorts of examples on using »<em>cfg4j</em>«.<br/>1. <a href="">Source (GitHub)</a><br/>2. <a href="">Sample apps (GitHub)</a><br/>3. <a href="">Artifacts (Maven Central)</a><br/>'
#
# Use the call for action to show a button on the frontpage
#
# To make internal links, just use a permalink like this
# url: /getting-started/
#
# To style the button in different colors, use no value
# to use the main color or success, alert or secondary.
# To change colors see sass/_01_settings_colors.scss
#
callforaction:
  url: https://twitter.com/norbert_potocki
  text: Follow on Twitter for updates ›
  style: alert
permalink: /index.html
#
# This is a nasty hack to make the navigation highlight
# this page as active in the topbar navigation
#
homepage: true
---