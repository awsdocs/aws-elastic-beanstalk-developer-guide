# Getting started with \.NET Core on Linux<a name="dotnet-linux-getting-started"></a>

To get started with \.NET Core on Linux applications on AWS Elastic Beanstalk, all you need is an application [source bundle](applications-sourcebundle.md) to upload as your first application version, and then to deploy it to an environment\. When you create an environment, Elastic Beanstalk allocates all of the AWS resources needed to run a highly scalable web application\.

## Launching an environment with a sample \.NET Core on Linux application<a name="dotnet-linux-getstarted-samples"></a>

Elastic Beanstalk provides single\-page sample applications for each platform\.


**Samples**  

|  Supported configurations  |  Environment type  |  Source bundle  |  Description  | 
| --- | --- | --- | --- | 
|  \.NET Core on Linux  |  Web server  |  [dotnet\-core\-linux\.zip](samples/dotnet-core-linux.zip)  |  Single\-page application\.  | 
|  \.NET Core on Linux  |  Web server  |  [dotnet\-core\-linux\-multiple\-apps\.zip](samples/dotnet-core-linux-multiple-apps.zip)  |  Two web applications that run on the same web server\.  | 

Download the sample application and deploy it to Elastic Beanstalk by following these steps\.

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

## Next steps<a name="dotnet-linux-getstarted-next"></a>

After you have an environment running an application, you can deploy a new version of the application or a different application at any time\. Deploying a new application version is very quick because it doesn't require provisioning or restarting Amazon EC2 instances\.

After you deploy a sample application or two and are ready to start developing and running \.NET Core applications locally, see [Setting up your \.NET Core on Linux development environment](dotnet-linux-devenv.md)\.