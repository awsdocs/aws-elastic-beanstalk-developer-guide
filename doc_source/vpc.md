# Using Elastic Beanstalk with Amazon VPC<a name="vpc"></a>

You can use an [Amazon Virtual Private Cloud](https://docs.aws.amazon.com/vpc/latest/userguide/) \(Amazon VPC\) to create a secure network for your Elastic Beanstalk application and related AWS resources\. When you create your environment, you choose which VPC, subnets, and security groups are used for your application instances and load balancer\. You can use any VPC configuration that you like as long as it meets the following requirements\.

**VPC requirements**
+ **Internet Access** – Instances can have access to the internet through one of the following methods:
  + **Public Subnet** – Instances have a public IP address and use an internet gateway to access the internet\.
  + **Private Subnet** – Instances use a NAT device to access the internet\.
**Note**  
If you configure [VPC endpoints](vpc-vpce.md) in your VPC to connect to both the `elasticbeanstalk` and `elasticbeanstalk-healthd` services, internet access is optional, and is only required if your application specifically needs it\. Without VPC endpoints, your VPC must have access to the internet\.  
The default VPC that Elastic Beanstalk sets up for you provides internet access\.

  Elastic Beanstalk doesn't support proxy settings like `HTTPS_PROXY` for configuring a web proxy\.
+ **NTP** – Instances in your Elastic Beanstalk environment use Network Time Protocol \(NTP\) to synchronize the system clock\. If instances are unable to communicate on UDP port 123, the clock may go out of sync, causing issues with Elastic Beanstalk health reporting\. Ensure that your VPC security groups and network ACLs allow inbound and outbound UDP traffic on port 123 to avoid these issues\.

The [elastic\-beanstalk\-samples](https://github.com/awsdocs/elastic-beanstalk-samples/) repository provides AWS CloudFormation templates that you can use to create a VPC for use with your Elastic Beanstalk environments\.

**To create resources with a AWS CloudFormation template**

1. Clone the samples repository or download a template using the links in the [README](https://github.com/awsdocs/elastic-beanstalk-samples/tree/master/cfn-templates/README.md)\.

1. Open the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation/home)\.

1. Choose **Create stack**\.

1. Choose **Upload a template to Amazon S3**\.

1. Choose **Upload file** and upload the template file from your local machine\.

1. Choose **Next** and follow the instructions to create a stack with the resources in the template\.

When stack creation completes, check the **Outputs** tab to find the VPC ID and subnet IDs\. Use these to configure the VPC in the new environment wizard [network configuration category](environments-create-wizard.md#environments-create-wizard-network)\.

**Topics**
+ [Public VPC](#services-vpc-public)
+ [Public/private VPC](#services-vpc-privatepublic)
+ [Private VPC](#services-vpc-private)
+ [Example: Launching an Elastic Beanstalk application in a VPC with bastion hosts](vpc-bastion-host.md)
+ [Example: Launching an Elastic Beanstalk in a VPC with Amazon RDS](vpc-rds.md)
+ [Using Elastic Beanstalk with VPC endpoints](vpc-vpce.md)

## Public VPC<a name="services-vpc-public"></a>

**AWS CloudFormation template** – [vpc\-public\.yaml](https://github.com/awsdocs/elastic-beanstalk-samples/tree/master/cfn-templates/vpc-public.yaml)

**Settings \(load balanced\)**
+ **Load balancer visibility** – Public
+ **Load balancer subnets** – Both public subnets
+ **Instance public IP** – Enabled
+ **Instance subnets** – Both public subnets
+ **Instance security groups** – Add the default security group

**Settings \(single instance\)**
+ **Instance subnets** – One of the public subnets
+ **Instance security groups** – Add the default security group

A basic *public\-only* VPC layout includes one or more public subnets, an internet gateway, and a default security group that allows traffic between resources in the VPC\. When you create an environment in the VPC, Elastic Beanstalk creates additional resources that vary depending on the environment type\.

**VPC resources**
+ **Single instance** – Elastic Beanstalk creates a security group for the application instance that allows traffic on port 80 from the internet, and assigns the instance an Elastic IP to give it a public IP address\. The environment's domain name resolves to the instance's public IP address\.
+ **Load balanced** – Elastic Beanstalk creates a security group for the load balancer that allows traffic on port 80 from the internet, and a security group for the application instances that allows traffic from the load balancer's security group\. The environment's domain name resolves to the load balancer's public domain name\.

This is similar to the way that Elastic Beanstalk manages networking when you use the default VPC\. Security in a public subnet depends on the load balancer and instance security groups created by Elastic Beanstalk\. It is the least expensive configuration as it does not require a NAT Gateway\.

## Public/private VPC<a name="services-vpc-privatepublic"></a>

**AWS CloudFormation template** – [vpc\-privatepublic\.yaml](https://github.com/awsdocs/elastic-beanstalk-samples/tree/master/cfn-templates/vpc-privatepublic.yaml)

**Settings \(load balanced\)**
+ **Load balancer visibility** – Public
+ **Load balancer subnets** – Both public subnets
+ **Instance public IP** – Disabled
+ **Instance subnets** – Both private subnets
+ **Instance security groups** – Add the default security group

For additional security, add private subnets to your VPC to create a *public\-private* layout\. This layout requires a load balancer and NAT gateway in the public subnets, and lets you run your application instances, database, and any other resources in private subnets\. Instances in private subnets can only communicate with the internet through the load balancer and NAT gateway\.

## Private VPC<a name="services-vpc-private"></a>

**AWS CloudFormation template** – [vpc\-private\.yaml](https://github.com/awsdocs/elastic-beanstalk-samples/tree/master/cfn-templates/vpc-private.yaml)

**Settings \(load balanced\)**
+ **Load balancer visibility** – Private
+ **Load balancer subnets** – Both private subnets
+ **Instance public IP** – Disabled
+ **Instance subnets** – Both private subnets
+ **Instance security groups** – Add the default security group

For internal applications that shouldn't have access from the internet, you can run everything in private subnets and configure the load balancer to be internally facing \(change **Load balancer visibility** to **Internal**\)\. This template creates a VPC with no public subnets and no internet gateway\. Use this layout for applications that should only be accessible from the same VPC or an attached VPN\.

### Running an Elastic Beanstalk environment in a private VPC<a name="services-vpc-private-beanstalk"></a>

When you create your Elastic Beanstalk environment in a private VPC, the environment doesn't have access to the internet\. Your application might need access to the Elastic Beanstalk service or other services\. Your environment might use enhanced health reporting, and in this case the environment instances send health information to the enhanced health service\. And Elastic Beanstalk code on environment instances sends traffic to other AWS services, and other traffic to non\-AWS endpoints \(for example, to download dependency packages for your application\)\. Here are some steps you might need to take in this case to ensure that your environment works properly\.
+ *Configure VPC endpoints for Elastic Beanstalk* – Elastic Beanstalk and its enhanced health service support VPC endpoints, which ensure that traffic to these services stays inside the Amazon network and doesn't require internet access\. For more information, see [Using Elastic Beanstalk with VPC endpoints](vpc-vpce.md)\.
+ *Configure VPC endpoints for additional services* – Elastic Beanstalk instances send traffic to several other AWS services on your behalf: Amazon Simple Storage Service \(Amazon S3\), Amazon Simple Queue Service \(Amazon SQS\), AWS CloudFormation, and Amazon CloudWatch Logs\. You must configure VPC endpoints for these services too\. For detailed information about VPC endpoints, including per\-service links, see [VPC Endpoints](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints.html) in the *Amazon VPC User Guide*\.
**Note**  
Some AWS services, including Elastic Beanstalk, support VPC endpoints in a limited number of AWS Regions\. When you design your private VPC solution, verify that Elastic Beanstalk and the other dependent services mentioned here support VPC endpoints in the AWS Region that you choose\.
+ *Provide a private Docker image* – In a [Docker](create_deploy_docker.md) environment, code on the environment's instances might try to pull your configured Docker image from the internet during environment creation and fail\. To avoid this failure, [build a custom Docker image](single-container-docker-configuration.md#single-container-docker-configuration.dockerfile) on your environment, or use a Docker image stored in [Amazon Elastic Container Registry](https://docs.aws.amazon.com/AmazonECR/latest/userguide/) \(Amazon ECR\) and [configure a VPC endpoint for the Amazon ECR service](https://docs.aws.amazon.com/AmazonECR/latest/userguide/vpc-endpoints.html)\.
+ *Enable DNS names* – Elastic Beanstalk code on environment instances sends traffic to all AWS services using their public endpoints\. To ensure that this traffic goes through, choose the **Enable DNS name** option when you configure all interface VPC endpoints\. This adds a DNS entry in your VPC that maps the public service endpoint to the interface VPC endpoint\.
**Important**  
If your VPC isn't private and has public internet access, and if **Enable DNS name** is disabled for any VPC endpoint, traffic to the respective service travels through the public internet\. This is probably not what you intend\. It's easy to detect this issue with a private VPC, because it prevents this traffic from going through and you receive errors\. However, with a public facing VPC, you get no indication\.
+ *Include application dependencies* – If your application has dependencies such as language runtime packages, it might try to download and install them from the internet during environment creation and fail\. To avoid this failure, include all dependency packages in your application's source bundle\.
+ *Use a current platform version* – Be sure that your environment uses a platform version that was released on February 24, 2020 or later\. Specifically, use a platform version that was released in or after one of these two updates: [Linux Update 2020\-02\-28](https://docs.aws.amazon.com/elasticbeanstalk/latest/relnotes/release-2020-02-28-linux.html), [Windows Update 2020\-02\-24](https://docs.aws.amazon.com/elasticbeanstalk/latest/relnotes/release-2020-02-24-windows.html)\.
**Note**  
The reason for needing an updated platform version is that older versions had an issue that would prevent DNS entries created by the **Enable DNS name** option from working properly for Amazon SQS\.