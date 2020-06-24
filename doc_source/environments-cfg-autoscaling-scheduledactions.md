# Scheduled Auto Scaling actions<a name="environments-cfg-autoscaling-scheduledactions"></a>

To optimize your environment's use of Amazon EC2 instances through predictable periods of peak traffic, configure your Amazon EC2 Auto Scaling group to change its instance count on a schedule\. You can configure your environment with a recurring action to scale up each day in the morning, and scale down at night when traffic is low\. For example, if you have a marketing event that will drive traffic to your site for a limited period of time, you can schedule a one\-time event to scale up when it starts, and another to scale down when it ends\.

You can define up to 120 active scheduled actions per environment\. Elastic Beanstalk also retains up to 150 expired scheduled actions, which you can reuse by updating their settings\.

## Configuring scheduled actions<a name="environments-cfg-autoscaling-scheduledactions-console"></a>

You can create scheduled actions for your environment's Auto Scaling group in the Elastic Beanstalk console\.

**To configure scheduled actions in the Elastic Beanstalk console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Configuration**\.

1. In the **Capacity** configuration category, choose **Edit**\.

1. In the **Time\-based scaling** section, choose **Add scheduled action**\.  
![\[Elastic Beanstalk Auto Scaling scheduled actions configuration window\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environment-cfg-autoscaling-scheduledactions.png)

1. Fill in the following scheduled action settings:
   + **Name** – Specify a unique name of up to 255 alphanumeric characters, with no spaces\.
   + **Instances** – Choose the minimum and maximum instance count to apply to the Auto Scaling group\.
   + **Desired capacity** \(optional\) – Set the initial desired capacity for the Auto Scaling group\. After the scheduled action is applied, triggers adjust the desired capacity based on their settings\.
   + **Occurrence** – Choose **Recurring** to repeat the scaling action on a schedule\.
   + **Start time** – For one\-time actions, choose the date and time to run the action\.

     For recurrent actions, a start time is optional\. Specify it to choose the earliest time the action is performed\. After this time, the action recurs according to the **Recurrence** expression\.
   + **Recurrence** – Use a [Cron](http://en.wikipedia.org/wiki/Cron#CRON_expression) expression to specify the frequency with which you want the scheduled action to occur\. For example, `30 6 * * 2` runs the action every Tuesday at 6:30 AM UTC\.
   + **End time** \(optional\) – Optional for recurrent actions\. If specified, the action recurs according to the **Recurrence** expression, and is not performed again after this time\.

     When a scheduled action ends, Auto Scaling doesn't automatically go back to its previous settings\. Configure a second scheduled action to return Auto Scaling to the original settings as needed\.

1. Choose **Add**\. 

1. Choose **Apply**\.
**Note**  
Scheduled actions will not be saved until applied\.

## The aws:autoscaling:scheduledaction namespace<a name="environments-cfg-autoscaling-scheduledactions-namespace"></a>

If you need to configure a large number of scheduled actions, you can use [configuration files](ebextensions.md) or [the Elastic Beanstalk API](environment-configuration-methods-after.md#configuration-options-after-awscli-commandline) to apply the configuration option changes from a YAML or JSON file\. These methods also let you access the [`Suspend` option](command-options-general.md#command-options-general-autoscalingscheduledaction) to temporarily deactivate a recurrent scheduled action\.

**Note**  
When working with scheduled action configuration options outside of the console, use ISO 8601 time format to specify start and end times in UTC\. For example, 2015\-04\-28T04:07:02Z\. For more information about ISO 8601 time format, see [Date and Time Formats](http://www.w3.org/TR/NOTE-datetime)\. The dates must be unique across all scheduled actions\.

Elastic Beanstalk provides configuration options for scheduled action settings in the [`aws:autoscaling:scheduledaction`](command-options-general.md#command-options-general-autoscalingscheduledaction) namespace\. Use the `resource_name` field to specify the name of the scheduled action\.

**Example Scheduled\-scale\-up\-specific\-time\-long\.config**  
This configuration file instructs Elastic Beanstalk to scale out from five instances to 10 instances at 2015\-12\-12T00:00:00Z\.  

```
option_settings:
  - namespace: aws:autoscaling:scheduledaction
    resource_name: ScheduledScaleUpSpecificTime
    option_name: MinSize
    value: '5'
  - namespace: aws:autoscaling:scheduledaction
    resource_name: ScheduledScaleUpSpecificTime
    option_name: MaxSize
    value: '10'
  - namespace: aws:autoscaling:scheduledaction
    resource_name: ScheduledScaleUpSpecificTime
    option_name: DesiredCapacity
    value: '5'
  - namespace: aws:autoscaling:scheduledaction
    resource_name: ScheduledScaleUpSpecificTime
    option_name: StartTime
    value: '2015-12-12T00:00:00Z'
```

**Example Scheduled\-scale\-up\-specific\-time\.config**  
To use the shorthand syntax with the EB CLI or configuration files, prepend the resource name to the namespace\.  

```
option_settings:
  ScheduledScaleUpSpecificTime.aws:autoscaling:scheduledaction:
    MinSize: '5'
    MaxSize: '10'
    DesiredCapacity: '5'
    StartTime: '2015-12-12T00:00:00Z'
```

**Example Scheduled\-scale\-down\-specific\-time\.config**  
This configuration file instructs Elastic Beanstalk to scale in at 2015\-12\-12T07:00:00Z\.  

```
option_settings:
  ScheduledScaleDownSpecificTime.aws:autoscaling:scheduledaction:
    MinSize: '1'
    MaxSize: '1'
    DesiredCapacity: '1'
    StartTime: '2015-12-12T07:00:00Z'
```

**Example Scheduled\-periodic\-scale\-up\.config**  
This configuration file instructs Elastic Beanstalk to scale out every day at 9AM\. The action is scheduled to begin May 14, 2015 and end January 12, 2016\.  

```
option_settings:
  ScheduledPeriodicScaleUp.aws:autoscaling:scheduledaction:
    MinSize: '5'
    MaxSize: '10'
    DesiredCapacity: '5'
    StartTime: '2015-05-14T07:00:00Z'
    EndTime: '2016-01-12T07:00:00Z'
    Recurrence: 0 9 * * *
```

**Example Scheduled\-periodic\-scale\-down\.config**  
This configuration file instructs Elastic Beanstalk to scale in to no running instance every day at 6PM\. If you know that your application is mostly idle outside of business hours, you can create a similar scheduled action\. If your application must be down outside of business hours, change `MaxSize` to `0`\.  

```
option_settings:
  ScheduledPeriodicScaleDown.aws:autoscaling:scheduledaction:
    MinSize: '0'
    MaxSize: '1'
    DesiredCapacity: '0'
    StartTime: '2015-05-14T07:00:00Z'
    EndTime: '2016-01-12T07:00:00Z'
    Recurrence: 0 18 * * *
```

**Example Scheduled\-weekend\-scale\-down\.config**  
This configuration file instructs Elastic Beanstalk to scale in every Friday at 6PM\. If you know that your application doesn’t receive as much traffic over the weekend, you can create a similar scheduled action\.  

```
option_settings:
  ScheduledWeekendScaleDown.aws:autoscaling:scheduledaction:
    MinSize: '1'
    MaxSize: '4'
    DesiredCapacity: '1'
    StartTime: '2015-12-12T07:00:00Z'
    EndTime: '2016-01-12T07:00:00Z'
    Recurrence: 0 18 * * 5
```