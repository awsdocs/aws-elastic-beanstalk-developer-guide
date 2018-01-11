# Listing and Connecting to Server Instances<a name="using-features.ec2connect"></a>

 You can view a list of Amazon EC2 instances running your Elastic Beanstalk application environment through the AWS Management Console\. You can connect to the instances using any SSH client\. For more information about listing and connecting to Server Instances using the AWS Toolkit for Eclipse, see [Listing and Connecting to Server Instances](create_deploy_Java.ec2connect.md)\. You can connect to the instances running Windows using Remote Desktop\. For more information about listing and connecting to Server Instances using the AWS Toolkit for Visual Studio, see [Listing and Connecting to Server Instances](create_deploy_NET.ec2connect.md)\.

**Important**  
You must create an Amazon EC2 key pair and configure your Elastic Beanstalk–provisioned Amazon EC2 instances to use the Amazon EC2 key pair before you can access your Elastic Beanstalk–provisioned Amazon EC2 instances\. You can set up your Amazon EC2 key pairs using the [AWS Management Console](https://console.aws.amazon.com/)\. For instructions on creating a key pair for Amazon EC2, go to the *Amazon EC2 Getting Started Guide*\. For more information on how to configure your Amazon EC2 instances to use an Amazon EC2 key pair, see [EC2 Key Pair](using-features.managing.ec2.md#using-features.managing.ec2.keypair)\.   
Elastic Beanstalk does not enable remote connections to EC2 instances in a Windows container by default except for legacy Windows containers\. \(Beanstalk configures EC2 instances in legacy Windows containers to use port 3389 for RDP connections\.\) You can enable remote connections to your EC2 instances running Windows by adding a rule to a security group that authorizes inbound traffic to the instances\. We strongly recommend that you remove the rule when you end your remote connection\. You can add the rule again the next time you need to log in remotely\. For more information, see [Adding a Rule for Inbound RDP Traffic to a Windows Instance](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/authorizing-access-to-an-instance.html#authorizing-access-to-an-instance-rdp) and [Connect to Your Windows Instance](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/EC2Win_GetStarted.html#connecting_to_windows_instance) in the *Amazon Elastic Compute Cloud User Guide for Microsoft Windows*\.

**To view and connect to Amazon EC2 instances for an environment**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation \(left\) pane of the console, click **Load Balancers**\.  
![\[Elastic Beanstalk Load Balancer Console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/clearbox-find-lb-01_2.png)

1.  Load balancers created by Elastic Beanstalk will have a **awseb** in the name\. Find the load balancer for your environment and click it\.   
![\[AWS Elastic Load Balancing Window\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/clearbox-view-ec2-instances.png)

1.  Click the **Instances** tab in the bottom pane of the console window\.   
![\[AWS Elastic Load Balancing Window\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/clearbox-view-ec2-instances-1a.png)

    A list of the instances that the load balancer for your Elastic Beanstalk environment uses is displayed\. Make a note of an instance ID that you want to connect to\. 

1. Click the **Instances** link in the left side of the Amazon EC2 console, and find your instance ID in the list\.  
![\[Amazon EC2 Instances\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/clearbox-view-ec2-instances-3.png)

1. Right\-click the instance ID for the Amazon EC2 instance running in your environment's load balancer, and then select **Connect** from the context menu\.

1.  Make a note of the instance's public DNS address on the **Description** tab\.

1.  To connect to an instance running Linux, use the SSH client of your choice to connect to your instance and type **ssh \-i \.ec2/mykeypair\.pem ec2\-user@<public\-DNS\-of\-the\-instance> **\. For instructions on how to connect to an instance running Windows, see [Connect to your Windows Instance](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/EC2Win_GetStarted.html#connecting_to_windows_instance) in the *Amazon Elastic Compute Cloud Microsoft Windows Guide*\. 

 For more information on connecting to an Amazon EC2 instance, see the [Amazon Elastic Compute Cloud Getting Started Guide](http://docs.aws.amazon.com/AWSEC2/latest/GettingStartedGuide/)\. 