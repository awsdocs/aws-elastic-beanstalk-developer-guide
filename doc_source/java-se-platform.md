# Using the Elastic Beanstalk Java SE platform<a name="java-se-platform"></a>

**Important**  
Amazon Linux 2 platform versions are fundamentally different than Amazon Linux AMI platform versions \(preceding Amazon Linux 2\)\. These different platform generations are incompatible in several ways\. If you are migrating to an Amazon Linux 2 platform version, be sure to read the information in [Migrating your Elastic Beanstalk Linux application to Amazon Linux 2](using-features.migration-al.md)\.

The AWS Elastic Beanstalk Java SE platform is a set of [platform versions](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.javase) for Java web applications that can run on their own from a compiled JAR file\. You can compile your application locally or upload the source code with a build script to compile it on\-instance\. Java SE platform versions are grouped into platform branches, each of which corresponds to a major version of Java, for example *Java 8* and *Java 7*\.

**Note**  
Elastic Beanstalk doesn't parse your application's JAR file\. Keep files that Elastic Beanstalk needs outside of the JAR file\. For example, include the `cron.yaml` file of a [worker environment](using-features-managing-env-tiers.md) at the root of your application's source bundle, next to the JAR file\.

Configuration options are available in the Elastic Beanstalk console for [modifying the configuration of a running environment](environment-configuration-methods-after.md)\. To avoid losing your environment's configuration when you terminate it, you can use [saved configurations](environment-configuration-savedconfig.md) to save your settings and later apply them to another environment\.

To save settings in your source code, you can include [configuration files](ebextensions.md)\. Settings in configuration files are applied every time you create an environment or deploy your application\. You can also use configuration files to install packages, run scripts, and perform other instance customization operations during deployments\.

The Elastic Beanstalk Java SE platform includes an [nginx](https://www.nginx.com/) server that acts as a reverse proxy, serving cached static content and passing requests to your application\. The platform provides configuration options to configure the proxy server to serve static assets from a folder in your source code to reduce the load on your application\. For advanced scenarios, you can [include your own \.conf files](java-se-nginx.md) in your source bundle to extend Elastic Beanstalk's proxy configuration or overwrite it completely\. 

If you only provide a single JAR file for your application source \(on its own, not within a source bundle\), Elastic Beanstalk renames your JAR file to `application.jar`, and then runs it using `java -jar application.jar`\. To configure the processes that run on the server instances in your environment, include an optional [Procfile](java-se-procfile.md) in your source bundle\. A `Procfile` is required if you have more than one JAR in your source bundle root, or if you want to customize the java command to set JVM options\.

We recommend that you always provide a `Procfile` in the source bundle alongside your application\. This way you precisely control which processes Elastic Beanstalk runs for your application and which arguments these processes receive\.

To compile Java classes and run other build commands on the EC2 instances in your environment at deploy time, include a [Buildfile](java-se-buildfile.md) in your application source bundle\. A `Buildfile` lets you deploy your source code as\-is and build on the server instead of compiling JARs locally\. The Java SE platform includes common build tools to let you build on\-server\.

For details about the various ways you can extend an Elastic Beanstalk Linux\-based platform, see [Extending Elastic Beanstalk Linux platforms](platforms-linux-extend.md)\.

## Configuring your Java SE environment<a name="java-se-options"></a>

The Java SE platform settings let you fine\-tune the behavior of your Amazon EC2 instances\. You can edit the Elastic Beanstalk environment's Amazon EC2 instance configuration using the Elastic Beanstalk console\.

Use the Elastic Beanstalk console to enable log rotation to Amazon S3 and configure variables that your application can read from the environment\.

**To configure your Java SE environment in the Elastic Beanstalk console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Configuration**\.

1. In the **Software** configuration category, choose **Edit**\.

### Log options<a name="java-se-options-logs"></a>

The Log Options section has two settings:
+ **Instance profile** – Specifies the instance profile that has permission to access the Amazon S3 bucket associated with your application\.
+ **Enable log file rotation to Amazon S3** – Specifies whether log files for your application's Amazon EC2 instances should be copied to the Amazon S3 bucket associated with your application\.

### Static files<a name="java-se-options-staticfiles"></a>

To improve performance, the **Static files** section lets you configure the proxy server to serve static files \(for example, HTML or images\) from a set of directories inside your web application\. For each directory, you set the virtual path to directory mapping\. When the proxy server receives a request for a file under the specified path, it serves the file directly instead of routing the request to your application\.

For details about configuring static files using the Elastic Beanstalk console, see [Serving static files](environment-cfg-staticfiles.md)\.

### Environment properties<a name="java-se-options-properties"></a>

The **Environment Properties** section lets you specify environment configuration settings on the Amazon EC2 instances that are running your application\. Environment properties are passed in as key\-value pairs to the application\.

Inside the Java SE environment running in Elastic Beanstalk, environment variables are accessible using the `System.getenv()`\. For example, you could read a property named `API_ENDPOINT` to a variable with the following code:

```
String endpoint = System.getenv("API_ENDPOINT");
```

See [Environment properties and other software settings](environments-cfg-softwaresettings.md) for more information\.

## Java SE configuration namespace<a name="java-se-namespaces"></a>

You can use a [configuration file](ebextensions.md) to set configuration options and perform other instance configuration tasks during deployments\. Configuration options can be defined by the Elastic Beanstalk service or the platform that you use and are organized into *namespaces*\.

The Java SE platform doesn't define any platform\-specific namespaces\. You can configure the proxy to serve static files by using the `aws:elasticbeanstalk:environment:proxy:staticfiles` namespace\. For details and an example, see [Serving static files](environment-cfg-staticfiles.md)\.

Elastic Beanstalk provides many configuration options for customizing your environment\. In addition to configuration files, you can also set configuration options using the console, saved configurations, the EB CLI, or the AWS CLI\. See [Configuration options](command-options.md) for more information\.

## The Amazon Linux AMI \(preceding Amazon Linux 2\) Java SE platform<a name="java-se.alami"></a>

If your Elastic Beanstalk Java SE environment uses an Amazon Linux AMI platform version \(preceding Amazon Linux 2\), read the additional information in this section\.

### Java SE configuration namespaces<a name="java-se.alami.namespaces"></a>

You can use a [configuration file](ebextensions.md) to set configuration options and perform other instance configuration tasks during deployments\. Configuration options can be defined by the Elastic Beanstalk service or the platform that you use and are organized into *namespaces*\.

The Java SE platform supports one platform\-specific configuration namespace in addition to the [namespaces supported by all platforms](command-options-general.md)\. The `aws:elasticbeanstalk:container:java:staticfiles` namespace lets you define options that map paths on your web application to folders in your application source bundle that contain static content\.

For example, this [option\_settings](ebextensions-optionsettings.md) snippet defines two options in the static files namespace\. The first maps the path `/public` to a folder named `public`, and the second maps the path `/images` to a folder named `img`:

```
option_settings:
  aws:elasticbeanstalk:container:java:staticfiles:
    /html: statichtml
    /images: staticimages
```

The folders that you map using this namespace must be actual folders in the root of your source bundle\. You cannot map a path to a folder in a JAR file\.

Elastic Beanstalk provides many configuration options for customizing your environment\. In addition to configuration files, you can also set configuration options using the console, saved configurations, the EB CLI, or the AWS CLI\. See [Configuration options](command-options.md) for more information\.