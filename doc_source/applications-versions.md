# Managing Application Versions<a name="applications-versions"></a>

Elastic Beanstalk creates an application version whenever you upload source code\. This usually occurs when you create an environment or upload and deploy code using the [environment management console](environments-console.md) or [EB CLI](eb-cli3.md)\. Elastic Beanstalk deletes these application versions according to the application's lifecycle policy and when you delete the application\. For details about application lifecycle policy, see [Configuring Application Version Lifecycle Settings](applications-lifecycle.md)\.

You can also upload a source bundle without deploying it from the [application management console](applications-console.md)\. Elastic Beanstalk stores source bundles in Amazon Simple Storage Service \(Amazon S3\) and doesn't automatically delete them\.

You can apply tags to an application version when you create it, and edit tags of existing application versions\. For details, see [Tagging Application Versions](applications-versions-tagging.md)\.

**To create a new application version**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Choose an application\.

1. In the navigation pane, choose **Application Versions**\.

1. Choose **Upload**\.  
![\[Uploading an application version on the Application Versions page of the Elastic Beanstalk console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/applications-version-upload.png)

1. Enter a **Version label** for this version\.

1. Choose **Browse** to specify the location of the [source bundle](applications-sourcebundle.md)\.
**Note**  
The source bundle's file size limit is 512 MB\.

1. Optionally, provide a brief description, and add tag keys and values\.

1. Choose **Upload**\.  
![\[Upload Application Version dialog box on the Application Versions page of the Elastic Beanstalk console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/applications-version-upload-dialog.png)

The file you specified is associated with your application\. You can deploy the application version to a new or existing environment\.

Over time, your application can accumulate many application versions\. To save storage space and avoid hitting the [application version limit](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_elastic_beanstalk), you can configure Elastic Beanstalk to delete old versions automatically\.

**Note**  
Deleting an application version doesn't affect environments currently running that version\.

**To delete an application version**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Choose an application\.

1. In the navigation pane, choose **Application Versions**\.

1. In the list of application versions, select the box next to the application version that you want to delete, and then choose **Delete**\.  
![\[Deleting an application version on the Application Versions page of the Elastic Beanstalk console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/applications-version-delete.png)

1. \(Optional\) To leave the application source bundle for this application version in your Amazon Simple Storage Service \(Amazon S3\) bucket, clear the box for **Delete versions from Amazon S3**\.  
![\[Delete Application Versions dialog box in the Elastic Beanstalk console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/applications-version-delete-s3.png)

1. Choose **Apply**\.

If you configure application lifecycle settings, they're applied when you create new application versions\. For example, if you configure a maximum of 25 application versions, Elastic Beanstalk deletes the oldest version when you upload a 26th version\. If you set a maximum age of 90 days, any versions older than 90 days are deleted when you upload a new version\. For details, see [Configuring Application Version Lifecycle Settings](applications-lifecycle.md)\.

If you don't choose to delete the source bundle from Amazon S3, Elastic Beanstalk deletes the version from its records\. However, the source bundle is left in your [Elastic Beanstalk storage bucket](AWSHowTo.S3.md)\. The application version limit applies only to versions Elastic Beanstalk tracks\. Therefore, you can delete versions to stay within the limit, but retain all source bundles in Amazon S3\.

**Note**  
The application version limit doesn't apply to source bundles, but you might still incur Amazon S3 charges, and retain personal information beyond the time you need it\. Elastic Beanstalk never deletes source bundles automatically\. You should delete source bundles when you no longer need them\.