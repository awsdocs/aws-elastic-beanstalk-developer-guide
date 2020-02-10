# Configuration options<a name="command-options"></a>

Elastic Beanstalk defines a large number of configuration options that you can use to configure your environment's behavior and the resources that it contains\. Configuration options are organized into namespaces like `aws:autoscaling:asg`, which defines options for an environment's Auto Scaling group\.

The Elastic Beanstalk console and EB CLI set configuration options when you create an environment, including options that you set explicitly, and [recommended values](#configuration-options-recommendedvalues) defined by the client\. You can also set configuration options in saved configurations and configuration files\. If the same option is set in multiple locations, the value used is determined by the [order of precedence](#configuration-options-precedence)\.

Configuration option settings can be composed in text format and saved prior to environment creation, applied during environment creation using any supported client, and added, modified or removed after environment creation\. For a detailed breakdown of all of the available methods for working with configuration options at each of these three stages, read the following topics:
+ [Setting configuration options before environment creation](environment-configuration-methods-before.md)
+ [Setting configuration options during environment creation](environment-configuration-methods-during.md)
+ [Setting configuration options after environment creation](environment-configuration-methods-after.md)

For a complete list of namespaces and options, including default and supported values for each, see [General options for all environments](command-options-general.md) and [Platform specific options](command-options-specific.md)\.

## Precedence<a name="configuration-options-precedence"></a>

During environment creation, configuration options are applied from multiple sources with the following precedence, from highest to lowest:
+ **Settings applied directly to the environment** – Settings specified during a create environment or update environment operation on the Elastic Beanstalk API by any client, including the Elastic Beanstalk console, EB CLI, AWS CLI, and SDKs\. The Elastic Beanstalk console and EB CLI also apply [recommended values](#configuration-options-recommendedvalues) for some options that apply at this level unless overridden\.
+ **Saved Configurations** – Settings for any options that are not applied directly to the environment are loaded from a saved configuration, if specified\.
+ **Configuration Files \(\.ebextensions\)** – Settings for any options that are not applied directly to the environment, and also not specified in a saved configuration, are loaded from configuration files in the `.ebextensions` folder at the root of the application source bundle\.

  Configuration files are executed in alphabetical order\. For example, `.ebextensions/01run.config` is executed before `.ebextensions/02do.config`\.
+ **Default Values** – If a configuration option has a default value, it only applies when the option is not set at any of the above levels\.

If the same configuration option is defined in more than one location, the setting with the highest precedence is applied\. When a setting is applied from a saved configuration or settings applied directly to the environment, the setting is stored as part of the environment's configuration\. These settings can be removed [with the AWS CLI](environment-configuration-methods-after.md#configuration-options-remove-awscli) or [with the EB CLI](environment-configuration-methods-after.md#configuration-options-remove-ebcli)\.

Settings in configuration files are not applied directly to the environment and cannot be removed without modifying the configuration files and deploying a new application version\. If a setting applied with one of the other methods is removed, the same setting will be loaded from configuration files in the source bundle\.

For example, say you set the minimum number of instances in your environment to 5 during environment creation, using either the Elastic Beanstalk console, a command line option, or a saved configuration\. The source bundle for your application also includes a configuration file that sets the minimum number of instances to 2\.

When you create the environment, Elastic Beanstalk sets the `MinSize` option in the `aws:autoscaling:asg` namespace to 5\. If you then remove the option from the environment configuration, the value in the configuration file is loaded, and the minimum number of instances is set to 2\. If you then remove the configuration file from the source bundle and redeploy, Elastic Beanstalk uses the default setting of 1\.

## Recommended values<a name="configuration-options-recommendedvalues"></a>

The Elastic Beanstalk Command Line Interface \(EB CLI\) and Elastic Beanstalk console provide recommended values for some configuration options\. These values can be different from the default values and are set at the API level when your environment is created\. Recommended values allow Elastic Beanstalk to improve the default environment configuration without making backwards incompatible changes to the API\.

For example, both the EB CLI and Elastic Beanstalk console set the configuration option for EC2 instance type \(`InstanceType` in the `aws:autoscaling:launchconfiguration` namespace\)\. Each client provides a different way of overriding the default setting\. In the console you can choose a different instance type from a drop down menu on the **Configuration Details** page of the **Create New Environment** wizard\. With the EB CLI, you can use the `--instance_type` parameter for [eb create](eb3-create.md)\.

Because the recommended values are set at the API level, they will override values for the same options that you set in configuration files or saved configurations\. The following options are set:

**Elastic Beanstalk console**
+ Namespace: `aws:autoscaling:launchconfiguration`

  Option Names: `IamInstanceProfile`, `EC2KeyName`, `InstanceType`
+ Namespace: `aws:autoscaling:updatepolicy:rollingupdate`

  Option Names: `RollingUpdateType` and `RollingUpdateEnabled`
+ Namespace: `aws:elasticbeanstalk:application`

  Option Name: `Application Healthcheck URL`
+ Namespace: `aws:elasticbeanstalk:command`

  Option Name: `DeploymentPolicy`, `BatchSize` and `BatchSizeType`
+ Namespace: `aws:elasticbeanstalk:environment`

  Option Name: `ServiceRole`
+ Namespace: `aws:elasticbeanstalk:healthreporting:system`

  Option Name: `SystemType` and `HealthCheckSuccessThreshold`
+ Namespace: `aws:elasticbeanstalk:sns:topics`

  Option Name: `Notification Endpoint`
+ Namespace: `aws:elasticbeanstalk:sqsd`

  Option Name: `HttpConnections`
+ Namespace: `aws:elb:loadbalancer`

  Option Name: `CrossZone`
+ Namespace: `aws:elb:policies`

  Option Names: `ConnectionDrainingTimeout` and `ConnectionDrainingEnabled`

**EB CLI**
+ Namespace: `aws:autoscaling:launchconfiguration`

  Option Names: `IamInstanceProfile`, `InstanceType`
+ Namespace: `aws:autoscaling:updatepolicy:rollingupdate`

  Option Names: `RollingUpdateType` and `RollingUpdateEnabled`
+ Namespace: `aws:elasticbeanstalk:command`

  Option Name: `BatchSize` and `BatchSizeType`
+ Namespace: `aws:elasticbeanstalk:environment`

  Option Name: `ServiceRole`
+ Namespace: `aws:elasticbeanstalk:healthreporting:system`

  Option Name: `SystemType`
+ Namespace: `aws:elb:loadbalancer`

  Option Name: `CrossZone`
+ Namespace: `aws:elb:policies`

  Option Names: `ConnectionDrainingEnabled`