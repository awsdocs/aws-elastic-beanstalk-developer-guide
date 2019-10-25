# Getting Started with \.NET on Elastic Beanstalk<a name="dotnet-getstarted"></a>

To get started with \.NET applications on AWS Elastic Beanstalk, all you need is an application [source bundle](applications-sourcebundle.md) to upload as your first application version and to deploy to an environment\. When you create an environment, Elastic Beanstalk allocates all of the AWS resources needed to run a highly scalable web application\.

## Launching an Environment with a Sample \.NET Application<a name="dotnet-getstarted-samples"></a>

Elastic Beanstalk provides single page sample applications for each platform as well as more complex examples that show the use of additional AWS resources such as Amazon RDS and language or platform\-specific features and APIs\.


**Samples**  

|  Name  |  Supported Configurations  |  Environment Type  |  Source  |  Description  | 
| --- | --- | --- | --- | --- | 
|  \.NET Default  |  WS 2012 R2 WS 2012 R2 Server Core WS 2012 WS 2008 R2  |  Web Server  |   [dotnet\-asp\-v1\.zip](samples/dotnet-asp-v1.zip)   |  ASP\.NET web application with a single page configured to be displayed at the website root\.  | 
|  ASP\.NET MVC5  |  WS 2012 R2  |  Web Server  |  [dotnet\-aspmvc5\-v1\.zip](samples/dotnet-aspmvc5-v1.zip)  |  ASP\.NET web application with a classic model\-view\-control architecture\.  | 

Download any of the sample applications and deploy it to Elastic Beanstalk by using the following procedure\.

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

## Next Steps<a name="dotnet-getstarted-next"></a>

After you have an environment running an application, you can [deploy a new version](using-features.deploy-existing-version.md) of the application or a completely different application at any time\. Deploying a new application version is very quick because it doesn't require provisioning or restarting EC2 instances\.

After you've deployed a sample application or two and are ready to start developing locally, see [the next section](dotnet-devenv.md) to set up a \.NET development environment\.