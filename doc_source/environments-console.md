# Using the Elastic Beanstalk environment management console<a name="environments-console"></a>

 The Elastic Beanstalk console provides a management page for each of your AWS Elastic Beanstalk environments\. From this page, you can manage your environment's configuration and perform common actions including restarting the web servers running in your environment, cloning the environment, or rebuilding it from scratch\.

![\[Elastic Beanstalk environment management console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/gettingstarted-dashboard.png)

**To access the environment management console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

You see the environment overview page\. The console's navigation pane shows the name of the application to which the environment belongs, with related application management pages, and the environment name, with environment management pages\.

**Topics**
+ [Environment overview](#environments-dashboard)
+ [Environment actions](#environments-dashboard-actions)
+ [Configuration](#environments-console-configuration)
+ [Logs](#environments-console-logs)
+ [Health](#environments-console-health)
+ [Monitoring](#environments-console-monitoring)
+ [Alarms](#environments-console-alarms)
+ [Managed updates](#environments-console-managedupdates)
+ [Events](#environments-console-events)
+ [Tags](#environments-console-tags)

## Environment overview<a name="environments-dashboard"></a>

To view the environment overview page, choose the environment name on the navigation pane, if it's the current environment\. Alternatively, navigate to the environment from the application page or from the main environment list on the **Environments** page\.

The top pane on the environment overview page shows top level information about your environment\. This includes its name, its URL, its current health status, the name of the currently deployed application version, and the platform version that the application is running on\. Below the overview pane you can see the five most recent environment events\.

Choose **Refresh** to update the information shown\. The overview page contains the following information and options\.

### URL<a name="environments-dashboard-url"></a>

The environment's **URL** is located at the top of the overview, below the environment name\. This is the URL of the web application that the environment is running\.

### Health<a name="environments-dashboard-health"></a>

The overall health of the environment\. With [Enhanced health reporting and monitoring](health-enhanced.md) enabled, the environment status is shown with a **Causes** button you can choose to view more information about the current status\.

For [Basic health reporting](using-features.healthstatus.md) environments, a link to the [Monitoring Console](environment-health-console.md) is shown\.

### Running version<a name="environments-dashboard-version"></a>

The name of the application version that is deployed and running on your environment\. Choose **Upload and deploy** to upload a [source bundle](applications-sourcebundle.md) and deploy it to your environment\. This option creates a new application version\.

### Platform<a name="environments-dashboard-config"></a>

Shows the name of the platform version running on your environment—typically a combination of the architecture, operating system \(OS\), language, and application server \(collectively, the *platform branch*\), with a specific platform version number\. Choose **Change** to select a different platform version\. This option is available only if another version of the platform branch is available\.

Updating the platform version using this option replaces instances running in your environment with new instances\.

![\[Update platform version dialog\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environment-management-update-platform-version.png)

**Note**  
When you first use Elastic Beanstalk, only the latest \(recommended\) version of each platform branch is available for use\. **Change** first becomes available when a new platform version is released for the branch\. After upgrading, you have the option to change back to the previous version\.

### Recent events<a name="environments-dashboard-events"></a>

The **Recent Events** section of the environment overview page shows the most recent events emitted by your environment\. This list is updated in real time when your environment is being updated\.

Choose **Show all** to open the **Events** page\.

## Environment actions<a name="environments-dashboard-actions"></a>

The environment overview page contains an **Environment actions** menu that you can use to perform common operations on your environment\. This menu is shown on the right side of the environment header under the **Create New Environment** option\.

**Note**  
Some actions are only available under certain conditions, and will be disabled unless these conditions are met\.

### Load configuration<a name="environments-dashboard-actions-load"></a>

Load a previously saved configuration\. Configurations are saved to your application and can be loaded by any associated environment\. If you've made changes to your environment's configuration, you can load a saved configuration to undo those changes\. You can also load a configuration that you saved from a different environment running the same application to propagate configuration changes between them\. 

### Save configuration<a name="environments-dashboard-actions-save"></a>

Save the current configuration of your environment to your application\. Before you make changes to your environment's configuration, save the current configuration so that you can roll back later, if needed\. You can also apply a saved configuration when you launch a new environment\. 

### Swap environment URLs<a name="environments-dashboard-actions-swap"></a>

Swap the CNAME of the current environment with a new environment\. After a CNAME swap, all traffic to the application using the environment URL goes to the new environment\. When you are ready to deploy a new version of your application, you can launch a separate environment under the new version\. When the new environment is ready to start taking requests, perform a CNAME swap to start routing traffic to the new environment with no interruption of service\. For more information, see [Blue/Green deployments with Elastic Beanstalk](using-features.CNAMESwap.md)\. 

### Clone environment<a name="environments-dashboard-actions-clone"></a>

Launch a new environment with the same configuration as your currently running environment\. 

### Clone with latest platform<a name="environments-dashboard-actions-cloneupgrade"></a>

Clone your current environment with the latest version of the in\-use Elastic Beanstalk platform\. This option is available only when a newer version of the current environment's platform is available for use\. 

### Abort current operation<a name="environments-dashboard-actions-abort"></a>

Stop an in\-progress environment update\. Aborting an operation can cause some of the instances in your environment to be in a different state than others, depending on how far the operation progressed\. This option is available only when your environment is being updated\. 

### Restart app servers<a name="environments-dashboard-actions-restart"></a>

Restart the web server running on your environment's instances\. This option does not terminate or restart any AWS resources\. If your environment is acting strangely in response to some bad requests, restarting the application server can restore functionality temporarily while you troubleshoot the root cause\. 

### Rebuild environment<a name="environments-dashboard-actions-rebuild"></a>

Terminate all resources in the running environment and build a new environment with the same settings\. This operation takes several minutes, equivalent to deploying a new environment from scratch\. Any Amazon RDS instances running in your environment's data tier are deleted during a rebuild\. If you need the data, create a snapshot\. You can create a snapshot manually [in the RDS console](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_CreateSnapshot.html) or configure your data tier's Deletion Policy to create a snapshot automatically before deleting the instance \(this is the default setting when you create a data tier\)\. 

### Terminate environment<a name="environments-dashboard-actions-terminate"></a>

Terminate all resources in the running environment, and remove the environment from the application\. If you have an RDS instance running in a data tier and need to retain the data, be sure to take a snapshot before terminating your environment\. You can create a snapshot manually [in the RDS console](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_CreateSnapshot.html) or configure your data tier's Deletion Policy to create a snapshot automatically before deleting the instance \(this is the default setting when you create a data tier\)\. 

### Restore environment<a name="environments-dashboard-actions-restore"></a>

If the environment has been terminated in the last hour, you can restore it from this page\. After an hour, you can [restore it from the application overview page](environment-management-rebuild.md#environment-management-rebuild-terminated)\.

## Configuration<a name="environments-console-configuration"></a>

The **Configuration overview** page shows the current configuration of your environment and its resources, including Amazon EC2 instances, load balancer, notifications, and health monitoring settings\. Use the settings on this page to customize the behavior of your environment during deployments, enable additional features, and modify the instance type and other settings that you chose during environment creation\.

![\[The configuration overview page of the Elastic Beanstalk console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environments-cfg-console.overview.png)

For more information, see [Configuring Elastic Beanstalk environments](customize-containers.md)\.

## Logs<a name="environments-console-logs"></a>

The **Logs** page lets you retrieve logs from the EC2 instances in your environment\. When you request logs, Elastic Beanstalk sends a command to the instances, which then upload logs to your Elastic Beanstalk storage bucket in Amazon S3\. When you request logs on this page, Elastic Beanstalk automatically deletes them from Amazon S3 after 15 minutes\.

You can also configure your environment's instances to upload logs to Amazon S3 for permanent storage after they have been rotated locally\.

![\[The environment logs page of the Elastic Beanstalk console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environment-management-logs.png)

For more information, see [Viewing logs from Amazon EC2 instances in your Elastic Beanstalk environment](using-features.logging.md)\.

## Health<a name="environments-console-health"></a>

If enhanced health monitoring is enabled, the **Enhanced health overview** page shows live health information about every instance in your environment\. Enhanced health monitoring enables Elastic Beanstalk to closely monitor the resources in your environment so that it can assess the health of your application more accurately\.

When enhanced health monitoring is enabled, this page shows information about the requests served by the instances in your environment and metrics from the operating system, including latency, load, and CPU utilization\.

![\[The enhanced health overview page of the Elastic Beanstalk console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/enhanced-health-instances.png)

For more information, see [Enhanced health reporting and monitoring](health-enhanced.md)\.

## Monitoring<a name="environments-console-monitoring"></a>

The **Monitoring** page shows an overview of health information for your environment\. This includes the default set of metrics provided by Elastic Load Balancing and Amazon EC2, and graphs that show how the environment's health has changed over time\. You can use the options on this page to configure additional graphs for resource\-specific metrics, and add alarms for any metric supported by the in\-use health reporting system\.

![\[The monitoring page of the Elastic Beanstalk console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/enhanced-health-monitoring.png)

For more information, see [Monitoring environment health in the AWS management console](environment-health-console.md)\.

## Alarms<a name="environments-console-alarms"></a>

The **Existing alarms** page shows information about any alarms that you have configured for your environment\. You can use the options on this page to modify or delete alarms\.

![\[The existing alarms page of the Elastic Beanstalk console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-env-alarms.png)

For more information, see [Manage alarms](using-features.alarms.md)\.

## Managed updates<a name="environments-console-managedupdates"></a>

The **Managed updates overview** page shows information about upcoming and completed managed platform updates and instance replacement\. These features let you configure your environment to update to the latest platform version automatically during a weekly maintenance window that you choose\.

In between platform releases, you can choose to have your environment replace all of its Amazon EC2 instances during the maintenance window\. This can help alleviate issues that occur when your application runs for extended periods of time\.

For more information, see [Managed platform updates](environment-platform-update-managed.md)\.

## Events<a name="environments-console-events"></a>

The **Events** page shows the event stream for your environment\. Elastic Beanstalk outputs event messages whenever you interact with the environment, and when any of your environment's resources are created or modified as a result\.

![\[The events page of the Elastic Beanstalk console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environment-events.png)

For more information, see [Viewing an Elastic Beanstalk environment's event stream](using-features.events.md)\.

## Tags<a name="environments-console-tags"></a>

The **Tags** page shows the tags that Elastic Beanstalk applied to the environment when you created it, and any tags that you added\. You can add, edit, and delete custom tags\. You can't edit or delete the tags that Elastic Beanstalk applied\.

Environment tags are applied to every resource that Elastic Beanstalk creates to support your application\.

![\[The tags page of the Elastic Beanstalk console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environments-management-tags.png)

For more information, see [Tagging resources in your Elastic Beanstalk environments](using-features.tagging.md)\.