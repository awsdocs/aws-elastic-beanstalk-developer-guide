# Environment manifest \(`env.yaml`\)<a name="environment-cfg-manifest"></a>

You can include a YAML formatted environment manifest in the root of your application source bundle to configure the environment name, solution stack and [environment links](environment-cfg-links.md) to use when creating your environment\.

This file format includes support for environment groups\. To use groups, specify the environment name in the manifest with a \+ symbol at the end\. When you create or update the environment, specify the group name with `--group-name` \(AWS CLI\) or `--env-group-suffix` \(EB CLI\)\. For more information on groups, see [Creating and updating groups of Elastic Beanstalk environments](environment-mgmt-compose.md)\.

The following example manifest defines a web server environment with a link to a worker environment component that it is dependent upon\. The manifest uses groups to allow creating multiple environments with the same source bundle:

**`~/myapp/frontend/env.yaml`**

```
AWSConfigurationTemplateVersion: 1.1.0.0
SolutionStack: 64bit Amazon Linux 2015.09 v2.0.6 running Multi-container Docker 1.7.1 (Generic)
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
Tags:
  Cost Center: WebApp Dev
CName: front-A08G28LG+
EnvironmentName: front+
EnvironmentLinks:
  "WORKERQUEUE" : "worker+"
```

The following keys are supported\.
+ **AWSConfigurationTemplateVersion** \(required\) – The configuration template version \(1\.1\.0\.0\)\.

  ```
  AWSConfigurationTemplateVersion: 1.1.0.0
  ```
+ **Platform** – The Amazon Resource Name \(ARN\) of the environment's platform version\. You can specify the platform by ARN or solution stack name\.

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

  The value of the link variable varies depending on the type of the linked environment\. For a web server environment, the link is the environment's CNAME\. For a worker environment, the link is the name of the environment's Amazon Simple Queue Service \(Amazon SQS\) queue\.

The **CName**, **EnvironmentName** and **EnvironmentLinks** keys can be used to create [environment groups](environment-mgmt-compose.md) and [links to other environments](environment-cfg-links.md)\. These features are currently supported when using the EB CLI, AWS CLI or an SDK\.