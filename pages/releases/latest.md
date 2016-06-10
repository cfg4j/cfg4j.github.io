---
layout: page-fullwidth
title: "»<em>cfg4j</em>« 4.4.x documentation"
permalink: "/releases/4.4.x/"
header:
  image: "cfg4-header.png"
  background-color: "#7E7E7E"
---
<div class="row">
<div class="medium-4 medium-push-8 columns" markdown="1">
<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>
</div><!-- /.medium-4.columns -->



<div class="medium-8 medium-pull-4 columns" markdown="1">
{% include _improve_content.html %}

## Installation

You can start using **cfg4j** in your app by simply including it as a dependency. It consists of **cfg4j-core** module (required) and
a number of plugins (e.g. *cfg4j-consul*, *cfg4j-git*) which add support for additional configuration sources. Below is the configuration
for Gradle and Maven that will get you started quickly. Once you add cfg4j dependency proceed to the *usage* section.

* Gradle

{% highlight text %}
dependencies {
  compile group: "org.cfg4j", name:"cfg4j-core", version: "4.4.0"
  
  // Optional plug-ins
  
  // Consul integration
  compile group: "org.cfg4j", name:"cfg4j-consul", version: "4.4.0"
  
  // Git integration
  compile group: "org.cfg4j", name:"cfg4j-git", version: "4.4.0"
}
{% endhighlight %}

* Maven

{% highlight text %}
<dependencies>
  <dependency>
    <groupId>org.cfg4j</groupId>
    <artifactId>cfg4j-core</artifactId>
    <version>4.4.0</version>
  </dependency>
  
  <!-- Optional plug-ins -->
  
  <!-- Consul integration -->
  <dependency> 
    <groupId>org.cfg4j</groupId>
    <artifactId>cfg4j-consul</artifactId>
    <version>4.4.0</version>
  </dependency>
  
  <!-- Git integration -->
  <dependency>
    <groupId>org.cfg4j</groupId>
    <artifactId>cfg4j-git</artifactId>
    <version>4.4.0</version>
  </dependency>
</dependencies>
{% endhighlight %}

## Quick start

You may want to read [the following (a bit lengthy) article about configuration management using cfg4j](http://potocki.io/post/141230472743/configuration-management-for-distributed-systems)
to get better understanding of the core configuration management concepts.

0. Add **cfg4j-core** and **cfg4j-git** dependencies to your project (see previous paragraph for detail).

1. Use the following code in your application to connect to the sample configuration source (git repository).
Also see [this sample app](https://github.com/cfg4j/cfg4j-sample-apps/tree/master/git-bind).
{% highlight java %}
public class Cfg4jPoweredApplication {

  // "Reksio" is a friendly dog that has many friends
  public interface ReksioConfig {  
    List<String> friends();  
  }

  public static void main(String... args) {
  
    // Select our sample git repository as the configuration source
    ConfigurationSource source = new GitConfigurationSourceBuilder()
          .withRepositoryURI("https://github.com/cfg4j/cfg4j-git-sample-config.git")
          .build();
          
    // Create configuration provider backed by the source
    ConfigurationProvider provider = new ConfigurationProviderBuilder()
          .withConfigurationSource(source)
          .withReloadStrategy(new PeriodicalReloadStrategy(5, TimeUnit.SECONDS))
          .build();
    
    // Get info about our dog. When you add more friends in the configuration file, object below will automatically reflect that change (by mutating friends() list).
    ReksioConfig reksioConfig = configurationProvider.bind("reksio", ReksioConfig.class);
    
    // Display friends of Reksio!
    System.out.println(reksioConfig.friends());
  }

}
{% endhighlight %}

* Optional steps
    1. Fork the [sample configuration repository](https://github.com/cfg4j/cfg4j-git-sample-config).
    2. Add your configuration to the "*application.properties*" file and commit the changes.
    3. Update the code above to point to your fork.



## Tutorial

### Introduction
Using cfg4j is quite simple. It boils down to the following steps:

1. Point to the ```ConfigurationSource``` that stores your configs (e.g. Consul K-V store, git repository, local files, classpath resources).
2. Create ```ConfigurationProvider``` backed by your configuration source.
3. Access your configuration in one of the two ways:
    1. Directly through ConfigurationProvider methods (e.g. ```getProperty("my.property", Integer.class)```)
    2. Through a binding mechanism. Simply create an interface with methods representing your configuration variables
       and ask ConfigurationProvider to create a configuration object implementing your interface
       (i.e. calling ```bind("my.configuration.set", MyInterface.class)```). Every time the configuration changes
       your object will be automatically updated with new values.
4. (optional) If you want you can customize the ConfigurationProvider behavior by using:
    * ```ReloadStrategy``` - those allow configuration to be automatically reloaded. There's ```PeriodicalReloadStrategy``` provided
        that can reload configuration after every specified time period. You can also implement your own strategy.
    * ```Environment``` - Use it to specify where your configuration is located inside ConfigurationSource. It's especially useful
        when you have multiple configuration sets (e.g. multi-tenant) in one source (e.g. under different paths, on
        different git branches, etc.). Read more below.

### Configuration source
Obviously, you need your configuration to be stored somewhere before you can access it. You can store it in a K-V store (e.g. Consul K-V),
files (remote git repository, local files, classpath resources) or a relational database. If you're unsure what the best practices are,
you can consult [this article]().

Once you have your configuration in place, create a ```ConfigurationSource`` object that will fetch it. Here's an example using Consul K-V.

{% highlight java %}
ConfigurationSource source = new ConsulConfigurationSourceBuilder().build();
{% endhighlight %}

To learn more about supported configuration sources and their capabilities head to the [PLUGINS section](#plugins).

### Configuration provider
Once we have ```ConfigurationSource``` in place, we need ```ConfigurationProvider``` that will provide your app with a uniform interface
for reading configs.

You can obtain ```ConfigurationProvider``` using builder:

{% highlight java %}
ConfigurationSource source = ... (see above)
ConfigurationProvider provider = new ConfigurationProviderBuilder().withConfigurationSource(source).build();
{% endhighlight %}

You can also use DI container (like Spring)
{% highlight java %}
@Bean
public ConfigurationProvider configurationProvider(ConfigurationSource source) {
  return new ConfigurationProviderBuilder().withConfigurationSource(source).build();
}
{% endhighlight %}

### Accessing configuration
Once you have ```ConfigurationProvider``` in hand you can use it for accessing configuration. Here's a bunch of examples (values that you need
to put in your configuration source are listed in comments next to examples).

* Get primitive (and boxed) types directly from Provider (also see [this sample app](https://github.com/cfg4j/cfg4j-sample-apps/tree/master/git-simple)).
{% highlight java %}
Boolean property = provider.getProperty("some.property", Boolean.class); // some.property=false

Integer property = provider.getProperty("some.other.property", Integer.class); // some.other.property=1234

float[] floats = provider.getProperty("my.floatsArray", float[].class); // my.floatsArray=1.2,99.999,0.15

URL url = provider.getProperty("my.url", URL.class); // my.url=http://www.cfg4j.org
{% endhighlight %}

* Get collections directly from Provider
{% highlight java %}
List<String> stringList = provider.getProperty("some.string.list", new GenericType<List<String>>() {}); // some.string.list=how,are,you

Map<String, Integer> pairs = provider.getProperty("some.map", new GenericType<Map<String, Integer>>() {}); // some.map=a=1,b=33,c=-10
{% endhighlight %}

* Utilize object binding (also see [this sample app](https://github.com/cfg4j/cfg4j-sample-apps/tree/master/git-bind)).
{% highlight java %}
// Define your configuration interface
public interface MyConfiguration {
  Boolean featureToggle();
  File[] someFiles();
  Map<String, Integer> prices();
}

// someFeature.featureToggle=true
// someFeature.someFiles=/temp/fileA.txt,/temp/fileB.txt
// someFeature.prices=apple=1,candy=33,car=10
MyConfiguration config = simpleConfigurationProvider.bind("someFeature", MyConfiguration.class);

// Use it (when configuration changes your object will be auto-updated!)
if(config.featureToggle()) {
  ...
}

{% endhighlight %}


### Configuration reloading
Being able to change your configuration without bringing the service down is extremely important for web apps. **CFG4J** provides
runtime configuration reloading which can be configured using ```ReloadStrategy```. When configuration gets
reloaded all bound configuration objects (see "Accessing configuration" above) will be updated.

* Reload configuration periodically (also see [this sample app](https://github.com/cfg4j/cfg4j-sample-apps/tree/master/git-simple)).
{% highlight java %}
// Reload every second
ConfigurationProvider provider = new ConfigurationProviderBuilder()
        .withConfigurationSource(...)
        .withReloadStrategy(new PeriodicalReloadStrategy(1, TimeUnit.SECONDS))
        .build();
{% endhighlight %}

* Reload on-demand (e.g. triggered by push mechanism)
{% highlight java %}
// Implement your strategy
public class MyReloadStrategy implements ReloadStrategy {

  public void register(Reloadable resource) {
      ...
      resource.reload();
      ...
  }

}

ConfigurationProvider provider = new ConfigurationProviderBuilder()
        .withReloadStrategy(new MyReloadStrategy())
        .build();
{% endhighlight %}

### Multi-tenant support (a.k.a. environment selection)
You may want to store multiple configuration sets in a single configuration store (e.g. to support different environments [dev, test, prod] 
in your app or simplify because you want to maintain one store for multiple tenants).

To support this use case cfg4j introduces *Environments*. ```Environment``` is simply a way to tell the provider where in the store is your
configuration set located. Because each source may support environments differently (e.g. in git you could use separate branches or directories,
in K-V store you could use different paths, etc.).

For now we're keeping it simple. ```Environment``` has a name that is interpreted by the source. It's up to the source what interpretation
to use. Consult documentation for each of the sources (plugins) for more detail.

You can provide ```Environment``` to the provider in the following way:
{% highlight java %}
ConfigurationProvider provider = new ConfigurationProviderBuilder()
        .withConfigurationSource(new ConsulConfigurationSourceBuilder().build())
        .withEnvironment(new ImmutableEnvironment("myApplication/prod"))  // each conf variable will be prefixed with this value when fetching from Consul
        .build();
{% endhighlight %}

### Different file types support (YAML, properties)
When reading from ```ConfigurationSource``` that is backed by files (e.g. git repository) you have an option of using either plain
**properties** files or **YAML** files. Cfg4j will automatically recognize the file by its **extension** (*.properties* or *.yaml*).

### Merging configurations from multiple sources
You can merge configs from multiple configuration sources. To do that use ```MergeConfigurationSource``` as seen below:

{% highlight java %}
ConfigurationSource source1 = ...
ConfigurationSource source2 = ...

ConfigurationSource mergedSource = new MergeConfigurationSource(source1, source2);
{% endhighlight %}

### Fallback in case of failure
When working in a distributed environment failure happen all the time. You may consider using multiple sources. When one fails another
one will be used as a fallback. You can use ```FallbackConfigurationSource``` in the following way:

{% highlight java %}
ConfigurationSource source1 = ...
ConfigurationSource source2 = ...

ConfigurationSource mergedSource = new FallbackConfigurationSource(source1, source2);
{% endhighlight %}

### Metrics
Most of the cfg4j components are instrumented. Metrics are exposed with support from **[Dropwizard's Metrics library](http://metrics.dropwizard.io)**.
You can see the number of invocations (e.g. how many times your configuration was reloaded) and execution time (e.g. how long does it take
to fetch configuration from external configuration source - git repository or Consul).
To enable instrumentation you have to provide ```ConfigurationProvider``` with Metric's ```MetricRegistry```. To easily differentiate
between metrics for different provided you can specify prefix prepended to all metric names constructed for the given provider.

{% highlight java %}
@Autowired
private MetricRegistry metricRegistry;

@Bean
public ConfigurationProvider configurationProvider() {
  ...

  return new ConfigurationProviderBuilder()
      ...
      .withMetrics(metricRegistry, "firstProvider.") // Enable metrics with prefix
      .build();
}
{% endhighlight %}

## Extensions

### Classpath
* Read configuration from multiple files located in classpath. Also see [this sample app](https://github.com/cfg4j/cfg4j-sample-apps/tree/master/classpath-bind).

{% highlight java %}
// Specify which files to load. Configuration from both files will be merged.
ConfigFilesProvider configFilesProvider = () -> Arrays.asList(Paths.get("application.properties"), Paths.get("otherConfig.properties"));

// Use classpath as configuration store
ConfigurationSource source = new ClasspathConfigurationSource(configFilesProvider);

// Create provider
return new ConfigurationProviderBuilder()
    .withConfigurationSource(source)
    .build();
{% endhighlight %}

-------------------------

### Files
* Read configuration from multiple files located in a given path. Also see [this sample app](https://github.com/cfg4j/cfg4j-sample-apps/tree/master/files-bind).

{% highlight java %}
// Specify which files to load. Configuration from both files will be merged.
ConfigFilesProvider configFilesProvider = () -> Arrays.asList(Paths.get("application.properties"), Paths.get("otherConfig.properties"));

// Use local files as configuration store
ConfigurationSource source = new FilesConfigurationSource(configFilesProvider);

// (optional) Select path to use
Environment environment = new ImmutableEnvironment("/config/root/path/");

// (optional) Reload configuration every 5 seconds
ReloadStrategy reloadStrategy = new PeriodicalReloadStrategy(5, TimeUnit.SECONDS);

// Create provider
return new ConfigurationProviderBuilder()
    .withConfigurationSource(source)
    .withEnvironment(environment)
    .withReloadStrategy(reloadStrategy)
    .build();
{% endhighlight %}

-------------------------

### Git
*  Use **remote** or **local** git repository that reloades configuration every 5 seconds. Also see [this sample app](https://github.com/cfg4j/cfg4j-sample-apps/tree/master/git-simple).

{% highlight java %}
  // Use Git repository as configuration store
ConfigurationSource source = new GitConfigurationSourceBuilder()
    .withRepositoryURI("https://your/git/repo.git")
    .build();
    
// (optional) Reload configuration every 5 seconds
ReloadStrategy reloadStrategy = new PeriodicalReloadStrategy(5, TimeUnit.SECONDS);

return new ConfigurationProviderBuilder()
    .withConfigurationSource(source)
    .withReloadStrategy(new PeriodicalReloadStrategy(5, TimeUnit.SECONDS))
    .build();
{% endhighlight %}

* Read configuration from a given files (defaults to *application.properties*). Also see [this sample app](https://github.com/cfg4j/cfg4j-sample-apps/tree/master/git-multi-file).
{% highlight java %}

ConfigFilesProvider configFilesProvider = () -> Arrays.asList(Paths.get("application.properties"), Paths.get("otherConfig.properties"));

// Use Git repository as configuration store
ConfigurationSource source = new GitConfigurationSourceBuilder()
    .withRepositoryURI("https://your/git/repo.git")
    .withConfigFilesProvider(configFilesProvider)
    .build();
    
// (optional) Reload configuration every 5 seconds
ReloadStrategy reloadStrategy = new PeriodicalReloadStrategy(5, TimeUnit.SECONDS);

return new ConfigurationProviderBuilder()
    .withConfigurationSource(source)
    .withEnvironment(environment)
    .build();
{% endhighlight %}

#### Multi-tenant support (multiple configuration sets per source)

* Read configuration from a given branch (defaults to master). Also see [this sample app](https://github.com/cfg4j/cfg4j-sample-apps/tree/master/git-bind).
{% highlight java %}
// Use Git repository as configuration store
ConfigurationSource source = new GitConfigurationSourceBuilder()
    .withRepositoryURI("https://your/git/repo.git")
    .build();
    
// Select branch to use (use new DefaultEnvironment()) for master
Environment environment = new ImmutableEnvironment("myBranch");

// (optional) Reload configuration every 5 seconds
ReloadStrategy reloadStrategy = new PeriodicalReloadStrategy(5, TimeUnit.SECONDS);

return new ConfigurationProviderBuilder()
    .withConfigurationSource(source)
    .withEnvironment(environment)
    .build();
{% endhighlight %}

* Read configuration from files (see examples above to change list of files) at a given path.
{% highlight java %}
// Use Git repository as configuration store
ConfigurationSource source = new GitConfigurationSourceBuilder()
    .withRepositoryURI("https://your/git/repo.git")
    .build();

// Select branch and path
Environment environment = new ImmutableEnvironment("myBranch/folder/on/branch");

// (optional) Reload configuration every 5 seconds
ReloadStrategy reloadStrategy = new PeriodicalReloadStrategy(5, TimeUnit.SECONDS);

return new ConfigurationProviderBuilder()
    .withConfigurationSource(source)
    .withEnvironment(environment)
    .build();
{% endhighlight %}

-------------------------


### Consul
* Connect to **local** Consul agent and reload configuration every 5 seconds. Also see [this sample app](https://github.com/cfg4j/cfg4j-sample-apps/tree/master/consul-bind).
{% highlight java %}
// Use Consul service as configuration store
ConfigurationSource source = new ConsulConfigurationSourceBuilder().build();

// (optional) Reload configuration every 5 seconds
ReloadStrategy reloadStrategy = new PeriodicalReloadStrategy(5, TimeUnit.SECONDS);

// Create provider
return new ConfigurationProviderBuilder()
  .withConfigurationSource(source)
  .withReloadStrategy(reloadStrategy)
  .build();
{% endhighlight %}

* Connect to **remote** Consul agent and reload configuration every 5 seconds.
{% highlight java %}
// Use Consul service as configuration store
ConfigurationSource source = new URL("http", "192.168.0.1", 8500, "");

// (optional) Reload configuration every 5 seconds
ReloadStrategy reloadStrategy = new PeriodicalReloadStrategy(5, TimeUnit.SECONDS);

// Create provider
return new ConfigurationProviderBuilder()
  .withConfigurationSource(source)
  .withReloadStrategy(reloadStrategy)
  .build();
{% endhighlight %}

#### Multi-tenant support (multiple configuration sets per repository)

* Read configuration from a given prefix (defaults to no prefix).
{% highlight java %}
// Use Consul service as configuration store
ConfigurationSource source = new ConsulConfigurationSource();
    
// Set k-v store directory (i.e. /us-west-2/)
Environment environment = new ImmutableEnvironment("us-west-2");

// (optional) Reload configuration every 5 seconds
ReloadStrategy reloadStrategy = new PeriodicalReloadStrategy(5, TimeUnit.SECONDS);

// Create provider
return new ConfigurationProviderBuilder()
    .withConfigurationSource(source)
    .withEnvironment(environment)
    .build();
{% endhighlight %}


{% include _improve_content.html %}

</div><!-- /.medium-8.columns -->
</div><!-- /.row -->
