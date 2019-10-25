# Creating an AWS Elastic Beanstalk Environment<a name="using-features.environments"></a>

You can deploy multiple environments when you need to run multiple versions of an application\. For example, you might have development, integration, and production environments\.

**Note**  
For instructions on creating and managing environments with the EB CLI, see [Managing Elastic Beanstalk Environments with the EB CLI](eb-cli3-getting-started.md)\.

The **Create New Environment** wizard in the AWS Management Console guides you through the creation of an environment step by step, with a bevy of options for configuring the resources that Elastic Beanstalk deploys on your behalf\. If you are just getting started, you can use the default values for many of these options without issue\.

**Note**  
Creating an environment requires the permissions in the Elastic Beanstalk full access managed policy\. See [Elastic Beanstalk User Policy](concepts-roles-user.md) for details\.

Follow this procedure to launch a new environment running the default application\. These steps are simplified to get your environment up and running quickly\. See [The Create New Environment Wizard](environments-create-wizard.md) for more detailed instructions with descriptions of all of the available options\.

**To launch an environment with a sample application \(console\)**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Choose an existing application or [create one](applications.md)\.

1. From the **Actions** menu in the upper\-right corner, choose **Create environment**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/application-actions-createnewenvironment.png)

1. Choose the **Web server environment** or **Worker environment** [environment tier](concepts.md#concepts-tier)\. You can't change an environment's tier after creation\.
**Note**  
The [\.NET on Windows Server platform](create_deploy_NET.md) doesn't support the worker environment tier\.

1. Choose a **Platform** that matches the language used by your application\.
**Note**  
Elastic Beanstalk supports multiple [versions](concepts.platforms.md) for most of the platforms that are listed\. By default, the console selects the latest version of the language, web container, or framework [supported by Elastic Beanstalk](concepts.platforms.md)\. If your application requires an earlier version, choose **Configure more options**, as described in step 7\.

1. For **App code**, choose **Sample application**\.

1. To further customize your environment, choose **Configure more options**\. You can set the following options only during environment creation:
   + Environment name
   + Domain name
   + Platform version \(configuration\)
   + VPC
   + Tier

   You can change the following settings after environment creation, but they require new instances or other resources to be provisioned and can take a long time to apply:
   + Instance type, root volume, key pair, and AWS Identity and Access Management \(IAM\) role
   + Internal Amazon RDS database
   + Load balancer

   For details on all available settings, see [The Create New Environment Wizard](environments-create-wizard.md)\.

1. Choose **Create environment**\.

While Elastic Beanstalk creates your environment, you are redirected to the [The AWS Elastic Beanstalk Environment Management Console](environments-console.md)\. Once the environment health turns green, click on the URL next to the environment name to view the running application\. This URL is generally accessible from the Internet unless you configure your environment to use a [custom VPC with an internal load balancer](environments-create-wizard.md#environments-create-wizard-network)\.

**Topics**
+ [The Create New Environment Wizard](environments-create-wizard.md)
+ [Clone an Elastic Beanstalk Environment](using-features.managing.clone.md)
+ [Terminate an Elastic Beanstalk Environment](using-features.terminating.md)
+ [Creating Elastic Beanstalk Environments with the AWS CLI](environments-create-awscli.md)
+ [Creating Elastic Beanstalk Environments with the API](environments-create-api.md)
+ [Constructing a Launch Now URL](launch-now-url.md)
+ [Creating and Updating Groups of AWS Elastic Beanstalk Environments](environment-mgmt-compose.md)