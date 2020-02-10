# Using the Elastic Beanstalk Java SE platform<a name="java-se-platform"></a>

The AWS Elastic Beanstalk Java SE platform is a set of [platform versions](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.javase) for Java web applications that can run on their own from a compiled JAR file\. You can compile your application locally or upload the source code with a build script to compile it on\-instance\. Each platform version corresponds to a major version of Java, including *Java 8* and *Java 7*\.

**Note**  
Elastic Beanstalk doesn't parse your application's JAR file\. Keep files that Elastic Beanstalk needs outside of the JAR file\. For example, include the `cron.yaml` file of a [worker environment](using-features-managing-env-tiers.md) at the root of your application's source bundle, next to the JAR file\.

Platform\-specific configuration options are available in the AWS Management Console for [modifying the configuration of a running environment](environment-configuration-methods-after.md)\. To avoid losing your environment's configuration when you terminate it, you can use [saved configurations](environment-configuration-savedconfig.md) to save your settings and later apply them to another environment\.

To save settings in your source code, you can include [configuration files](ebextensions.md)\. Settings in configuration files are applied every time you create an environment or deploy your application\. You can also use configuration files to install packages, run scripts, and perform other instance customization operations during deployments\.

The Elastic Beanstalk Java SE platform includes an [nginx](https://www.nginx.com/) server that acts as a reverse proxy, serving cached static content and passing requests to your application\. The platform provides configuration options to configure the proxy server to serve static assets from a folder in your source code to reduce the load on your application\. For advanced scenarios, you can [include your own \.conf files](java-se-nginx.md) in your source bundle to extend Elastic Beanstalk's proxy configuration or overwrite it completely\. 

If you only have one JAR file, Elastic Beanstalk renames your JAR file to `application.jar`, and then runs it using `java -jar application.jar`\. To configure the processes that run on the server instances in your environment, include an optional [Procfile](java-se-procfile.md) in your source bundle\. A `Procfile` is required if you have more than one JAR in your source bundle root, or if you want to customize the java command to set JVM options\.

To compile Java classes and run other build commands on the EC2 instances in your environment at deploy time, include a [Buildfile](java-se-buildfile.md) in your application source bundle\. A `Buildfile` lets you deploy your source code as\-is and build on the server instead of compiling JARs locally\. The Java SE platform includes common build tools to let you build on\-server\.

**Execution Order**  
When you include multiple types of configuration in your application source bundle, they are executed in the following order\. Each step does not begin until the previous step completes\.   
Step 1: `commands`, `files` and `packages` defined in [configuration files](ebextensions.md)
Step 2: `Buildfile` command
Step 3: `container_commands` in configuration files
Step 4: `Procfile` commands \(all commands are run simultaneously\)
For more information on using `commands`, `files`, `packages` and `container_commands` in configuration files, see [Customizing software on Linux servers](customize-containers-ec2.md)\.

## Configuring your Java SE environment<a name="java-se-options"></a>

The Elastic Beanstalk Java SE platform provides a few platform\-specific options in addition to the standard options that all platforms have\. These options let you configure the nginx proxy that runs in front of your application to serve static files\.

You can use the Elastic Beanstalk console to enable log rotation to Amazon S3 and configure variables that your application can read from the environment\.

**To configure your Java SE environment in the Elastic Beanstalk console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. Choose **Configuration**\.

1. In the **Software** configuration category, choose **Modify**\.

### Log options<a name="java-se-options-logs"></a>

The Log Options section has two settings:
+ **Instance profile** – Specifies the instance profile that has permission to access the Amazon S3 bucket associated with your application\.
+ **Enable log file rotation to Amazon S3** – Specifies whether log files for your application's Amazon EC2 instances should be copied to your Amazon S3 bucket associated with your application\.

### Static files<a name="java-se-options-staticfiles"></a>

To improve performance, you can configure the proxy server to serve static files \(for example, HTML or images\) from a set of directories inside your web application\. When the proxy server receives a request for a file under the specified path, it serves the file directly instead of routing the request to your application\. You can set the virtual path and directory mappings in the **Static Files** section of the **Modify software** configuration page\.

For details about configuring static files using the Elastic Beanstalk console, see [Serving static files](environment-cfg-staticfiles.md)\.

### Environment properties<a name="java-se-options-properties"></a>

The **Environment Properties** section lets you specify environment configuration settings on the Amazon EC2 instances that are running your application\. Environment properties are passed in as key\-value pairs to the application\.

Inside the Java SE environment running in Elastic Beanstalk, environment variables are accessible using the `System.getenv()`\. For example, you could read a property named `API_ENDPOINT` to a variable with the following code:

```
String endpoint = System.getenv("API_ENDPOINT");
```

See [Environment properties and other software settings](environments-cfg-softwaresettings.md) for more information\.

## Java SE configuration namespaces<a name="java-se-namespaces"></a>

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

## Amazon Linux 2 considerations<a name="java-se-al2"></a>


|  | 
| --- |
| AWS Elastic Beanstalk support for Amazon Linux 2 is in beta release and is subject to change\. | 

Elastic Beanstalk provides Amazon Linux 2 Java SE platform versions for *Amazon Corretto 8* and *Amazon Corretto 11*\. The Corretto platform versions are different than previous Java SE platform versions based on Amazon Linux AMI in a few ways, both generic \(apply to all Amazon Linux 2 platforms\) and platform specific \(apply to Amazon Linux 2 Corretto platform versions\)\. For details, see [Migrating your Elastic Beanstalk Linux application to Amazon Linux 2](using-features.migration-al.md)\.

We recommend that you provide a source bundle \(ZIP file\) containing your application and a `Procfile`\. This way you precisely control which processes Elastic Beanstalk runs for your application and which arguments these processes receive\.