# Monitoring environment health in the AWS management console<a name="environment-health-console"></a>

You can access operational information about your application from the Elastic Beanstalk console\. The console displays your environment's status and application health at a glance\. In the console's **Environments** page and in each application's page, the environments on the list are color\-coded to indicate status\.

**To monitor an environment in the Elastic Beanstalk console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Monitoring**\.

The Monitoring page shows you overall statistics about your environment, such as CPU utilization and average latency\. In addition to the overall statistics, you can view monitoring graphs that show resource usage over time\. You can click any of the graphs to view more detailed information\.

**Note**  
By default, only basic CloudWatch metrics are enabled, which return data in five\-minute periods\. You can enable more granular one\-minute CloudWatch metrics by editing your environment's configuration settings\. 

## Overview<a name="environment-health-console-overview"></a>

An overview of the environment's health is shown near the top of the screen\.

![\[Environment health overview section on the environment monitoring page of the Elastic Beanstalk console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/enhanced-health-overview.png)

The overview section shows a customizable summary of the activity in your environment over period of time\. Choose the **Period** drop\-down and select a length of time to view information for a period between one minute and one day\.

## Monitoring graphs<a name="environment-health-console-graphs"></a>

Below the overview are graphs that show data about overall environment health over a period of time\. Choose the **Period** drop\-down and select a length of time to set the time between each two plot points to a period between one minute and one day\. Choose the **Time Range** drop\-down and select a length of time to set the graph time axis to a period between three hours and two weeks\.

![\[Environment health monitoring section on the environment monitoring page of the Elastic Beanstalk console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/enhanced-health-monitoring.png)

## Customizing the monitoring console<a name="environment-health-console-customize"></a>

Choose **Edit** next to either monitoring section to customize the information shown\.

![\[Customizing a section on the environment monitoring page of the Elastic Beanstalk console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/health-monitoring-customize.png)

To remove any of the existing items, choose the ![\[X icon\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/x.png) in the top right corner\.

**To add an overview or graph**

1. Choose **Edit** in the **Overview** or **Monitoring** section\.

1. Select a **Resource**\. The supported resources are your environment's Auto Scaling group, Elastic Load Balancing load balancer, and the environment itself\.

1. Select a **CloudWatch metric** for the resource\. See [Publishing Amazon CloudWatch custom metrics for an environment](health-enhanced-cloudwatch.md) for a full list of supported metrics\.

1. Select a **Statistic**\. The default statistic is the average value of the selected cloudwatch metric during the time range \(overview\) or between plot points \(graph\)\.

1. Enter a **Description**\. The description is the label for the item shown in the monitoring console\.

1. Choose **Add**\.

1. Repeat the previous steps to add more items or choose **Save** to finish modifying the section\.

For more information about metrics and dimensions for each resource, see [Amazon CloudWatch Metrics, Namespaces, and Dimensions Reference](https://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/CW_Support_For_AWS.html) in the *Amazon CloudWatch User Guide*\.

[Elastic Load Balancing](https://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/elb-metricscollected.html) and [Amazon EC2](https://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/ec2-metricscollected.html) metrics are enabled for all environments\.

With [enhanced health](health-enhanced.md), the EnvironmentHealth metric is enabled and a graph is added to the monitoring console automatically\. Additional metrics become available for use in the monitoring console when you enabled them in the environment configuration\. Enhanced health also adds the [Health page](health-enhanced-console.md#health-enhanced-console-healthpage) to the management console\.

**Note**  
When you enable additional CloudWatch metrics for your environment, it takes a few minutes for them to start being reported and appear in the list of metrics that you use to add graphs and overview stats\.

See [Publishing Amazon CloudWatch custom metrics for an environment](health-enhanced-cloudwatch.md) for a list of available enhanced health metrics\.