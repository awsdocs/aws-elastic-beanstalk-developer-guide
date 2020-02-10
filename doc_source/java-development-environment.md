# Setting up your Java development environment<a name="java-development-environment"></a>

Set up a Java development environment to test your application locally prior to deploying it to AWS Elastic Beanstalk\. This topic outlines development environment setup steps and links to installation pages for useful tools\.

For common setup steps and tools that apply to all languages, see [Configuring your development machine for use with Elastic Beanstalk](chapter-devenv.md)\.

**Topics**
+ [Installing the Java development kit](#java-development-environment-jdk)
+ [Installing a web container](#java-development-environment-tomcat)
+ [Downloading libraries](#java-development-environment-libraries)
+ [Installing the AWS SDK for Java](#java-development-environment-sdk)
+ [Installing an IDE or text editor](#java-development-environment-ide)
+ [Installing the AWS toolkit for Eclipse](#java-development-environment-toolkit)

## Installing the Java development kit<a name="java-development-environment-jdk"></a>

Install the Java Development Kit \(JDK\)\. If you don't have a preference, get the latest version\. Download the JDK at [oracle\.com](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 

The JDK includes the Java compiler, which you can use to build your source files into class files that can be executed on an Elastic Beanstalk web server\.

## Installing a web container<a name="java-development-environment-tomcat"></a>

If you don't already have another web container or framework, install the appropriate version of Tomcat:
+  [Download Tomcat 8 \(requires Java 7 or later\)](http://tomcat.apache.org/download-80.cgi) 
+  [Download Tomcat 7 \(requires Java 6 or later\)](http://tomcat.apache.org/download-70.cgi) 

## Downloading libraries<a name="java-development-environment-libraries"></a>

Elastic Beanstalk platforms include few libraries by default\. Download libraries that your application will use and save them in your project folder to deploy in your application source bundle\.

If you've installed Tomcat locally, you can copy the servlet API and JavaServer Pages \(JSP\) API libraries from the installation folder\. If you deploy to a Tomcat platform version, you don't need to include these files in your source bundle, but you do need to have them in your `classpath` to compile any classes that use them\.

JUnit, Google Guava, and Apache Commons provide several useful libraries\. Visit their home pages to learn more:
+  [Download JUnit](https://github.com/junit-team/junit/wiki/Download-and-Install) 
+  [Download Google Guava](https://code.google.com/p/guava-libraries/) 
+  [Download Apache Commons](http://commons.apache.org/downloads/) 

## Installing the AWS SDK for Java<a name="java-development-environment-sdk"></a>

If you need to manage AWS resources from within your application, install the AWS SDK for Java\. For example, with the AWS SDK for Java, you can use Amazon DynamoDB \(DynamoDB\) to share session states of Apache Tomcat applications across multiple web servers\. For more information, see [Manage Tomcat Session State with Amazon DynamoDB](http://docs.aws.amazon.com/AWSSdkDocsJava/latest/DeveloperGuide/java-dg-tomcat-session-manager.html) in the AWS SDK for Java documentation\.

Visit the [AWS SDK for Java home page](https://aws.amazon.com/sdk-for-java/) for more information and installation instructions\.

## Installing an IDE or text editor<a name="java-development-environment-ide"></a>

Integrated development environments \(IDEs\) provide a wide range of features that facilitate application development\. If you haven't used an IDE for Java development, try Eclipse and IntelliJ and see which works best for you\.
+  [Install Eclipse IDE for Java EE Developers](https://www.eclipse.org/downloads/) 
+  [Install IntelliJ](https://www.jetbrains.com/idea/) 

**Note**  
An IDE might add files to your project folder that you might not want to commit to source control\. To prevent committing these files to source control, use `.gitignore` or your source control tool's equivalent\.

If you just want to begin coding and don't need all of the features of an IDE, consider [installing Sublime Text](http://www.sublimetext.com/)\.

## Installing the AWS toolkit for Eclipse<a name="java-development-environment-toolkit"></a>

The [AWS Toolkit for Eclipse](java-eclipsetoolkit.md) is an open source plug\-in for the Eclipse Java IDE that makes it easier for developers to develop, debug, and deploy Java applications using AWS\. Visit the [AWS Toolkit for Eclipse home page](https://aws.amazon.com/eclipse/) for installation instructions\. 