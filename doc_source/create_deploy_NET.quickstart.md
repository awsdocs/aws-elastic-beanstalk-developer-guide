# Tutorial: How to deploy a \.NET sample application using Elastic Beanstalk<a name="create_deploy_NET.quickstart"></a>

In this tutorial, you will learn how to deploy a \.NET sample application to AWS Elastic Beanstalk using the AWS Toolkit for Visual Studio\.

**Note**  
This tutorial uses a sample ASP\.NET Web application that you can download [here](samples/dotnet-aspmvc5-v1.zip)\. It also uses the [Toolkit for Visual Studio](https://aws.amazon.com/visualstudio/) and was tested using Visual Studio Professional 2012\.

## Create the environment<a name="aws-elastic-beanstalk-tutorial-step-1-create-environment"></a>

First, use the Create New Application wizard in the Elastic Beanstalk console to create the application environment\. For **Platform**, choose **\.NET**\.

**To launch an environment \(console\)**

1. Open the Elastic Beanstalk console using this preconfigured link: [console\.aws\.amazon\.com/elasticbeanstalk/home\#/newApplication?applicationName=tutorials&environmentType=LoadBalanced](https://console.aws.amazon.com/elasticbeanstalk/home#/newApplication?applicationName=tutorials&environmentType=LoadBalanced)

1. For **Platform**, select the platform and platform branch that match the language used by your application\.

1. For **Application code**, choose **Sample application**\.

1. Choose **Review and launch**\.

1. Review the available options\. Choose the available option you want to use, and when you're ready, choose **Create app**\.

When the environment is up and running, add an Amazon RDS database instance that the application uses to store data\. For **DB engine**, choose **sqlserver\-ex**\.

**To add a DB instance to your environment**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Configuration**\.

1. In the **Database** configuration category, choose **Edit**\.

1. Choose a DB engine, and enter a user name and password\.

1. Choose **Apply**\.

## Publish your application to Elastic Beanstalk<a name="aws-elastic-beanstalk-tutorial-step-2-publish-application"></a>

Use the AWS Toolkit for Visual Studio to publish your application to Elastic Beanstalk\.

**To publish your application to Elastic Beanstalk**

1. Ensure that your environment launched successfully by checking the **Health** status in the Elastic Beanstalk console\. It should be **Ok** \(green\)\.

1. In Visual Studio, open **BeanstalkDotNetSample\.sln**\.
**Note**  
If you haven't done so already, you can get the sample [here](samples/dotnet-aspmvc5-v1.zip)\.

1. On the **View** menu, choose **Solution Explorer**\.

1. Expand **Solution ‘BeanstalkDotNetSample’ \(2 projects\)**\.

1. Open the context \(right\-click\) menu for **MVC5App**, and then choose **Publish to AWS**\.  
![\[Elastic Beanstalk .NET tutorial Solution Explorer publish to AWS\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/dot-net-tutorial-visual-studio-solution-explorer.png)

1. On the **Publish to AWS Elastic Beanstalk** page, for **Deployment Target**, choose the environment that you just created, and then choose **Next**\.  
![\[Elastic Beanstalk .NET tutorial publish to AWS Elastic Beanstalk deployment target\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/dot-net-tutorial-visual-studio-publish-to-aws-elastic-beanstalk.png)

1. On the **Application Options** page, accept all of the defaults, and then choose **Next**\.  
![\[Elastic Beanstalk .NET tutorial publish to AWS Elastic Beanstalk application options\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/dot-net-tutorial-visual-studio-application-options.png)

1. On the **Review** page, choose **Deploy**\.  
![\[Elastic Beanstalk .NET tutorial review and deploy\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/dot-net-tutorial-visual-studio-review-and-deploy.png)

1. If you want to monitor deployment status, use the **NuGet Package Manager** in Visual Studio\.  
![\[Elastic Beanstalk .NET tutorial monitor status NuGet package manager\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/dot-net-tutorial-visual-studio-nuget-package-manager.png)

   When the application has successfully been deployed, the **Output** box displays **completed successfully**\.  
![\[Elastic Beanstalk .NET tutorial output completed successfully\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/dot-net-tutorial-visual-studio-output-completed-successfully.png)

1. Return to the Elastic Beanstalk console\. In the navigation pane, choose **Go to environment**\.

   Your ASP\.NET application opens in a new tab\.  
![\[Elastic Beanstalk .NET tutorial see your ASP.NET application running in the web browser\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/dot-net-tutorial-my-asp-net-application.png)

## Clean up your AWS resources<a name="aws-elastic-beanstalk-tutorial-step-3-clean-up-your-resources"></a>

After your application has deployed successfully, learn more about Elastic Beanstalk by [watching the video](http://bit.ly/1sSvFzg) in the application\.

If you are done working with Elastic Beanstalk for now, you can terminate your \.NET environment\.

**To terminate your Elastic Beanstalk environment**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. Choose **Environment actions** and then choose **Terminate environment**\.

Elastic Beanstalk cleans up all AWS resources associated with your environment, including EC2 instances, DB instance, load balancer, security groups, CloudWatch alarms, etc\.

For more information, see [Creating and deploying \.NET applications on Elastic Beanstalk](create_deploy_NET.md), the [ AWS \.NET Development Blog ](http://aws.amazon.com/blogs/developer/category/net/), or the [AWS Application Management Blog](http://aws.amazon.com/blogs/devops/)\.