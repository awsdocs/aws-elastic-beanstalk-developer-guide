# Configuring Amazon Virtual Private Cloud \(Amazon VPC\) with Elastic Beanstalk<a name="using-features.managing.vpc"></a>

[Amazon Virtual Private Cloud](https://docs.aws.amazon.com/vpc/latest/userguide/) \(Amazon VPC\) is the networking service that routes traffic securely to the EC2 instances that run your application in Elastic Beanstalk\. If you don't configure a VPC when you launch your environment, Elastic Beanstalk uses the default VPC\.

You can launch your environment in a custom VPC to customize networking and security settings\. Elastic Beanstalk lets you choose which subnets to use for your resources, and how to configure IP addresses for the instances and load balancer in your environment\. An environment is locked to a VPC when you create it, but you can change subnet and IP address settings on a running environment\.

**Note**  
If you created your AWS account before December 4, 2013, you might have environments using the Amazon EC2\-Classic network configuration in some AWS Regions instead of Amazon VPC\. For information on migrating your environments from an EC2\-Classic to a VPC network configuration, see [Migrating Elastic Beanstalk environments from EC2\-Classic to a VPC](vpc-ec2migration.md)\.

## Configuring VPC settings in the Elastic Beanstalk console<a name="environments-cfg-vpc-console"></a>

If you chose a custom VPC when you created your environment, you can modify its VPC settings in the Elastic Beanstalk console\.

**To configure your environment's VPC settings**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Configuration**\.

1. In the **Network** configuration category, choose **Edit**\.

The following settings are available\.

**Topics**
+ [VPC](#environments-cfg-vpc-console-vpc)
+ [Load balancer visibility](#environments-cfg-vpc-console-lbvisibility)
+ [Load balancer subnets](#environments-cfg-vpc-console-lbsubnets)
+ [Instance public IP address](#environments-cfg-vpc-console-ec2ip)
+ [Instance subnets](#environments-cfg-vpc-console-ec2subnets)
+ [Database subnets](#environments-cfg-vpc-console-dbsubnets)

### VPC<a name="environments-cfg-vpc-console-vpc"></a>

Choose a VPC for your environment\. You can only change this setting during environment creation\.

![\[VPC section in the Modify network configuration page on the Elastic Beanstalk console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environments-cfg-vpc-console-vpc.png)

### Load balancer visibility<a name="environments-cfg-vpc-console-lbvisibility"></a>

For a load\-balanced environment, choose the load balancer scheme\. By default, the load balancer is public, with a public IP address and domain name\. If your application only serves traffic from within your VPC or a connected VPN, deselect this option and choose private subnets for your load balancer to make the load balancer internal and disable access from the Internet\.

### Load balancer subnets<a name="environments-cfg-vpc-console-lbsubnets"></a>

For a load\-balanced environment, choose the subnets that your load balancer uses to serve traffic\. For a public application, choose public subnets\. Use subnets in multiple availability zones for high availability\. For an internal application, choose private subnets and disable load balancer visibility\.

![\[Load balancer settings section in the Modify network configuration page on the Elastic Beanstalk console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environments-cfg-vpc-console-lb.png)

### Instance public IP address<a name="environments-cfg-vpc-console-ec2ip"></a>

If you choose public subnets for your application instances, enable public IP addresses to make them routable from the Internet\.

### Instance subnets<a name="environments-cfg-vpc-console-ec2subnets"></a>

Choose subnets for your application instances\. Choose at least one subnet for each availability zone that your load balancer uses\. If you choose private subnets for your instances, your VPC must have a NAT gateway in a public subnet that the instances can use to access the Internet\.

![\[Instance settings section in the Modify network configuration page on the Elastic Beanstalk console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environments-cfg-vpc-console-ec2.png)

### Database subnets<a name="environments-cfg-vpc-console-dbsubnets"></a>

When you run an Amazon RDS database attached to your Elastic Beanstalk environment, choose subnets for your database instances\. For high availability, make the database multi\-AZ and choose a subnet for each availability zone\. To ensure that your application can connect to your database, run both in the same subnets\.

## The aws:ec2:vpc namespace<a name="environments-cfg-vpc-namespace"></a>

You can use the configuration options in the `aws:ec2:vpc` namespace to configure your environment's network settings\.

The following [configuration file](ebextensions.md) uses options in this namespace to set the environment's VPC and subnets for a public\-private configuration\. In order to set the VPC ID in a configuration file, the file must be included in the application source bundle during environment creation\. See [Setting configuration options during environment creation](environment-configuration-methods-during.md) for other methods of configuring these settings during environment creation\.

**Example \.ebextensions/vpc\.config – Public\-private**  

```
option_settings:
   aws:ec2:vpc:
      VPCId: vpc-087a68c03b9c50c84
      AssociatePublicIpAddress: 'false'
      ELBScheme: public
      ELBSubnets: subnet-0fe6b36bcb0ffc462,subnet-032fe3068297ac5b2
      Subnets: subnet-026c6117b178a9c45,subnet-0839e902f656e8bd1
```

This example shows a public\-public configuration, where the load balancer and EC2 instances run in the same public subnets\.

**Example \.ebextensions/vpc\.config – Public\-public**  

```
option_settings:
   aws:ec2:vpc:
      VPCId: vpc-087a68c03b9c50c84
      AssociatePublicIpAddress: 'true'
      ELBScheme: public
      ELBSubnets: subnet-0fe6b36bcb0ffc462,subnet-032fe3068297ac5b2
      Subnets: subnet-0fe6b36bcb0ffc462,subnet-032fe3068297ac5b2
```