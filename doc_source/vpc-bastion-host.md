# Example: Launching an Elastic Beanstalk application in a VPC with bastion hosts<a name="vpc-bastion-host"></a>

If your Amazon EC2 instances are located inside a private subnet, you will not be able to connect to them remotely\. To connect to your instances, you can set up bastion servers in the public subnet to act as proxies\. For example, you can set up SSH port forwarders or RDP gateways in the public subnet to proxy the traffic going to your database servers from your own network\. This section provides an example of how to create a VPC with a private and public subnet\. The instances are located inside the private subnet, and the bastion host, NAT gateway, and load balancer are located inside the public subnet\. Your infrastructure will look similar to the following diagram:

![\[Elastic Beanstalk and VPC topology with bastion host\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-vpc-bastion-topo-ngw.png)

To deploy an Elastic Beanstalk application inside a VPC using a bastion host, complete the steps described in the following subsections\.

**Topics**
+ [Create a VPC with a public and private subnet](#vpc-bastion-host-create)
+ [Create and configure the bastion host security group](#vpc-bastion-create-host-sg)
+ [Update the instance security group](#vpc-bastion-update-instance-sg)
+ [Create a bastion host](#vpc-bastion-host-launch)

## Create a VPC with a public and private subnet<a name="vpc-bastion-host-create"></a>

Complete all of the procedures in [Public/private VPC](vpc.md#services-vpc-privatepublic)\. When deploying the application, you must specify an Amazon EC2 key pair for the instances so you can connect to them remotely\. For more information about how to specify the instance key pair, see [Your Elastic Beanstalk environment's Amazon EC2 instances](using-features.managing.ec2.md)\.

## Create and configure the bastion host security group<a name="vpc-bastion-create-host-sg"></a>

Create a security group for the bastion host, and add rules that allow inbound SSH traffic from the Internet, and outbound SSH traffic to the private subnet that contains the Amazon EC2 instances\.

**To create the bastion host security group**

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

**To configure the bastion host security group**

1. In the list of security groups, select the check box for the security group you just created for your bastion host\.

1. On the **Inbound Rules** tab, choose **Edit**\.

1. If needed, choose **Add another rule**\.

1. If your bastion host is a Linux instance, under **Type**, select **SSH**\.

   If your bastion host is a Windows instance, under **Type**, select **RDP**\.

1. Enter the desired source CIDR range in the **Source** field and choose **Save**\.  
![\[Bastion host security group\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/vpc-bh-sg-inbound.png)

1. On the **Outbound Rules** tab, choose **Edit**\.

1. If needed, choose **Add another rule**\.

1. Under **Type**, select the type that you specified for the inbound rule\.

1. In the **Source** field, enter the CIDR range of the subnet of the hosts in the VPC's private subnet\.

   To find it:

   1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

   1. In the navigation pane, choose **Subnets**\.

   1. Note the value under **IPv4 CIDR** for each **Availability Zone** in which you have hosts that you want the bastion host to bridge to\.
**Note**  
If you have hosts in multiple availability zones, create an outbound rule for each one of these availability zones\.  
![\[VPC subnets\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/vpc-subnets.png)

1. Choose **Save**\.

## Update the instance security group<a name="vpc-bastion-update-instance-sg"></a>

By default, the security group you created for your instances does not allow incoming traffic\. While Elastic Beanstalk will modify the default group for the instances to allow SSH traffic, you must modify your custom instance security group to allow RDP traffic if your instances are Windows instances\.

**To update the instance security group for RDP**

1. In the list of security groups, select the check box for the instance security group\.

1. On the **Inbound** tab, choose **Edit**\.

1. If needed, choose **Add another rule**\.

1. Enter the following values, and choose **Save**\.   
**Type**  
`RDP`  
**Protocol**  
`TCP`  
**Port Range**  
`3389`  
**Source**  
Enter the ID of the bastion host security group \(e\.g\., `sg-8a6f71e8`\) and choose **Save**\.

## Create a bastion host<a name="vpc-bastion-host-launch"></a>

To create a bastion host, you launch an Amazon EC2 instance in your public subnet that will act as the bastion host\.

For more information about setting up a bastion host for Windows instances in the private subnet, see [ Controlling Network Access to EC2 Instances Using a Bastion Server ](http://aws.amazon.com/blogs/security/controlling-network-access-to-ec2-instances-using-a-bastion-server/)\.

For more information about setting up a bastion host for Linux instances in the private subnet, see [ Securely Connect to Linux Instances Running in a Private Amazon VPC ](http://aws.amazon.com/blogs/security/securely-connect-to-linux-instances-running-in-a-private-amazon-vpc/)\.