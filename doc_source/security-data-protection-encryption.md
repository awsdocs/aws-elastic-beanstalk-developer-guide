# Protecting data using encryption<a name="security-data-protection-encryption"></a>

Elastic Beanstalk stores various objects in an Amazon Simple Storage Service \(Amazon S3\) bucket that it creates for each AWS Region in which you create environments\. For details, see [Using Elastic Beanstalk with Amazon S3](AWSHowTo.S3.md)\. 

You provide some of the stored objects and send them to Elastic Beanstalk, for example, application versions and source bundles\. Elastic Beanstalk generates other objects, for example, log files\. In addition to the data that Elastic Beanstalk stores, your application can transfer and/or store data as part of its operation\.

Data protection refers to protecting data while *in transit* \(as it travels to and from Elastic Beanstalk\) and *at rest* \(while it is stored in AWS data centers\)\.

## Encryption in transit<a name="security-data-protection-encryption.in-transit"></a>

You can achieve data protection in transit in two ways: encrypt the connection using Secure Sockets Layer \(SSL\), or use client\-side encryption \(where the object is encrypted before it is sent\)\. Both methods are valid for protecting your application data\. To secure the connection, encrypt it using SSL whenever your application, its developers and administrators, and its end users send or receive any objects\. For details about encrypting web traffic to and from your application, see [Configuring HTTPS for your Elastic Beanstalk environment](configuring-https.md)\.

Client\-side encryption isn't a valid method for protecting your source code in application versions and source bundles that you upload\. Elastic Beanstalk needs access to these objects, so they can't be encrypted\. Therefore, be sure to secure the connection between your development or deployment environment and Elastic Beanstalk\.

## Encryption at rest<a name="security-data-protection-encryption.at-rest"></a>

To protect your application's data at rest, learn about data protection in the storage service that your application uses\. For example, see [Data Protection in Amazon RDS](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/DataDurability.html) in the *Amazon RDS User Guide*, [Data Protection in Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/dev/DataDurability.html) in the *Amazon Simple Storage Service Developer Guide*, or [Encrypting Data and Metadata in EFS](https://docs.aws.amazon.com/efs/latest/ug/encryption.html) in the *Amazon Elastic File System User Guide*\.

Elastic Beanstalk doesn't turn on default encryption for the Amazon S3 bucket that it creates\. This means that by default, objects are stored unencrypted in the bucket \(and are accessible only by users authorized to read the bucket\)\. If your application requires encryption at rest, you can configure your account's buckets for default encryption\. For more information, see [Amazon S3 Default Encryption for S3 Buckets](https://docs.aws.amazon.com/AmazonS3/latest/dev/bucket-encryption.html) in the *Amazon Simple Storage Service Developer Guide*\.

For more information about data protection, see the [AWS Shared Responsibility Model and GDPR](http://aws.amazon.com/blogs/security/the-aws-shared-responsibility-model-and-gdpr/) blog post on the *AWS Security Blog*\.

For other Elastic Beanstalk security topics, see [AWS Elastic Beanstalk security](security.md)\.