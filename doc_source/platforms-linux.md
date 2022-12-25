# Elastic Beanstalk Linux platforms<a name="platforms-linux"></a>

AWS Elastic Beanstalk provides a variety of platforms on which you can build your applications\. You design your web application to one of these platforms, and Elastic Beanstalk deploys your code to the platform version you selected to create an active application environment\.

Elastic Beanstalk provides platforms for different programming languages, application servers, and Docker containers\. Some platforms have multiple concurrently\-supported versions\.

For full coverage of Elastic Beanstalk platforms, see [Elastic Beanstalk platforms](concepts-all-platforms.md)\.

Many of the platforms that Elastic Beanstalk supports are based on the Linux operating system \(OS\)\. Specifically, these platforms are based on Amazon Linux, a Linux distribution provided by AWS\. Elastic Beanstalk Linux platforms use Amazon Elastic Compute Cloud \(Amazon EC2\) instances, and these instances run Amazon Linux\. To learn more, see [Amazon Linux](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/amazon-linux-ami-basics.html) in the *Amazon EC2 User Guide for Linux Instances*\.

The Elastic Beanstalk Linux platforms provide a lot of functionality out of the box\. You can extend the platforms in several ways to support your application\. For details, see [Extending Elastic Beanstalk Linux platforms](platforms-linux-extend.md)\.

**Important**  
The *Let's Encrypt* cross\-signed DST Root CA X3 certificate *expired* on *September 30, 2021*\. Due to this, Beanstalk environments running on the Amazon Linux 2 and Amazon Linux AMI operating systems might not be able to connect to servers using *Let's Encrypt* certificates\.  
On October 3, 2021 Elastic Beanstalk released new platform versions for Amazon Linux AMI and Amazon Linux 2 with the updated CA certificates\. To receive these updates and address this issue turn on [Managed Updates](environment-platform-update-managed.md) or [update your platforms manually](using-features.platform.upgrade.md#using-features.platform.upgrade.config)\. For more information, see the [platform update release notes](https://docs.aws.amazon.com/elasticbeanstalk/latest/relnotes/release-2021-10-03-linux.html) in the *AWS Elastic Beanstalk Release Notes*\.  
You can also apply the manual workarounds described in this [AWS Knowledge Center article](https://aws.amazon.com/premiumsupport/knowledge-center/ec2-expired-certificate/)\. Since Elastic Beanstalk provides AMIs with locked GUIDs, we recommend that you use the sudo yum install command in the article\. Alternatively, you can also use the sudo sed command in the article if you prefer to manually modify the system in place\.

**Topics**
+ [Linux platform versions](#platforms-linux.versions)
+ [List of Elastic Beanstalk Linux platforms](#platforms-linux.list)
+ [Extending Elastic Beanstalk Linux platforms](platforms-linux-extend.md)

## Linux platform versions<a name="platforms-linux.versions"></a>

AWS provides two versions of Amazon Linux: [Amazon Linux 2](https://aws.amazon.com/amazon-linux-2/) and [Amazon Linux AMI](https://aws.amazon.com/amazon-linux-ami/)\. Some key improvements in Amazon Linux 2 compared to Amazon Linux AMI are:
+ Amazon Linux 2 offers long\-term support\.
+ Amazon Linux 2 is available as virtual machine images for on\-premises development and testing\.
+ Amazon Linux 2 comes with updated components: the Linux kernel, C library, compiler, and tools\. It also uses the systemd service and systems manager as opposed to System V init system in Amazon Linux AMI\.

Elastic Beanstalk maintains platform versions with both Amazon Linux versions\. For details about supported platform versions, see [Elastic Beanstalk supported platforms](concepts.platforms.md)\.

**Note**  
Amazon Linux 2 platform versions are incompatible with previous Amazon Linux AMI platform versions\. If you're migrating your Elastic Beanstalk application to Amazon Linux 2, read [Migrating your Elastic Beanstalk Linux application to Amazon Linux 2](using-features.migration-al.md)\.

## List of Elastic Beanstalk Linux platforms<a name="platforms-linux.list"></a>

The following list mentions the Linux platforms that Elastic Beanstalk supports for different programming languages, as well as for Docker containers, and links to chapters about them in this developer guide\.
+ [Docker](create_deploy_docker.md)
+ [Go](create_deploy_go.md)
+ [Java](create_deploy_Java.md)
+ [\.NET Core on Linux](create-deploy-dotnet-core-linux.md)
+ [Node\.js](create_deploy_nodejs.md)
+ [PHP](create_deploy_PHP_eb.md)
+ [Python](create-deploy-python-apps.md)
+ [Ruby](create_deploy_Ruby.md)