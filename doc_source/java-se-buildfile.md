# Building JARs on\-server with a Buildfile<a name="java-se-buildfile"></a>

You can build your application's class files and JAR\(s\) on the EC2 instances in your environment by invoking a build command from a `Buildfile` file in your source bundle\.

A `Buildfile` file has the same syntax as a `Procfile` file, but commands in a `Buildfile` file are only run once and must terminate upon completion, whereas commands in a `Procfile` file are expected to run for the life of the application and will be restarted if they terminate\. To run the JARs in your application, use a `Procfile` instead\.

Add a file named `Buildfile` \(case sensitive\) to the root of your source bundle and configure it to invoke a build command in the following manner:

 **Buildfile** 

```
build: mvn assembly:assembly -DdescriptorId=jar-with-dependencies
```

The above example runs Apache Maven to build a web application from source code\. Check out the [Java web application samples](java-getstarted.md#java-getstarted-samples) for a sample application that uses this feature\.

The Java SE platform includes the following build tools, which you can invoke from your build script:
+ `javac` – Java compiler
+ `ant` – Apache Ant
+ `mvn` – Apache Maven
+ `gradle` – Gradle


|  | 
| --- |
| AWS Elastic Beanstalk support for Amazon Linux 2 is in beta release and is subject to change\. | 

All Amazon Linux 2 platforms support a uniform Buildfile feature\. For the new Corretto platform versions running Amazon Linux 2, expand the *Buildfile* section in [Extending Elastic Beanstalk Linux platforms](platforms-linux-extend.md)\.