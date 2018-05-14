# Environment Properties and Other Software Settings<a name="environments-cfg-softwaresettings"></a>

You can use **environment properties** to pass secrets, endpoints, debug settings, and other information to your application\. Environment properties help you run your application in multiple environments for different purposes, such as development, testing, staging, and production\.

**Environment Variables**  
In most cases, environment properties are passed to your application as *environment variables*, but the behavior is platform dependent\. For example, [the Java SE platform](java-se-platform.md) sets environment variables that you retrieve with `System.getenv`, while [the Tomcat platform](java-tomcat-platform.md) sets Java system properties that you retrieve with `System.getProperty`\.

In addition to the standard set of options available for all environments, most Elastic Beanstalk platforms let you specify language\-specific or framework\-specific settings\. These can take the following forms\.

**Platform\-Specific Settings**
+ **Preset environment properties** – The Ruby platform uses environment properties for framework settings, such as `RACK_ENV` and `BUNDLE_WITHOUT`\.
+ **Placeholder environment properties** – The Tomcat platform defines an environment property named `JDBC_CONNECTION_STRING` that is not set to any value\. This type of setting was more common on older platform versions\.
+ **Configuration options** – Most platforms define [configuration options](command-options.md) in platform\-specific or shared namespaces, such as `aws:elasticbeanstalk:xray` or `aws:elasticbeanstalk:container:python`\.

For information about platform\-specific options, see the platform topic for your language or framework:
+ Go – [Using the AWS Elastic Beanstalk Go Platform](go-environment.md)
+ Java SE – [Using the AWS Elastic Beanstalk Java SE Platform](java-se-platform.md)
+ Tomcat – [Using the AWS Elastic Beanstalk Tomcat Platform](java-tomcat-platform.md)
+ \.NET – [Using the AWS Elastic Beanstalk \.NET Platform](create_deploy_NET.container.console.md)
+ Node\.js – [Using the AWS Elastic Beanstalk Node\.js Platform](create_deploy_nodejs.container.md)
+ PHP – [Using the AWS Elastic Beanstalk PHP Platform](create_deploy_PHP.container.md)
+ Python – [Using the AWS Elastic Beanstalk Python Platform](create-deploy-python-container.md)
+ Ruby – [Using the AWS Elastic Beanstalk Ruby Platform](create_deploy_Ruby.container.md)

Also, when you [add a database to your environment](using-features.managing.db.md), Elastic Beanstalk sets environment properties, such as `RDS_HOSTNAME`, that you can read in your application code to construct a connection object or string\.

**Topics**
+ [Configuring Environment Properties](#environments-cfg-softwaresettings-console)
+ [Software Setting Namespaces](#environments-cfg-softwaresettings-configfiles)
+ [Accessing Environment Properties](#environments-cfg-softwaresettings-accessing)
+ [Configuring AWS X\-Ray Debugging](environment-configuration-debugging.md)
+ [Viewing Logs from Your Elastic Beanstalk Environment's Amazon EC2 Instances](environments-cfg-logging.md)

## Configuring Environment Properties<a name="environments-cfg-softwaresettings-console"></a>

Environment properties appear in the Elastic Beanstalk console under **Software Configuration**\.

**To configure environment properties in the Elastic Beanstalk console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. Choose **Configuration**\.

1. On the **Software** configuration card, choose **Modify**\.

1. Under **Environment properties**, enter key\-value pairs\.

1. Choose **Save**, and then choose **Apply**\.

**Environment Property Limits**
+ **Keys** can contain any alphanumeric characters and the following symbols: `_ . : / + \ - @`

  The symbols listed are valid for environment property keys, but might not be valid for environment variable names on your environment's platform\. For compatibility with all platforms, limit environment properties to the following pattern: `[A-Z_][A-Z0-9_]*`
+ **Values** can contain any alphanumeric characters, white space, and the following symbols: `_ . : / = + \ - @ ' "`
**Note**  
Single and double quotation marks in values must be escaped\.
+ **Keys** can contain up to 128 characters\. **Values** can contain up to 256 characters\.
+ **Keys** and **values** are case sensitive\.
+ The combined size of all environment properties cannot exceed 4,096 bytes when stored as strings with the format *key*=*value*\.

## Software Setting Namespaces<a name="environments-cfg-softwaresettings-configfiles"></a>

You can use a [configuration file](ebextensions.md) to set configuration options and perform other instance configuration tasks during deployments\. Configuration options can be defined by the Elastic Beanstalk service or the platform that you use and are organized into *namespaces*\.

You can use Elastic Beanstalk [configuration files](ebextensions.md) to set environment properties and configuration options in your source code\. Use the [`aws:elasticbeanstalk:application:environment` namespace](command-options-general.md#command-options-general-elasticbeanstalkapplicationenvironment) to define environment properties\.

**Example \.ebextensions/options\.config**  

```
option_settings:
  aws:elasticbeanstalk:application:environment:
    API_ENDPOINT: www.example.com/api
```

If you use configuration files or AWS CloudFormation templates to create [custom resources](environment-resources.md), you can use an AWS CloudFormation function to get information about the resource and assign it to an environment property dynamically during deployment\. The following example from the the [elastic\-beanstalk\-samples](https://github.com/awslabs/elastic-beanstalk-docs/) GitHub repository uses the [Ref function](ebextensions-functions.md) to get the ARN of an Amazon SNS topic that it creates, and assigns it to an environment property named `NOTIFICATION_TOPIC`\.

**Example \.ebextensions/[sns\-topic\.config](https://github.com/awslabs/elastic-beanstalk-docs/blob/master/configuration-files/aws-provided/resource-configuration/sns-topic.config)**  

```
Resources:
  NotificationTopic:
    Type: AWS::SNS::Topic

option_settings:
  aws:elasticbeanstalk:application:environment:
    NOTIFICATION_TOPIC: '`{"Ref" : "NotificationTopic"}`'
```

You can also use this feature to propagate information from [AWS CloudFormation pseudo parameters](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/pseudo-parameter-reference.html)\. This example gets the current region and assigns it to a property named `AWS_REGION`\.

**Example \.ebextensions/[env\-regionname\.config](https://github.com/awslabs/elastic-beanstalk-docs/blob/master/configuration-files/aws-provided/instance-configuration/env-regionname.config)**  

```
option_settings:
  aws:elasticbeanstalk:application:environment:
    AWS_REGION: '`{"Ref" : "AWS::Region"}`'
```

Most Elastic Beanstalk platforms define additional namespaces with options for configuring software that runs on the instance, such as the reverse proxy that relays requests to your application\. For more information about the namespaces available for your platform, see the following:
+ Go – [The aws:elasticbeanstalk:container:golang:staticfiles Namespace](go-environment.md#go-namespaces)
+ Java SE – [The aws:elasticbeanstalk:container:java:staticfiles Namespace](java-se-platform.md#java-se-namespaces)
+ Tomcat – [Tomcat Configuration Namespaces](java-tomcat-platform.md#java-tomcat-namespaces)
+ \.NET – [The aws:elasticbeanstalk:container:dotnet:apppool Namespace](create_deploy_NET.container.console.md#dotnet-namespaces)
+ Node\.js – [Node\.js Configuration Namespaces](create_deploy_nodejs.container.md#nodejs-namespaces)
+ PHP – [The aws:elasticbeanstalk:container:php:phpini Namespace](create_deploy_PHP.container.md#php-namespaces)
+ Python – [Python Configuration Namespaces](create-deploy-python-container.md#python-namespaces)
+ Ruby – [Ruby Configuration Namespaces](create_deploy_Ruby.container.md#ruby-namespaces)

Elastic Beanstalk provides many configuration options for customizing your environment\. In addition to configuration files, you can also set configuration options using the console, saved configurations, the EB CLI, or the AWS CLI\. See [Configuration Options](command-options.md) for more information\.

## Accessing Environment Properties<a name="environments-cfg-softwaresettings-accessing"></a>

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
+ [\.NET](create_deploy_NET.container.console.md#dotnet-console-properties) – `appConfig`

  ```
  NameValueCollection appConfig = ConfigurationManager.AppSettings;
  string endpoint = appConfig["API_ENDPOINT"];
  ```
**Note**  
Elastic Beanstalk doesn't support passing environment variables to \.NET Core applications and multiple\-application IIS deployments that use a [deployment manifest](dotnet-manifest.md)\.
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

Outside of application code, such as in a script that runs during deployment, you can access environment properties with the [`get-config` platform script](custom-platforms-scripts.md)\. See the [elastic\-beanstalk\-samples](https://github.com/awslabs/elastic-beanstalk-docs/search?utf8=%E2%9C%93&q=get-config) GitHub repository for example configurations that use `get-config`\.