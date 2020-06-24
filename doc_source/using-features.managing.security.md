# Your AWS Elastic Beanstalk environment security<a name="using-features.managing.security"></a>

Elastic Beanstalk provides several options that control the security of your environment and of the Amazon EC2 instances in it\. This topic discusses the configuration of these options\.

**Topics**
+ [Configuring your environment security](#using-features.managing.security.console)
+ [Environment security configuration namespaces](#using-features.managing.security.namespaces)

## Configuring your environment security<a name="using-features.managing.security.console"></a>

You can modify your Elastic Beanstalk environment security configuration in the Elastic Beanstalk console\.

**To configure environment security in the Elastic Beanstalk console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Configuration**\.

1. In the **Security** configuration category, choose **Edit**\.

The following settings are available\.

**Topics**
+ [Service role](#using-features.managing.security.servicerole)
+ [EC2 key pair](#using-features.managing.security.keypair)
+ [IAM instance profile](#using-features.managing.security.profile)

![\[The Elastic Beanstalk security configuration page\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-env-config-security-page.png)

### Service role<a name="using-features.managing.security.servicerole"></a>

Select a [service role](iam-servicerole.md) to associate with your Elastic Beanstalk environment\. Elastic Beanstalk assumes the service role when it accesses other AWS services on your behalf\. For details, see [Managing Elastic Beanstalk service roles](iam-servicerole.md)\. 

### EC2 key pair<a name="using-features.managing.security.keypair"></a>

You can securely log in to the Amazon Elastic Compute Cloud \(Amazon EC2\) instances provisioned for your Elastic Beanstalk application with an Amazon EC2 key pair\. For instructions on creating a key pair, see [Creating a Key Pair Using Amazon EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html#having-ec2-create-your-key-pair) in the *Amazon EC2 User Guide for Linux Instances*\. 

**Note**  
When you create a key pair, Amazon EC2 stores a copy of your public key\. If you no longer need to use it to connect to any environment instances, you can delete it from Amazon EC2\. For details, see [Deleting Your Key Pair](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html#delete-key-pair) in the *Amazon EC2 User Guide for Linux Instances*\.

Choose an **EC2 key pair** from the drop\-down menu to assign it to your environment's instances\. When you assign a key pair, the public key is stored on the instance to authenticate the private key, which you store locally\. The private key is never stored on AWS\.

For more information about connecting to Amazon EC2 instances, see [Connect to Your Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstances.html) and [Connecting to Linux/UNIX Instances from Windows using PuTTY](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html) in the *Amazon EC2 User Guide for Linux Instances*\. 

### IAM instance profile<a name="using-features.managing.security.profile"></a>

An [instance profile](concepts-roles-instance.md) is an IAM role that is applied to instances launched in your Elastic Beanstalk environment\. Amazon EC2 instances assume the instance profile role to sign requests to AWS and access APIs, for example, to upload logs to Amazon S3\.

The first time you create an environment in the Elastic Beanstalk console, Elastic Beanstalk prompts you to create an instance profile with a default set of permissions\. You can add permissions to this profile to provide your instances access to other AWS services\. For details, see [Managing Elastic Beanstalk instance profiles](iam-instanceprofile.md)\.

## Environment security configuration namespaces<a name="using-features.managing.security.namespaces"></a>

Elastic Beanstalk provides [configuration options](command-options.md) in the following namespaces to enable you to customize the security of your environment:
+ [`aws:elasticbeanstalk:environment`](command-options-general.md#command-options-general-elasticbeanstalkenvironment) – Configure the environment's service role using the `ServiceRole` option\.
+ [`aws:autoscaling:launchconfiguration`](command-options-general.md#command-options-general-autoscalinglaunchconfiguration) – Configure permissions for the environment's Amazon EC2 instances using the `EC2KeyName` and `IamInstanceProfile` options\.

The EB CLI and Elastic Beanstalk console apply recommended values for the preceding options\. You must remove these settings if you want to use configuration files to configure the same\. See [Recommended values](command-options.md#configuration-options-recommendedvalues) for details\.