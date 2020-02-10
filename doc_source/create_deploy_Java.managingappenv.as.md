# Configuring Auto Scaling using AWS Toolkit for Eclipse<a name="create_deploy_Java.managingappenv.as"></a>

Amazon EC2 Auto Scaling is an Amazon web service designed to automatically launch or terminate Amazon EC2 instances based on user\-defined triggers\. Users can set up *Auto Scaling groups* and associate *triggers* with these groups to automatically scale computing resources based on metrics such as bandwidth usage or CPU utilization\. Amazon EC2 Auto Scaling works with Amazon CloudWatch to retrieve metrics for the server instances running your application\.

 Amazon EC2 Auto Scaling lets you take a group of Amazon EC2 instances and set various parameters to have this group automatically increase or decrease in number\. Amazon EC2 Auto Scaling can add or remove Amazon EC2 instances from that group to help you seamlessly deal with traffic changes to your application\. 

 Amazon EC2 Auto Scaling also monitors the health of each Amazon EC2 instance that it launches\. If any instance terminates unexpectedly, Amazon EC2 Auto Scaling detects the termination and launches a replacement instance\. This capability enables you to maintain a fixed, desired number of Amazon EC2 instances automatically\. 

Elastic Beanstalk provisions Amazon EC2 Auto Scaling for your application\. Under **Auto Scaling**, on your environment's **Configuration** tab inside the Toolkit for Eclipse, you can edit the Elastic Beanstalk environment's Auto Scaling configuration\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-eclipse-as.png)

The following sections discuss how to configure Auto Scaling parameters for your application\. 

## Launch configuration<a name="create_deploy_Java.managingappenv.as.launchconfig"></a>

You can edit the launch configuration to control how your Elastic Beanstalk application provisions Amazon EC2 Auto Scaling resources\. 

Use the **Minimum Instance Count** and **Maximum Instance Count** settings to specify the minimum and maximum size of the Auto Scaling group that your Elastic Beanstalk application uses\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-eclipse-as-instances.png)

**Note**  
To maintain a fixed number of Amazon EC2 instances, set the **Minimum Instance Count** and **Maximum Instance Count** text boxes to the same value\. 

For **Availability Zones**, specify the number of Availability Zones you want your Amazon EC2 instances to be in\. It is important to set this number if you want to build fault\-tolerant applications: If one Availability Zone goes down, your instances will still be running in your other Availability Zones\. 

**Note**  
 Currently, it is not possible to specify which Availability Zone your instance will be in\. 

## Triggers<a name="create_deploy_Java.managingappenv.as.trigger"></a>

 A *trigger* is an Amazon EC2 Auto Scaling mechanism that you set to tell the system when to increase \(*scale out*\) and decrease \(*scale in*\) the number of instances\. You can configure triggers to *fire* on any metric published to Amazon CloudWatch, such as CPU utilization, and determine whether the specified conditions have been met\. When your upper or lower thresholds for the metric have been breached for the specified period of time, the trigger launches a long\-running process called a *scaling activity*\. 

 You can define a scaling trigger for your Elastic Beanstalk application using the AWS Toolkit for Eclipse\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-eclipse-as-trigger.png)

 You can configure the following list of trigger parameters in the **Scaling Trigger** section of the **Configuration** tab for your environment inside the Toolkit for Eclipse\.
+ For **Trigger Measurement**, specify the metric for your trigger\. 
+ For **Trigger Statistic**, specify which statistic the trigger will use—**Minimum**, **Maximum**, **Sum**, or **Average**\. 
+ For **Unit of Measurement**, specify the units for the trigger measurement\. 
+ For **Measurement Period**, specify how frequently Amazon CloudWatch measures the metrics for your trigger\. For **Breach Duration**, specify the amount of time a metric can be beyond its defined limit \(as specified for **Upper Threshold** and **Lower Threshold**\) before the trigger fires\.
+ For **Scale\-up Increment** and **Scale\-down Increment**, specify how many Amazon EC2 instances to add or remove when performing a scaling activity\. 

For more information on Amazon EC2 Auto Scaling, see the *Amazon EC2 Auto Scaling* section on [Amazon Elastic Compute Cloud Documentation](https://aws.amazon.com/documentation/ec2/)\.