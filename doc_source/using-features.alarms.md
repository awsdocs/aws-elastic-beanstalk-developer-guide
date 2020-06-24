# Manage alarms<a name="using-features.alarms"></a>

You can create alarms for metrics that you are monitoring by using the Elastic Beanstalk console\. Alarms help you monitor changes to your AWS Elastic Beanstalk environment so that you can easily identify and mitigate problems before they occur\. For example, you can set an alarm that notifies you when CPU utilization in an environment exceeds a certain threshold, ensuring that you are notified before a potential problem occurs\. For more information, see [Using Elastic Beanstalk with Amazon CloudWatch](AWSHowTo.cloudwatch.md)\.

**Note**  
Elastic Beanstalk uses CloudWatch for monitoring and alarms, meaning CloudWatch costs are applied to your AWS account for any alarms that you use\.

For more information about monitoring specific metrics, see [Basic health reporting](using-features.healthstatus.md)\.

**To check the state of your alarms**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Alarms**\.  
![\[Environment alarms page of the Elastic Beanstalk console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-env-alarms.png)

   The page displays a list of existing alarms\. If any alarms are in the alarm state, they are flagged with ![\[warning icon\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/warning.png) \(warning\)\.

1. To filter alarms, choose the drop\-down menu, and then select a filter\.

1. To edit or delete an alarm, choose ![\[cog icon\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/cog.png) \(edit\) or ![\[X icon\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/x.png) \(delete\), respectively\.

**To create an alarm**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Monitoring**\.

1. Locate the metric for which you want to create an alarm, and then choose ![\[bell icon\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/bell.png) \(alarm\)\. The **Add alarm** page is displayed\.  
![\[Add alarm dialog of the Elastic Beanstalk console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-env-alarm-create.png)

1. Enter details about the alarm:
   + **Name**: A name for this alarm\.
   + **Description** \(optional\): A short description of what this alarm is\.
   + **Period**: The time interval between readings\.
   + **Threshold**: Describes the behavior and value that the metric must exceed in order to trigger an alarm\.
   + **Change state after**: The amount a time after a threshold has been exceed that triggers a change in state of the alarm\.
   + **Notify**: The Amazon SNS topic that is notified when an alarm changes state\.
   + **Notify when state changes to**:
     + **OK**: The metric is within the defined threshold\.
     + **Alarm**: The metric exceeded the defined threshold\.
     + **Insufficient data**: The alarm has just started, the metric is not available, or not enough data is available for the metric to determine the alarm state\. 

1. Choose **Add**\. The environment status changes to gray while the environment updates\. You can view the alarm that you created by choosing **Alarms** in the navigation pane\.