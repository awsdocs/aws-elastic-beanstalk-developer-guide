# Using Elastic Beanstalk Saved Configurations<a name="environment-configuration-savedconfig"></a>

You can save your environment's configuration as an object in Amazon S3 that can be applied to other environments during environment creation, or applied to a running environment\. *Saved configurations* are YAML formatted templates that define an environment's [platform configuration](concepts.platforms.md), [tier](concepts.md#concepts-tier), [configuration option](command-options.md) settings, and tags\.

Create a saved configuration from the current state of your environment in the Elastic Beanstalk Management Console\.

**To save an environment's configuration**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. Choose **Actions** and then choose **Save Configuration**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environment-cfg-saveconfiguration.png)

1. Type a configuration name and description and then choose **Save**\.

The saved configuration includes any settings that you have applied to the environment with the console or any other client that uses the Elastic Beanstalk API\. You can then apply the saved configuration to your environment at a later date to restore it to its previous state, or apply it to a new environment during [environment creation](environments-create-wizard.md)\.

You can download a configuration using the EB CLI [eb config](eb3-config.md) command, as shown in the following example, *NAME* is the name of your saved configuration\. 

```
eb config get NAME
```

**To apply a saved configuration during environment creation \(AWS Management Console\)**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Choose an application\.

1. Choose **Saved Configurations**\.

1. Choose a saved configuration, and then choose **Launch environment**\.

1. Proceed through the wizard to create your environment\.

Saved configurations do not include settings applied with [configuration files](ebextensions.md) in your application's source code\. If the same setting is applied in both a configuration file and saved configuration, the setting in the saved configuration takes precedence\. Likewise, options specified in the AWS Management Console override options in saved configurations\. For more information, see [Precedence](command-options.md#configuration-options-precedence)\.

Saved configurations are stored in the Elastic Beanstalk S3 bucket in a folder named after your application\. For example, configurations for an application named `my-app` in the us\-west\-2 region for account number 123456789012 can be found at `s3://elasticbeanstalk-us-west-2-123456789012/resources/templates/my-app/`\.

View the contents of a saved configuration by opening it in a text editor\. The following example configuration shows the configuration of a web server environment launched with the Elastic Beanstalk Management Console\.

```
EnvironmentConfigurationMetadata:
  Description: Saved configuration from a multicontainer Docker environment created with the Elastic Beanstalk Management Console
  DateCreated: '1520633151000'
  DateModified: '1520633151000'
Platform:
  PlatformArn: arn:aws:elasticbeanstalk:us-east-2::platform/Java 8 running on 64bit Amazon Linux/2.5.0
OptionSettings:
  aws:elasticbeanstalk:command:
    BatchSize: '30'
    BatchSizeType: Percentage
  aws:elasticbeanstalk:sns:topics:
    Notification Endpoint: me@example.com
  aws:elb:policies:
    ConnectionDrainingEnabled: true
    ConnectionDrainingTimeout: '20'
  aws:elb:loadbalancer:
    CrossZone: true
  aws:elasticbeanstalk:environment:
    ServiceRole: aws-elasticbeanstalk-service-role
  aws:elasticbeanstalk:application:
    Application Healthcheck URL: /
  aws:elasticbeanstalk:healthreporting:system:
    SystemType: enhanced
  aws:autoscaling:launchconfiguration:
    IamInstanceProfile: aws-elasticbeanstalk-ec2-role
    InstanceType: t2.micro
    EC2KeyName: workstation-uswest2
  aws:autoscaling:updatepolicy:rollingupdate:
    RollingUpdateType: Health
    RollingUpdateEnabled: true
EnvironmentTier:
  Type: Standard
  Name: WebServer
AWSConfigurationTemplateVersion: 1.1.0.0
Tags:
  Cost Center: WebApp Dev
```

You can modify the contents of a saved configuration and save it in the same location in Amazon S3\. Any properly formatted saved configuration stored in the right location can be applied to an environment with the Elastic Beanstalk Management Console\.

The following keys are supported\.
+ **AWSConfigurationTemplateVersion** \(required\) – The configuration template version \(1\.1\.0\.0\)\.

  ```
  AWSConfigurationTemplateVersion: 1.1.0.0
  ```
+ **Platform** – The Amazon Resource Name \(ARN\) of the environment's platform configuration\. You can specify the platform by ARN or solution stack name\.

  ```
  Platform:
    PlatformArn: arn:aws:elasticbeanstalk:us-east-2::platform/Java 8 running on 64bit Amazon Linux/2.5.0
  ```
+ **SolutionStack** – The full name of the [solution stack](concepts.platforms.md) used to create the environment\.

  ```
  SolutionStack: 64bit Amazon Linux 2017.03 v2.5.0 running Java 8
  ```
+ **OptionSettings** – [Configuration option](command-options.md) settings to apply to the environment\. For example, the following entry sets the instance type to t2\.micro\.

  ```
  OptionSettings:
    aws:autoscaling:launchconfiguration:
    InstanceType: t2.micro
  ```
+ **Tags** – Up to 47 tags to apply to resources created within the environment\.

  ```
  Tags:
    Cost Center: WebApp Dev
  ```
+ **EnvironmentTier** – The type of environment to create\. For a web server environment, you can exclude this section \(web server is the default\)\. For a worker environment, use the following\.

  ```
  EnvironmentTier:
    Name: Worker
    Type: SQS/HTTP
  ```
+ **CName** – The CNAME for the environment\. Include a \+ character at the end of the name to enable groups\.

  ```
  CName: front-A08G28LG+
  ```
+ **EnvironmentName** – The name of the environment to create\. Include a \+ character at the end of the name to enable groups\.

  ```
  EnvironmentName: front+
  ```

  With groups enabled, you must specify a group name when you create the environments\. Elastic Beanstalk appends the group name to the environment name with a hyphen\. For example, with the environment name `front+` and the group name `dev`, Elastic Beanstalk will create the environment with the name `front-dev`\.
+ **EnvironmentLinks** – A map of variable names and environment names of dependencies\. The following example makes the `worker+` environment a dependency and tells Elastic Beanstalk to save the link information to a variable named `WORKERQUEUE`\.

  ```
  EnvironmentLinks:
    "WORKERQUEUE" : "worker+"
  ```

  The value of the link variable varies depending on the type of the linked environment\. For a web server environment, the link is the environment's CNAME\. For a worker environment, the link is the name of the environment's Amazon SQS queue\.

The **CName**, **EnvironmentName** and **EnvironmentLinks** keys can be used to create [environment groups](environment-mgmt-compose.md) and [links to other environments](environment-cfg-links.md)\. These features are currently supported when using the EB CLI, AWS CLI or an SDK\. When using these features, you can include the saved configuration in your source code as an [environment manifest](environment-cfg-manifest.md) instead of referencing a saved configuration stored in Amazon S3\. See the corresponding topics for more information\.

See the following topics for alternate methods of creating and applying saved configurations\.
+ [Setting Configuration Options Before Environment Creation](environment-configuration-methods-before.md)
+ [Setting Configuration Options During Environment Creation](environment-configuration-methods-during.md)
+ [Setting Configuration Options After Environment Creation](environment-configuration-methods-after.md)