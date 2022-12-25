# The AWS Toolkit for Visual Studio \- Working with \.Net Core<a name="dotnet-toolkit-linux"></a>

The AWS Toolkit for Visual Studio is a plugin to the Visual Studio IDE\. With the toolkit you can deploy and manage applications in Elastic Beanstalk while you are working in your Visual Studio environment\.

This topic shows how you can do the following tasks using the AWS Toolkit for Visual Studio:
+ Create an ASP\.NET Core web application using a Visual Studio template\.
+ Create an Elastic Beanstalk Amazon Linux environment\.
+ Deploy the ASP\.NET Core web application to the new Amazon Linux environment\.

This topic also explores how you can use the AWS Toolkit for Visual Studio to manage your Elastic Beanstalk application environments and monitor your application's health\.

**Topics**
+ [Prerequisites](#dotnet-toolkit-linux-core-tutorial-prereqs)
+ [Create a new application project](#dotnet-toolkit-linux-core-tutorial-create-project)
+ [Create an Elastic Beanstalk environment and deploy your application](#dotnet-toolkit-linux-core-tutorial-create-env-and-deploy)
+ [Terminating an environment](#dotnet-toolkit-linux-core-tutorial-terminate-env)
+ [Managing your Elastic Beanstalk application environments](create_deploy_NET-linux.managing.md)
+ [Monitoring application health](create_deploy_NET-linux.healthstatus.md)

## Prerequisites<a name="dotnet-toolkit-linux-core-tutorial-prereqs"></a>

Before you begin this tutorial, you need to install the AWS Toolkit for Visual Studio\. For instructions, see [Setting up the AWS Toolkit for Visual Studio](https://docs.aws.amazon.com/toolkit-for-visual-studio/latest/user-guide/getting-set-up.html)\.

If you have never used the toolkit before, the first thing you'll need to do after installing the toolkit is to register your AWS credentials with the toolkit\. For more information about this, see [Providing AWS Credentials](https://docs.aws.amazon.com/toolkit-for-visual-studio/latest/user-guide/credentials.html)\.

## Create a new application project<a name="dotnet-toolkit-linux-core-tutorial-create-project"></a>

If you don't have a \.NET Core application project in Visual Studio, you can easily create one using one of the Visual Studio project templates\.

**To create a new ASP\.NET Core web application project**

1. In Visual Studio, on the **File** menu, choose **New** and then choose **Project**\.

1. In the **Create a new project** dialog box, select **C\#**, select **Linux**, and then select **Cloud**\.

1. From the list of project templates that displays select **ASP\.NET Core Web Application**, and then select **Next**\.
**Note**  
If you don't see **ASP\.NET Core Web Application** listed in the project templates, you can install it in Visual Studio\.  
Scroll to the bottom of the template list and select the **Install more tools and features** link that is located under the template list\. 
If you are prompted to allow the Visual Studio application to make changes to your device, select **Yes**\.
Choose the **Workloads** tab, then select **ASP\.NET and web development\. ** 
Select the **Modify** button\. The **Visual Studio Installer** installs the project template\. 
After the installer completes, exit the panel to return to where you left off in Visual Studio \.

1. In the **Configure your new project** dialog box, enter a **Project name**\. The **Solution name** defaults to your project name\. Next, choose **Create**\.

1. In the **Create a new ASP\.NET Core web application** dialog box, select **\.NET Core**, and then select **ASP\.NET Core 3\.1**\. From the list of application types that are displayed, select **Web Application**, then select the **Create** button\.  
![\[Visual Studio screen shot for the create a new ASP.NET Core web application dialog box page\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-vs-linux-create-newapp-template.png)

 Visual Studio displays the **Creating Project** dialog box when it creates your application\. After Visual Studio completes generating your application, a panel with your application name is displayed\.

![\[Visual Studio screen shot application panel\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-vs-linux-web-application-display.png)

## Create an Elastic Beanstalk environment and deploy your application<a name="dotnet-toolkit-linux-core-tutorial-create-env-and-deploy"></a>

This section describes how to create an Elastic Beanstalk environment for your application and deploy your application to that environment\. 

**To create a new environment and deploy your application**

1.  In Visual Studio select **View**, then **Solution Explorer**\. 

1. In **Solution Explorer**, open the context \(right\-click\) menu for your application, and then select **Publish to AWS Elastic Beanstalk**\.  
![\[Visual Studio screen shot of the context menu of your application. The menu displays Publish to AWS Elastic Beanstalk as an option.\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-vs-linux-solution-explorer-menu.png)

1. In the **Publish to AWS Elastic Beanstalk** wizard, enter your account information\.

   1. For **Account profile to use**, select your **default** account or choose the **Add another account** icon to enter new account information\.

   1. For **Region**, select the Region where you want to deploy your application\. For information about available AWS Regions, see [AWS Elastic Beanstalk Endpoints and Quotas](https://docs.aws.amazon.com/general/latest/gr/elasticbeanstalk.html) in the *AWS General Reference*\. If you select a Region that is not supported by Elastic Beanstalk, then the option to deploy to Elastic Beanstalk is unavailable\.

   1. Select **Create a new application environment**, then choose **Next**\.  
![\[Visual Studio screen shot of the Publish to AWS Elastic Beanstalk dialog box\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-vs-linux-publish-to-aeb-dialogue.png)

1. On the **Application Environment** dialog box, enter the details for your new application environment\.

1. On the next **AWS** options dialog box, set Amazon EC2 options and other AWS related options for your deployed application\.

   1. For **Container type** select **64bit Amazon Linux 2 v*<n\.n\.n>* running \.NET Core\.** 
**Note**  
We recommend you select the current platform version of Linux\. This version contains the most recent security and bug fixes that are included in our latest Amazon Machine Image \(AMI\)\. 

   1. For **Instance Type**, select **t2\.micro**\. \(Choosing a micro instance type minimizes the cost associated with running the instance\.\)

   1. For **Key pair**, select **Create new key pair**\. Enter a name for the new key pair, and then choose **OK**\. \(In this example, we use **myuseastkeypair**\.\) A key pair enables remote\-desktop access to your Amazon EC2 instances\. For more information about Amazon EC2 key pairs, see [Using Credentials](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-credentials.html) in the *Amazon Elastic Compute Cloud User Guide*\.

   1. For a simple, low traffic application, select **Single instance environment**\. For more information, see [Environment types](using-features-managing-env-types.md)

   1. Select **Next**\.  
![\[Visual Studio screen shot of the Publish to Amazon Web Services dialog box.\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-vs-linux-application-options.png)

    For more information about the AWS options that are not used in this example consider the following pages: 
   + For** Use custom AMI**, see [Using a custom Amazon machine image \(AMI\)](using-features.customenv.md)\.
   + If you don't select **Single instance environment**, you need to choose a **Load balance type**\. See [Load balancer for your Elastic Beanstalk environment](using-features.managing.elb.md) for more information\. 
   + Elastic Beanstalk uses the default [Amazon VPC](https://docs.aws.amazon.com/vpc/latest/userguide/) \(Amazon Virtual Private Cloud\) configuration if you didn't choose **Use non\-default VPC**\. For more information see [Using Elastic Beanstalk with Amazon VPC](vpc.md)\.
   + Choosing the **Enable Rolling Deployments** option splits a deployment into batches to avoid potential downtime during deployments\. For more information, see [Deploying applications to Elastic Beanstalk environments](using-features.deploy-existing-version.md)\.
   + Choosing the **Relational Database Access** option connects your Elastic Beanstalk environment to a previously created Amazon RDS database with *Amazon RDS DB Security Groups*\. For more information, see [Controlling Access with Security Groups](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Overview.RDSSecurityGroups.html) in the *Amazon RDS User Guide*\.

1. Select **Next** on the **Permissions** dialog box\.

1. Select **Next** on the **Applications Options** dialog box\.

1. Review your deployment options\. After you've verified your settings are correct, select **Deploy**\.

Your ASP\.NET Core web application is exported as a web deploy file\. This file is then uploaded to Amazon S3 and registered as a new application version with Elastic Beanstalk\. The Elastic Beanstalk deployment feature monitors your environment until it is available with the newly deployed code\. The **Status** for your environment is displayed on the Env:<environment name> tab\. After the status updates to **Environment is healthy**, you can select the URL address to launch the web application\.

![\[Visual Studio screen shot of the application status event details in the environment tab.\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-vs-linux-application-tab-event-status.png)

## Terminating an environment<a name="dotnet-toolkit-linux-core-tutorial-terminate-env"></a>

 To avoid incurring charges for unused AWS resources, you can use the AWS Toolkit for Visual Studio to terminate a running environment\.

**Note**  
You can always launch a new environment using the same version later\. 

**To terminate an environment**

1. Expand the Elastic Beanstalk node and the application node\. In **AWS Explorer** open the context \(right\-click\) menu for your application environment and select **Terminate Environment**\.

1. When prompted, select **Yes** to confirm that you want to terminate the environment\. It takes a few minutes for Elastic Beanstalk to terminate the AWS resources running in the environment\. 

The **Status** for your environment on the Env:<environment name> tab changes to **Terminating** and is eventually **Terminated**\.

![\[Visual Studio screen shot of the Status and other attributes in environment tab.\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-vs-linux-terminating-status.png)

**Note**  
 When you terminate your environment, the CNAME associated with the terminated environment becomes available for anyone to use\. 