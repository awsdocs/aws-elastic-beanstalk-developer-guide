# Tutorial: How to Deploy a \.NET Sample Application Using AWS Elastic Beanstalk<a name="create_deploy_NET.quickstart"></a>

In this tutorial, you will learn how to deploy a \.NET sample application to AWS Elastic Beanstalk using the AWS Toolkit for Visual Studio\.

**Note**  
This tutorial uses a sample ASP\.NET Web application that you can download [here](samples/dotnet-aspmvc5-v1.zip)\. It also uses the [Toolkit for Visual Studio](https://aws.amazon.com/visualstudio/) and was tested using Visual Studio Professional 2012\.

## Create the Environment<a name="aws-elastic-beanstalk-tutorial-step-1-create-environment"></a>

First, use the Create New Application wizard in the Elastic Beanstalk console to create the application environment\.

**To create the environment**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.
**Note**  
If the New Environment wizard does not show the screens described below, see \.

1. Choose **Create New Application**\.  
![\[Elastic Beanstalk .NET tutorial create new application wizard\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/dot-net-tutorial-create-new-application.png)

1. On the **Application Information** page, enter an **Application name**, and then choose **Next**\.  
![\[Elastic Beanstalk .NET tutorial enter application name\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/dot-net-tutorial-enter-application-name.png)

1. On the **New Environment** page, choose **Create web server**\.  
![\[Elastic Beanstalk .NET tutorial create web server\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/dot-net-tutorial-create-web-server.png)

1. On the **Environment Type** page, for **Predefined configuration**, choose **IIS**\.  
![\[Elastic Beanstalk .NET tutorial environment type IIS\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/dot-net-tutorial-environment-type-iis.png)

1. For **Environment type**, accept the default, **Load balancing, auto scaling**, and then choose **Next**\.  
![\[Elastic Beanstalk .NET tutorial environment type load balancing auto scaling\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/dot-net-tutorial-environment-type-load-balancing-auto-scaling.png)

1. On the **Application Version** page, for **Source**, choose **Sample application**, and then choose **Next**\.  
![\[Elastic Beanstalk .NET tutorial application version sample application\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/dot-net-tutorial-application-version.png)

1. On the **Environment Information** page, accept all defaults, and then choose **Next**\.  
![\[Elastic Beanstalk .NET tutorial environment information\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/dot-net-tutorial-environment-information.png)

1. On the **Additional Resources** page, choose **Create an RDS DB instance with this environment**, and then choose **Next**\.  
![\[Elastic Beanstalk .NET tutorial create RDS database instance\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/dot-net-tutorial-create-rds-db-instance.png)

1. On the **Configuration Details** page, for **Instance type**, choose **t2\.micro**\.

   Accept the default values for the other fields, and then choose **Next**\.  
![\[Elastic Beanstalk .NET tutorial configuration details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/dot-net-tutorial-configuration-details.png)

1. On the **Environment Tags** page, leave both the **Key** and the **Value** fields blank, and then choose **Next**\.  
![\[Elastic Beanstalk .NET tutorial Environment Tags\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/dot-net-tutorial-environment-tags.png)

1. Choose **Next** on the **Permissions** page\. If you don't have a default instance profile and service role, Elastic Beanstalk creates them for you\.

1. On the **RDS Configuration** page, for **DB engine**, choose **sqlserver\-ex**\. For **Instance class**, choose **db\.t2\.micro**, and then increase the **Allocated storage** to **20 GB**\.  
![\[Elastic Beanstalk .NET tutorial RDS configuration\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/dot-net-tutorial-rds-configuration.png)

1. Create a **Username** and **Password**, and then choose **Next**\.

1. On the **Review Information** page, review the settings, and then choose **Launch**\.

   To check launch status, see the **Dashboard** page in the Elastic Beanstalk console\.  
![\[Elastic Beanstalk .NET tutorial Launching environment status page\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/dot-net-tutorial-launching-environment.png)

## Publish Your Application to Elastic Beanstalk<a name="aws-elastic-beanstalk-tutorial-step-2-publish-application"></a>

Use the AWS Toolkit for Visual Studio to publish your application to Elastic Beanstalk\.

**To publish your application to Elastic Beanstalk**

1. Ensure that your environment launched successfully by checking the **Health** status in the Elastic Beanstalk console\. It should be **Green**\.  
![\[Elastic Beanstalk .NET tutorial environment health Green\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/dot-net-tutorial-environment-health-green.png)

1. In Visual Studio, open **BeanstalkDotNetSample\.sln**\.
**Note**  
If you haven't done so already, you can get the sample [here](samples/dotnet-aspmvc5-v1.zip)\.

1. On the **View** menu, choose **Solution Explorer**\.

1. Expand **Solution ‘BeanstalkDotNetSample’ \(2 projects\)**\.

1. Open the context \(right\-click\) menu for **MVC5App**, and then choose **Publish to AWS**\.  
![\[Elastic Beanstalk .NET tutorial Solution Explorer Publish to AWS\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/dot-net-tutorial-visual-studio-solution-explorer.png)

1. On the **Publish to AWS Elastic Beanstalk** page, for **Deployment Target**, choose the environment that you just created, and then choose **Next**\.  
![\[Elastic Beanstalk .NET tutorial Publish to AWS Elastic Beanstalk Deployment Target\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/dot-net-tutorial-visual-studio-publish-to-aws-elastic-beanstalk.png)

1. On the **Application Options** page, accept all of the defaults, and then choose **Next**\.  
![\[Elastic Beanstalk .NET tutorial Publish to AWS Elastic Beanstalk Application Options\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/dot-net-tutorial-visual-studio-application-options.png)

1. On the **Review** page, choose **Deploy**\.  
![\[Elastic Beanstalk .NET tutorial Review and Deploy\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/dot-net-tutorial-visual-studio-review-and-deploy.png)

1. If you want to monitor deployment status, use the **NuGet Package Manager** in Visual Studio\.  
![\[Elastic Beanstalk .NET tutorial monitor status NuGet Package Manager\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/dot-net-tutorial-visual-studio-nuget-package-manager.png)

   When the application has successfully been deployed, the **Output** box displays **completed successfully**\.  
![\[Elastic Beanstalk .NET tutorial output completed successfully\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/dot-net-tutorial-visual-studio-output-completed-successfully.png)

1. Return to the Elastic Beanstalk console and choose the name of the application, which appears next to the environment name\.  
![\[Elastic Beanstalk .NET tutorial launch sample app from console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/dot-net-tutorial-launch-sample-app-from-console.png)

   Your ASP\.NET application opens in a new tab\.  
![\[Elastic Beanstalk .NET tutorial see your ASP.NET application running in the Web
                  browser\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/dot-net-tutorial-my-asp-net-application.png)

## Clean Up Your AWS Resources<a name="aws-elastic-beanstalk-tutorial-step-3-clean-up-your-resources"></a>

After your application has deployed successfully, learn more about Elastic Beanstalk by [watching the video](http://bit.ly/1sSvFzg) in the application\.

If you are done working with Elastic Beanstalk for now, you can terminate your \.NET environment\.

**To terminate your Elastic Beanstalk environment**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the management page for your environment\.

1. Choose **Actions** and then choose **Terminate Environment**\.

Elastic Beanstalk cleans up all AWS resources associated with your environment, including EC2 instances, DB instance, load balancer, security groups, CloudWatch alarms, etc\.

For more information, see [Creating and Deploying Elastic Beanstalk Applications in \.NET Using AWS Toolkit for Visual Studio](create_deploy_NET.md), the [ AWS \.NET Development Blog ](http://aws.amazon.com/blogs/developer/category/net/), or the [AWS Application Management Blog](http://aws.amazon.com/blogs/devops/)\.