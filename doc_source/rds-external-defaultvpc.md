# Launching and connecting to an external Amazon RDS instance in a default VPC<a name="rds-external-defaultvpc"></a>

To use an external database with an application that's running in Elastic Beanstalk you have two options\. Either, you can launch a DB instance with Amazon RDS\. Any instance that you launch with Amazon RDS is completely independent of Elastic Beanstalk and your Elastic Beanstalk environments\. This means that you can use any DB engine and instance type supported by Amazon RDS, even those that aren't used by Elastic Beanstalk\.

Or, as an alternative to launching a new DB instance, you can start with a database that was previously [created by Elastic Beanstalk](using-features.managing.db.md) and subsequently [decoupled](using-features.managing.db.md#using-features.decoupling.db) from a Beanstalk environment\. For more information, see [Adding a database to your Elastic Beanstalk environment](using-features.managing.db.md)\. With this option, you don't need to complete the procedure for launching a new database\. However, you do need to complete the subsequent procedures that are described in this topic\.

The following procedures describe the process for a [default VPC](https://docs.aws.amazon.com/vpc/latest/userguide/default-vpc.html)\. The process is the same if you're using a custom VPC\. The only additional requirements are that your environment and DB instance are in the same subnet, or in subnets that are allowed to communicate with each other\. For more information about configuring a custom VPC to use with Elastic Beanstalk, see [Using Elastic Beanstalk with Amazon VPC](vpc.md)\.

**Note**  
If you’re starting with a database that was created by Elastic Beanstalk and subsequently decoupled from a Beanstalk environment, you can skip the first group of steps and continue with the steps grouped under *To modify the inbound rules on your RDS instance's security group*\.
 If you plan to use the database that you decouple for a production environment, verify the storage type that the database uses is suitable for your workload\. For more information, see [DB Instance Storage](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_Storage.html) and [Modifying a DB instance](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Overview.DBInstance.Modifying.html) in the *Amazon RDS User Guide*\. 

**To launch an RDS DB instance in a default VPC**

1. Open the [RDS console](https://console.aws.amazon.com/rds/home)\.

1. In the navigation pane, choose **Databases**\.

1. Choose **Create database**\.

1. Choose **Standard Create**\.
**Important**  
Do not choose **Easy Create**\. If you choose it, you can't configure the necessary settings to launch this RDS DB\.

1. Under **Additional configuration**, for **Initial database name**, type **ebdb**\. 

1. Review the default settings and adjust these settings according to your specific requirements\. Pay attention to the following options:
   + **DB instance class** – Choose an instance size that has an appropriate amount of memory and CPU power for your workload\.
   + **Multi\-AZ deployment** – For high availability, set this to **Create an Aurora Replica/Reader node in a different AZ**\.
   + **Master username** and **Master password** – The database username and password\. Make a note of these settings because you will use them later\.

1. Verify the default settings for the remaining options, and then choose **Create database**\.

Next, modify the security group that's attached to your DB instance to allow inbound traffic on the appropriate port\. This is the same security group that you will attach to your Elastic Beanstalk environment later\. As a result, the rule that you add will grant inbound access permission to other resources in the same security group\.

**To modify the inbound rules on the security group that's attached to your RDS instance**

1. Open the [ Amazon RDS console](https://console.aws.amazon.com/rds/home)\.

1. Choose **Databases**\.

1. Choose the name of your DB instance to view its details\.

1. In the **Connectivity** section, make a note of the **Subnets**, **Security groups**, and **Endpoint** that are displayed on this page\. This is so you can use this information later\.

1. Under **Security**, you can see the security group that's associated with the DB instance\. Open the link to view the security group in the Amazon EC2 console\.  
![\[Connectivity section of a DB instance page in the Amazon RDS console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/rds-securitygroup.png)

1. In the security group details, choose **Inbound**\.

1. Choose **Edit**\.

1. Choose **Add Rule**\.

1. For **Type**, choose the DB engine that your application uses\.

1. For **Source**, type **sg\-** to view a list of available security groups\. Choose the security group that's associated with the Auto Scaling group that's used with your Elastic Beanstalk environment\. This is so that Amazon EC2 instances in the environment can have access to the database\.  
![\[Edit the inbound rules for a security group in the Amazon EC2 console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/ec2-securitygroup-rds.png)

1. Choose **Save**\.

Next, add the security group for the DB instance to your running environment\. In this procedure Elastic Beanstalk reprovisions all instances in your environment with the additional security group attached\.

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

Next, pass the connection information to your environment by using environment properties\. When you [add a DB instance to your environment](using-features.managing.db.md) with the Elastic Beanstalk console, Elastic Beanstalk uses environment properties, such as **RDS\_HOSTNAME**, to pass connection information to your application\. You can use the same properties\. By doing this, you use the same application code with both integrated DB instances and external DB instances\. Or, alternatively, you can choose your own property names\.

**To configure environment properties for an Amazon RDS DB instance**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Configuration**\.

1. In the **Software** configuration category, choose **Edit**\.

1. In the **Environment properties** section, define the variables that your application reads to construct a connection string\. For compatibility with environments that have an integrated RDS DB instance, use the following names and values\. You can find all values, except for your password, in the [RDS console](https://console.aws.amazon.com/rds/home)\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/rds-external-defaultvpc.html)  
![\[Environment properties configuration section with RDS properties added\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environment-cfg-envprops-rds.png)

1. Choose **Apply**\.

If you didn't program your application to read environment properties and construct a connection string yet, see the following language\-specific topics for instructions:
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