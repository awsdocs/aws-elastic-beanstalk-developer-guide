# Monitoring Environment Health in the AWS Management Console<a name="environment-health-console"></a>

You can access operational information about your application from the AWS Management Console at [https://console\.aws\.amazon\.com/elasticbeanstalk](https://console.aws.amazon.com/elasticbeanstalk)\. 

The AWS Management Console displays your environment's status and application health at a glance\. In the Elastic Beanstalk console applications page, each environment is color\-coded to indicate an environment's status\.

**To monitor an environment in the AWS Management Console**

1. Navigate to the [Environment Management Console](environments-console.md) for your environment

1. In the left navigation, click **Monitoring**\.

   The Monitoring page shows you overall statistics about your environment, such as CPU utilization and average latency\. In addition to the overall statistics, you can view monitoring graphs that show resource usage over time\. You can click any of the graphs to view more detailed information\.
**Note**  
By default, only basic CloudWatch metrics are enabled, which return data in five\-minute periods\. You can enable more granular one\-minute CloudWatch metrics by editing your environment's configuration settings\. 

## Overview<a name="environment-health-console-overview"></a>

An overview of the environment's health is shown near the top of the screen\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/enhanced-health-overview.png)

The overview panel shows a customizable summary of the activity in your environment over the last hour\. Click the **Time Range** drop\-down and select a different length of time to view information for a time period of five minutes to one day\.

## Monitoring Graphs<a name="environment-health-console-graphs"></a>

Below the overview are graphs that show data about overall environment health over the last twelve hours\. Click the **Time Range** drop\-down and select a different length of time to view information for a time period of three hours and two weeks\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/enhanced-health-monitoring.png)

## Customizing the Monitoring Console<a name="environment-health-console-customize"></a>

Click **Edit** next to either monitoring section to customize the information shown\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/health-monitoring-customize.png)

To remove any of the existing items, click the ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/x.png) in the top right corner\.

**To add an overview or graph**

1. Click **Edit** in the **Overview** or **Monitoring** section\.

1. Select a **Resource**\. The supported resources are your environment's Auto Scaling group, Elastic Load Balancing load balancer, and the environment itself\.

1. Select a **CloudWatch metric** for the resource\. See [Publishing Amazon CloudWatch Custom Metrics for an Environment](health-enhanced-cloudwatch.md) for a full list of supported metrics\.

1. Select a **Statistic**\. The default statistic is the average value of the selected cloudwatch metric during the time range \(overview\) or between plot points \(graph\)\.

1. Enter a **Description**\. The description is the label for the item shown in the monitoring console\.

1. Click **Add**\.

1. Repeat the previous steps to add more items or click **Save** to finish modifying the panel\.

For more information about metrics and dimensions for each resource, see [Amazon CloudWatch Metrics, Namespaces, and Dimensions Reference](https://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/CW_Support_For_AWS.html) in the *Amazon CloudWatch User Guide*\.

[Elastic Load Balancing](https://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/elb-metricscollected.html) and [Amazon EC2](https://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/ec2-metricscollected.html) metrics are enabled for all environments\.

With [enhanced health](health-enhanced.md), the EnvironmentHealth metric is enabled and a graph is added to the monitoring console automatically\. Additional metrics become available for use in the monitoring console when you enabled them in the environment configuration\. Enhanced health also adds the [Health page](health-enhanced-console.md#health-enhanced-console-healthpage) to the management console\.

**Note**  
When you enable additional CloudWatch metrics for your environment, it takes a few minutes for them to start being reported and appear in the list of metrics that you use to add graphs and overview stats\.

See [Publishing Amazon CloudWatch Custom Metrics for an Environment](health-enhanced-cloudwatch.md) for a list of available enhanced health metrics\.