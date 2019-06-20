# Environment Manifest \(`env.yaml`\)<a name="environment-cfg-manifest"></a>

You can include a YAML formatted environment manifest in the root of your application source bundle to configure the environment name, solution stack and [environment links](environment-cfg-links.md) to use when creating your environment\. An environment manifest uses the same format as [Saved Configurations](environment-configuration-savedconfig.md)\.

This file format includes support for environment groups\. To use groups, specify the environment name in the manifest with a \+ symbol at the end\. When you create or update the environment, specify the group name with `--group-name` \(AWS CLI\) or `--env-group-suffix` \(EB CLI\)\. For more information on groups, see [Creating and Updating Groups of AWS Elastic Beanstalk Environments](environment-mgmt-compose.md)\.

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

For more information on the saved configuration format and supported keys, see [Using Elastic Beanstalk Saved Configurations](environment-configuration-savedconfig.md)