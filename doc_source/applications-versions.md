# Managing Application Versions<a name="applications-versions"></a>

Elastic Beanstalk creates an application version whenever you upload source code\. This usually occurs when you create an environment or upload and deploy code using the [environment management console](environments-console.md) or [EB CLI](eb-cli3.md)\. Elastic Beanstalk deletes these application versions according to the application's lifecycle policy and when you delete the application\. For details about application lifecycle policy, see [Configuring Application Version Lifecycle Settings](applications-lifecycle.md)\.

You can also upload a source bundle without deploying it from the [application management console](applications-console.md)\. Elastic Beanstalk stores source bundles in Amazon Simple Storage Service \(Amazon S3\) and doesn't automatically delete them\.

You can apply tags to an application version when you create it, and edit tags of existing application versions\. For details, see [Tagging Application Versions](applications-versions-tagging.md)\.

**To create a new application version**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Choose an application\.

1. In the navigation pane, choose **Application Versions**\.

1. Choose **Upload**\. Use the on\-screen form to upload your application's [source bundle](applications-sourcebundle.md)\.
**Note**  
The source bundle's file size limit is 512 MB\.

1. Optionally, provide a brief description, and add tag keys and values\.

1. Choose **Upload**\.

The file you specified is associated with your application\. You can deploy the application version to a new or existing environment\.

Over time, your application can accumulate many application versions\. To save storage space and avoid hitting the [application version quota](https://docs.aws.amazon.com/general/latest/gr/elasticbeanstalk.html#limits_elastic_beanstalk), you can configure Elastic Beanstalk to delete old versions automatically\.

**Note**  
Deleting an application version doesn't affect environments currently running that version\.

**To delete an application version**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Choose an application\.

1. In the navigation pane, choose **Application Versions**\.

1. Use the on\-screen form to select application versions\.

1. Choose **Delete**\.

1. Use the on\-screen dialog to confirm deletion of the application versions\.

   You can optionally do the following on the **Application versions** page\.
   + Configure a maximum number of versions to keep
   + Configure a maximum age for application versions
   + Choose to keep the source bundle in Amazon S3

If you configure application lifecycle settings, they're applied when you create new application versions\. For example, if you configure a maximum of 25 application versions, Elastic Beanstalk deletes the oldest version when you upload a 26th version\. If you set a maximum age of 90 days, any versions older than 90 days are deleted when you upload a new version\. For details, see [Configuring Application Version Lifecycle Settings](applications-lifecycle.md)\.

If you don't choose to delete the source bundle from Amazon S3, Elastic Beanstalk deletes the version from its records\. However, the source bundle is left in your [Elastic Beanstalk storage bucket](AWSHowTo.S3.md)\. The application version quota applies only to versions Elastic Beanstalk tracks\. Therefore, you can delete versions to stay within the quota, but retain all source bundles in Amazon S3\.

**Note**  
The application version quota doesn't apply to source bundles, but you might still incur Amazon S3 charges, and retain personal information beyond the time you need it\. Elastic Beanstalk never deletes source bundles automatically\. You should delete source bundles when you no longer need them\.