# Listing and connecting to server instances<a name="create_deploy_Java.ec2connect"></a>

 You can view a list of Amazon EC2 instances running your Elastic Beanstalk application environment through the AWS Toolkit for Eclipse or from the AWS Management Console\. You can connect to these instances using Secure Shell \(SSH\)\. For information about listing and connecting to your server instances using the AWS Management Console, see [Listing and connecting to server instances](using-features.ec2connect.md)\. The following section steps you through viewing and connecting you to your server instances using the AWS Toolkit for Eclipse\.

**To view and connect to Amazon EC2 instances for an environment**

1. In the AWS Toolkit for Eclipse, click **AWS Explorer**\. Expand the **Amazon EC2** node, and then double\-click **Instances**\. 

1. In the Amazon EC2 Instances window, in the **Instance ID** column, right\-click the **Instance ID** for the Amazon EC2 instance running in your application's load balancer\. Then click **Open Shell**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-eclipse-EC2instances.png)

    Eclipse automatically opens the SSH client and makes the connection to the EC2 instance\. 

    For more information on connecting to an Amazon EC2 instance, see the [Amazon Elastic Compute Cloud Getting Started Guide](http://docs.aws.amazon.com/AWSEC2/latest/GettingStartedGuide/)\. 