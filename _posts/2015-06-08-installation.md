---
title: "installation"
bg: darkgray
color: orange
fa-icon: toggle-on
---

* Gradle
{% highlight text linenos=table %}
repositories {
    mavenCentral()
    jcenter()
}

dependencies {
  compile group: "org.cfg4j", name:"cfg4j", version: "3.3.1"
}
{% endhighlight %}

* Maven
{% highlight text linenos=table %}
<dependencies>
  <dependency>
    <groupId>org.cfg4j</groupId>
    <artifactId>cfg4j</artifactId>
    <version>3.3.1</version>
  </dependency>
</dependencies>
{% endhighlight %}