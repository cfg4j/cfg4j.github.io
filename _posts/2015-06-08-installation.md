---
title: "installation"
bg: darkgray
color: orange
fa-icon: toggle-on
---

* Gradle

{% highlight text linenos=table %}
dependencies {
  compile group: "org.cfg4j", name:"cfg4j-core", version: "4.0.1"
  
  // Optional plug-ins
  
  // Consul integration
  compile group: "org.cfg4j", name:"cfg4j-consul", version: "4.0.1"
  
  // Git integration
  compile group: "org.cfg4j", name:"cfg4j-git", version: "4.0.1"
}
{% endhighlight %}

* Maven

{% highlight text linenos=table %}
<dependencies>
  <dependency>
    <groupId>org.cfg4j</groupId>
    <artifactId>cfg4j-core</artifactId>
    <version>4.0.1</version>
  </dependency>
  
  <!-- Optional plug-ins -->
  
  <!-- Consul integration -->
  <dependency> 
    <groupId>org.cfg4j</groupId>
    <artifactId>cfg4j-consul</artifactId>
    <version>4.0.1</version>
  </dependency>
  
  <!-- Git integration -->
  <dependency>
    <groupId>org.cfg4j</groupId>
    <artifactId>cfg4j-git</artifactId>
    <version>4.0.1</version>
  </dependency>
</dependencies>
{% endhighlight %}