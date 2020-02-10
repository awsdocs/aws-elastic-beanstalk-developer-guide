# Listing and connecting to server instances<a name="create_deploy_NET.ec2connect"></a>

You can view a list of Amazon EC2 instances running your Elastic Beanstalk application environment through the AWS Toolkit for Visual Studio or from the AWS Management Console\. You can connect to these instances using Remote Desktop Connection\. For information about listing and connecting to your server instances using the AWS Management Console, see [Listing and connecting to server instances](using-features.ec2connect.md)\. The following section steps you through viewing and connecting you to your server instances using the AWS Toolkit for Visual Studio\.

**To view and connect to Amazon EC2 instances for an environment**

1.  In Visual Studio, in **AWS Explorer**, expand the **Amazon EC2** node and double\-click **Instances**\. 

1.  Right\-click the instance ID for the Amazon EC2 instance running in your application's load balancer in the **Instance** column and select **Open Remote Desktop** from the context menu\.   
![\[Open remote desktop dialog box\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-vs-rdp-login.png)

1.  Select **Use EC2 keypair to log on** and paste the contents of your private key file that you used to deploy your application in the **Private key** box\. Alternatively, enter your user name and password in the **User name** and **Password** text boxes\.
**Note**  
If the key pair is stored inside the Toolkit, the text box does not appear\.

1. Click **OK**\.