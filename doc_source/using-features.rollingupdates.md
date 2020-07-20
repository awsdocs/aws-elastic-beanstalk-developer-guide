# Elastic Beanstalk rolling environment configuration updates<a name="using-features.rollingupdates"></a>

When a [configuration change requires replacing instances](environments-updating.md), Elastic Beanstalk can perform the update in batches to avoid downtime while the change is propagated\. During a rolling update, capacity is only reduced by the size of a single batch, which you can configure\. Elastic Beanstalk takes one batch of instances out of service, terminates them, and then launches a batch with the new configuration\. After the new batch starts serving requests, Elastic Beanstalk moves on to the next batch\.

Rolling configuration update batches can be processed periodically \(time\-based\), with a delay between each batch, or based on health\. For time\-based rolling updates, you can configure the amount of time that Elastic Beanstalk waits after completing the launch of a batch of instances before moving on to the next batch\. This pause time allows your application to bootstrap and start serving requests\.

With health\-based rolling updates, Elastic Beanstalk waits until instances in a batch pass health checks before moving on to the next batch\. The health of an instance is determined by the health reporting system, which can be basic or enhanced\. With [basic health](using-features.healthstatus.md), a batch is considered healthy as soon as all instances in it pass Elastic Load Balancing \(ELB\) health checks\.

With [enhanced health reporting](health-enhanced.md), all of the instances in a batch must pass multiple consecutive health checks before Elastic Beanstalk will move on to the next batch\. In addition to ELB health checks, which check only your instances, enhanced health monitors application logs and the state of your environment's other resources\. In a web server environment with enhanced health, all instances must pass 12 health checks over the course of two minutes \(18 checks over three minutes for worker environments\)\. If any instance fails one health check, the count resets\.

If a batch doesn't become healthy within the rolling update timeout \(default is 30 minutes\), the update is canceled\. Rolling update timeout is a [configuration option](command-options.md) that is available in the `aws:autoscaling:updatepolicy:rollingupdate` namespace\. If your application doesn't pass health checks with `Ok` status but is stable at a different level, you can set the `HealthCheckSuccessThreshold` option in the [`aws:elasticbeanstalk:healthreporting:system`](command-options-general.md#command-options-general-elasticbeanstalkhealthreporting) namespace to change the level at which Elastic Beanstalk considers an instance to be healthy\.

If the rolling update process fails, Elastic Beanstalk starts another rolling update to roll back to the previous configuration\. A rolling update can fail due to failed health checks or if launching new instances causes you to exceed the quotas on your account\. If you hit a quota on the number of Amazon EC2 instances, for example, the rolling update can fail when it attempts to provision a batch of new instances\. In this case, the rollback fails as well\.

A failed rollback ends the update process and leaves your environment in an unhealthy state\. Unprocessed batches are still running instances with the old configuration, while any batches that completed successfully have the new configuration\. To fix an environment after a failed rollback, first resolve the underlying issue that caused the update to fail, and then initiate another environment update\.

An alternative method is to deploy the new version of your application to a different environment and then perform a CNAME swap to redirect traffic with zero downtime\. See [Blue/Green deployments with Elastic Beanstalk](using-features.CNAMESwap.md) for more information\.

## Rolling updates versus rolling deployments<a name="environments-cfg-rollingupdates-deployments"></a>

Rolling updates occur when you change settings that require new Amazon EC2 instances to be provisioned for your environment\. This includes changes to the Auto Scaling group configuration, such as instance type and key\-pair settings, and changes to VPC settings\. In a rolling update, each batch of instances is terminated before a new batch is provisioned to replace it\.

[Rolling deployments](using-features.rolling-version-deploy.md) occur whenever you deploy your application and can typically be performed without replacing instances in your environment\. Elastic Beanstalk takes each batch out of service, deploys the new application version, and then places it back in service\.

The exception to this is if you change settings that require instance replacement at the same time you deploy a new application version\. For example, if you change the [key name](command-options-general.md#command-options-general-autoscalinglaunchconfiguration) settings in a [configuration file](ebextensions.md) in your source bundle and deploy it to your environment, you trigger a rolling update\. Instead of deploying your new application version to each batch of existing instances, a new batch of instances is provisioned with the new configuration\. In this case, a separate deployment doesn't occur because the new instances are brought up with the new application version\.

Anytime new instances are provisioned as part of an environment update, there is a deployment phase where your application's source code is deployed to the new instances and any configuration settings that modify the operating system or software on the instances are applied\. [Deployment health check settings](using-features.rolling-version-deploy.md#environments-cfg-rollingdeployments-console) \(**Ignore health check**, **Healthy threshold**, and **Command timeout**\) also apply to health\-based rolling updates and immutable updates during the deployment phase\.

## Configuring rolling updates<a name="rollingupdates-configure"></a>

You can enable and configure rolling updates in the Elastic Beanstalk console\.

**To enable rolling updates**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Configuration**\.

1. In the **Rolling updates and deployments** configuration category, choose **Edit**\.

1. In the **Configuration updates** section, for **Rolling update type**, select one of the **Rolling** options\.  
![\[The configuration updates section on the modify rolling updates and deployments configuration page\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-config-rolling-updates-health.png)

1. Choose **Batch size**, **Minimum capacity**, and **Pause time** settings\.

1. Choose **Apply**\.

The **Configuration updates** section of the **Rolling updates and deployments** page has the following options for rolling updates:
+ **Rolling update type** – Elastic Beanstalk waits after it finishes updating a batch of instances before moving on to the next batch, to allow those instances to finish bootstrapping and start serving traffic\. Choose from the following options:
  + **Rolling based on Health** – Wait until instances in the current batch are healthy before placing instances in service and starting the next batch\.
  + **Rolling based on Time** – Specify an amount of time to wait between launching new instances and placing them in service before starting the next batch\.
  + **Immutable** – Apply the configuration change to a fresh group of instances by performing an [immutable update](environmentmgmt-updates-immutable.md)\.
+ **Batch size** – The number of instances to replace in each batch, between **1** and **10000**\. By default, this value is one\-third of the minimum size of the Auto Scaling group, rounded up to a whole number\.
+ **Minimum capacity** – The minimum number of instances to keep running while other instances are updated, between **0** and **9999**\. The default value is either the minimum size of the Auto Scaling group or one less than the maximum size of the Auto Scaling group, whichever number is lower\.
+ **Pause time** \(time\-based only\) – The amount of time to wait after a batch is updated before moving on to the next batch, to allow your application to start receiving traffic\. Between 0 seconds and one hour\.

## The aws:autoscaling:updatepolicy:rollingupdate namespace<a name="rollingupdate-namespace"></a>

You can also use the [configuration options](command-options.md) in the `aws:autoscaling:updatepolicy:rollingupdate` namespace to configure rolling updates\. 

Use the `RollingUpdateEnabled` option to enable rolling updates, and `RollingUpdateType` to choose the update type\. The following values are supported for `RollingUpdateType`:
+ `Health` – Wait until instances in the current batch are healthy before placing instances in service and starting the next batch\.
+ `Time` – Specify an amount of time to wait between launching new instances and placing them in service before starting the next batch\.
+ `Immutable` – Apply the configuration change to a fresh group of instances by performing an [immutable update](environmentmgmt-updates-immutable.md)\.

When you enable rolling updates, set the `MaxBatchSize` and `MinInstancesInService` options to configure the size of each batch\. For time\-based and health\-based rolling updates, you can also configure a `PauseTime` and `Timeout`, respectively\.

For example, to launch up to five instances at a time, while maintaining at least two instances in service, and wait five minutes and 30 seconds between batches, specify the following options and values\.

**Example \.ebextensions/timebased\.config**  

```
option_settings:
  aws:autoscaling:updatepolicy:rollingupdate:
    RollingUpdateEnabled: true
    MaxBatchSize: 5
    MinInstancesInService: 2
    RollingUpdateType: Time
    PauseTime: PT5M30S
```

To enable health\-based rolling updates, with a 45\-minute timeout for each batch, specify the following options and values\.

**Example \.ebextensions/healthbased\.config**  

```
option_settings:
  aws:autoscaling:updatepolicy:rollingupdate:
    RollingUpdateEnabled: true
    MaxBatchSize: 5
    MinInstancesInService: 2
    RollingUpdateType: Health
    Timeout: PT45M
```

`Timeout` and `PauseTime` values must be specified in [ISO8601 duration](http://en.wikipedia.org/wiki/ISO_8601#Durations): `PT#H#M#S`, where each \# is the number of hours, minutes, or seconds, respectively\.

The EB CLI and Elastic Beanstalk console apply recommended values for the preceding options\. You must remove these settings if you want to use configuration files to configure the same\. See [Recommended values](command-options.md#configuration-options-recommendedvalues) for details\.