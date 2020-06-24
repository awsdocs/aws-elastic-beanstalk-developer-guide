# Launching and connecting to an external Amazon RDS instance in EC2 classic<a name="rds-external-ec2classic"></a>

If you use EC2 Classic \(no VPC\) with AWS Elastic Beanstalk, the procedure changes slightly due to differences in how security groups work\. In EC2 Classic, DB instances can't use EC2 security groups, so they get a DB security group that works only with Amazon RDS\.

You can add rules to a DB security group that allow ingress from EC2 security groups, but you cannot attach a DB security group to your environment's Auto Scaling group\. To avoid creating a dependency between the DB security group and your environment, you must create a third security group in Amazon EC2, add a rule in the DB security group to grant ingress to the new security group, and then assign it to the Auto Scaling group in your Elastic Beanstalk environment\.

**To launch an RDS instance in EC2 classic \(no VPC\)**

1. Open the [RDS management console](https://console.aws.amazon.com/rds/home)\.

1. Choose **Create database**\.

1. Proceed through the wizard\. Note the values that you enter for the following options:
   + **Master Username**
   + **Master Password**

1. When you reach **Configure advanced settings**, for **Network and Security** settings, choose the following:
   + **VPC** – **Not in VPC**\. If this option isn't available, your account might not support [EC2\-Classic](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-supported-platforms.html), or you may have chosen an [instance type that is only available in VPC](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-vpc.html#vpc-only-instance-types)\.
   + **Availability Zone** – **No Preference**
   + **DB Security Group\(s\)** – **Create new Security Group**

1. Configure the remaining options and choose **Create database**\. Note the values that you enter for the following options:
   + **Database Name**
   + **Database Port**

In EC2\-Classic, your DB instance will have a DB security group instead of a VPC security group\. You can't attach a DB security group to your Elastic Beanstalk environment, so you need to create a new security group that you can authorize to access the DB instance and attach to your environment\. We will refer to this as a *bridge security group* and name it **webapp\-bridge**\.

**To create a bridge security group**

1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/v2/home)\.

1. Choose **Security Groups** under **Network & Security** in the navigation sidebar\.

1. Choose **Create Security Group**\.

1. For **Security group name**, type **webapp\-bridge**\.

1. For **Description**, type **Provide access to DB instance from Elastic Beanstalk environment instances\.**

1. For **VPC**, leave the default selected\.

1. Choose **Create**

Next, modify the security group attached to your DB instance to allow inbound traffic from the bridge security group\.

**To modify the ingress rules on your RDS instance's security group**

1. Open the [Amazon RDS console](https://console.aws.amazon.com/rds/home)\.

1. Choose **Databases**\.

1. Choose the name of your DB instance to view its details\.

1. In the **Connectivity** section, under **Security**, the security group associated with the DB instance is shown\. Open the link to view the security group in the Amazon EC2 console\.

1. In the security group details, set **Connection Type** to **EC2 Security Group**\.

1. Set **EC2 Security Group Name** to the name of the bridge security group that you created\.

1. Choose **Authorize**\.

Next, add the bridge security group to your running environment\. This procedure requires all instances in your environment to be reprovisioned with the additional security group attached\.

**To add a security group to your environment**
+ Do one of the following:
  + To add a security group using the Elastic Beanstalk console

    1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

    1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

    1. In the navigation pane, choose **Configuration**\.

    1. In the **Instances** configuration category, choose **Edit**\.

    1. Under **EC2 security groups**, choose the security group to attach to the instances, in addition to the instance security group that Elastic Beanstalk creates\.

    1. Choose **Apply**\.

    1. Read the warning, and then choose **Confirm**\.
  + To add a security group using a [configuration file](ebextensions.md), use the [`securitygroup-addexisting.config` ](https://github.com/awsdocs/elastic-beanstalk-samples/tree/master/configuration-files/aws-provided/security-configuration/securitygroup-addexisting.config) example file\.

Next, pass the connection information to your environment by using environment properties\. When you [add a DB instance to your environment](using-features.managing.db.md) with the Elastic Beanstalk console, Elastic Beanstalk uses environment properties like **RDS\_HOSTNAME** to pass connection information to your application\. You can use the same properties, which will let you use the same application code with both integrated DB instances and external DB instances, or choose your own property names\.

**To configure environment properties**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Configuration**\.

1. In the **Software** configuration category, choose **Edit**\.

1. In the **Environment Properties** section, define the variables that your application reads to construct a connection string\. For compatibility with environments that have an integrated RDS instance, use the following:
   + **RDS\_DB\_NAME** – The **DB Name** shown in the Amazon RDS console\.
   + **RDS\_USERNAME** – The **Master Username** that you enter when you add the database to your environment\.
   + **RDS\_PASSWORD** – The **Master Password** that you enter when you add the database to your environment\.
   + **RDS\_HOSTNAME** – The **Endpoint** of the DB instance shown in the Amazon RDS console\. 
   + **RDS\_PORT** – The **Port** shown in the Amazon RDS console\.  
![\[Environment properties configuration section with RDS properties added\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environment-cfg-envprops-rds.png)

1. Choose **Apply**

If you haven't programmed your application to read environment properties and construct a connection string yet, see the following language\-specific topics for instructions:
+ Java SE – [Connecting to a database \(Java SE platforms\)](java-rds.md#java-rds-javase)
+ Java with Tomcat – [Connecting to a database \(Tomcat platforms\)](java-rds.md#java-rds-tomcat)
+ Node\.js – [Connecting to a database](create-deploy-nodejs.rds.md#nodejs-rds-connect)
+ \.NET – [Connecting to a database](create_deploy_NET.rds.md#dotnet-rds-connect)
+ PHP – [Connecting to a database with a PDO or MySQLi](create_deploy_PHP.rds.md#php-rds-connect)
+ Python – [Connecting to a database](create-deploy-python-rds.md#python-rds-connect)
+ Ruby – [Connecting to a database](create_deploy_Ruby.rds.md#ruby-rds-connect)

Finally, depending on when your application reads environment variables, you might need to restart the application server on the instances in your environment\.

**To restart your environment's app servers**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. Choose **Environment actions**, and then choose **Restart app server\(s\)**\.