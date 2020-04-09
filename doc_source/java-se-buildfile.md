# Building JARs on\-server with a Buildfile<a name="java-se-buildfile"></a>

You can build your application's class files and JAR\(s\) on the EC2 instances in your environment by invoking a build command from a `Buildfile` file in your source bundle\.

Commands in a `Buildfile` are only run once and must terminate upon completion, whereas commands in a [Procfile](java-se-procfile.md) are expected to run for the life of the application and will be restarted if they terminate\. To run the JARs in your application, use a `Procfile`\.

For details about the placement and syntax of a `Buildfile`, expand the *Buildfile and Procfile* section in [Extending Elastic Beanstalk Linux platforms](platforms-linux-extend.md)\.

The following `Buildfile` example runs Apache Maven to build a web application from source code\. For a sample application that uses this feature, see [Java web application samples](java-getstarted.md#java-getstarted-samples)\.

**Example Buildfile**  

```
build: mvn assembly:assembly -DdescriptorId=jar-with-dependencies
```

The Java SE platform includes the following build tools, which you can invoke from your build script:
+ `javac` – Java compiler
+ `ant` – Apache Ant
+ `mvn` – Apache Maven
+ `gradle` – Gradle