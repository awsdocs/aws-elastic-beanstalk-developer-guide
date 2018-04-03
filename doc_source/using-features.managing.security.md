# Your AWS Elastic Beanstalk Environment Security<a name="using-features.managing.security"></a>

Elastic Beanstalk provides several options that control the security of your environment and of the Amazon EC2 instances in it\. This topic discusses the configuration of these options\.

**Topics**
+ [Configuring Your Environment Security](#using-features.managing.security.console)
+ [Environment Security Configuration Namespaces](#using-features.managing.security.namespaces)

## Configuring Your Environment Security<a name="using-features.managing.security.console"></a>

You can modify your Elastic Beanstalk environment security configuration in the Elastic Beanstalk console\.

**To configure environment security in the Elastic Beanstalk console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. Choose **Configuration**\.

1. On the **Security** configuration card, choose **Modify**\.

The following settings are available\.

**Topics**
+ [Service role](#using-features.managing.security.servicerole)
+ [EC2 key pair](#using-features.managing.security.keypair)
+ [IAM Instance Profile](#using-features.managing.security.profile)

![\[The Elastic Beanstalk security configuration page\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-env-config-security-page.png)

### Service role<a name="using-features.managing.security.servicerole"></a>

Select a [service role](iam-servicerole.md) to associate with your Elastic Beanstalk environment\. Elastic Beanstalk assumes the service role when it accesses other AWS services on your behalf\. For details, see [Managing Elastic Beanstalk Service Roles](iam-servicerole.md)\. 

### EC2 key pair<a name="using-features.managing.security.keypair"></a>

You can securely log in to the Amazon EC2 instances provisioned for your Elastic Beanstalk application with an Amazon EC2 key pair\. For instructions on creating a key pair for Amazon EC2, see the [Amazon Elastic Compute Cloud Getting Started Guide](http://docs.aws.amazon.com/AWSEC2/latest/GettingStartedGuide/)\. 

Choose an **EC2 key pair** from the drop\-down menu to assign it to your environment's instances\. When you assign a key pair, the public key is stored on the instance to authenticate the private key, which you store locally\. The private key is never stored in AWS\.

For more information about connecting to Amazon EC2 instances, see [Connect to Your Instance](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstances.html) and [ Connecting to Linux/UNIX Instances from Windows using PuTTY](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html) in the *Amazon Elastic Compute Cloud User Guide*\. 

### IAM Instance Profile<a name="using-features.managing.security.profile"></a>

An [instance profile](concepts-roles-instance.md) is an IAM role that is applied to instances launched in your Elastic Beanstalk environment\. Amazon EC2 instances assume the instance profile role to sign requests to AWS and access APIs, for example, to upload logs to Amazon S3\.

The first time you create an environment in the Elastic Beanstalk console, Elastic Beanstalk prompts you to create an instance profile with a default set of permissions\. You can add permissions to this profile to provide your instances access to other AWS services\. For details, see [Managing Elastic Beanstalk Instance Profiles](iam-instanceprofile.md)\.

## Environment Security Configuration Namespaces<a name="using-features.managing.security.namespaces"></a>

Elastic Beanstalk provides [configuration options](command-options.md) in the following namespaces to enable you to customize the security of your environment:
+ [`aws:elasticbeanstalk:environment`](command-options-general.md#command-options-general-elasticbeanstalkenvironment) – Configure the environment's service role using the `ServiceRole` option\.
+ [`aws:autoscaling:launchconfiguration`](command-options-general.md#command-options-general-autoscalinglaunchconfiguration) – Configure permissions for the environment's Amazon EC2 instances using the `EC2KeyName` and `IamInstanceProfile` options\.

The EB CLI and Elastic Beanstalk console apply recommended values for the preceding options\. You must remove these settings if you want to use configuration files to configure the same\. See [Recommended Values](command-options.md#configuration-options-recommendedvalues) for details\.