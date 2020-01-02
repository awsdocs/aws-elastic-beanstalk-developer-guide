# Launching and Connecting to an External Amazon RDS Instance in a Default VPC<a name="rds-external-defaultvpc"></a>

To use an external database with an application running in Elastic Beanstalk, first launch a DB instance with Amazon RDS\. Any instance that you launch with Amazon RDS is completely independent of Elastic Beanstalk and your Elastic Beanstalk environments, and is not dependent on Elastic Beanstalk for configuration\. This means that you can use any DB engine and instance type supported by Amazon RDS, even those not used by Elastic Beanstalk\.

The following procedures describe the process for a [default VPC](https://docs.aws.amazon.com/vpc/latest/userguide/default-vpc.html)\. The process is the same if you are using a custom VPC\. The only additional requirements are that your environment and DB instance are in the same subnet, or in subnets that are allowed to communicate with each other\. See [Using Elastic Beanstalk with Amazon Virtual Private Cloud](vpc.md) for details on configuring a custom VPC for use with Elastic Beanstalk\.

**To launch an RDS DB instance in a default VPC**

1. Open the [RDS console](https://console.aws.amazon.com/rds/home)\.

1. Choose **Databases** in the navigation pane\.

1. Choose **Create database**\.

1. Choose a database engine\. Choose **Next**\.

1. Choose a use case, if prompted\.

1. Under **Specify DB details**, review the default settings and adjust as necessary\. Pay attention to the following options:
   + **DB instance class** – Choose an instance size that has an appropriate amount of memory and CPU power for your workload\.
   + **Multi\-AZ deployment** – For high availability, set to **Create replica in different zone**\.
   + **Master username** and **Master password** – The database username and password\. Make a note of these settings because you'll use them later\.

1. Choose **Next**\.

1. Under **Database options**, for **Database name**, type **ebdb**\. Make a note of the **Database port** value for use later\.

1. Verify the default settings for the remaining options, and choose **Create database**\.

Next, modify the security group attached to your DB instance to allow inbound traffic on the appropriate port\. This is the same security group that you will attach to your Elastic Beanstalk environment later, so the rule that you add will grant ingress permission to other resources in the same security group\.

**To modify the ingress rules on your RDS instance's security group**

1. Open the [ Amazon RDS console](https://console.aws.amazon.com/rds/home)\.

1. Choose **Databases**\.

1. Choose the name of your DB instance to view its details\.

1. In the **Connectivity** section, note the **Subnets**, **Security groups**, and **Endpoint** shown on this page so you can use this information later\.

1. Under **Security**, you can see the security group associated with the DB instance\. Open the link to view the security group in the Amazon EC2 console\.  
![\[Connectivity section of a DB instance page in the Amazon RDS console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/rds-securitygroup.png)

1. In the security group details, choose **Inbound**\.

1. Choose **Edit**\.

1. Choose **Add Rule**\.

1. For **Type**, choose the DB engine that your application uses\.

1. For **Source**, type **sg\-** to view a list of available security groups\. Choose the current security group to allow resources in the security group to receive traffic on the database port from other resources in the same group\.  
![\[Edit the inbound rules for a security group in the Amazon EC2 console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/ec2-securitygroup-rds.png)

1. Choose **Save**\.

Next, add the DB instance's security group to your running environment\. This procedure causes Elastic Beanstalk to reprovision all instances in your environment with the additional security group attached\.

**To add a security group to your environment**
+ Do one of the following:
  + To add a security group using the Elastic Beanstalk console

    1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

    1. Navigate to the [management page](environments-console.md) for your environment\.

    1. Choose **Configuration**\.

    1. In the **Instances** configuration category, choose **Modify**\.

    1. Under **EC2 security groups**, choose the security group to attach to the instances, in addition to the instance security group that Elastic Beanstalk creates\.

    1. Choose **Apply**\.

    1. Read the warning, and then choose **Confirm**\.
  + To add a security group using a [configuration file](ebextensions.md), use the [`securitygroup-addexisting.config` ](https://github.com/awsdocs/elastic-beanstalk-samples/tree/master/configuration-files/aws-provided/security-configuration/securitygroup-addexisting.config) example file\.

Next, pass the connection information to your environment by using environment properties\. When you [add a DB instance to your environment](using-features.managing.db.md) with the Elastic Beanstalk console, Elastic Beanstalk uses environment properties like **RDS\_HOSTNAME** to pass connection information to your application\. You can use the same properties, which will let you use the same application code with both integrated DB instances and external DB instances, or choose your own property names\.

**To configure environment properties for an Amazon RDS DB instance**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. Choose **Configuration**\.

1. In the **Software** configuration category, choose **Modify**\.

1. In the **Environment properties** section, define the variables that your application reads to construct a connection string\. For compatibility with environments that have an integrated RDS DB instance, use the following\.
   + **RDS\_HOSTNAME** – The hostname of the DB instance\.

     Amazon RDS console label – **Endpoint** \(this is the hostname\)
   + **RDS\_PORT** – The port on which the DB instance accepts connections\. The default value varies among DB engines\.

     Amazon RDS console label – **Port**
   + **RDS\_DB\_NAME** – The database name, ebdb\.

     Amazon RDS console label – **DB Name**
   + **RDS\_USERNAME** – The user name that you configured for your database\.

     Amazon RDS console label – **Username**
   + **RDS\_PASSWORD** – The password that you configured for your database\.  
![\[Environment Properties section with RDS properties added\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environment-cfg-envprops-rds.png)

1. Choose **Apply**\.

If you haven't programmed your application to read environment properties and construct a connection string yet, see the following language\-specific topics for instructions:
+ Java SE – [Connecting to a Database \(Java SE Platforms\)](java-rds.md#java-rds-javase)
+ Java with Tomcat – [Connecting to a Database \(Tomcat Platforms\)](java-rds.md#java-rds-tomcat)
+ Node\.js – [Connecting to a Database](create-deploy-nodejs.rds.md#nodejs-rds-connect)
+ \.NET – [Connecting to a Database](create_deploy_NET.rds.md#dotnet-rds-connect)
+ PHP – [Connecting to a Database with a PDO or MySQLi](create_deploy_PHP.rds.md#php-rds-connect)
+ Python – [Connecting to a Database](create-deploy-python-rds.md#python-rds-connect)
+ Ruby – [Connecting to a Database](create_deploy_Ruby.rds.md#ruby-rds-connect)

Finally, depending on when your application reads environment variables, you might need to restart the application server on the instances in your environment\.

**To restart your environment's app servers**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. Choose **Actions**, and then choose **Restart App Server\(s\)**\.