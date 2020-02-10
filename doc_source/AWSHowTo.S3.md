# Using Elastic Beanstalk with Amazon S3<a name="AWSHowTo.S3"></a>

Amazon Simple Storage Service \(Amazon S3\) provides highly durable, fault\-tolerant data storage\.

Elastic Beanstalk creates an Amazon S3 bucket named `elasticbeanstalk-region-account-id` for each region in which you create environments\. Elastic Beanstalk uses this bucket to store objects, for example temporary configuration files, that are required for the proper operation of your application\.

Elastic Beanstalk doesn't turn on default encryption for the Amazon S3 bucket that it creates\. This means that by default, objects are stored unencrypted in the bucket \(and are accessible only by authorized users\)\. Some applications require all objects to be encrypted when they are stored—on a hard drive, in a database, etc\. \(also known as *encryption at rest*\)\. If you have this requirement, you can configure your account's buckets for default encryption\. For more details, see [Amazon S3 Default Encryption for S3 Buckets](https://docs.aws.amazon.com/AmazonS3/latest/dev/bucket-encryption.html) in the *Amazon Simple Storage Service Developer Guide*\.

## Contents of the Elastic Beanstalk Amazon S3 bucket<a name="AWSHowTo.S3.content"></a>

The following table lists some objects that Elastic Beanstalk stores in your `elasticbeanstalk-*` Amazon S3 bucket\. The table also shows which objects have to be deleted manually\. To avoid unnecessary storage costs, and to ensure that personal information isn't retained, be sure to manually delete these objects when you no longer need them\.


|  **Object**  |  **When stored?**  |  **When deleted?**  | 
| --- | --- | --- | 
|  [**Application versions**](applications-versions.md)  |  When you create an environment or deploy your application code to an existing environment, Elastic Beanstalk stores an application version in Amazon S3 and associates it with the environment\.  |  During application deletion, and according to [Version lifecycle](applications-lifecycle.md)\.  | 
|  [**Source bundles**](applications-versions.md)  |  When you upload a new application version using the Elastic Beanstalk console or the EB CLI, Elastic Beanstalk stores a copy of it in Amazon S3, and sets it as your environment's source bundle\.  |  *Manually\.* When you delete an application version, you can choose **Delete versions from Amazon S3** to also delete the related source bundle\. For details, see [Managing application versions](applications-versions.md)\.  | 
|  [**Custom platforms**](custom-platforms.md)  |  When you create a custom platform, Elastic Beanstalk temporarily stores related data in Amazon S3\.  |  Upon successful completion of the custom platform's creation\.  | 
|  [**Log files**](using-features.logging.md)  |  You can request Elastic Beanstalk to retrieve instance log files \(tail or bundle logs\) and store them in Amazon S3\. You can also enable log rotation and configure your environment to publish logs automatically to Amazon S3 after they are rotated\.  |  Tail and bundle logs: 15 minutes after they are created\. Rotated logs: *Manually\.*  | 
|  [**Saved configurations**](environment-configuration-savedconfig.md)  |  *Manually\.*  |  *Manually\.*  | 

## Deleting objects in the Elastic Beanstalk Amazon S3 bucket<a name="AWSHowTo.S3.delete-objects"></a>

When you terminate an environment or delete an application, Elastic Beanstalk deletes most related objects from Amazon S3\. To minimize storage costs of a running application, routinely delete objects that your application doesn't need\. In addition, pay attention to objects that you have to delete manually, as listed in [Contents of the Elastic Beanstalk Amazon S3 bucket](#AWSHowTo.S3.content)\. To ensure that private information isn't unnecessarily retained, delete these objects when you don't need them anymore\.
+ Delete application versions that you don't expect to use in your application anymore\. When you delete an application version, you can select **Delete versions from Amazon S3** to also delete the related source bundle—a copy of your application's source code and configurations files, which Elastic Beanstalk uploaded to Amazon S3 when you deployed an application or uploaded an application version\. To learn how to delete an application version, see [Managing application versions](applications-versions.md)\.
+ Delete rotated logs that you don't need\. Alternatively, download them or move them to Amazon S3 Glacier for further analysis\.
+ Delete saved configurations that you aren't going to use in any environment anymore\.

## Deleting the Elastic Beanstalk Amazon S3 bucket<a name="AWSHowTo.S3.delete-bucket"></a>

Elastic Beanstalk applies a bucket policy to buckets it creates to allow environments to write to the bucket and prevent accidental deletion\. If you need to delete a bucket that Elastic Beanstalk created, first delete the bucket policy from the **Permissions** section of the bucket properties in the Amazon S3 console\.

**Warning**  
If you delete a bucket that Elastic Beanstalk created in your account, and you still have existing applications and running environments in the corresponding region, your applications might stop working correctly\. For example:  
When an environment scales out, Elastic Beanstalk should be able to find the environment's application version in the Amazon S3 bucket and use it to start new Amazon EC2 instances\.
When you create a custom platform, Elastic Beanstalk uses temporary Amazon S3 storage during the creation process\.
We recommend that you delete specific unnecessary objects from your Elastic Beanstalk Amazon S3 bucket, instead of deleting the entire bucket\.

**To delete an Elastic Beanstalk storage bucket \(console\)**

1. Open the [Amazon S3 console](https://console.aws.amazon.com/s3)\.

1. Open the Elastic Beanstalk storage bucket's page by choosing the bucket name\.  
![\[Opening a bucket's page on the Amazon S3 console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/services-s3-open-bucket.png)

1. Choose the **Permissions** tab\.

1. Choose **Bucket Policy**\.

1. Choose **Delete**\.  
![\[Bucket policy editor on the Amazon S3 console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/services-s3-bucket-policy.png)

1. Go back to the Amazon S3 console's main page, and then select the Elastic Beanstalk storage bucket by clicking its line anywhere outside of the bucket name\.  
![\[Selecting a bucket on the Amazon S3 console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/services-s3-select-bucket.png)

1. Choose **Delete Bucket**\.

1. Type the name of the bucket, and then choose **Confirm**\.