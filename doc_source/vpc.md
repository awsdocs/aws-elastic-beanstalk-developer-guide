# Using Elastic Beanstalk with Amazon Virtual Private Cloud<a name="vpc"></a>

Amazon Virtual Private Cloud \(Amazon VPC\) enables you to define a virtual network in your own isolated section within the Amazon Web Services \(AWS\) cloud, known as a *virtual private cloud \(VPC\)*\. Using VPC, you can deploy a new class of web applications on Elastic Beanstalk, including internal web applications \(such as your recruiting application\), web applications that connect to an on\-premise database \(using a VPN connection\), as well as private web service backends\. Elastic Beanstalk launches your AWS resources, such as instances, into your VPC\. Your VPC closely resembles a traditional network, with the benefits of using AWS's scalable infrastructure\. You have complete control over your VPC; you can select the IP address range, create subnets, and configure routes and network gateways\. To protect the resources in each subnet, you can use multiple layers of security, including security groups and network access control lists\. For more information about Amazon VPC, go to the [Amazon VPC User Guide](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/)\.

**Note**  
Elastic Beanstalk does not currently support linux proxy settings \(HTTP\_PROXY, HTTPS\_PROXY and NO\_PROXY\) for configuring a web proxy\. Instances in your environment must have access to the Internet directly or through a NAT device\.

**Important**  
Instances in your Elastic Beanstalk environment use Network Time Protocol \(NTP\) to syncronize the system clock\. If instances are unable to communicate on UDP port 123, the clock may go out of sync, causing issues with Elastic Beanstalk health reporting\. Ensure that your VPC security groups and network ACLs allow inbound and outbound UDP traffic on port 123 to avoid these issues\.

## What VPC Configurations Do I Need?<a name="vpc-requirements"></a>

When you use Amazon VPC with Elastic Beanstalk, you can launch Elastic Beanstalk resources, such as Amazon EC2 instances, in a public or private subnet\. The subnets that you require depend on your Elastic Beanstalk application environment type and whether the resources you launch are public or private\. The following scenarios discuss sample VPC configurations that you might use for a particular environment\.


+ [Single\-instance environments](#w3ab1c27c76b9b7)
+ [Load\-balancing, autoscaling environments](#w3ab1c27c76b9b9)
+ [Extend your own network into AWS](#w3ab1c27c76b9c11)

### Single\-instance environments<a name="w3ab1c27c76b9b7"></a>

For single\-instance environments, Elastic Beanstalk assigns an Elastic IP address \(a static, public IP address\) to the instance so that it can communicate directly with the Internet\. No additional network interface, such as a network address translator \(NAT\), is required for a single\-instance environment\.

If you have a single\-instance environment without any associated private resources, such as a back\-end Amazon RDS DB instance, create a VPC with one public subnet, and include the instance in that subnet\. For more information, see [Example: Launching a Single\-Instance Environment without Any Associated Private Resources in a VPC](vpc-single-instance.md)\.

If you have resources that you don't want public, create a VPC with one public subnet and one private subnet\. Add all of your public resources, such as the single Amazon EC2 instance, in the public subnet, and add private resources such as a back\-end Amazon RDS DB instance in the private subnet\. If you do launch an Amazon RDS DB instance in a VPC, you must create at least two different private subnets that are in different Availability Zones \(an Amazon RDS requirement\)\.

### Load\-balancing, autoscaling environments<a name="w3ab1c27c76b9b9"></a>

For load\-balancing, autoscaling environments, you can either create a public and private subnet for your VPC, or use a single public subnet\. In the case of a load\-balancing, autoscaling environment, with both a public and private subnet, Amazon EC2 instances in the private subnet require Internet connectivity\. Consider the following scenarios:


+ [You want your Amazon EC2 instances to have a private IP address](#w3ab1c27c76b9b9b7)
+ [You have resources that are private](#w3ab1c27c76b9b9b9)
+ [You don't have any private resources](#w3ab1c27c76b9b9c11)
+ [You require direct access to your Amazon EC2 instances in a private subnet](#w3ab1c27c76b9b9c13)

#### You want your Amazon EC2 instances to have a private IP address<a name="w3ab1c27c76b9b9b7"></a>

Create a public and private subnet for your VPC in each Availability Zone \(an Elastic Beanstalk requirement\)\. Then add your public resources, such as the load balancer and NAT, to the public subnet\. Elastic Beanstalk assigns them a unique Elastic IP addresses \(a static, public IP address\)\. Launch your Amazon EC2 instances in the private subnet so that Elastic Beanstalk assigns them private IP addresses\.

Without a public IP address, an Amazon EC2 instance can't directly communicate with the Internet\. Although Amazon EC2 instances in a private subnet can't send outbound traffic by default, neither can they receive unsolicited inbound connections from the Internet\.

To enable communication between the private subnet, and the public subnet and the Internet beyond the public subnet, create routing rules that do the following:

+ Route all inbound traffic to your Amazon EC2 instances through the load balancer\.

+ Route all outbound traffic from your Amazon EC2 instances through the NAT device\.

#### You have resources that are private<a name="w3ab1c27c76b9b9b9"></a>

If you have associated resources that are private, such as a back\-end Amazon RDS DB instance, launch the resources in private subnets\. 

**Note**  
Amazon RDS requires at least two subnets, each in a separate Availability Zone\. For more information, see [Example: Launching an Elastic Beanstalk in a VPC with Amazon RDS](vpc-rds.md)\.

#### You don't have any private resources<a name="w3ab1c27c76b9b9c11"></a>

You can create a single public subnet for your VPC\. If you want to use a single public subnet, you must choose the **Associate Public IP Address** option to add the load balancer and your Amazon EC2 instances to the public subnet\. Elastic Beanstalk assigns a public IP address to each Amazon EC2 instance, and eliminates the need for a NAT device to allow the instances to communicate with the Internet\.

For more information, see [Example: Launching a Load\-Balancing, Autoscaling Environment with Public and Private Resources in a VPC](vpc-basic.md)\.

#### You require direct access to your Amazon EC2 instances in a private subnet<a name="w3ab1c27c76b9b9c13"></a>

If you require direct access to your Amazon EC2 instances in a private subnet \(for example, if you want to use SSH to sign in to an instance\), create a bastion host in the public subnet that proxies requests from the Internet\. From the Internet, you can connect to your instances by using the bastion host\. For more information, see [Example: Launching an Elastic Beanstalk Application in a VPC with Bastion Hosts](vpc-bastion-host.md)\.

### Extend your own network into AWS<a name="w3ab1c27c76b9c11"></a>

If you want to extend your own network into the cloud and also directly access the Internet from your VPC, create a VPN gateway\. For more information about creating a VPN Gateway, see [Scenario 3: VPC with Public and Private Subnets and Hardware VPN Access](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Scenario3.html) in the *Amazon VPC User Guide*\.