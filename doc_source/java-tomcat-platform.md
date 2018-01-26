# Using the AWS Elastic Beanstalk Tomcat Platform<a name="java-tomcat-platform"></a>

The AWS Elastic Beanstalk Tomcat platform is a set of environment configurations for Java web applications that can run in a Tomcat web container\. Each configuration corresponds to a major version of Tomcat, including *Java 8 with Tomcat 8* and *Java 7 with Tomcat 7*\.

Platform\-specific configuration options are available in the AWS Management Console for modifying the configuration of a running environment\. To avoid losing your environment's configuration when you terminate it, you can use saved configurations to save your settings and later apply them to another environment\.

To save settings in your source code, you can include configuration files\. Settings in configuration files are applied every time you create an environment or deploy your application\. You can also use configuration files to install packages, run scripts, and perform other instance customization operations during deployments\.

Elastic Beanstalk Tomcat platform configurations include a reverse proxy that forwards requests to your application\. The default server is [Apache HTTP Server \(version 2\.2\)](http://httpd.apache.org/docs/2.2/) but you can use configuration options to use nginx instead\. Elastic Beanstalk also provides configuration options to configure the proxy server to serve static assets from a folder in your source code to reduce the load on your application\. For advanced scenarios, you can include your own `.conf` files in your source bundle to extend Elastic Beanstalk's proxy configuration or overwrite it completely\.

You must package Java applications in a web application archive \(WAR\) file with a specific structure\. For information on the required structure and how it relates to the structure of your project directory, see \.

To run multiple applications on the same web server, you can bundle multiple WAR files into a single source bundle\. Each application in a multiple WAR source bundle runs at either the root path \(`ROOT.war` runs at `myapp.elasticbeanstalk.com/`\) or at a path directly beneath it \(`app2.war` runs at `myapp.elasticbeanstalk.com/app2/`, as determined by the name of the WAR\. In a single WAR source bundle, the application always runs at the root path\.

Settings applied in the AWS Management Console override the same settings in configuration files, if they exist\. This lets you have default settings in configuration files, and override them with environment specific settings in the console\. For more information about precedence, and other methods of changing settings, see \.

## Configuring Your Tomcat Environment<a name="java-tomcat-options"></a>

For Tomcat platform configurations on Elastic Beanstalk, Elastic Beanstalk provides a few platform\-specific options in addition to the standard options it provides for all environments\. These options let you configure the Java virtual machine \(JVM\) that runs on your environment's web servers, and define system properties that provide information configuration strings to your application\.

You can use the AWS Management Console to enable log rotation to Amazon S3 and configure variables that your application can read from the environment\.

**To configure your Tomcat environment in the Elastic Beanstalk console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the management page for your environment\.

1. Choose **Configuration**\.

1. On the **Software** configuration card, choose **Modify**\.

### JVM Container Options<a name="java-tomcat-options-jvm"></a>

The heap size in the Java virtual machine \(JVM\) determines how many objects your application can create in memory before *[garbage collection](https://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/introduction.html)* occurs\. You can modify the **Initial JVM Heap Size \(\-Xms argument\)** and a **Maximum JVM Heap Size \(\-Xmx argument\)**\. A larger initial heap size allows more objects to be created before garbage collection occurs, but it also means that the garbage collector will take longer to compact the heap\. The maximum heap size specifies the maximum amount of memory the JVM can allocate when expanding the heap during heavy activity\.

**Note**  
The available memory depends on the EC2 instance type\. For more information about the EC2 instance types available for your Elastic Beanstalk environment, see [Instance Types](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-types.html) in the *Amazon Elastic Compute Cloud User Guide for Linux*\. 

The *permanent generation* is a section of the JVM heap that stores class definitions and associated metadata\. To modify the size of the permanent generation, type the new size in the **Maximum JVM PermGen Size \(\-XX:MaxPermSize argument\)** field\. This setting only applies to Java 7 and earlier\.

### Log Options<a name="java-tomcat-options-logs"></a>

The Log Options section has two settings:

+ **Instance profile** – Specifies the instance profile that has permission to access the Amazon S3 bucket associated with your application\.

+ **Enable log file rotation to Amazon S3** – Specifies whether log files for your application's Amazon EC2 instances should be copied to your Amazon S3 bucket associated with your application\.

### Environment Properties<a name="java-tomcat-options-properties"></a>

The **Environment Properties** section lets you specify environment configuration settings on the Amazon EC2 instances that are running your application\. Environment properties are passed in as key\-value pairs to the application\. 

The Tomcat platform defines a placeholder property named `JDBC_CONNECTION_STRING` for Tomcat environments for passing a connection string to an external database\.

**Note**  
If you attach an RDS DB instance to your environment, construct the JDBC connection string dynamically from the RDS environment properties provided by Elastic Beanstalk\. Use JDBC\_CONNECTION\_STRING only for database instances that are not provisioned using Elastic Beanstalk\.  
For more information about using Amazon Relational Database Service \(Amazon RDS\) with your Java application, see 

Inside the Tomcat environment running in Elastic Beanstalk, environment variables are accessible using the `System.getProperty()`\. For example, you could read a property named `API_ENDPOINT` to a variable with the following code:

```
String endpoint = System.getProperty("API_ENDPOINT");
```

See  for more information\.

## Tomcat Configuration Namespaces<a name="java-tomcat-namespaces"></a>

You can use a configuration file to set configuration options and perform other instance configuration tasks during deployments\. Configuration options can be defined by the Elastic Beanstalk service or the platform that you use and are organized into *namespaces*\.

The Tomcat platform supports options in the following namespaces in addition to the options supported for all Elastic Beanstalk environments:

+ `aws:elasticbeanstalk:container:tomcat:jvmoptions` – Modify JVM settings\. Options in this namespace correspond to options in the management console as follows:

  + `Xms` – **JVM command line options**

  + `Xmx` – **JVM command line options**

  + `XX:MaxPermSize` – **Maximum JVM permanent generation size**

  + `JVM Options` – **JVM command line options**

+ `aws:elasticbeanstalk:environment:proxy` – Choose the proxy server and configure response compression\.

+ `aws:elasticbeanstalk:environment:proxy:staticfiles` – Configure the proxy to serve static assets from a path in your source bundle\.

The following example configuration file shows the use of the Tomcat\-specific configuration options:

**Example \.ebextensions/tomcat\-settings\.config**  

```
option_settings:
  aws:elasticbeanstalk:container:tomcat:jvmoptions:
    Xms: 512m
    Xmx: 512m
    JVM Options: '-Xmn128m'
  aws:elasticbeanstalk:application:environment:
    API_ENDPOINT: mywebapi.zkpexsjtmd.us-west-2.elasticbeanstalk.com
  aws:elasticbeanstalk:environment:proxy:
    GzipCompression: 'true'
    ProxyServer: nginx
  aws:elasticbeanstalk:environment:proxy:staticfiles:
    /pub: public
```

Elastic Beanstalk provides many configuration options for customizing your environment\. In addition to configuration files, you can also set configuration options using the console, saved configurations, the EB CLI, or the AWS CLI\. See  for more information\.