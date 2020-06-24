# Using the \.NET Core on Linux platform<a name="dotnet-linux-platform"></a>

The AWS Elastic Beanstalk \.NET Core on Linux platform is a set of [platform versions](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.dotnetlinux) for \.NET Core applications that run on the Linux operating system\.

For details about the various ways you can extend an Elastic Beanstalk Linux\-based platform, see [Extending Elastic Beanstalk Linux platforms](platforms-linux-extend.md)\. Following are some platform\-specific considerations\.

## Introduction to the \.NET Core on Linux platform<a name="dotnet-linux-platform.intro"></a>

### Proxy server<a name="dotnet-linux-platform.proxy"></a>

The Elastic Beanstalk \.NET Core on Linux platform includes a reverse proxy that forwards requests to your application\. By default, Elastic Beanstalk uses [nginx](https://www.nginx.com/) as the proxy server\. You can choose to use no proxy server, and configure [Kestrel](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/servers/kestrel) as your web server\. Kestrel is included by default in ASP\.NET Core project templates\.

### Application structure<a name="dotnet-linux-platform.publish-apps"></a>

You can publish *runtime\-dependent* applications that use the \.NET Core runtime provided by Elastic Beanstalk\. You can also publish *self\-contained* applications that include the \.NET Core runtime and your application's dependencies in the source bundle\. To learn more, see [Bundling applications for the \.NET Core on Linux platform](dotnet-linux-platform-bundle-app.md)\.

### Platform configuration<a name="dotnet-linux-platform.configuration"></a>

To configure the processes that run on the server instances in your environment, include an optional [Procfile](dotnet-linux-procfile.md) in your source bundle\. A `Procfile` is required if you have more than one application in your source bundle\.

We recommend that you always provide a `Procfile` in the source bundle with your application\. This way you precisely control which processes Elastic Beanstalk runs for your application\.

Configuration options are available in the Elastic Beanstalk console for [modifying the configuration of a running environment](environment-configuration-methods-after.md)\. To avoid losing your environment's configuration when you terminate it, you can use [saved configurations](environment-configuration-savedconfig.md) to save your settings and later apply them to another environment\.

To save settings in your source code, you can include [configuration files](ebextensions.md)\. Settings in configuration files are applied every time you create an environment or deploy your application\. You can also use configuration files to install packages, run scripts, and perform other instance customization operations during deployments\.

Settings applied in the Elastic Beanstalk console override the same settings in configuration files, if they exist\. This lets you have default settings in configuration files, and override them with environment\-specific settings in the console\. For more information about precedence, and other methods of changing settings, see [Configuration options](command-options.md)\.

## Configuring your \.NET Core on Linux environment<a name="dotnet-linux-options"></a>

The \.NET Core on Linux platform settings enable you to fine\-tune the behavior of your Amazon EC2 instances\. You can edit the Elastic Beanstalk environment's Amazon EC2 instance configuration using the Elastic Beanstalk console\.

Use the Elastic Beanstalk console to enable log rotation to Amazon S3 and configure variables that your application can read from the environment\.

**To configure your \.NET Core on Linux environment using the Elastic Beanstalk console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Configuration**\.

1. In the **Software** configuration category, choose **Edit**\.

### Log options<a name="dotnet-linux-logs"></a>

The **Log Options** section has two settings:
+ **Instance profile** – Specifies the instance profile that has permission to access the Amazon S3 bucket associated with your application\.
+ **Enable log file rotation to Amazon S3** – Specifies whether log files for your application's Amazon EC2 instances should be copied to the Amazon S3 bucket associated with your application\.

### Environment properties<a name="dotnet-linux-options-properties"></a>

The **Environment Properties** section enables you to specify environment configuration settings on the Amazon EC2 instances that are running your application\. Environment properties are passed in as key\-value pairs to the application\.

Inside the \.NET Core on Linux environment running in Elastic Beanstalk, environment variables are accessible using `Environment.GetEnvironmentVariable("variable-name")`\. For example, you could read a property named `API_ENDPOINT` to a variable with the following code\.

```
string endpoint = Environment.GetEnvironmentVariable("API_ENDPOINT");
```

See [Environment properties and other software settings](environments-cfg-softwaresettings.md) for more information\.

## \.NET Core on Linux configuration namespace<a name="dotnet-linux-namespace"></a>

You can use a [configuration file](ebextensions.md) to set configuration options and perform other instance configuration tasks during deployments\. Configuration options can be defined by the Elastic Beanstalk service or the platform that you use and are organized into *namespaces*\.

The \.NET Core on Linux platform supports options in the following namespace, in addition to the [options supported for all Elastic Beanstalk environments](command-options-general.md):
+ `aws:elasticbeanstalk:environment:proxy` – Choose to use nginx or no proxy server\. Valid values are `nginx` or `none`\.

The following example configuration file shows the use of the \.NET Core on Linux\-specific configuration options\. 

**Example \.ebextensions/proxy\-settings\.config**  

```
option_settings:
  aws:elasticbeanstalk:environment:proxy:
    ProxyServer: none
```

Elastic Beanstalk provides many configuration options for customizing your environment\. In addition to configuration files, you can also set configuration options using the console, saved configurations, the EB CLI, or the AWS CLI\. See [Configuration options](command-options.md) for more information\.