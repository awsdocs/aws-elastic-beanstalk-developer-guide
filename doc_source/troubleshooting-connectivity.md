# Connectivity<a name="troubleshooting-connectivity"></a>

**Issue:** *Unable to connect to Amazon RDS from Elastic Beanstalk\.*

To connect RDS to your Elastic Beanstalk application, do the following:
+ Make sure RDS is in the same Region as your Elastic Beanstalk application\. 
+ Make sure the RDS security group for your instance has an authorization for the Amazon EC2 security group you are using for your Elastic Beanstalk environment\. For instructions on how to find the name of your EC2 security group using the AWS Management Console, see [Security groups](using-features.managing.ec2.md#using-features.managing.ec2.securitygroups)\. For more information about configuring your EC2 security group, go to the "Authorizing Network Access to an Amazon EC2 Security Group" section of [Working with DB Security Groups](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_WorkingWithSecurityGroups.html) in the *Amazon Relational Database Service User Guide*\.
+ For Java, make sure the MySQL JAR file is in your WEB\-INF/lib\. See [Adding an Amazon RDS DB instance to your Java application environment](java-rds.md) for more details\.

**Issue:** *Servers that were created in the Elastic Beanstalk console do not appear in the Toolkit for Eclipse*

You can manually import servers by following the instructions at [Importing existing environments into Eclipse](java-eclipsetoolkit.md#create_deploy_Java.howto.importenv)\.