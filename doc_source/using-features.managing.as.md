# Auto Scaling group for your Elastic Beanstalk environment<a name="using-features.managing.as"></a>

Your AWS Elastic Beanstalk environment includes an *Auto Scaling group* that manages the [Amazon EC2 instances](using-features.managing.ec2.md) in your environment\. In a single\-instance environment, the Auto Scaling group ensures that there is always one instance running\. In a load\-balanced environment, you configure the group with a range of instances to run, and Auto Scaling adds or removes instances as needed, based on load\.

The Auto Scaling group also applies the launch configuration for the instances in your environment\. You can [modify the launch configuration](using-features.managing.ec2.md) to change the instance type, key pair, Amazon Elastic Block Store \(Amazon EBS\) storage, and other settings that can only be configured when you launch an instance\.

The Auto Scaling group uses two Amazon CloudWatch alarms to trigger scaling operations\. The default triggers scale when the average outbound network traffic from each instance is higher than 6 MiB or lower than 2 MiB over a period of five minutes\. To use Auto Scaling effectively, [configure triggers](environments-cfg-autoscaling-triggers.md) that are appropriate for your application, instance type, and service requirements\. You can scale based on several statistics including latency, disk I/O, CPU utilization, and request count\.

To optimize your environment's use of Amazon EC2 instances through predictable periods of peak traffic, [configure your Auto Scaling group to change its instance count on a schedule](environments-cfg-autoscaling-scheduledactions.md)\. You can schedule changes to your group's configuration that recur daily or weekly, or schedule one\-time changes to prepare for marketing events that will drive a lot of traffic to your site\.

As an option, Elastic Beanstalk can combine On\-Demand and [Spot](#environments-cfg-autoscaling-spot) Instances for your environment\.

Auto Scaling monitors the health of each Amazon EC2 instance that it launches\. If any instance terminates unexpectedly, Auto Scaling detects the termination and launches a replacement instance\. To configure the group to use the load balancer's health check mechanism, see [Auto Scaling health check setting](environmentconfig-autoscaling-healthchecktype.md)\.

You can configure Auto Scaling for your environment using the [Elastic Beanstalk console](#environments-cfg-autoscaling-console), the [EB CLI](#environments-cfg-autoscaling-ebcli), or [configuration options](#environments-cfg-autoscaling-namespace)\.

**Topics**
+ [Spot instance support](#environments-cfg-autoscaling-spot)
+ [Auto Scaling group configuration using the Elastic Beanstalk console](#environments-cfg-autoscaling-console)
+ [Auto Scaling group configuration using the EB CLI](#environments-cfg-autoscaling-ebcli)
+ [Configuration options](#environments-cfg-autoscaling-namespace)
+ [Auto Scaling triggers](environments-cfg-autoscaling-triggers.md)
+ [Scheduled Auto Scaling actions](environments-cfg-autoscaling-scheduledactions.md)
+ [Auto Scaling health check setting](environmentconfig-autoscaling-healthchecktype.md)

## Spot instance support<a name="environments-cfg-autoscaling-spot"></a>

To take advantage of Amazon EC2 [Spot Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-spot-instances.html), you can enable a Spot option for your environment\. Your environment's Auto Scaling group then combines Amazon EC2 purchase options and maintains a mix of On\-Demand and Spot Instances\.

**Important**  
Demand for Spot Instances can vary significantly from moment to moment, and the availability of Spot Instances can also vary significantly depending on how many unused Amazon EC2 instances are available\. It's always possible that your Spot Instance might be interrupted\. Therefore, you must ensure that your application is prepared for a Spot Instance interruption\. For more information, see [Spot Instance Interruptions](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/spot-interruptions.html) in the *Amazon EC2 User Guide for Linux Instances*\.

Elastic Beanstalk provides several configuration options to support the Spot feature\. They're discussed in the following sections about configuring your Auto Scaling Group\. Two of these options deserve special attention: `SpotFleetOnDemandBase` and `SpotFleetOnDemandAboveBasePercentage` \(both in the `aws:ec2:instances` namespace\)\. Consider how they relate to the `MinSize` option \(in the `aws:autoscaling:asg` namespace\)\. Only `MinSize` determines your environments's initial capacity—the number of instances you want running at a minimum\. `SpotFleetOnDemandBase` doesn't affect initial capacity; when Spot is enabled, this option only determines how many On\-Demand Instances are provisioned before any Spot Instances are considered\. When `SpotFleetOnDemandBase` is less than `MinSize`, you still get exactly `MinSize` instances as initial capacity; at least `SpotFleetOnDemandBase` of them must be On\-Demand Instances\. When `SpotFleetOnDemandBase` is greater than `MinSize`, then as your environment scales out, you're guaranteed to get at least an additional `SpotFleetOnDemandBase - MinSize` Instances that are On\-Demand before satisfying the `SpotFleetOnDemandBase` requirement\.

In production environments, Spot Instances are particularly useful as part of a scalable, load\-balanced environment\. We don't recommend using Spot in a single\-instance environment\. If Spot Instances aren't available, you might lose the entire capacity \(a single instance\) of your environment\. You may still wish to use a Spot Instance in a single instance environment for development or testing\. When you do, be sure to set both `SpotFleetOnDemandBase` and `SpotFleetOnDemandAboveBasePercentage` to zero\. Any other settings result in an On\-Demand Instance\.

**Notes**  
Some older AWS accounts might provide Elastic Beanstalk with default instance types that don't support Spot Instances \(for example, t1\.micro\)\. If you enable Spot Instance requests and you see the error None of the instance types you specified supports Spot, be sure to configure instance types that support Spot\. To choose Spot Instance types, use the [Spot Instance Advisor](https://aws.amazon.com/ec2/spot/instance-advisor/)\.
Enabling Spot Instance requests requires using Amazon EC2 launch templates\. When you configure this feature during environment creation or updates, Elastic Beanstalk attempts to configure your environment to use Amazon EC2 launch templates \(if the environment isn't using them already\)\. In this case, if your user policy lacks the necessary permissions, environment creation or updates might fail\. Therefore, we recommend that you use our managed user policy or add the required permissions to your custom policies\. For details about the required permissions, see [Creating a custom user policy](AWSHowTo.iam.managed-policies.md#AWSHowTo.iam.policies)\.

The following examples demonstrate different scenarios of setting the various scaling options\. All examples assume a load\-balanced environment with Spot Instance requests enabled\.

**Example 1: On\-Demand and Spot as part of initial capacity**  <a name="environments-cfg-autoscaling-spot-example1"></a>


**Option settings**  

|  **Option**  |  **Namespace**  |  **Value**  | 
| --- | --- | --- | 
|  `MinSize`  |  `aws:autoscaling:asg`  |  `10`  | 
|  `MaxSize`  |  `aws:autoscaling:asg`  |  `24`  | 
|  `SpotFleetOnDemandBase`  |  `aws:ec2:instances`  |  `4`  | 
|  `SpotFleetOnDemandAboveBasePercentage`  |  `aws:ec2:instances`  |  `50`  | 
In this example, the environment starts with ten instances, of which seven are On\-Demand \(four base, and 50% of the six above base\) and three are Spot\. The environment can scale out up to 24 instances\. As it scales out, the portion of On\-Demand in the part of the fleet above the four base On\-Demand instances is kept at 50%, up to a maximum of 24 instances overall, of which 14 are On\-Demand \(four base, and 50% of the 20 above base\) and ten are Spot\.

**Example 2: All On\-Demand initial capacity**  <a name="environments-cfg-autoscaling-spot-example1"></a>


**Option settings**  

|  **Option**  |  **Namespace**  |  **Value**  | 
| --- | --- | --- | 
|  `MinSize`  |  `aws:autoscaling:asg`  |  `4`  | 
|  `MaxSize`  |  `aws:autoscaling:asg`  |  `24`  | 
|  `SpotFleetOnDemandBase`  |  `aws:ec2:instances`  |  `4`  | 
|  `SpotFleetOnDemandAboveBasePercentage`  |  `aws:ec2:instances`  |  `50`  | 
In this example, the environment starts with four instances, all of which are On\-Demand\. The environment can scale out up to 24 instances\. As it scales out, the portion of On\-Demand in the part of the fleet above the four base On\-Demand instances is kept at 50%, up to a maximum of 24 instances overall, of which 14 are On\-Demand \(four base, and 50% of the 20 above base\) and ten are Spot\.

**Example 3: Additional On\-Demand base beyond initial capacity**  <a name="environments-cfg-autoscaling-spot-example1"></a>


**Option settings**  

|  **Option**  |  **Namespace**  |  **Value**  | 
| --- | --- | --- | 
|  `MinSize`  |  `aws:autoscaling:asg`  |  `3`  | 
|  `MaxSize`  |  `aws:autoscaling:asg`  |  `24`  | 
|  `SpotFleetOnDemandBase`  |  `aws:ec2:instances`  |  `4`  | 
|  `SpotFleetOnDemandAboveBasePercentage`  |  `aws:ec2:instances`  |  `50`  | 
In this example, the environment starts with three instances, all of which are On\-Demand\. The environment can scale out up to 24 instances\. The first additional instance above the initial three is On\-Demand, to complete the four base On\-Demand instances\. As it scales out further, the portion of On\-Demand in the part of the fleet above the four base On\-Demand instances is kept at 50%, up to a maximum of 24 instances overall, of which 14 are On\-Demand \(four base, and 50% of the 20 above base\) and ten are Spot\.

## Auto Scaling group configuration using the Elastic Beanstalk console<a name="environments-cfg-autoscaling-console"></a>

You can configure how Auto Scaling works by editing **Capacity** on the environment's **Configuration** page in the [Elastic Beanstalk console](environments-console.md)\.

**To configure the Auto Scaling group in the Elastic Beanstalk console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Configuration**\.

1. In the **Capacity** configuration category, choose **Edit**\.

1. In the **Auto Scaling group** section, configure the following settings\.
   + **Environment type** – Select **Load balanced**\.
   + **Min instances** – The minimum number of EC2 instances that the group should contain at any time\. The group starts with the minimum count and adds instances when the scale\-up trigger condition is met\.
   + **Max instances** – The maximum number of EC2 instances that the group should contain at any time\.
**Note**  
If you use rolling updates, be sure that the maximum instance count is higher than the [**Minimum instances in service** setting](using-features.rollingupdates.md#rollingupdates-configure) for rolling updates\.
   + **Fleet composition** – Select **Combined purchase options and instance types** to enable Spot Instance requests\.

     For this example, select **On\-Demand Instances**\.
   + **Instance type** – The type of Amazon EC2 instance launched to run your application\. For details, see [Instance type](using-features.managing.ec2.md#using-features.managing.ec2.instancetypes)\.
   + **AMI ID** – The machine image that Elastic Beanstalk uses to launch Amazon EC2 instances in your environment\. For details, see [AMI ID](using-features.managing.ec2.md#using-features.managing.ec2.customami)\.
   + **Availability Zones** – Choose the number of Availability Zones to spread your environment's instances across\. By default, the Auto Scaling group launches instances evenly across all usable zones\. To concentrate your instances in fewer zones, choose the number of zones to use\. For production environments, use at least two zones to ensure that your application is available in case one Availability Zone goes out\.
   + **Placement** \(optional\) – Choose the Availability Zones to use\. Use this setting if your instances need to connect to resources in specific zones, or if you have purchased [reserved instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts-on-demand-reserved-instances.html), which are zone\-specific\. If you al Cooldown: '720' Custom Availability Zones: 'us\-west\-2a,us\-west\-2b' MaxSize: '4' so set the number of zones, you must choose at least that many custom zones\.

     If you launch your environment in a custom VPC, you cannot configure this option\. In a custom VPC, you choose Availability Zones for the subnets that you assign to your environment\.
   + **Scaling cooldown** – The amount of time, in seconds, to wait for instances to launch or terminate after scaling, before continuing to evaluate triggers\. For more information, see [Scaling Cooldowns](https://docs.aws.amazon.com/autoscaling/ec2/userguide/Cooldown.html)\.  
![\[Elastic Beanstalk Auto Scaling configuration window\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environment-cfg-autoscaling.png)

1. Choose **Apply**\.

## Auto Scaling group configuration using the EB CLI<a name="environments-cfg-autoscaling-ebcli"></a>

When you create an environment using the [eb create](eb3-create.md) command, you can specify a few options related to your environment's Auto Scaling group\. These options help you control the environment's capacity\.

`--single`  
An option to create the environment with a single Amazon EC2 instance and without a load balancer\. Without this option, a load\-balanced environment is created\.

`--﻿instance-types`  
A list of Amazon EC2 instance types you want your environment to use\.

`--enable-spot``--spot-max-price`  
Options related to enabling and configuring Spot Instance requests\.

The following example creates an environment and configures the Auto Scaling group to enable Spot Instance requests for the new environment, with three possible instance types to use \.

```
$ eb create --enable-spot -﻿-﻿instance-types "t2.micro,t3.micro,t3.small"
```

## Configuration options<a name="environments-cfg-autoscaling-namespace"></a>

Elastic Beanstalk provides [configuration options](command-options.md) for Auto Scaling settings in two namespaces: [`aws:autoscaling:asg`](command-options-general.md#command-options-general-autoscalingasg) and `aws:ec2:instances`\.

### The aws:autoscaling:asg namespace<a name="environments-cfg-autoscaling-namespace.asg"></a>

The [`aws:autoscaling:asg`](command-options-general.md#command-options-general-autoscalingasg) namespace provides options for overall scale and availability\.

The following [configuration file](ebextensions.md) example configures the Auto Scaling group to use two to four instances, specific availability zones, and a cooldown period of 12 minutes \(720 seconds\)\.

```
option_settings:
  aws:autoscaling:asg:
    Availability Zones: Any
    Cooldown: '720'
    Custom Availability Zones: 'us-west-2a,us-west-2b'
    MaxSize: '4'
    MinSize: '2'
```

### The aws:ec2:instances namespace<a name="environments-cfg-autoscaling-namespace.instances"></a>

The [`aws:ec2:instances`](command-options-general.md#command-options-general-ec2instances) namespace provides options related to your environment's instances, including Spot Instance management\. It complements [`aws:autoscaling:launchconfiguration`](command-options-general.md#command-options-general-autoscalinglaunchconfiguration) and [`aws:autoscaling:asg`](command-options-general.md#command-options-general-autoscalingasg)\.

When you update your environment configuration and remove one or more instance types from the `InstanceTypes` option, Elastic Beanstalk terminates any Amazon EC2 instances running on any of the removed instance types\. Your environment's Auto Scaling group then launches new instances, as necessary to complete the desired capacity, using your current specified instance types\.

The following [configuration file](ebextensions.md) example configures the Auto Scaling group to enable Spot Instance requests for your environment, with three possible instance types to use, at least one On\-Demand Instance as baseline capacity, and a sustained 33% of On\-Demand Instances for any additional capacity\.

```
option_settings:
  aws:ec2:instances:
    EnableSpot: true
    InstanceTypes: 't2.micro,t3.micro,t3.small'
    SpotFleetOnDemandBase: '1'
    SpotFleetOnDemandAboveBasePercentage: '33'
```

To choose Spot Instance types, use the [Spot Instance Advisor](https://aws.amazon.com/ec2/spot/instance-advisor/)\.