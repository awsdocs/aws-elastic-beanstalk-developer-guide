# Canceling environment configuration updates and application deployments<a name="using-features.rollingupdates.cancel"></a>

You can cancel in\-progress updates that are triggered by environment configuration changes\. You can also cancel the deployment of a new application version in progress\. For example, you might want to cancel an update if you decide you want to continue using the existing environment configuration instead of applying new environment configuration settings\. Or, you might realize that the new application version that you are deploying has problems that will cause it to not start or not run properly\. By canceling an environment update or application version update, you can avoid waiting until the update or deployment process is done before you begin a new attempt to update the environment or application version\.

**Note**  
During the cleanup phase in which old resources that are no longer needed are removed, after the last batch of instances has been updated, you can no longer cancel the update\.

Elastic Beanstalk performs the rollback the same way that it performed the last successful update\. For example, if you have time\-based rolling updates enabled in your environment, then Elastic Beanstalk will wait the specified pause time between rolling back changes on one batch of instances before rolling back changes on the next batch\. Or, if you recently turned on rolling updates, but the last time you successfully updated your environment configuration settings was without rolling updates, Elastic Beanstalk will perform the rollback on all instances simultaneously\.

You cannot stop Elastic Beanstalk from rolling back to the previous environment configuration once it begins to cancel the update\. The rollback process continues until all instances in the environment have the previous environment configuration or until the rollback process fails\. For application version deployments, canceling the deployment simply stops the deployment; some instances will have the new application version and others will continue to run the existing application version\. You can deploy the same or another application version later\.

For more information about rolling updates, see [Elastic Beanstalk rolling environment configuration updates](using-features.rollingupdates.md)\. For more information about batched application version deployments, see [Deployment policies and settings](using-features.rolling-version-deploy.md)\.

**To cancel an update**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. On the environment overview page, choose **Environment actions**, and then choose **Abort current operation**\.