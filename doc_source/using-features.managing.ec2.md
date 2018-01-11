# EC2 Instance Launch Configuration<a name="using-features.managing.ec2"></a>

The Auto Scaling Group in your environment manages the EC2 instances that run your application\. Changes to the Auto Scaling Group's launch configuration require replacement of all instances and will trigger a rolling update or immutable update, depending on which one is configured\.


+ [Configuring Your Environment's EC2 Instances](#environments-cfg-autoscaling-console)
+ [The aws:autoscaling:launchconfiguration Namespace](#environments-cfg-autoscaling-namespace)

## Configuring Your Environment's EC2 Instances<a name="environments-cfg-autoscaling-console"></a>

You can modify your Elastic Beanstalk environment's Auto Scaling Group configuration in the Elastic Beanstalk console\.

**To configure EC2 Instances in the Elastic Beanstalk console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the management page for your environment\.

1. Choose **Configuration**\.

1. Choose **Instances**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-env-config-instances.png)

The following settings are available:


+ [Instance Type](#using-features.managing.ec2.instancetypes)
+ [Security Groups](#using-features.managing.ec2.securitygroups)
+ [EC2 Key Pair](#using-features.managing.ec2.keypair)
+ [Instance Profile](#using-features.managing.ec2.profile)
+ [Monitoring Interval](#using-features.managing.ec2.monitoring-interval)
+ [Custom AMI ID](#using-features.managing.ec2.customami)
+ [Root Volume \(Boot Device\)](#using-features.managing.ec2.rootvolume)

### Instance Type<a name="using-features.managing.ec2.instancetypes"></a>

The **Instance type** setting determines the type of EC2 instance launched to run your application\. Choose an instance that is powerful enough to run your application under load, but not so powerful that it is idle most of the time\. For development purposes, the t2 family of instances provides a moderate amount of power with the ability to burst for short periods of time\.

For large scale, high availability applications, use a pool of instances to ensure that capacity is not greatly affected if any single instance goes down\. Start with an instance type that allows you to run five instances under moderate load during normal hours\. If any instance fails, the rest of the instances can absorb the rest of the traffic\. The capacity buffer also allows time for the environment to scale up as traffic begins to rise during peak hours\.

For more information about EC2 instance families and types, see [Instance Types](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-types.html) in the *Amazon Elastic Compute Cloud User Guide*\. 

### Security Groups<a name="using-features.managing.ec2.securitygroups"></a>

The security groups attached to your instances determine which traffic is allowed to reach the instances \(ingress\), and which traffic is allowed to leave the instances \(egress\)\. Elastic Beanstalk creates a security group that allows traffic from the load balancer on the standard ports for HTTP \(80\) and HTTPS \(443\)\.

You can specify additional security groups that you have created to allow traffic on other ports or from other sources\. For example, you can create a security group for SSH access that allows ingress on port 22 from a restricted IP address range or, for additional security, from a bastion host to which only you have access\.

**Note**  
To allow traffic between environment A's instances and environment B's instances, you can add a rule to the security group that Elastic Beanstalk attached to environment B, and specify the security group that Elastic Beanstalk attached to environment A\. This allows ingress from, or egress to, environment A's instances\. However, doing so creates a dependency between the two security groups\. If you later try to terminate environment A, Elastic Beanstalk will not be able to delete the environment's security group, because environment B's security group is dependent on it\.  
A safer approach would be to create a separate security group, attach it to environment A, and specify it in a rule of environment B's security group\.

For more information on Amazon EC2 security groups, see [Amazon EC2 Security Groups](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html) in the *Amazon Elastic Compute Cloud User Guide*\.

### EC2 Key Pair<a name="using-features.managing.ec2.keypair"></a>

You can securely log in to the Amazon EC2 instances provisioned for your Elastic Beanstalk application with an Amazon EC2 key pair\. For instructions on creating a key pair for Amazon EC2, see the [Amazon Elastic Compute Cloud Getting Started Guide](http://docs.aws.amazon.com/AWSEC2/latest/GettingStartedGuide/)\. 

Choose an **EC2 key pair** from the drop down menu to assign it to your environment's instances\. When you assign a key pair, the public key is stored on the instance to authenticate the private key, which you store locally\. The private key is never stored in AWS\.

For more information on connecting to Amazon EC2 instances, see [Connect to Your Instance](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstances.html) and [ Connecting to Linux/UNIX Instances from Windows using PuTTY](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html) in the *Amazon Elastic Compute Cloud User Guide*\. 

### Instance Profile<a name="using-features.managing.ec2.profile"></a>

An instance profile is an IAM role that is applied to instances launched in your Elastic Beanstalk environment\. EC2 instances assume the instance profile role to sign requests to AWS and access APIs, for example, to upload logs to Amazon S3\.

The first time you create an environment in the AWS Management Console, Elastic Beanstalk prompts you to create an instance profile with a default set of permissions\. You can add permissions to this profile to provide your instances access to other AWS services\. For details, see 

### Monitoring Interval<a name="using-features.managing.ec2.monitoring-interval"></a>

By default, the instances in your environment publish basic health metrics to CloudWatch at 5 minute intervals at no additional cost\.

For more detailed reporting, you can set the **Monitoring interval** to **1 minute** to increase the frequency with which the resources in your environment publish basic health metrics to CloudWatch\. Amazon CloudWatch service charges apply for one\-minute interval metrics\. See [Amazon CloudWatch](http://aws.amazon.com/cloudwatch/) for more information\.

### Custom AMI ID<a name="using-features.managing.ec2.customami"></a>

The Amazon Machine Image \(AMI\) is the Amazon Linux or Windows Server machine image that AWS Elastic Beanstalk uses to launch EC2 instances in your environment\. Elastic Beanstalk provides machine images that contain the tools and resources required to run your application\.

Elastic Beanstalk selects a default AMI for your environment based on the region, platform, and instance type that you choose\. If you have created a custom AMI, replace the default AMI ID with yours\.

### Root Volume \(Boot Device\)<a name="using-features.managing.ec2.rootvolume"></a>

Each instance in your environment is configured with a root volume\. The root volume is the Amazon EBS block device attached to the instance to store the operating system, libraries, scripts, and your application source code\. By default, all platforms use general purpose SSD block devices for storage\.

You can modify **Root volume type** to use use magnetic storage or provisioned IOPS SSD volume types, and increase the volume size if needed\. For provisioned IOPS volumes, you must also select the number of IOPS to provision\. Select the volume type that meets your performance and price requirements\.

For more information, see [Amazon EBS Volume Types](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSVolumeTypes.html) and [Amazon EBS Product Details](http://aws.amazon.com/ebs/details/)\.

## The aws:autoscaling:launchconfiguration Namespace<a name="environments-cfg-autoscaling-namespace"></a>

You can use the configuration options in the `aws:autoscaling:launchconfiguration` namespace to configure your Auto Scaling Group, including additional options that are not available in the console\.

The following configuration file configures the basic options shown above an an additional option, `BlockDeviceMappings`, which isn't available in the console\.

```
option_settings:
  aws:autoscaling:launchconfiguration:
    InstanceType: m1.small
    SecurityGroups: my-securitygroup
    EC2KeyName: my-keypair
    MonitoringInterval: "1 minute"
    ImageId: "ami-cbab67a2"
    IamInstanceProfile: "ElasticBeanstalkProfile"
    BlockDeviceMappings: "/dev/sdj=:100,/dev/sdh=snap-51eef269,/dev/sdb=ephemeral0"
```

`BlockDeviceMappings` lets you configure additional block devices for your instances\. For more information about block device mappings, see [Block Device Mapping](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/block-device-mapping-concepts.html) in the *Amazon Elastic Cloud Computer User Guide*\.

EB CLI and Elastic Beanstalk console apply recommended values for the preceding options\. These settings must be removed if you want to use configuration files to configure the same\. See  for details\.