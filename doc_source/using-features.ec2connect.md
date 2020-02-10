# Listing and connecting to server instances<a name="using-features.ec2connect"></a>

 You can view a list of Amazon EC2 instances running your AWS Elastic Beanstalk application environment through the Elastic Beanstalk console\. You can connect to the instances using any SSH client\. You can connect to the instances running Windows using Remote Desktop\.

Some notes about specific development environments:
+ For more information about listing and connecting to server instances using the AWS Toolkit for Eclipse, see [Listing and connecting to server instances](create_deploy_Java.ec2connect.md)\.
+ For more information about listing and connecting to server instances using the AWS Toolkit for Visual Studio, see [Listing and connecting to server instances](create_deploy_NET.ec2connect.md)\.

**Important**  
Before you can access your Elastic Beanstalk–provisioned Amazon EC2 instances, you must create an Amazon EC2 key pair and configure your Elastic Beanstalk–provisioned Amazon EC2instances to use the Amazon EC2 key pair\. You can set up your Amazon EC2 key pairs using the [AWS Management Console](https://console.aws.amazon.com/)\. For instructions on creating a key pair for Amazon EC2, see the *Amazon EC2 Getting Started Guide*\. For more information on how to configure your Amazon EC2 instances to use an Amazon EC2 key pair, see [EC2 key pair](using-features.managing.security.md#using-features.managing.security.keypair)\.   
By default, Elastic Beanstalk does not enable remote connections to EC2 instances in a Windows container except for those in legacy Windows containers\. \(Elastic Beanstalk configures EC2 instances in legacy Windows containers to use port 3389 for RDP connections\.\) You can enable remote connections to your EC2 instances running Windows by adding a rule to a security group that authorizes inbound traffic to the instances\. We strongly recommend that you remove the rule when you end your remote connection\. You can add the rule again the next time you need to log in remotely\. For more information, see [Adding a Rule for Inbound RDP Traffic to a Windows Instance](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/authorizing-access-to-an-instance.html#authorizing-access-to-an-instance-rdp) and [Connect to Your Windows Instance](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/EC2Win_GetStarted.html#connecting_to_windows_instance) in the *Amazon Elastic Compute Cloud User Guide for Microsoft Windows*\.

**To view and connect to Amazon EC2 instances for an environment**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane of the console, choose **Load Balancers**\.  
![\[Amazon EC2 console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/clearbox-find-lb-01_2.png)

1.  Load balancers created by Elastic Beanstalk have **awseb** in the name\. Find the load balancer for your environment and click it\.   
![\[Amazon EC2 load balancers page\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/clearbox-view-ec2-instances.png)

1.  Choose the **Instances** tab in the bottom pane of the console\.   
![\[Instances tab in the Elastic Load Balancing page\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/clearbox-view-ec2-instances-1a.png)

    A list of the instances that the load balancer for your Elastic Beanstalk environment uses is displayed\. Make a note of an instance ID that you want to connect to\. 

1. In the navigation pane of the Amazon EC2 console, choose **Instances**, and find your instance ID in the list\.  
![\[Choosing an Amazon EC2 instance\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/clearbox-view-ec2-instances-3.png)

1. Right\-click the instance ID for the Amazon EC2 instance running in your environment's load balancer, and then select **Connect** from the context menu\.

1.  Make a note of the instance's public DNS address on the **Description** tab\.

1.  Connect to an instance running Linux by using the SSH client of your choice, and then type **ssh \-i \.ec2/mykeypair\.pem ec2\-user@<public\-DNS\-of\-the\-instance> **\.

For more information on connecting to an Amazon EC2 Linux instance, see [Getting Started with Amazon EC2 Linux Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html) in the *Amazon EC2 User Guide for Linux Instances*\.

If your Elastic Beanstalk environment uses the [\.NET on Windows Server platform](create_deploy_NET.container.console.md), see [Getting Started with Amazon EC2 Windows Instances](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/EC2_GetStarted.html) in the *Amazon EC2 User Guide for Windows Instances*\.