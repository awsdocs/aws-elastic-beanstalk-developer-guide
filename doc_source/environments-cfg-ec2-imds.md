# Configuring the instance metadata service on your environment's instances<a name="environments-cfg-ec2-imds"></a>

*Instance metadata* is data about an Amazon Elastic Compute Cloud \(Amazon EC2\) instance that applications can use to configure or manage the running instance\. The instance metadata service \(IMDS\) is an on\-instance component that code on the instance uses to securely access instance metadata\. This code can be Elastic Beanstalk platform code on your environment instances, the AWS SDK that your application might be using, and even your application's own code\. For more information, see [Instance metadata and user data](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html) in the *Amazon EC2 User Guide for Linux Instances*\.

Code can access instance metadata from a running instance using one of two methods: Instance Metadata Service Version 1 \(IMDSv1\) or Instance Metadata Service Version 2 \(IMDSv2\)\. IMDSv2 uses session\-oriented requests and mitigates several types of vulnerabilities that could be used to try to access the IMDS\. For details about these two methods, see [Configuring the instance metadata service](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/configuring-instance-metadata-service.html) in the *Amazon EC2 User Guide for Linux Instances*\.

**Topics**
+ [Platform support for IMDS](#environments-cfg-ec2-imds.plat)
+ [Choosing IMDS methods](#environments-cfg-ec2-imds.choose)
+ [Configuring IMDS using the Elastic Beanstalk console](#environments-cfg-ec2-imds.console)
+ [The aws:autoscaling:launchconfiguration namespace](#environments-cfg-ec2-imds.namespace)

## Platform support for IMDS<a name="environments-cfg-ec2-imds.plat"></a>

Older Elastic Beanstalk platform versions supported IMDSv1\. Newer Elastic Beanstalk platform versions \(all [Amazon Linux 2 platform versions](using-features.migration-al.md)\) support both IMDSv1 and IMDSv2\. You can configure your environment to support both methods \(the default\) or disable IMDSv1\.

**Note**  
Disabling IMDSv1 requires using Amazon EC2 launch templates\. When you configure this feature during environment creation or updates, Elastic Beanstalk attempts to configure your environment to use Amazon EC2 launch templates \(if the environment isn't using them already\)\. In this case, if your user policy lacks the necessary permissions, environment creation or updates might fail\. Therefore, we recommend that you use our managed user policy or add the required permissions to your custom policies\. For details about the required permissions, see [Creating a custom user policy](AWSHowTo.iam.managed-policies.md#AWSHowTo.iam.policies)\.

## Choosing IMDS methods<a name="environments-cfg-ec2-imds.choose"></a>

When making a decision about the IMDS methods you want your environment to support, consider the following cases:
+ *AWS SDK* – If your application uses an AWS SDK for any language, make sure you use an up\-to\-date version of the SDK\. The AWS SDKs make IMDS calls, and newer SDK versions use IMDSv2 whenever possible\. If you ever disable IMDSv1, and your application uses an old SDK version, IMDS calls might fail\.
+ *Your application code* – If your application makes IMDS calls, consider using the AWS SDK to make the calls instead of making direct HTTP requests\. This way you don't need to make code changes to switch between IMDS methods\. The AWS SDK uses IMDSv2 whenever possible\.
+ *Elastic Beanstalk platform code* – Our code makes IMDS calls through the AWS SDK, and therefore uses IMDSv2 on all supporting platform versions\. If your code uses an up\-to\-date AWS SDK and makes all IMDS calls through the SDK, you can safely disable IMDSv1\.

## Configuring IMDS using the Elastic Beanstalk console<a name="environments-cfg-ec2-imds.console"></a>

You can modify your Elastic Beanstalk environment's Amazon EC2 instance configuration in the Elastic Beanstalk console\.

**To configure IMDS on your Amazon EC2 instances in the Elastic Beanstalk console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Configuration**\.

1. In the **Instances** configuration category, choose **Edit**\.  
![\[IMDS option on Elastic Beanstalk instances configuration window\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-env-config-ec2-imds.png)

1. Set **Disable IMDSv1** to enforce IMDSv2\. Clear **Disable IMDSv1** to enable both IMDSv1 and IMDSv2\.

1. Choose **Apply**\.

## The aws:autoscaling:launchconfiguration namespace<a name="environments-cfg-ec2-imds.namespace"></a>

You can use a [configuration option](command-options.md) in the `aws:autoscaling:launchconfiguration` namespace to configure IMDS on your environment's instances\.

The following [configuration file](ebextensions.md) example disables IMDSv1 using the `DisableIMDSv1` option\.

```
option_settings:
  aws:autoscaling:launchconfiguration:
    DisableIMDSv1: true
```