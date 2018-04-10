# Using Elastic Beanstalk with Amazon Virtual Private Cloud<a name="vpc"></a>

You can use an [Amazon Virtual Private Cloud](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/) \(Amazon VPC\) to create a secure network for your Elastic Beanstalk application and related AWS resources\. When you create your environment, you choose which VPC, subnets, and security groups are used for your application instances and load balancer\. You can use any VPC configuration that you like as long as it meets the following requirements\.

**VPC Requirements**
+ **Internet Access** – Instances must have access to the Internet through one of the following methods\.
  + **Public Subnet** – Instances have a public IP address and use an Internet Gateway to access the Internet\.
  + **Private Subnet** – Instances use a NAT device to access the Internet\.

  Elastic Beanstalk does not support proxy settings like `HTTPS_PROXY` for configuring a web proxy\.
+ **NTP** – Instances in your Elastic Beanstalk environment use Network Time Protocol \(NTP\) to synchronize the system clock\. If instances are unable to communicate on UDP port 123, the clock may go out of sync, causing issues with Elastic Beanstalk health reporting\. Ensure that your VPC security groups and network ACLs allow inbound and outbound UDP traffic on port 123 to avoid these issues\.

The [elastic\-beanstalk\-samples](https://github.com/awslabs/elastic-beanstalk-samples/) repository provides AWS CloudFormation templates that you can use to create a VPC for use with your Elastic Beanstalk environments\.

**To create resources with a AWS CloudFormation template**

1. Clone the samples repository or download a template using the links in the [README](https://github.com/awslabs/elastic-beanstalk-samples/tree/master/cfn-templates/README.md)\.

1. Open the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation/home)\.

1. Choose **Create stack**\.

1. Choose **Upload a template to Amazon S3**\.

1. Choose **Upload file** and upload the template file from your local machine\.

1. Choose **Next** and follow the instructions to create a stack with the resources in the template\.

When stack creation completes, check the **Outputs** tab to find the VPC ID and subnet IDs\. Use these to configure the VPC in the new environment wizard [network card](environments-create-wizard.md#environments-create-wizard-network)\.

## VPC with Public Subnets<a name="services-vpc-public"></a>

**AWS CloudFormation template** – [vpc\-public\.yaml](https://github.com/awslabs/elastic-beanstalk-samples/tree/master/cfn-templates/vpc-public.yaml)

**Settings \(load balanced\)**
+ **Load balancer visibility** – Public
+ **Load balancer subnets** – Both public subnets
+ **Instance public IP** – Enabled
+ **Instance subnets** – Both public subnets
+ **Instance security groups** – Add the default security group

**Settings \(single instance\)**
+ **Instance subnets** – One of the public subnets
+ **Instance security groups** – Add the default security group

A basic *public\-only* VPC layout includes one or more public subnets, an Internet Gateway, and a default security group that allows traffic between resources in the VPC\. When you create an environment in the VPC, Elastic Beanstalk creates additional resources that vary depending on the environment type\.

**VPC Resources**
+ **Single Instance** – Elastic Beanstalk creates a security group for the application instance that allows traffic on port 80 from the Internet, and assigns the instance an Elastic IP to give it a public IP address\. The environment's domain name resolves to the instance's public IP address\.
+ **Load Balanced** – Elastic Beanstalk creates a security group for the load balancer that allows traffic on port 80 from the Internet, and a security group for the application instances that allows traffic from the load balancer's security group\. The environment's domain name resolves to the load balancer's public domain name\.

This is similar to the way that Elastic Beanstalk manages networking when you use the default VPC\. Security in a public subnet depends on the load balancer and instance security groups created by Elastic Beanstalk\. It is the least expensive configuration as it does not require a NAT Gateway\.

## VPC with Private and Public Subnets<a name="services-vpc-privatepublic"></a>

**AWS CloudFormation template** – [vpc\-privatepublic\.yaml](https://github.com/awslabs/elastic-beanstalk-samples/tree/master/cfn-templates/vpc-privatepublic.yaml)

**Settings \(load balanced\)**
+ **Load balancer visibility** – Public
+ **Load balancer subnets** – Both public subnets
+ **Instance public IP** – Disabled
+ **Instance subnets** – Both private subnets
+ **Instance security groups** – Add the default security group

For additional security, add private subnets to your VPC to create a *public\-private* layout\. This layout requires a load balancer and NAT gateway in the public subnets, and lets you run your application instances, database, and any other resources in private subnets\. Instances in private subnets can only communicate with the Internet through the load balancer and NAT gateway\.

For internal applications that you don't want to be accessible from the Internet, you can run everything in private subnets and configure the load balancer to be internal facing \(change **Load balancer visibility** to **Internal**\. Use this layout for applications that should only be accessible from the same VPC or an attached VPN\.

**Topics**
+ [VPC with Public Subnets](#services-vpc-public)
+ [VPC with Private and Public Subnets](#services-vpc-privatepublic)
+ [Example: Launching an Elastic Beanstalk Application in a VPC with Bastion Hosts](vpc-bastion-host.md)
+ [Example: Launching an Elastic Beanstalk in a VPC with Amazon RDS](vpc-rds.md)