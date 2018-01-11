# Environment Properties and Other Software Settings<a name="environments-cfg-softwaresettings"></a>

You can use **environment properties** to pass secrets, endpoints, debug settings, and other information to your application\. Environment properties help you run your application in multiple environments for different purposes, such as development, testing, staging, and production\.

**Environment Variables**  
In most cases, environment properties are passed to your application as *environment variables*, but the behavior is platform dependent\. For example, the Java SE platform sets environment variables that you retrieve with `System.getenv`, while the Tomcat platform sets Java system properties that you retrieve with `System.getProperty`\.

In addition to the standard set of options available for all environments, most Elastic Beanstalk platforms let you set language\-specific or framework\-specific settings\. These can take the following forms\.

**Platform\-Specific Settings**

+ **Preset environment properties** – The Ruby platform uses environment properties for framework settings like `RACK_ENV` and `BUNDLE_WITHOUT`\.

+ **Placeholder environment properties** – The Tomcat platform defines an environment property named `JDBC_CONNECTION_STRING` that is not set to any value\. This type of setting was more common on older platform versions\.

+ **Configuration options** – Most platforms define configuration options in platform\-specific or shared namespaces like `aws:elasticbeanstalk:xray` or `aws:elasticbeanstalk:container:python`\.

For information about platform\-specific options, see the platform topic for your language or framework:

+ Go – 

+ Java SE – 

+ Tomcat – 

+ \.NET – 

+ Node\.js – 

+ PHP – 

+ Python – 

+ Ruby – 

Also, when you add a database to your environment, Elastic Beanstalk sets environment properties such as `RDS_HOSTNAME` that you can read in your application code to construct a connection object or string\.

## Configuring Environment Properties<a name="environments-cfg-softwaresettings-console"></a>

Environment properties appear in the console under **Software Configuration**\.

**To configure environment properties in the Elastic Beanstalk console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the management page for your environment\.

1. Choose **Configuration**\.

1. In the **Software Configuration** section, choose the settings icon \( ![\[Edit\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/cog.png)\)\.

1. Under **Environment Properties**, enter key\-value pairs\.

1. Choose **Apply**\.

**Environment Property Limits**

+ **Keys** can contain any alphanumeric characters and the following symbols: `_`, `.`, `:`, `/`, `+`, `\`, `-`, `@`

  The symbols listed are valid for environment property keys, but may not be valid for environment variable names on your environment's platform\. For compatibility with all platforms, limit environment properties to the following pattern: `[A-Z_][A-Z0-9_]*`

+ **Values** can contain any alphanumeric characters, white space, and the following symbols: `_`, `.`, `:`, `/`, `=`, `+`, `\`, `-`, `@`, `'`\*, `"`\*

  \*Single and double quotation marks in values must be escaped\.

+ **Keys** can contain up to 128 characters\. **Values** can contain up to 256 characters\.

+ **Keys** and **values** are case sensitive\.

+ The combined size of all environment properties cannot exceed 4,096 bytes when stored as strings with the format *key*=*value*\.

## Software Setting Namespaces<a name="environments-cfg-softwaresettings-configfiles"></a>

You can use a configuration file to set configuration options and perform other instance configuration tasks during deployments\. Configuration options can be defined by the Elastic Beanstalk service or the platform that you use and are organized into *namespaces*\.

You can use Elastic Beanstalk configuration files to set environment properties and configuration options in your source code\. Use the `aws:elasticbeanstalk:application:environment` namespace to define environment properties\.

**Example \.ebextensions/options\.config**  

```
option_settings:
  aws:elasticbeanstalk:application:environment:
    API_ENDPOINT: www.example.com/api
```

If you use configuration files or AWS CloudFormation templates to create custom resources, you can use an AWS CloudFormation function to get information about the resource and assign it to an environment property dynamically during deployment\. The following example from the the [elastic\-beanstalk\-docs](https://github.com/awslabs/elastic-beanstalk-docs/) GitHub repository uses the Ref function to get the ARN of an Amazon SNS topic that it creates, and assigns it to an environment property named `NOTIFICATION_TOPIC`\.

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

Most Elastic Beanstalk platforms define additional namespaces with options for configuring software that runs on the instance, such as the reverse proxy that relays requests to your application\. For more information about the namespaces available for your platform, see one of the following sections:

+ Go – 

+ Java SE – 

+ Tomcat – 

+ \.NET – 

+ Node\.js – 

+ PHP – 

+ Python – 

+ Ruby – 

Elastic Beanstalk provides many configuration options for customizing your environment\. In addition to configuration files, you can also set configuration options using the console, saved configurations, the EB CLI, or the AWS CLI\. See  for more information\.

## Accessing Environment Properties<a name="environments-cfg-softwaresettings-accessing"></a>

In most cases, you access environment properties in your application code like an environment variable\. In general, however, environment properties are passed only to the application and can't be viewed by connecting an instance in your environment and running `env`\.

+ Go – `os.Getenv`

  ```
  endpoint := os.Getenv("API_ENDPOINT")
  ```

+ Java SE – `System.getenv`

  ```
  String endpoint = System.getenv("API_ENDPOINT");
  ```

+ Tomcat – `System.getProperty`

  ```
  String endpoint = System.getProperty("API_ENDPOINT");
  ```

+ \.NET – `appConfig`

  ```
  NameValueCollection appConfig = ConfigurationManager.AppSettings;
  string endpoint = appConfig["API_ENDPOINT"];
  ```

+ Node\.js – `process.env`

  ```
  var endpoint = process.env.API_ENDPOINT
  ```

+ PHP – `$_SERVER`

  ```
  $endpoint = $_SERVER['API_ENDPOINT'];
  ```

+ Python – `os.environ`

  ```
  import os
  endpoint = os.environ['API_ENDPOINT']
  ```

+ Ruby – `ENV`

  ```
  endpoint = ENV['API_ENDPOINT']
  ```

Outside of application code, such as in a script that runs during deployment, you can access environment properties with the `get-config` platform script\. See the [elastic\-beanstalk\-docs](https://github.com/awslabs/elastic-beanstalk-docs/search?utf8=%E2%9C%93&q=get-config) GitHub repository for example configurations that use `get-config`\.