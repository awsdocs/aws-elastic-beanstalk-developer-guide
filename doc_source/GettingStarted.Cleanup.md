# Step 5: Clean up<a name="GettingStarted.Cleanup"></a>

Congratulations\! You have successfully deployed a sample application to the AWS Cloud, uploaded a new version, and modified its configuration to add a second Auto Scaling instance\. To ensure that you're not charged for any services you aren't using, delete all application versions and terminate the environment\. This also deletes the AWS resources that the environment created for you\.

**To delete the application and all associated resources**

1. Delete all application versions\.

   1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

   1. In the navigation pane, choose **Applications**, and then choose **getting\-started\-app**\.

   1. In the navigation pane, find your application's name and choose **Application versions**\.

   1. On the **Application versions** page, select all application versions that you want to delete\.

   1. Choose **Actions**, and then choose **Delete**\.

   1. Turn on **Delete versions from Amazon S3**\.

   1. Choose **Delete**, and then choose **Done**\.

1. Terminate the environment\.

   1. In the navigation pane, choose **getting\-started\-app**, and then choose **GettingStartedApp\-env** in the environment list\.

   1. Choose **Environment actions**, and then choose **Terminate Environment**\.

   1. Confirm that you want to terminate **GettingStartedApp\-env** by typing the environment name, and then choose **Terminate**\.

1. Delete the getting\-started\-app application\.

   1. In the navigation pane, choose the **getting\-started\-app**\.

   1. Choose **Actions**, and then choose **Delete application**\.

   1. Confirm that you want to delete **getting\-started\-app** by typing the application name, and then choose **Delete**\.