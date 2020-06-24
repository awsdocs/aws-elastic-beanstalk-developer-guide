# Setting configuration options before environment creation<a name="environment-configuration-methods-before"></a>

AWS Elastic Beanstalk supports a large number of [configuration options](command-options.md) that let you modify the settings that are applied to resources in your environment\. Several of these options have default values that can be overridden to customize your environment\. Other options can be configured to enable additional features\.

Elastic Beanstalk supports two methods of saving configuration option settings\. Configuration files in YAML or JSON format can be included in your application's source code in a directory named `.ebextensions` and deployed as part of your application source bundle\. You create and manage configuration files locally\.

Saved configurations are templates that you create from a running environment or JSON options file and store in Elastic Beanstalk\. Existing saved configurations can also be extended to create a new configuration\.

**Note**  
Settings defined in configuration files and saved configurations have lower precedence than settings configured during or after environment creation, including recommended values applied by the Elastic Beanstalk console and [EB CLI](eb-cli3.md)\. See [Precedence](command-options.md#configuration-options-precedence) for details\.

Options can also be specified in a JSON document and provided directly to Elastic Beanstalk when you create or update an environment with the EB CLI or AWS CLI\. Options provided directly to Elastic Beanstalk in this manner override all other methods\.

For a full list of available options, see [Configuration options](command-options.md)\.

**Topics**
+ [Configuration files \(`.ebextensions`\)](#configuration-options-before-ebextensions)
+ [Saved configurations](#configuration-options-before-savedconfig)
+ [JSON document](#configuration-options-before-json)
+ [EB CLI configuration](#configuration-options-before-configyml)

## Configuration files \(`.ebextensions`\)<a name="configuration-options-before-ebextensions"></a>

Use `.ebextensions` to configure options that are required to make your application work, and provide default values for other options that can be overridden at a higher level of [precedence](command-options.md#configuration-options-precedence)\. Options specified in `.ebextensions` have the lowest level of precedence and are overridden by settings at any other level\.

To use configuration files, create a folder named `.ebextensions` at the top level of your project's source code\. Add a file with the extension `.config` and specify options in the following manner:

```
option_settings:
  - namespace:  namespace
    option_name:  option name
    value:  option value
  - namespace:  namespace
    option_name:  option name
    value:  option value
```

For example, the following configuration file sets the application's health check url to `/health`:

`healthcheckurl.config`

```
option_settings:
  - namespace:  aws:elasticbeanstalk:application
    option_name:  Application Healthcheck URL
    value:  /health
```

In JSON:

```
{
 "option_settings" :
    [
      {
        "namespace" : "aws:elasticbeanstalk:application",
        "option_name" : "Application Healthcheck URL",
        "value" : "/health"
      }
    ]
}
```

This configures the Elastic Load Balancing load balancer in your Elastic Beanstalk environment to make an HTTP request to the path `/health` to each EC2 instance to determine if it is healthy or not\.

**Note**  
YAML relies on consistent indentation\. Match the indentation level when replacing content in an example configuration file and ensure that your text editor uses spaces, not tab characters, to indent\.

Include the `.ebextensions` directory in your [Application Source Bundle](applications-sourcebundle.md) and deploy it to a new or existing Elastic Beanstalk environment\.

Configuration files support several sections in addition to `option_settings` for customizing the software and files that run on the servers in your environment\. For more information, see [\.Ebextensions](ebextensions.md)\.

## Saved configurations<a name="configuration-options-before-savedconfig"></a>

Create a saved configuration to save settings that you have applied to an existing environment during or after environment creation by using the Elastic Beanstalk console, EB CLI, or AWS CLI\. Saved configurations belong to an application and can be applied to new or existing environments for that application\.

**Topics**
+ [Elastic Beanstalk console](#configuration-options-before-savedconfig-console)
+ [EB CLI](#configuration-options-before-savedconfig-ebcli)
+ [AWS CLI](#configuration-options-before-savedconfig-awscli)

### Elastic Beanstalk console<a name="configuration-options-before-savedconfig-console"></a>

**To create a saved configuration \(Elastic Beanstalk console\)**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. Choose **Environment actions**, and then choose **Save configuration**\.

1. Use the on\-screen dialog box to complete the action\.

Saved configurations are stored in the Elastic Beanstalk S3 bucket in a folder named after your application\. For example, configurations for an application named `my-app` in the us\-west\-2 region for account number 123456789012 can be found at `s3://elasticbeanstalk-us-west-2-123456789012/resources/templates/my-app`\.

### EB CLI<a name="configuration-options-before-savedconfig-ebcli"></a>

The [EB CLI](eb-cli3.md) also provides subcommands for interacting with saved configurations under [eb config](eb3-config.md):

**To create a saved configuration \(EB CLI\)**

1. Save the attached environment's current configuration:

   ```
   ~/project$ eb config save --cfg my-app-v1
   ```

   The EB CLI saves the configuration to `~/project/.elasticbeanstalk/saved_configs/my-app-v1.cfg.yml`

1. Modify the saved configuration locally if needed\.

1. Upload the saved configuration to S3:

   ```
   ~/project$ eb config put my-app-v1
   ```

### AWS CLI<a name="configuration-options-before-savedconfig-awscli"></a>

Create a saved configuration from a running environment with `aws elasticbeanstalk create-configuration-template`

**To create a saved configuration \(AWS CLI\)**

1. Identify your Elastic Beanstalk environment's environment ID with `describe-environments`:

   ```
   $ aws elasticbeanstalk describe-environments --environment-name my-env
   {
       "Environments": [
           {
               "ApplicationName": "my-env",
               "EnvironmentName": "my-env",
               "VersionLabel": "89df",
               "Status": "Ready",
               "Description": "Environment created from the EB CLI using \"eb create\"",
               "EnvironmentId": "e-vcghmm2zwk",
               "EndpointURL": "awseb-e-v-AWSEBLoa-1JUM8159RA11M-43V6ZI1194.us-west-2.elb.amazonaws.com",
               "SolutionStackName": "64bit Amazon Linux 2015.03 v2.0.2 running Multi-container Docker 1.7.1 (Generic)",
               "CNAME": "my-env-nfptuqaper.elasticbeanstalk.com",
               "Health": "Green",
               "AbortableOperationInProgress": false,
               "Tier": {
                   "Version": " ",
                   "Type": "Standard",
                   "Name": "WebServer"
               },
               "HealthStatus": "Ok",
               "DateUpdated": "2015-10-01T00:24:04.045Z",
               "DateCreated": "2015-09-30T23:27:55.768Z"
           }
       ]
   }
   ```

1. Save the environment's current configuration with `create-configuration-template`:

   ```
   $ aws elasticbeanstalk create-configuration-template --environment-id e-vcghmm2zwk --application-name my-app --template-name v1
   ```

Elastic Beanstalk saves the configuration to your Elastic Beanstalk bucket in Amazon S3\.

## JSON document<a name="configuration-options-before-json"></a>

If you use the AWS CLI to create and update environments, you can also provide configuration options in JSON format\. A library of configuration files in JSON is useful if you use the AWS CLI to create and manage environments\.

For example, the following JSON document sets the application's health check url to `/health`:

**\~/ebconfigs/healthcheckurl\.json**

```
[
  {
    "Namespace": "aws:elasticbeanstalk:application",
    "OptionName": "Application Healthcheck URL",
    "Value": "/health"
  }
]
```

## EB CLI configuration<a name="configuration-options-before-configyml"></a>

In addition to supporting saved configurations and direct environment configuration with eb config commands, the EB CLI has a configuration file with an option named `default_ec2_keyname` that you can use to specify an Amazon EC2 key pair for SSH access to the instances in your environment\. The EB CLI uses this option to set the `EC2KeyName` configuration option in the `aws:autoscaling:launchconfiguration` namespace\. 

**\~/workspace/my\-app/\.elasticbeanstalk/config\.yml**

```
branch-defaults:
  master:
    environment: my-env
  develop:
    environment: my-env-dev
deploy:
  artifact: ROOT.war
global:
  application_name: my-app
  default_ec2_keyname: my-keypair
  default_platform: Tomcat 8 Java 8
  default_region: us-west-2
  profile: null
  sc: git
```