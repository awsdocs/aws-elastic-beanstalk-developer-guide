# Using the Elastic Beanstalk \.NET platform<a name="create_deploy_NET.container.console"></a>

AWS Elastic Beanstalk supports a number of platforms for different versions of the \.NET programming framework and Windows Server\. See [\.NET on Windows Server with IIS](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.net) in the *AWS Elastic Beanstalk Platforms* document for a full list\.

Elastic Beanstalk provides [configuration options](command-options.md) that you can use to customize the software that runs on the EC2 instances in your Elastic Beanstalk environment\. You can configure environment variables needed by your application, enable log rotation to Amazon S3, and set \.NET framework settings\.

Configuration options are available in the Elastic Beanstalk console for [modifying the configuration of a running environment](environment-configuration-methods-after.md)\. To avoid losing your environment's configuration when you terminate it, you can use [saved configurations](environment-configuration-savedconfig.md) to save your settings and later apply them to another environment\.

To save settings in your source code, you can include [configuration files](ebextensions.md)\. Settings in configuration files are applied every time you create an environment or deploy your application\. You can also use configuration files to install packages, run scripts, and perform other instance customization operations during deployments\.

Settings applied in the Elastic Beanstalk console override the same settings in configuration files, if they exist\. This lets you have default settings in configuration files, and override them with environment\-specific settings in the console\. For more information about precedence, and other methods of changing settings, see [Configuration options](command-options.md)\.

## Configuring your \.NET environment in the Elastic Beanstalk console<a name="dotnet-console"></a>

You can use the Elastic Beanstalk console to enable log rotation to Amazon S3, configure variables that your application can read from the environment, and change \.NET framework settings\.

**To configure your \.NET environment in the Elastic Beanstalk console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Configuration**\.

1. In the **Software** configuration category, choose **Edit**\.

### Container options<a name="dotnet-console-framework"></a>
+ **Target \.NET runtime** – Set to `2.0` to run CLR v2\.
+ **Enable 32\-bit applications** – Set to `True` to run 32\-bit applications\.

### Log options<a name="dotnet-console-logs"></a>

The Log Options section has two settings:
+ **Instance profile** – Specifies the instance profile that has permission to access the Amazon S3 bucket associated with your application\.
+ **Enable log file rotation to Amazon S3** – Specifies whether log files for your application's Amazon EC2 instances should be copied to the Amazon S3 bucket associated with your application\.

### Environment properties<a name="dotnet-console-properties"></a>

The **Environment Properties** section lets you specify environment configuration settings on the Amazon EC2 instances that are running your application\. These settings are passed in as key\-value pairs to the application\. Use `System.GetEnvironmentVariable` to read them\. Identical keys can exist in both `web.config` and as environment properties\. Use the `System.Configuration` namespace to read values from `web.config`\.

```
NameValueCollection appConfig = ConfigurationManager.AppSettings;
string endpoint = appConfig["API_ENDPOINT"];
```

See [Environment properties and other software settings](environments-cfg-softwaresettings.md) for more information\.

## The aws:elasticbeanstalk:container:dotnet:apppool namespace<a name="dotnet-namespaces"></a>

You can use a [configuration file](ebextensions.md) to set configuration options and perform other instance configuration tasks during deployments\. Configuration options can be defined by the Elastic Beanstalk service or the platform that you use and are organized into *namespaces*\.

The \.NET platform defines options in the `aws:elasticbeanstalk:container:dotnet:apppool` namespace that you can use to configure the \.NET runtime\.

The following example configuration file shows settings for each of the options available in this namespace:

**Example \.ebextensions/dotnet\-settings\.config**  

```
option_settings:
  aws:elasticbeanstalk:container:dotnet:apppool:
    Target Runtime: 2.0
    Enable 32-bit Applications: True
```

Elastic Beanstalk provides many configuration options for customizing your environment\. In addition to configuration files, you can also set configuration options using the console, saved configurations, the EB CLI, or the AWS CLI\. See [Configuration options](command-options.md) for more information\.