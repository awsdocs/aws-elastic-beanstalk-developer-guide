# Creating an Elastic Beanstalk environment<a name="using-features.environments"></a>

An AWS Elastic Beanstalk *environment* is a collection of AWS resources running an application version\. You can deploy multiple environments when you need to run multiple versions of an application\. For example, you might have development, integration, and production environments\.

The following procedure launches a new environment running the default application\. These steps are simplified to get your environment up and running quickly, using default option values\. For detailed instructions with descriptions of the many options you can use to configure the resources that Elastic Beanstalk deploys on your behalf, see [The create new environment wizard](environments-create-wizard.md)\.

**Notes**  
For instructions on creating and managing environments with the EB CLI, see [Managing Elastic Beanstalk environments with the EB CLI](eb-cli3-getting-started.md)\.
Creating an environment requires the permissions in the Elastic Beanstalk full access managed policy\. See [Elastic Beanstalk user policy](concepts-roles-user.md) for details\.

**To launch an environment with a sample application \(console\)**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Applications**, and then choose an existing application's name in the list or [create one](applications.md)\.

1. On the application overview page, choose **Create a new environment**\.  
![\[The application overview page with a list of application environments on the Elastic Beanstalk console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/applications-mgmt-environments.png)

1. Choose the **Web server environment** or **Worker environment** [environment tier](concepts.md#concepts-tier)\. You can't change an environment's tier after creation\.
**Note**  
The [\.NET on Windows Server platform](create_deploy_NET.md) doesn't support the worker environment tier\.  
![\[The Select environment tier page on the Elastic Beanstalk console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/wizard-choosetier.png)

1. For **Platform**, select the platform and platform branch that match the language your application uses\.
**Note**  
Elastic Beanstalk supports multiple [versions](concepts.platforms.md) for most of the platforms that are listed\. By default, the console selects the recommended version for the platform and platform branch you choose\. If your application requires a different version, you can select it here, or choose **Configure more options**, as described in step 7\. For information about supported platform versions, see [Elastic Beanstalk supported platforms](concepts.platforms.md)\.

1. For **Application code**, choose **Sample application**\.

1. To further customize your environment, choose **Configure more options**\. You can set the following options only during environment creation:
   + Environment name
   + Domain name
   + Platform version
   + VPC
   + Tier

   You can change the following settings after environment creation, but they require new instances or other resources to be provisioned and can take a long time to apply:
   + Instance type, root volume, key pair, and AWS Identity and Access Management \(IAM\) role
   + Internal Amazon RDS database
   + Load balancer

   For details on all available settings, see [The create new environment wizard](environments-create-wizard.md)\.

1. Choose **Create environment**\.

While Elastic Beanstalk creates your environment, you are redirected to the [Elastic Beanstalk console](environments-console.md)\. When the environment health turns green, choose the URL next to the environment name to view the running application\. This URL is generally accessible from the internet unless you configure your environment to use a [custom VPC with an internal load balancer](environments-create-wizard.md#environments-create-wizard-network)\.

**Topics**
+ [The create new environment wizard](environments-create-wizard.md)
+ [Clone an Elastic Beanstalk environment](using-features.managing.clone.md)
+ [Terminate an Elastic Beanstalk environment](using-features.terminating.md)
+ [Creating Elastic Beanstalk environments with the AWS CLI](environments-create-awscli.md)
+ [Creating Elastic Beanstalk environments with the API](environments-create-api.md)
+ [Constructing a Launch Now URL](launch-now-url.md)
+ [Creating and updating groups of Elastic Beanstalk environments](environment-mgmt-compose.md)