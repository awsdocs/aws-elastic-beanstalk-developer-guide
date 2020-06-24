# Immutable environment updates<a name="environmentmgmt-updates-immutable"></a>

Immutable environment updates are an alternative to [rolling updates](using-features.rollingupdates.md)\. Immutable environment updates ensure that configuration changes that require replacing instances are applied efficiently and safely\. If an immutable environment update fails, the rollback process requires only terminating an Auto Scaling group\. A failed rolling update, on the other hand, requires performing an additional rolling update to roll back the changes\.

To perform an immutable environment update, Elastic Beanstalk creates a second, temporary Auto Scaling group behind your environment's load balancer to contain the new instances\. First, Elastic Beanstalk launches a single instance with the new configuration in the new group\. This instance serves traffic alongside all of the instances in the original Auto Scaling group that are running the previous configuration\.

When the first instance passes health checks, Elastic Beanstalk launches additional instances with the new configuration, matching the number of instances running in the original Auto Scaling group\. When all of the new instances pass health checks, Elastic Beanstalk transfers them to the original Auto Scaling group, and terminates the temporary Auto Scaling group and old instances\.

**Note**  
During an immutable environment update, the capacity of your environment doubles for a short time when the instances in the new Auto Scaling group start serving requests and before the original Auto Scaling group's instances are terminated\. If your environment has many instances, or you have a low [on\-demand instance quota](https://aws.amazon.com/ec2/faqs/#How_many_instances_can_I_run_in_Amazon_EC2), ensure that you have enough capacity to perform an immutable environment update\. If you are near the quota, consider using rolling updates instead\.

Immutable updates require [enhanced health reporting](health-enhanced.md) to evaluate your environment's health during the update\. Enhanced health reporting combines standard load balancer health checks with instance monitoring to ensure that the instances running the new configuration are [serving requests successfully](health-enhanced.md#health-enhanced-factors)\.

You can also use immutable updates to deploy new versions of your application, as an alternative to rolling deployments\. When you [configure Elastic Beanstalk to use immutable updates for application deployments](using-features.rolling-version-deploy.md), it replaces all instances in your environment every time you deploy a new version of your application\. If an immutable application deployment fails, Elastic Beanstalk reverts the changes immediately by terminating the new Auto Scaling group\. This can prevent partial fleet deployments, which can occur when a rolling deployment fails after some batches have already completed\.

**Warning**  
Some policies replace all instances during the deployment or update\. This causes all accumulated [Amazon EC2 burst balances](https://docs.aws.amazon.com/AWSEC2/latest/DeveloperGuide/burstable-performance-instances.html) to be lost\. It happens in the following cases:  
Managed platform updates with instance replacement enabled
Immutable updates
Deployments with immutable updates or traffic splitting enabled

If an immutable update fails, the new instances upload [bundle logs](using-features.logging.md) to Amazon S3 before Elastic Beanstalk terminates them\. Elastic Beanstalk leaves logs from a failed immutable update in Amazon S3 for one hour before deleting them, instead of the standard 15 minutes for bundle and tail logs\.

**Note**  
If you use immutable updates for application version deployments, but not for configuration, you might encounter an error if you attempt to deploy an application version that contains configuration changes that would normally trigger a rolling update \(for example, configurations that change instance type\)\. To avoid this, make the configuration change in a separate update, or configure immutable updates for both deployments and configuration changes\.

You can't perform an immutable update in concert with resource configuration changes\. For example, you can't change [settings that require instance replacement](environments-updating.md) while also updating other settings, or perform an immutable deployment with configuration files that change configuration settings or additional resources in your source code\. If you attempt to change resource settings \(for example, load balancer settings\) and concurrently perform an immutable update, Elastic Beanstalk returns an error\.

If your resource configuration changes aren't dependent on your source code change or on instance configuration, perform them in two updates\. If they are dependent, perform a [blue/green deployment](using-features.CNAMESwap.md) instead\.

## Configuring immutable updates<a name="updates-immutable-configure"></a>

You can enable and configure immutable updates in the Elastic Beanstalk console\.

**To enable immutable updates \(console\)**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Configuration**\.

1. In the **Rolling updates and deployments** configuration category, choose **Edit**\.

1. In the **Configuration Updates** section, set **Rolling update type** to **Immutable**\.  
![\[The configuration updates section on the modify rolling updates and deployments configuration page\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environments-mgmt-updates-immutable.png)

1. Choose **Apply**\.

## The aws:autoscaling:updatepolicy:rollingupdate namespace<a name="updates-immutable-namespace"></a>

You can also use the options in the `aws:autoscaling:updatepolicy:rollingupdate` namespace to configure immutable updates\. The following example [configuration file](ebextensions.md) enables immutable updates for configuration changes\.

**Example \.ebextensions/immutable\-updates\.config**  

```
option_settings:
  aws:autoscaling:updatepolicy:rollingupdate:
    RollingUpdateType: Immutable
```

The following example enables immutable updates for both configuration changes and deployments\.

**Example \.ebextensions/immutable\-all\.config**  

```
option_settings:
  aws:autoscaling:updatepolicy:rollingupdate:
    RollingUpdateType: Immutable
  aws:elasticbeanstalk:command:
    DeploymentPolicy: Immutable
```

The EB CLI and Elastic Beanstalk console apply recommended values for the preceding options\. You must remove these settings if you want to use configuration files to configure the same\. See [Recommended values](command-options.md#configuration-options-recommendedvalues) for details\.