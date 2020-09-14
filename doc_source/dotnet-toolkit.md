# The AWS Toolkit for Visual Studio<a name="dotnet-toolkit"></a>

Visual Studio provides templates for different programming languages and application types\. You can start with any of these templates\. The AWS Toolkit for Visual Studio also provides three project templates that bootstrap development of your application: AWS Console Project, AWS Web Project, and AWS Empty Project\. For this example, you'll create a new ASP\.NET Web Application\.

**To create a new ASP\.NET web application project**

1. In Visual Studio, on the **File** menu, click **New** and then click **Project**\.

1. In the **New Project** dialog box, click **Installed Templates**, click **Visual C\#**, and then click **Web**\. Click **ASP\.NET Empty Web Application**, type a project name, and then click **OK**\. 

**To run a project**

Do one of the following:

1. Press **F5**\.

1. Select **Start Debugging** from the **Debug** menu\.

## Test locally<a name="create_deploy_NET.sdlc.testlocal"></a>

Visual Studio makes it easy for you to test your application locally\. To test or run ASP\.NET web applications, you need a web server\. Visual Studio offers several options, such as Internet Information Services \(IIS\), IIS Express, or the built\-in Visual Studio Development Server\. To learn about each of these options and to decide which one is best for you, see [Web Servers in Visual Studio for ASP\.NET Web Projects](http://msdn.microsoft.com/en-us/library/58wxa9w5.aspx)\.

## Create an Elastic Beanstalk environment<a name="create_deploy_NET.sdlc.deploy"></a>

After testing your application, you are ready to deploy it to Elastic Beanstalk\.

**Note**  
[Configuration file](ebextensions.md) needs to be part of the project to be included in the archive\. Alternatively, instead of including the configuration files in the project, you can use Visual Studio to deploy all files in the project folder\. In **Solution Explorer**, right\-click the project name, and then click **Properties**\. Click the **Package/Publish Web** tab\. In the **Items to deploy** section, select **All Files in the Project Folder** in the drop\-down list\.

**To deploy your application to Elastic Beanstalk using the AWS toolkit for Visual Studio**

1. In **Solution Explorer**, right\-click your application and then select **Publish to AWS**\.

1. In the **Publish to AWS** wizard, enter your account information\.

   1. For **AWS account to use for deployment**, select your account or select **Other** to enter new account information\. 

   1. For **Region**, select the region where you want to deploy your application\. For information about available AWS Regions, see [AWS Elastic Beanstalk Endpoints and Quotas](https://docs.aws.amazon.com/general/latest/gr/elasticbeanstalk.html) in the *AWS General Reference*\. If you select a region that is not supported by Elastic Beanstalk, then the option to deploy to Elastic Beanstalk will become unavailable\.

   1.  Click **Deploy new application with template** and select **Elastic Beanstalk**\. Then click **Next**\.  
![\[Publish to AWS wizard 1\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-vs-create-newapp-template.png)

1. On the **Application** page, enter your application details\.

   1. For **Name**, type the name of the application\.

   1. For **Description**, type a description of the application\. This step is optional\.

   1. The version label of the application automatically appears in the **Deployment version label**\.

   1. Select **Deploy application incrementally** to deploy only the changed files\. An incremental deployment is faster because you are updating only the files that changed instead of all the files\. If you choose this option, an application version will be set from the Git commit ID\. If you choose to not deploy your application incrementally, then you can update the version label in the **Deployment version label** box\.   
![\[Publish to beanstalk wizard 2\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-vs-publish-beanstalk1.png)

   1. Click **Next**\.

1. On the **Environment** page, describe your environment details\.

   1. Select **Create a new environment for this application**\.

   1. For **Name**, type a name for your environment\.

   1. For **Description**, characterize your environment\. This step is optional\.

   1. Select the **Type** of environment that you want\.

      You can select either **Load balanced, auto scaled** or a **Single instance** environment\. For more information, see [Environment types](using-features-managing-env-types.md)\.
**Note**  
For single\-instance environments, load balancing, auto scaling, and the health check URL settings don't apply\.

   1. The environment URL automatically appears in the **Environment URL** once you move your cursor to that box\.

   1. Click **Check availability** to make sure the environment URL is available\.  
![\[Publish to beanstalk wizard 3\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-vs-publish-beanstalk2.png)

   1. Click **Next**\.

1. On the **AWS Options** page, configure additional options and security information for your deployment\. 

   1.  For **Container Type**, select **64bit Windows Server 2012 running IIS 8** or **64bit Windows Server 2008 running IIS 7\.5**\.

   1. For **Instance Type**, select **Micro**\. 

   1. For **Key pair**, select **Create new key pair**\. Type a name for the new key pair—in this example, we use **myuswestkeypair**—and then click **OK**\. A key pair enables remote\-desktop access to your Amazon EC2 instances\. For more information on Amazon EC2 key pairs, see [Using Credentials](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-credentials.html) in the *Amazon Elastic Compute Cloud User Guide*\. 

   1. Select an instance profile\.

      If you do not have an instance profile, select **Create a default instance profile**\. For information about using instance profiles with Elastic Beanstalk, see [Managing Elastic Beanstalk instance profiles](iam-instanceprofile.md)\.

   1. If you have a custom VPC that you would like to use with your environment, click **Launch into VPC**\. You can configure the VPC information on the next page\. For more information about Amazon VPC, go to [Amazon Virtual Private Cloud \(Amazon VPC\)](https://aws.amazon.com/vpc/)\. For a list of supported nonlegacy container types, see [Why are some platform versions marked legacy?](using-features.migration.md#using-features.migration.why)  
![\[Publish to beanstalk wizard 4\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-vs-publish-beanstalk3b_iam.png)

   1.  Click **Next**\. 

1. If you selected to launch your environment inside a VPC, the **VPC Options** page appears; otherwise, the **Additional Options** page appears\. Here you'll configure your VPC options\.  
![\[VPC options for load-balanced, scalable environment\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-vs-publish-beanstalk3b_vpc.png)  
![\[VPC options for single-instance environment\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-vs-publish-beanstalk3b_vpc-single.png)

   1. Select the VPC ID of the VPC in which you would like to launch your environment\. 

   1. For a load\-balanced, scalable environment, select **private** for **ELB Scheme** if you do not want your elastic load balancer to be available to the Internet\.

      For a single\-instance environment, this option is not applicable because the environment doesn't have a load balancer\. For more information, see [Environment types](using-features-managing-env-types.md)\.

   1. For a load\-balanced, scalable environment, select the subnets for the elastic load balancer and the EC2 instances\. If you created public and private subnets, make sure the elastic load balancer and the EC2 instances are associated with the correct subnet\. By default, Amazon VPC creates a default public subnet using 10\.0\.0\.0/24 and a private subnet using 10\.0\.1\.0/24\. You can view your existing subnets in the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

      For a single\-instance environment, your VPC only needs a public subnet for the instance\. Selecting a subnet for the load balancer is not applicable because the environment doesn't have a load balancer\. For more information, see [Environment types](using-features-managing-env-types.md)\.

   1. For a load\-balanced, scalable environment, select the security group you created for your instances, if applicable\.

      For a single\-instance environment, you don't need a NAT device\. Select the default security group\. Elastic Beanstalk assigns an Elastic IP address to the instance that lets the instance access the Internet\.

   1. Click **Next**\.

1. On the **Application Options** page, configure your application options\. 

   1. For Target framework, select **\.NET Framework 4\.0**\. 

   1. Elastic Load Balancing uses a health check to determine whether the Amazon EC2 instances running your application are healthy\. The health check determines an instance's health status by probing a specified URL at a set interval\. You can override the default URL to match an existing resource in your application \(e\.g\., `/myapp/index.aspx`\) by entering it in the **Application health check URL** box\. For more information about application health checks, see [Health check](environments-cfg-clb.md#using-features.managing.elb.healthchecks)\. 

   1. Type an email address if you want to receive Amazon Simple Notification Service \(Amazon SNS\) notifications of important events affecting your application\.

   1. The **Application Environment** section lets you specify environment variables on the Amazon EC2 instances that are running your application\. This setting enables greater portability by eliminating the need to recompile your source code as you move between environments\.

   1. Select the application credentials option you want to use to deploy your application\.  
![\[Publish to beanstalk wizard 6\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-vs-publish-beanstalk3a.png)

   1. Click **Next**\.

1. If you have previously set up an Amazon RDS database, the **Amazon RDS DB Security Group** page appears\. If you want to connect your Elastic Beanstalk environment to your Amazon RDS DB Instance, then select one or more security groups\. Otherwise, go on to the next step\. When you're ready, click **Next**\.  
![\[Publish to beanstalk wizard 7\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-vs-publish-beanstalk6b.png)

1.  Review your deployment options\. If everything is as you want, click **Deploy**\.   
![\[Publish to beanstalk wizard 8\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-vs-publish-beanstalk4.png)

   Your ASP\.NET project will be exported as a web deploy file, uploaded to Amazon S3, and registered as a new application version with Elastic Beanstalk\. The Elastic Beanstalk deployment feature will monitor your environment until it becomes available with the newly deployed code\. On the env:<environment name> tab, you will see status for your environment\.   
![\[Environment status\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-vs-env-status.png)

## Terminating an environment<a name="create_deploy_NET.terminating"></a>

To avoid incurring charges for unused AWS resources, you can terminate a running environment using the AWS Toolkit for Visual Studio\.

**Note**  
 You can always launch a new environment using the same version later\. 

**To terminate an environment**

1.  Expand the Elastic Beanstalk node and the application node in **AWS Explorer**\. Right\-click your application environment and select **Terminate Environment**\.

1. When prompted, click **Yes** to confirm that you want to terminate the environment\. It will take a few minutes for Elastic Beanstalk to terminate the AWS resources running in the environment\.  
![\[Elastic Beanstalk terminate environment dialog box\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-vs-terminate-confirm.png)
**Note**  
When you terminate your environment, the CNAME associated with the terminated environment becomes available for anyone to use\. 