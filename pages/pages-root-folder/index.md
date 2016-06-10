---
#
# Use the widgets beneath and the content will be
# inserted automagically in the webpage. To make
# this work, you have to use › layout: frontpage
#
layout: frontpage
header:
  image: "cfg4-header.png"
  background-color: "#7E7E7E"
  
widget1:
  title: 'Latest release: <a href="/releases/release_4_4_0/">4.4.0</a>'
  image: widget-features.jpg
  text: '<ul>
  <li>Lightweight</li>
  <li>Easy to use</li>
  <li>Seamless integration with Spring, Guice and other DI containers</li>
  <li>Designed for distributed environments
  <ul>
    <li>support for multiple environments [test, preprod, prod]</li>
    <li>caching</li>
    <li>multi-source support with fallback, merge and other strategies</li>
  </ul>
  </li>
  <li>Supports popular configuration stores
  <ul>
    <li>Git repository (article)</li>
    <li>Consul (article)</li>
    <li>Files</li>
    <li>System variables</li>
    <li><a href="">more..</a></li>
  </ul>
  <li>Supports multiple data formats
  <ul>
    <li>YAML</li>
    <li>JSON</li>
    <li>Java properties</li>
  </ul>
  </li>
  </li>
  <li>Modern design (extensible, heavily tested, well documented)</li>
  <li>Open source (MIT license)</li>
  </ul>'
widget2:
  title: '»<em>cfg4j</em>« documentation'
  url: '/releases/latest/#users-guide'
  image: widget-docs.jpg
  text: '<ul>
    <li><a href="/releases/latest/#users-guide">User&#39;s guide</a></li>
    <li><a href="/releases/latest/#extensions">Extensions</a></li>
    <li><a href="/releases/">Releases</a></li>
    <li><a href="/examples">Examples</a></li>
    <li><a href="http://www.javadoc.io/doc/org.cfg4j/cfg4j-core/4.4.0">Javadoc</a></li>
  </ul>'
widget3:
  title: 'Source code'
  url: 'http://www.cfg4j.org/examples/'
  image: widget-code.jpg
  text: '<ul>
  <li><a href="https://github.com/cfg4j/cfg4j">Source (GitHub)</a></li>
  <li><a href="https://github.com/cfg4j/cfg4j-sample-apps">Sample apps (GitHub)</a></li>
  <li><a href="https://github.com/cfg4j/cfg4j-pusher">cfg4j-pusher (GitHub)</a></li>
  <li><a href="https://github.com/cfg4j/cfg4j-git-sample-config">Sample git configuration (GitHub)</a></li>
  <li><a href="http://search.maven.org/#search%7Cga%7C1%7Ccfg4j">Artifacts (Maven Central)</a></li>
  </ul>'
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
  url: https://github.com/cfg4j/cfg4j
  text: Star cfg4j @ github ›
  style: alert
permalink: /index.html
#
# This is a nasty hack to make the navigation highlight
# this page as active in the topbar navigation
#
homepage: true
---
