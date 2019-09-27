# Step 5: Clean Up<a name="GettingStarted.Cleanup"></a>

Congratulations\! You have successfully deployed a sample application to the AWS Cloud, uploaded a new version, and modified its configuration to add a second Auto Scaling instance\. To ensure that you're not charged for any services you aren't using, delete all application versions and terminate the environment\. This also deletes the AWS resources that the environment created for you\.

**To delete the application and all associated resources**

1. Delete all application versions\.

   1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

   1. On the Elastic Beanstalk applications page, choose **getting\-started\-app**\.

   1. In the navigation pane, choose **Application versions**\.

   1. On the **Application Versions** page, select all application versions that you want to delete, and then choose **Delete**\.

   1. Confirm that you want to delete those versions by choosing **Delete**\.

   1. Choose **Done**\.

1. Terminate the environment\.

   1. Open the environment dashboard by choosing **getting\-started\-app**, and then choosing **GettingStartedApp\-env**\.

   1. Choose **Actions**, and then choose **Terminate Environment**\.

   1. Confirm that you want to terminate **GettingStartedApp\-env** by typing the environment name, and then choose **Terminate**\.

1. Delete the getting\-started\-app application\.

   1. Open the main Elastic Beanstalk dashboard by choosing **Elastic Beanstalk** in the upper left of the environment dashboard\.

   1. On the Elastic Beanstalk applications page, choose **Actions** for the **getting\-started\-app** application, and then choose **Delete application**\.

   1. Confirm that you want to delete **getting\-started\-app** by typing the application name, and then choose **Delete**\.