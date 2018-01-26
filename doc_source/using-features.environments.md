# Creating an AWS Elastic Beanstalk Environment<a name="using-features.environments"></a>

You can deploy multiple environments when you need to run multiple versions of an application\. For example, you might have development, integration, and production environments\.

**Note**  
For instructions on creating and managing environments with the EB CLI, see \.

The **Create New Environment** wizard in the AWS Management Console guides you through the creation of an environment step by step, with a bevy of options for configuring the resources that Elastic Beanstalk deploys on your behalf\. If you are just getting started, you can use the default values for many of these options without issue\.

**Note**  
Creating an environment requires the permissions in the Elastic Beanstalk full access managed policy\. See  for details\.

Follow this procedure to launch a new environment running the default application\. These steps are simplified to get your environment up and running quickly\. See  for more detailed instructions with descriptions of all of the available options\.

**To launch an environment with a sample application \(console\)**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Choose an application or create a new one\.

1. From the **Actions** menu in the upper right corner, choose **Create environment**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/application-actions-createnewenvironment.png)

1. Choose either the **Web server environment** or **Worker environment** environment tier\. You cannot change an environment's tier after creation\.
**Note**  
The \.NET on Windows Server platform doesn't support the worker environment tier\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/wizard-choosetier.png)

1. Choose a **Platform** that matches the language used by your application\.
**Note**  
Elastic Beanstalk supports multiple configurations for most platforms listed\. By default, the console selects the latest version of the language, web container, or framework supported by Elastic Beanstalk\. If your application requires an older version, choose **Configure more options**, as described below\.

1. For **App code**, choose **Sample application**\.

1. If you want to further customize your environment, choose **Configure more options**\. The following options can be set only during environment creation:

   + Environment name

   + Domain name

   + Platform configuration

   + VPC

   + Tier

   The following settings can be changed after environment creation, but require new instances or other resources to be provisioned and can take a long time to apply:

   + Instance type, root volume, key pair, and IAM role

   + Internal Amazon RDS database

   + Load balancer

   For details on all available settings, see \.

1. Choose **Create environment**\.

While Elastic Beanstalk creates your environment, you are redirected to the \. Once the environment health turns green, click on the URL next to the environment name to view the running application\. This URL is generally accessible from the Internet unless you configure your environment to use a custom VPC with an internal load balancer\.


+ [The Create New Environment Wizard](environments-create-wizard.md)
+ [Clone an Elastic Beanstalk Environment](using-features.managing.clone.md)
+ [Terminate an Elastic Beanstalk Environment](using-features.terminating.md)
+ [Creating Elastic Beanstalk Environments with the AWS CLI](environments-create-awscli.md)
+ [Creating Elastic Beanstalk Environments with the API](environments-create-api.md)
+ [Constructing a Launch Now URL](launch-now-url.md)
+ [Creating and Updating Groups of AWS Elastic Beanstalk Environments](environment-mgmt-compose.md)