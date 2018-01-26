# Example: Launching a Load\-Balancing, Autoscaling Environment with Public Instances in a VPC<a name="vpc-no-nat"></a>

You can deploy an Elastic Beanstalk application in a load balancing, autoscaling environment in a single public subnet\. Use this configuration if you have a single public subnet without any private resources associated with your Amazon EC2 instances\. In this configuration, Elastic Beanstalk assigns public IP addresses to the Amazon EC2 instances so that each can directly access the Internet through the VPC Internet gateway\. You do not need to create a network address translation \(NAT\) configuration in your VPC\.

![\[Elastic Beanstalk and VPC Topology\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-vpc-apip-topo.png)

To deploy an Elastic Beanstalk application in a load balancing, autoscaling environment in a single public subnet, you need to complete the following:


+ [Create a VPC with a Public Subnet](#vpc-no-nat-create)
+ [Deploy to Elastic Beanstalk](#vpc-no-nat-create-env)

## Create a VPC with a Public Subnet<a name="vpc-no-nat-create"></a>

**To create a VPC**

1. Sign in to the AWS Management Console and open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **VPC Dashboard** and then choose **Start VPC Wizard**\. 

1. Select **VPC with a Single Public Subnet** and then choose **Select**\.  
![\[Choose the first option in the wizard\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/vpc-wiz-single.png)

   A confirmation page shows the CIDR blocks used for the VPC and subnet\. The page also shows the subnet and the associated Availability Zone\.  
![\[Option 1 confirmation page\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/vpc-wiz-single-2.png)

1. Choose **Create VPC**\.

   AWS creates your VPC, subnet, Internet gateway, and route table\. Choose **OK** to exit the wizard\.

   After AWS successfully creates the VPC, it assigns the VPC a VPC ID\. You will need this for this for the next step\. To view your VPC ID, choose **Your VPCs** in the left pane of the [Amazon VPC console](https://console.aws.amazon.com/vpc/)\.  
![\[VPC ID\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-vpc-id.png)

## Deploy to Elastic Beanstalk<a name="vpc-no-nat-create-env"></a>

After you set up your VPC, you can create your environment inside it and deploy your application to Elastic Beanstalk\. You can do this using the Elastic Beanstalk console, or you can use the AWS toolkits, AWS CLI, EB CLI, or Elastic Beanstalk API\. If you use the Elastic Beanstalk console, you just need to upload your `.war` or `.zip` file and select the VPC settings inside the wizard\. Elastic Beanstalk then creates your environment inside your VPC and deploys your application\. Alternatively, you can use the AWS toolkits, AWS CLI, EB CLI, or Elastic Beanstalk API to deploy your application\. To do this, you need to define your VPC option settings in a configuration file and deploy this file with your source bundle\. This topic provides instructions for both methods\.


+ [Deploying with the Elastic Beanstalk Console](#vpc-no-nat-new-console)
+ [Deploying with the AWS Toolkits, AWS CLI, EB CLI, or Elastic Beanstalk API](#vpc-no-nat-new-options)

### Deploying with the Elastic Beanstalk Console<a name="vpc-no-nat-new-console"></a>

When you create an Elastic Beanstalk application or launch an environment, the Elastic Beanstalk console walks you through creating your environment inside a VPC\. For more information, see [Managing and Configuring AWS Elastic Beanstalk Applications](applications.md)\.

You'll need to select the VPC ID and subnet ID for your instance\. By default, VPC creates a public subnet using 10\.0\.0\.0/24\. You can view your subnet ID by clicking **Subnets** in the [Amazon VPC console](https://console.aws.amazon.com/vpc/)\. 

![\[Subnet ID for your VPC\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/vpc-one-subnet-pub.png)

### Deploying with the AWS Toolkits, AWS CLI, EB CLI, or Elastic Beanstalk API<a name="vpc-no-nat-new-options"></a>

When deploying your application to Elastic Beanstalk using the AWS toolkits, EB CLI, AWS CLI, or API, you can specify your VPC option settings in a file and deploy it with your source bundle\. See  for more information\.

When you create your configuration file with your option settings, you need to specify the following configuration options:[aws:ec2:vpc](command-options-general.md#command-options-general-ec2vpc) Namespace:

VPCId  
The identifier of your VPC\.

Subnets  
The identifier\(s\) of the subnet\(s\) to launch the instances in\.   
You can specify multiple identifiers by separating them with a comma\.

AssociatePublicIpAddress  
Specifies whether to launch instances in your VPC with public IP addresses\. Instances with public IP addresses do not require a NAT device to communicate with the Internet\. You must set the value to `true` if you want to include your load balancer and instances in a single public subnet\.

The following is an example of the option settings you could set when deploying your Elastic Beanstalk application inside a VPC\. 

```
option_settings:
  aws:ec2:vpc:
    VPCId: "vpd_id"
    Subnets: "instance_subnet, etc"
    AssociatePublicIpAddress: "true"
```