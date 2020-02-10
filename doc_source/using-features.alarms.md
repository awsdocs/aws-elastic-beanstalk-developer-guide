# Manage alarms<a name="using-features.alarms"></a>

You can create alarms for metrics that you are monitoring by using the Elastic Beanstalk console\. Alarms help you monitor changes to your AWS Elastic Beanstalk environment so that you can easily identify and mitigate problems before they occur\. For example, you can set an alarm that notifies you when CPU utilization in an environment exceeds a certain threshold, ensuring that you are notified before a potential problem occurs\. For more information, see [Using Elastic Beanstalk with Amazon CloudWatch](AWSHowTo.cloudwatch.md)\.

**Note**  
Elastic Beanstalk uses CloudWatch for monitoring and alarms, meaning CloudWatch costs are applied to your AWS account for any alarms that you use\.

For more information about monitoring specific metrics, see [Basic health reporting](using-features.healthstatus.md)\.

**To check the state of your alarms**

1. From the Elastic Beanstalk console applications page, click the environment name that you want to manage alarms for\.

1. From the navigation menu, click **Alarms** to see a list of alarms\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-env-nav-alarms.png)

   If any alarms is in the alarm state, they are flagged with ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/warning.png) \(warning\)\.

1. To filter alarms, click the drop\-down filter and select the filter that you want\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-env-alarm-filter.png)

1. To edit or delete an alarm, click ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/cog.png) \(edit\) or ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/x.png) \(delete\)\.

**To create an alarm**

1. From the Elastic Beanstalk console applications page, click the environment name that you want to add alarms to\.

1. From the navigation menu, click **Monitoring**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-env-nav-monitoring.png)

1. For the metric that you want to create an alarm for, click ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/bell.png)\. You are directed to the **Alarms** page\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-env-alarm-create.png)

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

1. Click **Add**\. The environment status changes to gray while the environment updates\. You can view the alarm that you created by going to the **Alarms** page\.