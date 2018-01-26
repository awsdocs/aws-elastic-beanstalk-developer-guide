# Example: Launching a Load\-Balancing, Autoscaling Environment with Public and Private Resources in a VPC<a name="vpc-basic"></a>

You can deploy an Elastic Beanstalk application in a load balancing, autoscaling environment in a VPC that has both a public and private subnet\. Use this configuration if you want Elastic Beanstalk to assign private IP addresses to your Amazon EC2 instances\. In this configuration, the Amazon EC2 instances in the private subnet require a load balancer and a network address translation \(NAT\) gateway in the public subnet\. The load balancer routes inbound traffic from the Internet to the Amazon EC2 instances\. You need to create a NAT gateway to route outbound traffic from the Amazon EC2 instances to the Internet\. Your infrastructure will look similar to the following diagram\.

![\[Elastic Beanstalk and VPC Topology\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-vpc-basic-topo-ngw.png)

**Note**  
In this configuration, you cannot connect to your instances remotely because the instances are located in a private subnet\. If you need to be able to connect to your instances, and your instances must be in a private subnet, you must implement a bastion host as described in [Example: Launching an Elastic Beanstalk Application in a VPC with Bastion Hosts](vpc-bastion-host.md)\.

To deploy an Elastic Beanstalk application inside a VPC using a NAT gateway, you need to complete the following:


+ [Create a VPC with a Public and Private Subnet](#vpc-basic-create)
+ [Create a Security Group for Your Instances](#create-instance-sg)
+ [Deploy to Elastic Beanstalk](#vpc-basic-create-env)

## Create a VPC with a Public and Private Subnet<a name="vpc-basic-create"></a>

You can use the [Amazon VPC console](https://console.aws.amazon.com/vpc/) to create a VPC\. 

**To create a VPC**

1. Sign in to the [Amazon VPC console](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **VPC Dashboard**\. Then choose **Start VPC Wizard**\.

1. Choose **VPC with Public and Private Subnets**, and then choose **Select**\.  
![\[Choose option 2 in the wizard\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/Case2_Wizard_Page2.png)

1. Your Elastic Load Balancing load balancer and your Amazon EC2 instances must be in the same Availability Zone so they can communicate with each other\. Choose the same Availability Zone from each **Availability Zone** list\.  
![\[Option 2 confirmation page\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/Case2_Wizard_Confirmation2.png)

1. Choose an Elastic IP address for your NAT gateway\.

1. Choose **Create VPC**\.

   The wizard begins to create your VPC, subnets, and internet gateway\. It also updates the main route table and creates a custom route table\. Finally, the wizard creates a NAT gateway in the public subnet\.
**Note**  
You can choose to launch a NAT instance in the public subnet instead of a NAT gateway\. For more information, see [Scenario 2: VPC with Public and Private Subnets \(NAT\)](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Scenario2.html) in the *Amazon VPC User Guide*\.

1. After the VPC is successfully created, you get a VPC ID\. You need this value for the next step\. To view your VPC ID, choose **Your VPCs** in the left pane of the [Amazon VPC console](https://console.aws.amazon.com/vpc/)\.  
![\[VPC ID\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-vpc-id.png)

## Create a Security Group for Your Instances<a name="create-instance-sg"></a>

You can optionally create a security group for your Elastic Beanstalk instances\. You can add rules to the security group to control inbound and outbound traffic for the instances associated with that security group\. To create a security group, perform the following procedure\. You should give this security group a meaningful name, such as `Instance Group`, that will allow you to easily identify the security group later\. For more information about security groups, see [Security Groups for Your VPC](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_SecurityGroups.html) in the *Amazon VPC User Guide*\. 

**To create a new security group**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Security Groups**\.

1. Choose **Create Security Group**\.

1. In the **Create Security Group** dialog box, enter the following and choose **Yes, Create**\.  
**Name tag** \(Optional\)  
Enter a name tag for the security group\.  
**Group name**  
Enter the name of the security group\.  
**Description**  
Enter a description for the security group\.  
**VPC**  
Select your VPC\.

   The security group is created and appears on the **Security Groups** page\. Notice that it has an ID \(e\.g\., `sg-xxxxxxxx`\)\. You might have to turn on the **Group ID** column by clicking **Show/Hide** in the top right corner of the page\.

## Deploy to Elastic Beanstalk<a name="vpc-basic-create-env"></a>

After you set up your VPC, you can create your environment inside it and deploy your application to Elastic Beanstalk\. You can do this using the Elastic Beanstalk console, or you can use the AWS toolkits, AWS CLI, EB CLI, or Elastic Beanstalk API\. If you use the Elastic Beanstalk console, you just need to upload your `.war` or `.zip` file and select the VPC settings inside the wizard\. Elastic Beanstalk then creates your environment inside your VPC and deploys your application\. Alternatively, you can use the AWS toolkits, AWS CLI, EB CLI, or Elastic Beanstalk API to deploy your application\. To do this, you need to define your VPC option settings in a configuration file and deploy this file with your source bundle\. This topic provides instructions for both methods\.


+ [Deploying with the Elastic Beanstalk Console](#vpc-basic-new-console)
+ [Deploying with the AWS Toolkits, AWS CLI, EB CLI, or Elastic Beanstalk API](#vpc-basic-new-options)

### Deploying with the Elastic Beanstalk Console<a name="vpc-basic-new-console"></a>

The Elastic Beanstalk console walks you through creating your new environment inside your VPC\. You need to provide a `.war` file \(for Java applications\) or a `.zip` file \(for all other applications\)\. In the **VPC Configuration** page of the Elastic Beanstalk environment wizard, you must make the following selections:

**VPC**  
Select your VPC

**VPC security group**  
Select the instance security group you created above\.

**ELB visibility**  
Select `External` if your load balancer should be publicly available, or select `Internal` if the load balancer should be available only within your VPC\.

Select the subnets for your load balancer and EC2 instances\. Be sure you select the public subnet for the load balancer, and the private subnet for your Amazon EC2 instances\. By default, the VPC creation wizard creates the public subnet in `10.0.0.0/24` and the private subnet in `10.0.1.0/24`\.

You can view your subnet IDs by choosing **Subnets** in the [Amazon VPC console](https://console.aws.amazon.com/vpc/)\.

![\[Subnet IDs for your VPC\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-vpc-subnets.png)

### Deploying with the AWS Toolkits, AWS CLI, EB CLI, or Elastic Beanstalk API<a name="vpc-basic-new-options"></a>

When deploying your application to Elastic Beanstalk using the AWS toolkits, EB CLI, AWS CLI, or API, you can specify your VPC option settings in a file and deploy it with your source bundle\. See  for more information\.

When you create your configuration file with your option settings, you need to specify the following configuration options:[aws:autoscaling:launchconfiguration](command-options-general.md#command-options-general-autoscalinglaunchconfiguration) Namespace:

EC2KeyName  
The name of the Amazon EC2 key pair to apply to the instances\.

InstanceType  
The instance type used to run your application in the environment\.  
The instance types available depend on platform, solution stack \(configuration\) and region\. To get a list of available instance types for your solution stack of choice, use the [DescribeConfigurationOptions ](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_DescribeConfigurationOptions.html)action in the API, [describe\-configuration\-options ](http://docs.aws.amazon.com/cli/latest/reference/elasticbeanstalk/describe-configuration-options.html)command in the [AWS CLI](https://aws.amazon.com/cli/)\.

SecurityGroups  
The identifier\(s\) of the security group\(s\) to apply to the instances\. In this example, this will be the identifier of the security group you created in [Create a Security Group for Your Instances](#create-instance-sg)\. If you did not create a security group, you can use the default security group for your VPC\.  
You can specify multiple identifiers by separating them with a comma\.[aws:ec2:vpc](command-options-general.md#command-options-general-ec2vpc) Namespace:

VPCId  
The identifier of your VPC\.

Subnets  
The identifier\(s\) of the subnet\(s\) to launch the instances in\. In this example, this is the ID of the private subnet\.   
You can specify multiple identifiers by separating them with a comma\.

ELBSubnets  
The identifier\(s\) of the subnet\(s\) for the load balancer\. In this example, this is the ID of the public subnet\.  
You can specify multiple identifiers by separating them with a comma\.

ELBScheme \(Optional\)  
Specify `internal` if you want to create an internal load balancer inside your VPC so that your Elastic Beanstalk application cannot be accessed from outside your VPC\.

DBSubnets \(Optional\)  
Contains the identifiers of the Amazon RDS DB subnets\. This is only used if you want to add an Amazon RDS DB Instance as part of your application\. For an example, see [Example: Launching an Elastic Beanstalk in a VPC with Amazon RDS](vpc-rds.md)\.  
When using `DBSubnets`, you need to create additional subnets in your VPC to cover all the Availability Zones in the region\. 

The following is an example of the option settings you could set when deploying your Elastic Beanstalk application inside a VPC\. 

```
option_settings:
  aws:autoscaling:launchconfiguration:
    EC2KeyName: "ec2_key_name"
    InstanceType: "instance_type"
    SecurityGroups: "security_group_id, etc"
  aws:ec2:vpc:
    VPCId: "vpc_id"
    Subnets: "instance_subnet, etc"
    ELBSubnets: "elb_subnet, etc"
```