# Auto Scaling Group for Your AWS Elastic Beanstalk Environment<a name="using-features.managing.as"></a>

Your Elastic Beanstalk includes an Auto Scaling group that manages the [Amazon EC2 instances](using-features.managing.ec2.md) in your environment\. In a single\-instance environment, the Auto Scaling group ensures that there is always one instance running\. In a load\-balanced environment, you configure the group with a range of instances to run, and Amazon EC2 Auto Scaling adds or removes instances as needed, based on load\.

The Auto Scaling group also manages the launch configuration for the instances in your environment\. You can [modify the launch configuration](using-features.managing.ec2.md) to change the instance type, key pair, Amazon Elastic Block Store \(Amazon EBS\) storage, and other settings that can only be configured when you launch an instance\.

The Auto Scaling group uses two Amazon CloudWatch alarms to trigger scaling operations\. The default triggers scale when the average outbound network traffic from each instance is higher than 6 MiB or lower than 2 MiB over a period of five minutes\. To use Amazon EC2 Auto Scaling effectively, [configure triggers](environments-cfg-autoscaling-triggers.md) that are appropriate for your application, instance type, and service requirements\. You can scale based on several statistics including latency, disk I/O, CPU utilization, and request count\.

To optimize your environment's use of Amazon EC2 instances through predictable periods of peak traffic, [configure your Auto Scaling group to change its instance count on a schedule](environments-cfg-autoscaling-scheduledactions.md)\. You can schedule changes to your group's configuration that recur daily or weekly, or schedule one\-time changes to prepare for marketing events that will drive a lot of traffic to your site\.

Amazon EC2 Auto Scaling monitors the health of each Amazon EC2 instance that it launches\. If any instance terminates unexpectedly, Amazon EC2 Auto Scaling detects the termination and launches a replacement instance\. To configure the group to use the load balancer's health check mechanism, see [Auto Scaling Health Check Setting](environmentconfig-autoscaling-healthchecktype.md)\.

**Topics**
+ [Configuring Your Environment's Auto Scaling Group](#environments-cfg-autoscaling-console)
+ [The aws:autoscaling:asg Namespace](#environments-cfg-autoscaling-namespace)
+ [Auto Scaling Triggers](environments-cfg-autoscaling-triggers.md)
+ [Scheduled Auto Scaling Actions](environments-cfg-autoscaling-scheduledactions.md)
+ [Auto Scaling Health Check Setting](environmentconfig-autoscaling-healthchecktype.md)

## Configuring Your Environment's Auto Scaling Group<a name="environments-cfg-autoscaling-console"></a>

You can configure how Amazon EC2 Auto Scaling works by editing **Capacity** on the environment's **Configuration** page in the [environment management console](environments-console.md)\.

**To configure scheduled actions in the Elastic Beanstalk console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. Choose **Configuration**\.

1. In the **Capacity** configuration category, choose **Modify**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-env-config-capacity.png)

1. In the **Auto Scaling Group** section, configure the following settings\.
   + **Environment type** – Select **Load balanced**\.
   + **Min instances** – The minimum number of EC2 instances that the group should contain at any time\. The group starts with the minimum count and adds instances when the scale\-up trigger condition is met\.
   + **Max instances** – The maximum number of EC2 instances that the group should contain at any time\.
**Note**  
If you use rolling updates, be sure that the maximum instance count is higher than the [**Minimum instances in service** setting](using-features.rollingupdates.md#rollingupdates-configure) for rolling updates\.
   + **Availability Zones** – Choose the number of Availability Zones to spread your environment's instances across\. By default, the Auto Scaling group launches instances evenly across all usable zones\. To concentrate your instances in fewer zones, choose the number of zones to use\. For production environments, use at least two zones to ensure that your application is available in case one Availability Zone goes out\.
   + **Placement** \(optional\) – Choose the Availability Zones to use\. Use this setting if your instances need to connect to resources in specific zones, or if you have purchased [reserved instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts-on-demand-reserved-instances.html), which are zone\-specific\. If you also set the number of zones, you must choose at least that many custom zones\.

     If you launch your environment in a custom VPC, you cannot configure this option\. In a custom VPC, you choose Availability Zones for the subnets that you assign to your environment\.
   + **Scaling cooldown** – The amount of time, in seconds, to wait for instances to launch or terminate after scaling, before continuing to evaluate triggers\. For more information, see [Scaling Cooldowns](https://docs.aws.amazon.com/autoscaling/ec2/userguide/Cooldown.html)\.  
![\[Elastic Beanstalk Auto Scaling Configuration Window\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environment-cfg-autoscaling.png)

1. Choose **Apply**\.

## The aws:autoscaling:asg Namespace<a name="environments-cfg-autoscaling-namespace"></a>

Elastic Beanstalk provides [configuration options](command-options.md) for Auto Scaling settings in the [`aws:autoscaling:asg`](command-options-general.md#command-options-general-autoscalingasg) namespace\.

```
option_settings:
  aws:autoscaling:asg:
    Availability Zones: Any
    Cooldown: '720'
    Custom Availability Zones: 'us-west-2a,us-west-2b'
    MaxSize: '4'
    MinSize: '2'
```