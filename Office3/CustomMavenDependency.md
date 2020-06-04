# Packaging of Maven Project into a jar and its attachment

## Creation of Maven Dependency

## Adding a custom jar as a maven dependency
Maven as a build managemet tool manages  builds, executions, dependencies.Whenever a dependency is added to a project, maven will search for it at repositories, download it and store it


There are two ways to attach a custom jar as a dependency.
* Adding a system dependency
* Or by publish the jar to a Local Maven Repository 
* Or by publish the jar to a Maven Central Repository

### Adding a system dependency
```java
  <dependencies>
    <dependency>
      <groupId>groupId</groupId>
      <artifactId>artifactId</artifactId>
      <version>1.0</version>
      <scope>system</scope>
      <systemPath>${basedir}/lib/xx.jar</systemPath>
    </dependency>
  </dependencies>
```

System scope was created to add dependencies(jar) that was created from other projects but now are packed into the JDK. 

But there is a drawback, the Assembly plugin and many other packaging plugin  ignores this kind of dependencies and it doesn’t pack them with the others.

### Publish the jar to a Local Maven Repository and then Include it as Dependency

### Publish the jar in Maven Central Repository and then Include it as Dependency
Please refer to the (Publish JAR To Central Maven Repository)[http://tutorials.jenkov.com/maven/publish-to-central-maven-repository.html]

