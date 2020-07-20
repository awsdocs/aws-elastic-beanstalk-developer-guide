# Your Elastic Beanstalk environment's Amazon EC2 instances<a name="using-features.managing.ec2"></a>

When you create a web server environment, AWS Elastic Beanstalk creates one or more Amazon Elastic Compute Cloud \(Amazon EC2\) virtual machines, known as *Instances*\.

The instances in your environment are configured to run web apps on the platform that you choose\. You can make changes to various properties and behaviors of your environment's instances during environment creation, after creation on a running environment, or as part of the source code that you deploy to the environment\. For details, see [Configuration options](command-options.md)\.

**Note**  
The [Auto Scaling group](using-features.managing.as.md) in your environment manages the Amazon EC2 instances that run your application\. When you make configuration changes described on this page, the launch configuration \(either an Amazon EC2 launch template or an Auto Scaling group launch configuration resource\) changes\. This change requires [replacement of all instances](environments-updating.md) and trigger a [rolling update](using-features.rollingupdates.md) or [immutable update](environmentmgmt-updates-immutable.md), depending on which one is configured\.

During environment creation you choose an [instance type](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-types.html) to determine the hardware of the host computer used to run your instances\. Elastic Beanstalk supports new instance types soon after Amazon EC2 introduces them, typically by the next [platform update](using-features.platform.upgrade.md)\.

Elastic Beanstalk supports several Amazon EC2 [instance purchasing options](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-purchasing-options.html): *On\-Demand Instances*, *Reserved Instances*, and *Spot Instances*\. An On\-Demand Instance is a pay\-as\-you\-go resource—there is no long\-term commitment required when you use it\. A Reserved Instance is a pre\-purchased billing discount applied automatically to matching On\-Demand instances in your environment\. A Spot Instance is an unused Amazon EC2 instance that is available for less than the On\-Demand price\. You can enable Spot Instances in your environment by setting a single option\. You can configure Spot Instance usage, including the mix of On\-Demand and Spot Instances, using additional options\. For more information, see [Auto Scaling group](using-features.managing.as.md)\.

**Topics**
+ [Configuring your environment's Amazon EC2 instances](#using-features.managing.ec2.console)
+ [The aws:autoscaling:launchconfiguration namespace](#using-features.managing.ec2.namespace)
+ [Configuring the instance metadata service on your environment's instances](environments-cfg-ec2-imds.md)

## Configuring your environment's Amazon EC2 instances<a name="using-features.managing.ec2.console"></a>

You can modify your Elastic Beanstalk environment's Amazon EC2 instance configuration in the Elastic Beanstalk console\.

**To configure Amazon EC2 instances in the Elastic Beanstalk console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Configuration**\.

1. In the **Instances** configuration category, choose **Edit**\. Make changes to settings in this category, and then choose **Apply**\. For setting descriptions, see the section [Instances category settings](#using-features.managing.ec2.console.instances) on this page\.

1. In the **Capacity** configuration category, choose **Edit**\. Make changes to settings in this category, and then choose **Continue**\. For setting descriptions, see the section [Capacity category settings](#using-features.managing.ec2.console.capacity) on this page\.

### Instances category settings<a name="using-features.managing.ec2.console.instances"></a>

The following settings related to Amazon EC2 instances are available in the **Instances** configuration category\.

**Topics**
+ [Monitoring interval](#using-features.managing.ec2.monitoring-interval)
+ [Root volume \(boot device\)](#using-features.managing.ec2.rootvolume)
+ [Instance metadata service](#using-features.managing.ec2.imds)
+ [Security groups](#using-features.managing.ec2.securitygroups)

![\[Amazon EC2 instance settings on Elastic Beanstalk instances configuration window\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-env-config-ec2-instances-page.png)

#### Monitoring interval<a name="using-features.managing.ec2.monitoring-interval"></a>

By default, the instances in your environment publish [basic health metrics](using-features.healthstatus.md) to Amazon CloudWatch at five\-minute intervals at no additional cost\.

For more detailed reporting, you can set the **Monitoring interval** to **1 minute** to increase the frequency with which the resources in your environment publish [basic health metrics](using-features.healthstatus.md#monitoring-basic-cloudwatch) to CloudWatch\. CloudWatch service charges apply for one\-minute interval metrics\. See [Amazon CloudWatch](https://aws.amazon.com/cloudwatch/) for more information\.

#### Root volume \(boot device\)<a name="using-features.managing.ec2.rootvolume"></a>

Each instance in your environment is configured with a root volume\. The root volume is the Amazon EBS block device attached to the instance to store the operating system, libraries, scripts, and your application source code\. By default, all platforms use general\-purpose SSD block devices for storage\.

You can modify **Root volume type** to use magnetic storage or provisioned IOPS SSD volume types and, if needed, increase the volume size\. For provisioned IOPS volumes, you must also select the number of IOPS to provision\. Select the volume type that meets your performance and price requirements\.

For more information, see [Amazon EBS Volume Types](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSVolumeTypes.html) in the *Amazon EC2 User Guide for Linux Instances* and [Amazon EBS Product Details](https://aws.amazon.com/ebs/details/)\.

#### Instance metadata service<a name="using-features.managing.ec2.imds"></a>

The instance metadata service \(IMDS\) is an on\-instance component that code on the instance uses to securely access instance metadata\. Code can access instance metadata from a running instance using one of two methods: Instance Metadata Service Version 1 \(IMDSv1\) or Instance Metadata Service Version 2 \(IMDSv2\)\. IMDSv2 is more secure\. Disable IMDSv1 to enforce IMDSv2\. For details, see [Configuring the instance metadata service on your environment's instances](environments-cfg-ec2-imds.md)\.

**Note**  
The IMDS section on this configuration page appears only for platform versions that support IMDSv2\.

#### Security groups<a name="using-features.managing.ec2.securitygroups"></a>

The security groups attached to your instances determine which traffic is allowed to reach the instances \(ingress\), and which traffic is allowed to leave the instances \(egress\)\. Elastic Beanstalk creates a security group that allows traffic from the load balancer on the standard ports for HTTP \(80\) and HTTPS \(443\)\.

You can specify additional security groups that you have created to allow traffic on other ports or from other sources\. For example, you can create a security group for SSH access that allows ingress on port 22 from a restricted IP address range or, for additional security, from a bastion host to which only you have access\.

**Note**  
To allow traffic between environment A's instances and environment B's instances, you can add a rule to the security group that Elastic Beanstalk attached to environment B, and specify the security group that Elastic Beanstalk attached to environment A\. This allows ingress from, or egress to, environment A's instances\. However, doing so creates a dependency between the two security groups\. If you later try to terminate environment A, Elastic Beanstalk will not be able to delete the environment's security group, because environment B's security group is dependent on it\.  
A safer approach would be to create a separate security group, attach it to environment A, and specify it in a rule of environment B's security group\.

For more information on Amazon Amazon EC2 security groups, see [Amazon EC2 Security Groups](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html) in the *Amazon EC2 User Guide for Linux Instances*\.

### Capacity category settings<a name="using-features.managing.ec2.console.capacity"></a>

The following settings related to Amazon EC2 instances are available in the **Capacity** configuration category\.

**Topics**
+ [Instance type](#using-features.managing.ec2.instancetypes)
+ [AMI ID](#using-features.managing.ec2.customami)

![\[Amazon EC2 instance settings on Elastic Beanstalk capacity configuration window\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-env-config-ec2-capacity-page.png)

#### Instance type<a name="using-features.managing.ec2.instancetypes"></a>

The **Instance type** setting determines the type of Amazon EC2 instance launched to run your application\. Choose an instance that is powerful enough to run your application under load, but not so powerful that it's idle most of the time\. For development purposes, the t2 family of instances provides a moderate amount of power with the ability to burst for short periods of time\.

For large\-scale, high\-availability applications, use a pool of instances to ensure that capacity is not greatly affected if any single instance goes down\. Start with an instance type that allows you to run five instances under moderate load during normal hours\. If any instance fails, the rest of the instances can absorb the rest of the traffic\. The capacity buffer also allows time for the environment to scale up as traffic begins to rise during peak hours\.

For more information about Amazon EC2 instance families and types, see [Instance Types](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-types.html) in the *Amazon EC2 User Guide for Linux Instances*\. 

When you enable Spot Instance requests for your environment, this configuration page shows a list of **Instance types** instead of a single setting\. You can select one or more instance types for your Spot Instances\. For details, see [Spot instance support](using-features.managing.as.md#environments-cfg-autoscaling-spot)\.

#### AMI ID<a name="using-features.managing.ec2.customami"></a>

The Amazon Machine Image \(AMI\) is the Amazon Linux or Windows Server machine image that Elastic Beanstalk uses to launch Amazon EC2 instances in your environment\. Elastic Beanstalk provides machine images that contain the tools and resources required to run your application\.

Elastic Beanstalk selects a default AMI for your environment based on the region, platform, and instance type that you choose\. If you have created a [custom AMI](using-features.customenv.md), replace the default AMI ID with yours\.

## The aws:autoscaling:launchconfiguration namespace<a name="using-features.managing.ec2.namespace"></a>

You can use the [configuration options](command-options.md) in the `aws:autoscaling:launchconfiguration` namespace to configure your environment's instances, including additional options that are not available in the console\.

The following [configuration file](ebextensions.md) example configures the basic options shown in this topic, the option `DisableIMDSv1` discussed in [IMDS](environments-cfg-ec2-imds.md), the options `EC2KeyName` and `IamInstanceProfile` discussed in [Security](using-features.managing.security.md), and an additional option, `BlockDeviceMappings`, which isn't available in the console\.

```
option_settings:
  aws:autoscaling:launchconfiguration:
    InstanceType: m1.small
    SecurityGroups: my-securitygroup
    MonitoringInterval: "1 minute"
    DisableIMDSv1: false
    EC2KeyName: my-keypair
    IamInstanceProfile: "aws-elasticbeanstalk-ec2-role"
    BlockDeviceMappings: "/dev/sdj=:100,/dev/sdh=snap-51eef269,/dev/sdb=ephemeral0"
```

`BlockDeviceMappings` lets you configure additional block devices for your instances\. For more information, see [Block Device Mapping](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/block-device-mapping-concepts.html) in the *Amazon EC2 User Guide for Linux Instances*\.

**Note**  
The `InstanceType` option is obsolete\. It's replaced by the newer and more powerful `InstanceTypes` option in the [`aws:ec2:instances`](command-options-general.md#command-options-general-ec2instances) namespace\. The new option allows you to specify a list of one or more instance types for your environment\. The first value on that list is equivalent to the value of the `InstanceType` option included in the `aws:autoscaling:launchconfiguration` namespace described here\. The recommended way to specify instance types is by using the new option\. If specified, the new option takes precedence over the old one\. For more information, see [The aws:ec2:instances namespace](using-features.managing.as.md#environments-cfg-autoscaling-namespace.instances)\.

The EB CLI and Elastic Beanstalk console apply recommended values for the preceding options\. You must remove these settings if you want to use configuration files to configure the same\. See [Recommended values](command-options.md#configuration-options-recommendedvalues) for details\.