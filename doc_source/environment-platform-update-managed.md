# Managed Platform Updates<a name="environment-platform-update-managed"></a>

AWS Elastic Beanstalk regularly releases [platform updates](using-features.platform.upgrade.md) to provide fixes, software updates, and new features\. With managed platform updates, you can configure your environment to automatically upgrade to the latest version of a platform during a scheduled [maintenance window](#environment-platform-update-managed-window)\. Your application remains in service during the update process with no reduction in capacity\. Managed updates are available on both single\-instance and load\-balanced environments\. 

**Note**  
This feature isn't available on [Windows Server platform versions](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.net) earlier than version 2 \(v2\)\.

You can configure your environment to automatically apply [patch version updates](#environment-platform-update-managed-versioning), or both patch and minor version updates\. Managed platform updates don't support updates across platform branches \(updates to different major versions of platform components such as operating system, runtime, or Elastic Beanstalk components\), because these can introduce changes that are backward incompatible\.

You can also configure Elastic Beanstalk to replace all instances in your environment during the maintenance window, even if a platform update isn't available\. Replacing all instances in your environment is helpful if your application encounters bugs or memory issues when running for a long period\.

On environments created on November 25, 2019 or later using the Elastic Beanstalk console, managed updates are enabled by default whenever possible\. Managed updates require [enhanced health](health-enhanced.md) to be enabled\. Enhanced health is enabled by default when you select one of the [configuration presets](environments-create-wizard.md#environments-create-wizard-presets), and disabled when you select **Custom configuration**\. The console can't enable managed updates for older platform versions that don't support enhanced health, or when enhanced health is disabled\. When the console enables managed updates for a new environment, the **Weekly update window** is set to a random day of the week at a random time\. **Update level** is set to **Minor and patch**, and **Instance replacement** is disabled\. You can disable or reconfigure managed updates before the final environment creation step\.

For an existing environment, use the Elastic Beanstalk console anytime to configure managed platform updates\.

**To configure managed platform updates**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. Choose **Configuration**\.

1. In the **Managed Updates** category, choose **Modify**\.  
![\[Managed updates configuration category on the Elastic Beanstalk console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environments-cfg-managedupdates.png)

1. Disable or enable **Managed updates**\.

1. If managed updates are enabled, select a maintenance window, and then select an **Update level**\.

1. \(Optional\) Select **Instance replacement** to enable weekly instance replacement\.

1. Choose **Apply**\.

Managed platform updates depend on [enhanced health reporting](health-enhanced.md) to determine that your application is healthy enough to consider the platform update successful\. See [Enabling AWS Elastic Beanstalk Enhanced Health Reporting](health-enhanced-enable.md) for instructions\.

**Topics**
+ [Permissions Required to Perform Managed Platform Updates](#environment-platform-update-managed-perms)
+ [Managed Update Maintenance Window](#environment-platform-update-managed-window)
+ [Minor and Patch Version Updates](#environment-platform-update-managed-versioning)
+ [Immutable Environment Updates](#environment-platform-update-managed-immutable)
+ [Managing Managed Updates](#environment-platform-update-managed-managing)
+ [Managed Action Option Namespaces](#environment-platform-update-managed-namespace)

## Permissions Required to Perform Managed Platform Updates<a name="environment-platform-update-managed-perms"></a>

Elastic Beanstalk needs permission to initiate a platform update on your behalf\. When you use the default [service role](concepts-roles-service.md) for your environment, the console adds the required permissions when you enable managed platform updates\. If you aren't using the default service role, or you're managing your environments with a different client, add the [`AWSElasticBeanstalkService`](iam-servicerole.md#iam-servicerole-update) managed policy to your service role\.

If you're using your account's [monitoring service\-linked role](using-service-linked-roles-monitoring.md) for your environment, you can't enable managed platform updates\. The service\-linked role doesn't have the required permissions\. Select a different role, and be sure it has the [`AWSElasticBeanstalkService`](iam-servicerole.md#iam-servicerole-update) managed policy\.

**Note**  
If you use [configuration files](ebextensions.md) to extend your environment to include additional resources, you might need to add permissions to your environment's service role\. Typically you need to add permissions when you reference these resources by name in other sections or files\.

If an update fails, you can find the reason for the failure on the [Managed Updates](#environment-platform-update-managed-managing) page\.

## Managed Update Maintenance Window<a name="environment-platform-update-managed-window"></a>

When AWS releases a new version of your environment's platform, Elastic Beanstalk schedules a managed platform update during the next weekly maintenance window\. Maintenance windows are two hours long\. Elastic Beanstalk starts a scheduled update during the maintenance window\. The update might not complete until after the window ends\.

**Note**  
In most cases, Elastic Beanstalk schedules your managed update to occur during your coming weekly maintenance window\. The system considers various aspects of update safety and service availability when scheduling managed updates\. In rare cases, an update might not be scheduled for the first coming maintenance window\. If this happens, the system tries again during the next maintenance window\. To manually apply the managed update, choose **Apply now** as explained in [Managing Managed Updates](#environment-platform-update-managed-managing) on this page\.

## Minor and Patch Version Updates<a name="environment-platform-update-managed-versioning"></a>

You can enable managed platform updates to apply patch version updates only, or for both minor and patch version updates\. Patch version updates provide bug fixes and performance improvements, and can include minor configuration changes to the on\-instance software, scripts, and configuration options\. Minor version updates provide support for new Elastic Beanstalk features\. You can't apply major version updates, which might make changes that are backward incompatible, with managed platform updates\.

In a platform version number, the second number is the minor update version, and the third number is the patch version\. For example, a version 2\.0\.7 platform version has a minor version of 0 and a patch version of 7\.

## Immutable Environment Updates<a name="environment-platform-update-managed-immutable"></a>

Managed platform updates perform [immutable environment updates](environmentmgmt-updates-immutable.md) to upgrade your environment to a new platform version\. Immutable updates update your environment without taking any instances out of service or modifying your environment, before confirming that instances running the new version pass health checks\. 

In an immutable update, Elastic Beanstalk deploys as many instances as are currently running with the new platform version\. The new instances begin to take requests alongside those running the old version\. If the new set of instances passes all health checks, Elastic Beanstalk terminates the old set of instances, leaving only instances with the new version\.

Managed platform updates always perform immutable updates, even when you apply them outside of the maintenance window\. If you change the platform version from the **Dashboard**, Elastic Beanstalk applies the update policy that you've chosen for configuration updates\.

**Warning**  
During managed platform updates with instance replacement enabled, immutable updates, and deployments with immutable updates enabled all instances are replaced\. This causes all accumulated [Amazon EC2 Burst Balances](https://docs.aws.amazon.com/AWSEC2/latest/DeveloperGuide/burstable-performance-instances.html) to be lost\.

## Managing Managed Updates<a name="environment-platform-update-managed-managing"></a>

The Elastic Beanstalk console shows detailed information about managed updates on the **Managed Updates** page\.

**To view information about managed updates \(console\)**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. Choose **Managed Updates**\.

The **Managed Updates Overview** section provides information about scheduled and pending managed updates\. The **History** section lists successful updates and failed attempts\.

You can choose to apply a scheduled update immediately, instead of waiting until the maintenance window\. 

**To apply a managed platform update immediately \(console\)**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. Choose **Managed Updates**\.

1. Choose **Apply now**\.  
![\[The managed updates page of the Elastic Beanstalk console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environment-management-managedupdateoverview.png)

1. Choose **Apply**\.  
![\[The Apply Managed Update page of the Elastic Beanstalk console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environment-management-applymanagedaction.png)

When you apply a managed platform update outside of the maintenance window, Elastic Beanstalk performs an immutable update\. If you update the environment's platform from the [Dashboard](environments-console.md#environments-dashboard), or by using a different client, Elastic Beanstalk uses the update type that you selected for [configuration changes](environments-updating.md)\.

If you don't have a managed update scheduled, your environment might already be running the latest version\. Other reasons for not having an update scheduled include:
+ A [minor version](#environment-platform-update-managed-versioning) update is available, but your environment is configured to automatically apply only patch version updates\.
+ Your environment hasn't been scanned since the update was released\. Elastic Beanstalk typically checks for updates every hour\.
+ An update is pending or already in progress\.

When your maintenance window starts or when you choose **Apply now**, scheduled updates go into pending status before execution\. 

## Managed Action Option Namespaces<a name="environment-platform-update-managed-namespace"></a>

You can use [configuration options](command-options.md) in the `[aws:elasticbeanstalk:managedactions](command-options-general.md#command-options-general-elasticbeanstalkmanagedactions)` and `[aws:elasticbeanstalk:managedactions:platformupdate](command-options-general.md#command-options-general-elasticbeanstalkmanagedactionsplatformupdate)` namespaces to enable and configure managed platform updates\.

The `ManagedActionsEnabled` option turns on managed platform updates\. Set this option to `true` to enable managed platform updates, and use the other options to configure update behavior\.

Use `PreferredStartTime` to configure the beginning of the weekly maintenance window in *day*:*hour*:*minute* format\.

Set `UpdateLevel` to `minor` or `patch` to apply both minor and patch version updates, or just patch version updates, respectively\.

When managed platform updates are enabled, you can enable instance replacement by setting the `InstanceRefreshEnabled` option to `true`\. When this setting is enabled, Elastic Beanstalk runs an immutable update on your environment every week, regardless of whether there is a new platform version available\.

The following example [configuration file](ebextensions.md) enables managed platform updates for patch version updates with a maintenance window starting at 9:00 AM UTC each Tuesday\.

**Example \.ebextensions/managed\-platform\-update\.config**  

```
option_settings:
  aws:elasticbeanstalk:managedactions:
    ManagedActionsEnabled: true
    PreferredStartTime: "Tue:09:00"
  aws:elasticbeanstalk:managedactions:platformupdate:
    UpdateLevel: patch
    InstanceRefreshEnabled: true
```