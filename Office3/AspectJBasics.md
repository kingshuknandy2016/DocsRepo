# AspectJ Understanding

Learn basics of **aspectJ** from the [Java Aspect Oriented Programming Tutorial with AspectJ (AOP)](https://o7planning.org/en/10257/java-aspect-oriented-programming-tutorial-with-aspectj#a27235)<br/>
A bit advanced understanding from [Intro to AspectJ](https://www.baeldung.com/aspectj)<br/>
An aspectJ with Maven Integration project with  different types of weaving refer to [Maven project example with AspectJ](https://github.com/barlog-m/aspectj-maven-example/tree/load-time-weaving)<br/>
[Implementing a Method Trace Infrastructure With Spring Boot and AspectJ](https://github.com/barlog-m/aspectj-maven-example/tree/master)

## AspectJ setup

Couple of ways:
* Install AspectJ development tools into Eclipse
* Install AspectJ Runtime into Your system

### Install AspectJ development tools into Eclipse
***Step 1:*** Go To Install New Software,click add, and then fill the details.<br/>
                Name: ajdt 4.7
                Location: http://download.eclipse.org/tools/ajdt/47/dev/update<br/>
                The files plugin will get installed.<br/>
                You can work with that.

### Install AspectJ Runtime into Your system
***Step 1:*** Go to the  [AspectJ home page](https://www.eclipse.org/aspectj/) <br/>
***Step 2:*** Download the jar<br/>
***Step 3:*** Run the Jar - ***java -jar aspectj-1.9.5.jar*** or double click on the jar.<br/>
        The Aspect J installer will come up. Install it.<br/>
        Note the installation directory.<br>
        Once you go to the lib directory you will be able to see the files <br/>***aspectjrt.jar***,<br/>***aspectjtools.jar***,<br/>***aspectjweaver.jar***


## AspectJ weaving
To indroduce ***advice** into a java class we need to add ***Runtime dependency*** and attach ***AspectJRuntime***

###Types of Weaving
* Compile-Time Weaving
* Post-Compile Weaving
* Load-Time Weaving

