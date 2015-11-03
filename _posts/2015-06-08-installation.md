---
title: "installation"
bg: darkgray
color: orange
fa-icon: toggle-on
---
You can start using **cfg4j** in your app by simply including it as a dependency. It consists of **cfg4j-core** module (required) and
a number of plugins (e.g. *cfg4j-consul*, *cfg4j-git*) which add support for additional configuration sources. Below is the configuration
for Gradle and Maven that will get you started quickly. Once you add cfg4j dependency proceed to the *usage* section.

* Gradle

{% highlight text linenos=table %}
dependencies {
  compile group: "org.cfg4j", name:"cfg4j-core", version: "4.1.3"
  
  // Optional plug-ins
  
  // Consul integration
  compile group: "org.cfg4j", name:"cfg4j-consul", version: "4.1.3"
  
  // Git integration
  compile group: "org.cfg4j", name:"cfg4j-git", version: "4.1.3"
}
{% endhighlight %}

* Maven

{% highlight text linenos=table %}
<dependencies>
  <dependency>
    <groupId>org.cfg4j</groupId>
    <artifactId>cfg4j-core</artifactId>
    <version>4.1.3</version>
  </dependency>
  
  <!-- Optional plug-ins -->
  
  <!-- Consul integration -->
  <dependency> 
    <groupId>org.cfg4j</groupId>
    <artifactId>cfg4j-consul</artifactId>
    <version>4.1.3</version>
  </dependency>
  
  <!-- Git integration -->
  <dependency>
    <groupId>org.cfg4j</groupId>
    <artifactId>cfg4j-git</artifactId>
    <version>4.1.3</version>
  </dependency>
</dependencies>
{% endhighlight %}