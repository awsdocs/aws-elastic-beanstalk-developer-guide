# Creating and deploying Java applications on Elastic Beanstalk<a name="create_deploy_Java"></a>

AWS Elastic Beanstalk supports several [platform versions](concepts.platforms.md) for Java applications, including multiple versions of Java with the Tomcat application server and Java\-only platform versions for applications that do not use Tomcat\.

[Apache Tomcat](java-tomcat-platform.md) is an open source web container for applications that use Java servlets and JavaServer Pages \(JSPs\) to serve HTTP requests\. Tomcat facilitates web application development by providing multithreading, declarative security configuration, and extensive customization\. Platform versions are available for each of Tomcat's current major versions\.

[Java SE platform versions](java-se-platform.md) \(without Tomcat\) are also provided for applications that don't use a web container, or use one other than Tomcat, such as Jetty or GlassFish\. You can include any library Java Archives \(JARs\) used by your application in the source bundle that you deploy to Elastic Beanstalk\.

AWS provides several tools for working with Java and Elastic Beanstalk\. Regardless of the platform version that you choose, you can use the [AWS SDK for Java](java-development-environment.md#java-development-environment-sdk) to use other AWS services from within your Java application\. The AWS SDK for Java is a set of libraries that allow you to use AWS APIs from your application code without writing the raw HTTP calls from scratch\.

If you use the Eclipse integrated development environment \(IDE\) to develop your Java application, you can also get the [AWS Toolkit for Eclipse](java-eclipsetoolkit.md)\. The AWS Toolkit for Eclipse is an open source plug\-in that lets you manage AWS resources, including Elastic Beanstalk applications and environments, from within the Eclipse IDE\.

If the command line is more your style, install the [Elastic Beanstalk Command Line Interface](eb-cli3.md) \(EB CLI\) and use it to create, monitor, and manage your Elastic Beanstalk environments from the command line\. If you run multiple environments for your application, the EB CLI integrates with Git to let you associate each of your environments with a different Git branch\.

The topics in this chapter assume some knowledge of Elastic Beanstalk environments\. If you haven't used Elastic Beanstalk before, try the [getting started tutorial](GettingStarted.md) to learn the basics\.

**Topics**
+ [Getting started with Java on Elastic Beanstalk](java-getstarted.md)
+ [Setting up your Java development environment](java-development-environment.md)
+ [Using the Elastic Beanstalk Tomcat platform](java-tomcat-platform.md)
+ [Using the Elastic Beanstalk Java SE platform](java-se-platform.md)
+ [Adding an Amazon RDS DB instance to your Java application environment](java-rds.md)
+ [Using the AWS Toolkit for Eclipse](java-eclipsetoolkit.md)
+ [Resources](create_deploy_Java.resources.md)