# Packaging of Maven Project into a jar and its attachment

## Packaging of the Maven Application
There are couple of plugins avaliable for the same.

They are:
* maven-jar-plugin
* Shade Plugin

## Adding acustom jar as a maven dependency
Maven as a build managemet tool manages  builds, executions, dependencies.Whenever a dependency is added to a project, maven will search for it at repositories, download it and store it


There are two ways to attach a custom jar as a dependency.
* Adding a system dependency
* Or by publish the jar to a Local Maven Repository 
* Installing the jar by using install-plugin
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
The idea is to create a maven repo that runs on our own computer instead or a remote server. Every file needed would be on the version control system so every developer can build the project once it is downloaded.

##### Step 1:Add the local repository to the project pom
```java
  <repositories>
    <repository>
      <id>my-local-repo</id>
      <url>file://${basedir}/my-repo</url>
    </repository>
  </repositories>
```
In this case the repository will be stored on a directory called “my-repo” and it will be located in the project root directory.
##### Step 2:Install jar in the repository through [install-plugin](http://maven.apache.org/plugins/maven-install-plugin/)
On maven 2.3 and onwards:

```java
mvn install:install-file -Dfile=<path-to-file> -DgroupId=<myGroup> -DartifactId=<myArtifactId> -Dversion=<myVersion> -Dpackaging=<myPackaging> -DlocalRepositoryPath=<path>
```

##### Step 2:Add the dependency to the project as usual.
```java
 <!--Custom Added Dependency-->
        <dependency>
            <groupId>com.custom.aspect</groupId>
            <artifactId>aspectIntegrationProj</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
```
### Installing the jar by using install-plugin
The idea is to store the jar into a project directory and install it into the m2repo directory (where all mvn dependencies are downloaded to) on build time. To get this working the dependency should be added to the repository as usual, and also add the plugin to the pom configuration:
```java
    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-install-plugin</artifactId>
        <version>2.4</version>
        <executions>
            <execution>
                <phase>initialize</phase>
                <goals>
                    <goal>install-file</goal>
                </goals>
                <configuration>
                    <groupId>myGroupId</groupId>
                    <artifactId>myArtifactId</artifactId>
                    <version>myVersion</version>
                    <packaging>jar</packaging>
                    <file>${basedir}/lib/xxx.jar</file>
                </configuration>
            </execution>
        </executions>
    </plugin>
```

### Publish the jar in Maven Central Repository and then Include it as Dependency
Please refer to the [Publish JAR To Central Maven Repository](http://tutorials.jenkov.com/maven/publish-to-central-maven-repository.html)
And also [How to manually publish JAR to maven central?](https://stackoverflow.com/questions/28846802/how-to-manually-publish-jar-to-maven-central)

