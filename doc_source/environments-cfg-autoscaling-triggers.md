# Auto Scaling Triggers<a name="environments-cfg-autoscaling-triggers"></a>

The Auto Scaling group in your Elastic Beanstalk environment uses two Amazon CloudWatch alarms to trigger scaling operations\. The default triggers scale when the average outbound network traffic from each instance is higher than 6 MiB or lower than 2 MiB over a period of five minutes\. To use Amazon EC2 Auto Scaling effectively, configure triggers that are appropriate for your application, instance type, and service requirements\. You can scale based on several statistics including latency, disk I/O, CPU utilization, and request count\.

## Configuring Auto Scaling Triggers<a name="environments-cfg-autoscaling-triggers-console"></a>

You can configure the triggers that adjust the number of instances in your environment's Auto Scaling group in the Elastic Beanstalk console\.

**To configure triggers in the Elastic Beanstalk console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. Choose **Configuration**\.

1. On the **Capacity** configuration card, choose **Modify**\.

1. In the **Scaling triggers** section, configure the following settings:
   + **Metric** – Metric used for your Auto Scaling trigger\.
   + **Statistic** – Statistic calculation the trigger should use, such as `Average`\.
   + **Unit** – Unit for the trigger metric, such as **Bytes**\.
   + **Period** – Specifies how frequently Amazon CloudWatch measures the metrics for your trigger\.
   + **Breach duration** – Amount of time, in minutes, a metric can be outside of the upper and lower thresholds before triggering a scaling operation\.
   + **Upper threshold** – If the metric exceeds this number for the breach duration, a scaling operation is triggered\. 
   + **Scale up increment** – The number of Amazon EC2 instances to add when performing a scaling activity\.
   + **Lower threshold** – If the metric falls below this number for the breach duration, a scaling operation is triggered\. 
   + **Scale down increment** – The number of Amazon EC2 instances to remove when performing a scaling activity\.  
![\[Elastic Beanstalk Auto Scaling triggers configuration page\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environment-cfg-autoscaling-triggers.png)

1. Choose **Apply**\.

## The aws:autoscaling:trigger Namespace<a name="environments-cfg-autoscaling-triggers-namespace"></a>

Elastic Beanstalk provides [configuration options](command-options.md) for Auto Scaling settings in the [`aws:autoscaling:trigger`](command-options-general.md#command-options-general-autoscalingtrigger) namespace\. Settings in this namespace are organized by the resource that they apply to\.

```
option_settings:
  AWSEBAutoScalingScaleDownPolicy.aws:autoscaling:trigger:
    LowerBreachScaleIncrement: '-1'
  AWSEBAutoScalingScaleUpPolicy.aws:autoscaling:trigger:
    UpperBreachScaleIncrement: '1'
  AWSEBCloudwatchAlarmHigh.aws:autoscaling:trigger:
    UpperThreshold: '6000000'
  AWSEBCloudwatchAlarmLow.aws:autoscaling:trigger:
    BreachDuration: '5'
    EvaluationPeriods: '1'
    LowerThreshold: '2000000'
    MeasureName: NetworkOut
    Period: '5'
    Statistic: Average
    Unit: Bytes
```