# Managed Platform Updates<a name="environment-platform-update-managed"></a>

Elastic Beanstalk regularly releases platform updates to provide fixes, software updates and new features\. With managed platform updates, you can configure your environment to automatically upgrade to the latest version of a platform during a scheduled maintenance window\. Your application remains in service during the update process with no reduction in capacity\. Managed updates are available on both single\-instance and load\-balanced environments\. 

**Note**  
This feature is not available for the \.NET on Windows Server platform\.

You can configure your environment to automatically apply patch version updates, or both patch and minor version updates\. Managed platform updates don't support major version updates, which may introduce changes that are backwards incompatible\.

When you enable managed platform updates, you can also configure AWS Elastic Beanstalk to replace all instances in your environment during the maintenance window, even if a platform update isn't available\. Replacing all instances in your environment is helpful if your application encounters bugs or memory issues when running for a long period\.

Use the Elastic Beanstalk environment management console to enable managed platform updates\.

**To enable managed platform updates**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the management page for your environment\.

1. Choose **Configuration**\.

1. On the **Managed Updates** card, choose **Modify**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environments-cfg-managedupdates.png)

1. Select **Enable managed updates**\.

1. Choose a maintenance window and then choose **Update level**\.

1. \(optional\) Select **Instance replacement** to enable weekly instance replacement\.

1. Choose **Save**, and then choose **Apply**\.

Managed platform updates depend on enhanced health reporting to determine that your application is healthy enough to consider the platform update successful\. See  for instructions\.


+ [Permissions Required to Perform Managed Platform Updates](#environment-platform-update-managed-perms)
+ [The Managed Update Maintenance Window](#environment-platform-update-managed-window)
+ [Minor and Patch Version Updates](#environment-platform-update-managed-versioning)
+ [Immutable Environment Updates](#environment-platform-update-managed-immutable)
+ [Managing Managed Updates](#environment-platform-update-managed-managing)
+ [Managed Action Option Namespaces](#environment-platform-update-managed-namespace)

## Permissions Required to Perform Managed Platform Updates<a name="environment-platform-update-managed-perms"></a>

Elastic Beanstalk needs permission to initiate a platform update on your behalf\. When you use the default service role for your environment, the console adds the required permissions when you enable managed platform updates\. If you aren't using the default service role, or you're managing your environments with a different client, add the `AWSElasticBeanstalkService` managed policy to your service role\.

If you're using a service\-linked role for your environment, you can't enable managed platform updates\. The service\-linked role doesn't have the required permissions\. Select a different role, and make sure it has the `AWSElasticBeanstalkService` managed policy\.

**Note**  
If you use configuration files to extend your environment to include additional resources, you might need to add additional permissions to your environment's service role\. Typically you need to add additional permissions when you reference these resources by name in other sections or files\.

If an update fails, you can find the reason for the failure on the Managed Updates page\.

## The Managed Update Maintenance Window<a name="environment-platform-update-managed-window"></a>

When AWS releases a new version of your environment's platform configuration, Elastic Beanstalk schedules a managed platform update during the next weekly maintenance window\. Maintenance windows are two hours long\. Elastic Beanstalk starts a scheduled update during the maintenance window, but the update might not complete until after the windows ends\.

## Minor and Patch Version Updates<a name="environment-platform-update-managed-versioning"></a>

You can enable managed platform updates to apply patch version updates only, or for both minor and patch version updates\. Patch version updates provide bug fixes and performance improvements, and can include minor configuration changes to the on\-instance software, scripts, and configuration options\. Minor version updates provide support for new Elastic Beanstalk features\. You can't apply major version updates, which might make changes that are backwards incompatible, with managed platform updates\.

In a platform version number, the second number is the minor update version, and the third number is the patch version\. For example, a version 2\.0\.7 platform version has a minor version of 0 and a patch version of 7\.

## Immutable Environment Updates<a name="environment-platform-update-managed-immutable"></a>

Managed platform updates perform immutable environment updates to upgrade your environment to a new platform version\. Immutable updates update your environment without taking any instances out of service or modifying your environment, prior to confirming that instances running the new configuration pass health checks\. 

In an immutable update, Elastic Beanstalk deploys as many instances as are currently running with the new platform version\. The new instances begin to take requests alongside those running the old version\. If the new set of instances passes all health checks, Elastic Beanstalk terminates the old set of instances, leaving only instances with the new configuration\.

Managed platform updates always perform immutable updates, even when you apply them outside of the maintenance window\. If you change the platform configuration from the **Dashboard**, Elastic Beanstalk applies the update policy that you've chosen for configuration updates\.

## Managing Managed Updates<a name="environment-platform-update-managed-managing"></a>

The Elastic Beanstalk environment management console shows detailed information about managed updates on the **Managed Updates** page\.

**To view information about managed updates \(console\)**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the management page for your environment\.

1. Choose **Managed Updates**\.

The **Managed Updates Overview** section provides information about scheduled and pending managed updates\. The **History** section lists successful updates and failed attempts\.

You can choose to apply a scheduled update immediately, instead of waiting until the maintenance window\. 

**To apply a managed platform update immediately \(console\)**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the management page for your environment\.

1. Choose **Managed Updates**\.

1. Choose **Apply now**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environment-management-managedupdateoverview.png)

1. Choose **Apply**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environment-management-applymanagedaction.png)

When you apply a managed platform update outside of the maintenance window, Elastic Beanstalk performs an immutable update\. If you update the environment's platform from the Dashboard, or by using a different client, Elastic Beanstalk uses the update type that you have selected for configuration changes\.

If you don't have a managed update scheduled, your environment may already be running the latest version\. Other reasons for not having an update scheduled include:

+ a minor version update is available, but your environment is configured to automatically apply only patch version updates\.

+ your environment hasn't been scanned since the update was released\. Elastic Beanstalk typically checks for updates every hour\.

+ an update is pending or already in progress\.

When your maintenance window starts or when you choose **Apply now**, scheduled updates goes into pending status prior to execution\. 

## Managed Action Option Namespaces<a name="environment-platform-update-managed-namespace"></a>

You can use configuration options in the `aws:elasticbeanstalk:managedactions` and `aws:elasticbeanstalk:managedactions:platformupdate` namespaces to enable and configure managed platform updates\.

The `ManagedActionsEnabled` option turns on managed platform updates\. Set this option to `true` to enable managed platform updates, and use the other options to configure update behavior\.

Use `PreferredStartTime` to configure the beginning of the weekly maintenance window in *day*:*hour*:*minute* format\.

Set `UpdateLevel` to `minor` or `patch` to apply both minor and patch version updates, or just patch version updates, respectively\.

When managed platform updates are enabled, you can enable instance replacement by setting the `InstanceRefreshEnabled` option to `true`\. When this setting is enabled, Elastic Beanstalk runs an immutable update on your environment every week, regardless of whether there is a new platform version available\.

The following example configuration file enables managed platform updates for patch version updates with a maintenance window starting at 10:00 AM UTC each Tuesday:

**Example \.ebextensions/managed\-platform\-update\.config**  

```
option_settings:
  aws:elasticbeanstalk:managedactions:
    ManagedActionsEnabled: true
    PreferredStartTime: "Tue:10:00"
  aws:elasticbeanstalk:managedactions:platformupdate:
    UpdateLevel: patch
    InstanceRefreshEnabled: true
```