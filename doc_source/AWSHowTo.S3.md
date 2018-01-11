# Using Elastic Beanstalk with Amazon Simple Storage Service<a name="AWSHowTo.S3"></a>

Amazon Simple Storage Service \(Amazon S3\) provides highly durable, fault\-tolerant data storage\. Behind the scenes, Amazon S3 stores objects redundantly on multiple devices across multiple facilities in a region\.

Elastic Beanstalk creates an Amazon S3 bucket named elasticbeanstalk\-*region*\-*account\-id* for each region in which you create environments\. Elastic Beanstalk uses this bucket to store application versions, logs, and other supporting files\.

Elastic Beanstalk applies a bucket policy to buckets it creates to allow environments to write to the bucket and prevent accidental deletion\. If you need to delete a bucket that Elastic Beanstalk created, first delete the bucket policy from the **Permissions** section of the bucket properties in the Amazon S3 Management Console\.

**To delete an Elastic Beanstalk storage bucket \(console\)**

1. Open the [Amazon S3 Management Console](https://console.aws.amazon.com/s3)

1. Select the Elastic Beanstalk storage bucket\.

1. Choose **Properties**\.

1. Choose **Permissions**\.

1. Choose **Edit Bucket Policy**\.

1. Choose **Delete**\.

1. Choose **OK**\.

1. Choose **Actions** and then choose **Delete Bucket**\.

1. Type the name of the bucket and then choose **Delete**\.