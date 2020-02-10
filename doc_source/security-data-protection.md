# Data protection in Elastic Beanstalk<a name="security-data-protection"></a>

AWS Elastic Beanstalk conforms to the AWS [shared responsibility model](http://aws.amazon.com/compliance/shared-responsibility-model/), which includes regulations and guidelines for data protection\. AWS is responsible for protecting the global infrastructure that runs all the AWS services\. AWS maintains control over data hosted on this infrastructure, including the security configuration controls for handling customer content and personal data\. AWS customers and APN partners, acting either as data controllers or data processors, are responsible for any personal data that they put in the AWS Cloud\. 

For data protection purposes, we recommend that you protect AWS account credentials and set up individual user accounts with AWS Identity and Access Management \(IAM\)\. Give each user only the permissions necessary to do their jobs\. We also recommend that you secure your data in the following ways:
+ Use multi\-factor authentication \(MFA\) with each account\.
+ Use Secure Sockets Layer \(SSL\)/Transport Layer Security \(TLS\) to communicate with AWS resources\.
+ Set up API and user activity logging with AWS CloudTrail\.
+ Use AWS encryption solutions and all default security controls within AWS services\.
+ Use advanced managed security services such as Amazon Macie, which assists in discovering and securing personal data that is stored in Amazon S3\.

We strongly recommend that you never put sensitive identifying information, such as your customers' account numbers, into free\-form fields such as **Name** fields\. This includes when you work with Elastic Beanstalk or other AWS services using the console, API, AWS CLI, or AWS SDKs\. Any data that you enter into Elastic Beanstalk or other services might get included in diagnostic logs\. When you provide a URL to an external server, don't include credentials information in the URL to validate your request to that server\.

For more information about data protection, see the [AWS Shared Responsibility Model and GDPR](http://aws.amazon.com/blogs/security/the-aws-shared-responsibility-model-and-gdpr/) blog post on the *AWS Security Blog*\.

For other Elastic Beanstalk security topics, see [AWS Elastic Beanstalk security](security.md)\.

**Topics**
+ [Protecting data using encryption](security-data-protection-encryption.md)
+ [Internetwork traffic privacy](security-data-protection-internetwork.md)