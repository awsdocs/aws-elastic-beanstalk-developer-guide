# Elastic Beanstalk Linux Platforms<a name="platforms-linux"></a>

AWS Elastic Beanstalk provides a variety of platforms on which you can build your applications\. You design your web application to one of these platforms, and Elastic Beanstalk deploys your code to the platform version you selected to create an active application environment\.

Elastic Beanstalk provides platforms for different programming languages, application servers, and Docker containers\. Some platforms have multiple concurrently\-supported versions\.

For full coverage of Elastic Beanstalk platforms, see [AWS Elastic Beanstalk Platforms](concepts-all-platforms.md)\.

Many of the platforms that Elastic Beanstalk supports are based on the Linux operating system \(OS\)\. Specifically, these platforms are based on Amazon Linux, a Linux distribution provided by AWS\. Elastic Beanstalk Linux platforms use Amazon Elastic Compute Cloud \(Amazon EC2\) instances, and these instances run Amazon Linux\. To learn more, see [Amazon Linux](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/amazon-linux-ami-basics.html) in the *Amazon EC2 User Guide for Linux Instances*\.

The Elastic Beanstalk Linux platforms provide a lot of functionality out of the box\. You can extend the platforms in several ways to support your application\. For details, see [Extending Elastic Beanstalk Linux Platforms](platforms-linux-extend.md)\.

**Topics**
+ [Linux Platform Versions](#platforms-linux.versions)
+ [List of Elastic Beanstalk Linux Platforms](#platforms-linux.list)
+ [Extending Elastic Beanstalk Linux Platforms](platforms-linux-extend.md)

## Linux Platform Versions<a name="platforms-linux.versions"></a>


|  | 
| --- |
| AWS Elastic Beanstalk support for Amazon Linux 2 is in beta release and is subject to change\. | 

AWS provides two versions of Amazon Linux: [Amazon Linux 2](https://aws.amazon.com/amazon-linux-2/) and [Amazon Linux AMI](https://aws.amazon.com/amazon-linux-ami/)\. Some key improvements in Amazon Linux 2 compared to Amazon Linux AMI are:
+ Amazon Linux 2 offers long\-term support\.
+ Amazon Linux 2 is available as virtual machine images for on\-premises development and testing\.
+ Amazon Linux 2 comes with updated components: the Linux kernel, C library, compiler, and tools\. It also uses the systemd service and systems manager as opposed to System V init system in Amazon Linux AMI\.

Elastic Beanstalk maintains platform versions with both Amazon Linux versions\. For details about supported platform versions, see [AWS Elastic Beanstalk Supported Platforms](concepts.platforms.md)\.

Amazon Linux 2 platform versions are incompatible with previous Amazon Linux AMI platform versions\. If you're migrating your Elastic Beanstalk application to Amazon Linux 2, read [Migrating Your Elastic Beanstalk Linux Application to Amazon Linux 2](using-features.migration-al.md)\.

## List of Elastic Beanstalk Linux Platforms<a name="platforms-linux.list"></a>

The following list mentions the Linux platforms that Elastic Beanstalk supports for different programming languages, as well as for Docker containers, and links to chapters about them in this developer guide\.
+ [Docker](create_deploy_docker.md)
+ [Go](create_deploy_go.md)
+ [Java](create_deploy_Java.md)
+ [Node\.js](create_deploy_nodejs.md)
+ [PHP](create_deploy_PHP_eb.md)
+ [Python](create-deploy-python-apps.md)
+ [Ruby](create_deploy_Ruby.md)