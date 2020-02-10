# Configuring Auto Scaling using the AWS toolkit for Visual Studio<a name="create_deploy_NET.managing.as"></a>

Amazon EC2 Auto Scaling is an Amazon web service designed to automatically launch or terminate Amazon EC2 instances based on user\-defined triggers\. Users can set up *Auto Scaling groups* and associate *triggers* with these groups to automatically scale computing resources based on metrics such as bandwidth usage or CPU utilization\. Amazon EC2 Auto Scaling works with Amazon CloudWatch to retrieve metrics for the server instances running your application\.

Amazon EC2 Auto Scaling lets you take a group of Amazon EC2 instances and set various parameters to have this group automatically increase or decrease in number\. Amazon EC2 Auto Scaling can add or remove Amazon EC2 instances from that group to help you seamlessly deal with traffic changes to your application\. 

 Amazon EC2 Auto Scaling also monitors the health of each Amazon EC2 instance that it launches\. If any instance terminates unexpectedly, Amazon EC2 Auto Scaling detects the termination and launches a replacement instance\. This capability enables you to maintain a fixed, desired number of Amazon EC2 instances automatically\. 

Elastic Beanstalk provisions Amazon EC2 Auto Scaling for your application\. You can edit the Elastic Beanstalk environment's Amazon EC2 instance configuration with the **Auto Scaling** tab inside your application environment tab in the AWS Toolkit for Visual Studio\.

![\[Elastic Beanstalk Auto Scaling configuration panel\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-vs-autoscaling.png)

The following section discusses how to configure Auto Scaling parameters for your application\. 

## Launch the configuration<a name="create_deploy_NET.managing.as.launchconfig"></a>

You can edit the launch configuration to control how your Elastic Beanstalk application provisions Amazon EC2 Auto Scaling resources\.

The **Minimum Instance Count** and **Maximum Instance Count** boxes let you specify the minimum and maximum size of the Auto Scaling group that your Elastic Beanstalk application uses\.

![\[Elastic Beanstalk Auto Scaling launch config configuration window\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-vs-autoscaling-launchconfig.png)

**Note**  
To maintain a fixed number of Amazon EC2 instances, set **Minimum Instance Count** and **Maximum Instance Count** to the same value\.

The **Availability Zones** box lets you specify the number of Availability Zones you want your Amazon EC2 instances to be in\. It is important to set this number if you want to build fault\-tolerant applications\. If one Availability Zone goes down, your instances will still be running in your other Availability Zones\. 

**Note**  
Currently, it is not possible to specify which Availability Zone your instance will be in\. 

## Triggers<a name="create_deploy_NET.managing.as.trigger"></a>

A *trigger* is an Amazon EC2 Auto Scaling mechanism that you set to tell the system when you want to increase \(*scale out*\) the number of instances, and when you want to decrease \(*scale in*\) the number of instances\. You can configure triggers to *fire* on any metric published to Amazon CloudWatch, such as CPU utilization, and determine if the conditions you specified have been met\. When the upper or lower thresholds of the conditions you have specified for the metric have been breached for the specified period of time, the trigger launches a long\-running process called a *Scaling Activity*\.

You can define a scaling trigger for your Elastic Beanstalk application using AWS Toolkit for Visual Studio\.

![\[Elastic Beanstalk Auto Scaling trigger\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-vs-autoscaling-triggers.png)

Amazon EC2 Auto Scaling triggers work by watching a specific Amazon CloudWatch metric for an instance\. Triggers include CPU utilization, network traffic, and disk activity\. Use the **Trigger Measurement** setting to select a metric for your trigger\.

The following list describes the trigger parameters you can configure using the AWS Management Console\.
+ You can specify which statistic the trigger should use\. You can select **Minimum**, **Maximum**, **Sum**, or **Average** for **Trigger Statistic**\.
+ For **Unit of Measurement**, specify the unit for the trigger measurement\.
+ The value in the **Measurement Period** box specifies how frequently Amazon CloudWatch measures the metrics for your trigger\. The **Breach Duration** is the amount of time a metric can be beyond its defined limit \(as specified for the **Upper Threshold** and **Lower Threshold**\) before the trigger fires\.
+ For **Upper Breach Scale Increment** and **Lower Breach Scale Increment**, specify how many Amazon EC2 instances to add or remove when performing a scaling activity\. 

For more information on Amazon EC2 Auto Scaling, see the *Amazon EC2 Auto Scaling* section on [Amazon Elastic Compute Cloud Documentation](https://aws.amazon.com/documentation/ec2/)\.