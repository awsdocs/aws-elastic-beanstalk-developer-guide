# Connectivity<a name="troubleshooting-connectivity"></a>

**Important**  
The *Let's Encrypt* cross\-signed DST Root CA X3 certificate *expired* on *September 30, 2021*\. Due to this, Beanstalk environments running on the Amazon Linux 2 and Amazon Linux AMI operating systems might not be able to connect to servers using *Let's Encrypt* certificates\.  
On October 3, 2021 Elastic Beanstalk released new platform versions for Amazon Linux AMI and Amazon Linux 2 with the updated CA certificates\. To receive these updates and address this issue turn on [Managed Updates](environment-platform-update-managed.md) or [update your platforms manually](using-features.platform.upgrade.md#using-features.platform.upgrade.config)\. For more information, see the [platform update release notes](https://docs.aws.amazon.com/elasticbeanstalk/latest/relnotes/release-2021-10-03-linux.html) in the *AWS Elastic Beanstalk Release Notes*\.  
You can also apply the manual workarounds described in this [AWS Knowledge Center article](https://aws.amazon.com/premiumsupport/knowledge-center/ec2-expired-certificate/)\. Since Elastic Beanstalk provides AMIs with locked GUIDs, we recommend that you use the sudo yum install command in the article\. Alternatively, you can also use the sudo sed command in the article if you prefer to manually modify the system in place\.

**Issue:** *Unable to connect to Amazon RDS from Elastic Beanstalk\.*

To connect RDS to your Elastic Beanstalk application, do the following:
+ Make sure RDS is in the same Region as your Elastic Beanstalk application\. 
+ Make sure the RDS security group for your instance has an authorization for the Amazon EC2 security group you are using for your Elastic Beanstalk environment\. For instructions on how to find the name of your EC2 security group using the AWS Management Console, see [Security groups](using-features.managing.ec2.md#using-features.managing.ec2.securitygroups)\. For more information about configuring your EC2 security group, go to the "Authorizing Network Access to an Amazon EC2 Security Group" section of [Working with DB Security Groups](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_WorkingWithSecurityGroups.html) in the *Amazon Relational Database Service User Guide*\.
+ For Java, make sure the MySQL JAR file is in your WEB\-INF/lib\. See [Adding an Amazon RDS DB instance to your Java application environment](java-rds.md) for more details\.

**Issue:** *Servers that were created in the Elastic Beanstalk console do not appear in the Toolkit for Eclipse*

You can manually import servers by following the instructions at [Importing existing environments into Eclipse](java-eclipsetoolkit.md#create_deploy_Java.howto.importenv)\.