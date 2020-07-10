# Environment properties and other software settings<a name="environments-cfg-softwaresettings"></a>

The **Modify software** configuration page lets you configure the software on the Amazon Elastic Compute Cloud \(Amazon EC2\) instances that run your application\. You can configure environment properties, AWS X\-Ray debugging, instance log storing and streaming, and platform\-specific settings\.

![\[Modify software configuration page\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environments-cfg-softwaresettings.png)

**Topics**
+ [Configure platform\-specific settings](#environments-cfg-softwaresettings-specific)
+ [Configuring environment properties](#environments-cfg-softwaresettings-console)
+ [Software setting namespaces](#environments-cfg-softwaresettings-configfiles)
+ [Accessing environment properties](#environments-cfg-softwaresettings-accessing)
+ [Configuring AWS X\-Ray debugging](environment-configuration-debugging.md)
+ [Viewing your Elastic Beanstalk environment logs](environments-cfg-logging.md)

## Configure platform\-specific settings<a name="environments-cfg-softwaresettings-specific"></a>

In addition to the standard set of options available for all environments, most Elastic Beanstalk platforms let you specify language\-specific or framework\-specific settings\. These appear in the **Platform options** section of the **Modify software** page, and can take the following forms\.
+ **Preset environment properties** – The Ruby platform uses environment properties for framework settings, such as `RACK_ENV` and `BUNDLE_WITHOUT`\.
+ **Placeholder environment properties** – The Tomcat platform defines an environment property named `JDBC_CONNECTION_STRING` that is not set to any value\. This type of setting was more common on older platform versions\.
+ **Configuration options** – Most platforms define [configuration options](command-options.md) in platform\-specific or shared namespaces, such as `aws:elasticbeanstalk:xray` or `aws:elasticbeanstalk:container:python`\.

**To configure platform\-specific settings in the Elastic Beanstalk console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Configuration**\.

1. In the **Software** configuration category, choose **Edit**\.

1. Under **Platform options**, make necessary option setting changes\.

1. Choose **Apply**\.

For information about platform\-specific options, and about getting environment property values in your code, see the platform topic for your language or framework:
+ Docker – [Configuring Docker environments](create_deploy_docker.container.console.md)
+ Go – [Using the Elastic Beanstalk Go platform](go-environment.md)
+ Java SE – [Using the Elastic Beanstalk Java SE platform](java-se-platform.md)
+ Tomcat – [Using the Elastic Beanstalk Tomcat platform](java-tomcat-platform.md)
+ \.NET Core on Linux – [Using the \.NET Core on Linux platform](dotnet-linux-platform.md)
+ \.NET – [Using the Elastic Beanstalk \.NET platform](create_deploy_NET.container.console.md)
+ Node\.js – [Using the Elastic Beanstalk Node\.js platform](create_deploy_nodejs.container.md)
+ PHP – [Using the Elastic Beanstalk PHP platform](create_deploy_PHP.container.md)
+ Python – [Using the Elastic Beanstalk Python platform](create-deploy-python-container.md)
+ Ruby – [Using the Elastic Beanstalk Ruby platform](create_deploy_Ruby.container.md)

## Configuring environment properties<a name="environments-cfg-softwaresettings-console"></a>

You can use **environment properties** to pass secrets, endpoints, debug settings, and other information to your application\. Environment properties help you run your application in multiple environments for different purposes, such as development, testing, staging, and production\.

In addition, when you [add a database to your environment](using-features.managing.db.md), Elastic Beanstalk sets environment properties, such as `RDS_HOSTNAME`, that you can read in your application code to construct a connection object or string\.

**Environment variables**  
In most cases, environment properties are passed to your application as *environment variables*, but the behavior is platform dependent\. For example, [the Java SE platform](java-se-platform.md) sets environment variables that you retrieve with `System.getenv`, while [the Tomcat platform](java-tomcat-platform.md) sets Java system properties that you retrieve with `System.getProperty`\. In general, properties are *not* visible if you connect to an instance and run `env`\.

**To configure environment properties in the Elastic Beanstalk console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Configuration**\.

1. In the **Software** configuration category, choose **Edit**\.

1. Under **Environment properties**, enter key\-value pairs\.  
![\[Environment properties section in the Modify software configuration page\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/wizard-software-environment.png)

1. Choose **Apply**\.

**Environment property limits**
+ **Keys** can contain any alphanumeric characters and the following symbols: `_ . : / + \ - @`

  The symbols listed are valid for environment property keys, but might not be valid for environment variable names on your environment's platform\. For compatibility with all platforms, limit environment properties to the following pattern: `[A-Z_][A-Z0-9_]*`
+ **Values** can contain any alphanumeric characters, white space, and the following symbols: `_ . : / = + \ - @ ' "`
**Note**  
Single and double quotation marks in values must be escaped\.
+ **Keys** can contain up to 128 characters\. **Values** can contain up to 256 characters\.
+ **Keys** and **values** are case sensitive\.
+ The combined size of all environment properties cannot exceed 4,096 bytes when stored as strings with the format *key*=*value*\.

## Software setting namespaces<a name="environments-cfg-softwaresettings-configfiles"></a>

You can use a [configuration file](ebextensions.md) to set configuration options and perform other instance configuration tasks during deployments\. Configuration options can be defined by the Elastic Beanstalk service or the platform that you use and are organized into *namespaces*\.

You can use Elastic Beanstalk [configuration files](ebextensions.md) to set environment properties and configuration options in your source code\. Use the [`aws:elasticbeanstalk:application:environment` namespace](command-options-general.md#command-options-general-elasticbeanstalkapplicationenvironment) to define environment properties\.

**Example \.ebextensions/options\.config**  

```
option_settings:
  aws:elasticbeanstalk:application:environment:
    API_ENDPOINT: www.example.com/api
```

If you use configuration files or AWS CloudFormation templates to create [custom resources](environment-resources.md), you can use an AWS CloudFormation function to get information about the resource and assign it to an environment property dynamically during deployment\. The following example from the [elastic\-beanstalk\-samples](https://github.com/awsdocs/elastic-beanstalk-samples/) GitHub repository uses the [Ref function](ebextensions-functions.md) to get the ARN of an Amazon SNS topic that it creates, and assigns it to an environment property named `NOTIFICATION_TOPIC`\.

**Notes**  
If you use an AWS CloudFormation function to define an environment property, the Elastic Beanstalk console displays the value of the property before the function is evaluated\. You can use the [`get-config` platform script](custom-platforms-scripts.md) to confirm the values of environment properties that are available to your application\. 
The [Multicontainer Docker](create_deploy_docker_ecs.md) platform doesn't use AWS CloudFormation to create container resources\. As a result, this platform doesn't support defining environment properties using AWS CloudFormation functions\.

**Example \.Ebextensions/[sns\-topic\.config](https://github.com/awsdocs/elastic-beanstalk-samples/tree/master/configuration-files/aws-provided/resource-configuration/sns-topic.config)**  

```
Resources:
  NotificationTopic:
    Type: AWS::SNS::Topic

option_settings:
  aws:elasticbeanstalk:application:environment:
    NOTIFICATION_TOPIC: '`{"Ref" : "NotificationTopic"}`'
```

You can also use this feature to propagate information from [AWS CloudFormation pseudo parameters](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/pseudo-parameter-reference.html)\. This example gets the current region and assigns it to a property named `AWS_REGION`\.

**Example \.Ebextensions/[env\-regionname\.config](https://github.com/awsdocs/elastic-beanstalk-samples/tree/master/configuration-files/aws-provided/instance-configuration/env-regionname.config)**  

```
option_settings:
  aws:elasticbeanstalk:application:environment:
    AWS_REGION: '`{"Ref" : "AWS::Region"}`'
```

Most Elastic Beanstalk platforms define additional namespaces with options for configuring software that runs on the instance, such as the reverse proxy that relays requests to your application\. For more information about the namespaces available for your platform, see the following:
+ Go – [Go configuration namespace](go-environment.md#go-namespaces)
+ Java SE – [Java SE configuration namespace](java-se-platform.md#java-se-namespaces)
+ Tomcat – [Tomcat configuration namespaces](java-tomcat-platform.md#java-tomcat-namespaces)
+ \.NET Core on Linux – [\.NET Core on Linux configuration namespace](dotnet-linux-platform.md#dotnet-linux-namespace)
+ \.NET – [The aws:elasticbeanstalk:container:dotnet:apppool namespace](create_deploy_NET.container.console.md#dotnet-namespaces)
+ Node\.js – [Node\.js configuration namespace](create_deploy_nodejs.container.md#nodejs-namespaces)
+ PHP – [The aws:elasticbeanstalk:container:php:phpini namespace](create_deploy_PHP.container.md#php-namespaces)
+ Python – [Python configuration namespaces](create-deploy-python-container.md#python-namespaces)
+ Ruby – [Ruby configuration namespaces](create_deploy_Ruby.container.md#ruby-namespaces)

Elastic Beanstalk provides many configuration options for customizing your environment\. In addition to configuration files, you can also set configuration options using the console, saved configurations, the EB CLI, or the AWS CLI\. See [Configuration options](command-options.md) for more information\.

## Accessing environment properties<a name="environments-cfg-softwaresettings-accessing"></a>

In most cases, you access environment properties in your application code like an environment variable\. In general, however, environment properties are passed only to the application and can't be viewed by connecting an instance in your environment and running `env`\.
+ [Go](go-environment.md#go-options-properties) – `os.Getenv`

  ```
  endpoint := os.Getenv("API_ENDPOINT")
  ```
+ [Java SE](java-se-platform.md#java-se-options-properties) – `System.getenv`

  ```
  String endpoint = System.getenv("API_ENDPOINT");
  ```
+ [Tomcat](java-tomcat-platform.md#java-tomcat-options-properties) – `System.getProperty`

  ```
  String endpoint = System.getProperty("API_ENDPOINT");
  ```
+ [\.NET Core on Linux](dotnet-linux-platform.md#dotnet-linux-options-properties) – `Environment.GetEnvironmentVariable`

  ```
  string endpoint = Environment.GetEnvironmentVariable("API_ENDPOINT");
  ```
+ [\.NET](create_deploy_NET.container.console.md#dotnet-console-properties) – `appConfig`

  ```
  NameValueCollection appConfig = ConfigurationManager.AppSettings;
  string endpoint = appConfig["API_ENDPOINT"];
  ```
+ [Node\.js](create_deploy_nodejs.container.md#nodejs-platform-console-envprops) – `process.env`

  ```
  var endpoint = process.env.API_ENDPOINT
  ```
+ [PHP](create_deploy_PHP.container.md#php-console-properties) – `$_SERVER`

  ```
  $endpoint = $_SERVER['API_ENDPOINT'];
  ```
+ [Python](create-deploy-python-container.md#create-deploy-python-custom-container-envprop) – `os.environ`

  ```
  import os
  endpoint = os.environ['API_ENDPOINT']
  ```
+ [Ruby](create_deploy_Ruby.container.md#create_deploy_Ruby.env.console.ruby.envprops) – `ENV`

  ```
  endpoint = ENV['API_ENDPOINT']
  ```

Outside of application code, such as in a script that runs during deployment, you can access environment properties with the [`get-config` platform script](custom-platforms-scripts.md)\. See the [elastic\-beanstalk\-samples](https://github.com/awsdocs/elastic-beanstalk-samples/search?utf8=%E2%9C%93&q=get-config) GitHub repository for example configurations that use `get-config`\.