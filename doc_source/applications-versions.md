# Managing Application Versions<a name="applications-versions"></a>

Elastic Beanstalk creates an application version whenever you upload source code\. This usually occurs when you create a new environment or upload and deploy code using the environment management console or EB CLI\. You can also upload a source bundle without deploying it from the application management console\.

**To create a new application version**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Choose an application\.

1. In the navigation pane, choose **Application Versions**\.

1. Choose **Upload**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/applications-version-upload.png)

1. Enter a **Version label** for this version\.

1. \(Optional\) Enter a brief **Description** for this version\.

1. Choose **Browse** to specify the location of the source bundle\.
**Note**  
The file size limit is 512 MB\.

1. Choose **Upload**\.

The file you specified is associated with your application\. You can deploy the application version to a new or existing environment\.

Over time, your application can accumulate a large number of application versions\. To save storage space and avoid hitting the [application version limit](http://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_elastic_beanstalk), you can configure Elastic Beanstalk to delete old versions automatically\.

**Note**  
Deleting an application version doesn't have any effect on environments currently running that version\.

**To delete an application version**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Choose an application\.

1. In the navigation pane, choose **Application versions**\.

1. In the list of application versions, select the check box next to the application version that you want to delete, and then click **Delete**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/applications-version-delete.png)

1. \(Optional\) To leave the application source bundle for this application version in your Amazon S3 bucket, uncheck **Delete versions from Amazon S3**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/applications-version-delete-s3.png)

1. Choose **Apply**\.

If you configure application lifecycle settings, they are applied when you create new application versions\. For example, if you configure a maximum of 25 application versions, Elastic Beanstalk deletes the oldest version when you upload a 26th version\. If you set a maximum age of 90 days, any versions more than 90 days old are deleted when you upload a new version\. For details, see [Configuring Application Version Lifecycle Settings](applications-lifecycle.md)\.

If you don't choose to delete the source bundle from Amazon S3, Elastic Beanstalk deletes the version from its records\. However, the source bundle is left in your Elastic Beanstalk storage bucket\. The application version limit applies only to versions Elastic Beanstalk tracks\. Therefore, if you need you can delete versions to stay within the limit, but retain all source bundles in Amazon S3\.