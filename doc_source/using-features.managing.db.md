# Adding a Database to Your Elastic Beanstalk Environment<a name="using-features.managing.db"></a>

Elastic Beanstalk provides integration with Amazon RDS to help you add a database instance to your Elastic Beanstalk environment\. You can use Elastic Beanstalk to add a MySQL, PostgreSQL, Oracle, or SQL Server database to your environment during or after environment creation\. When you add a database instance to your environment, Elastic Beanstalk provides connection information to your application by setting environment properties for the database hostname, port, username, password, and database name\.

A database instance that is part of your environment is tied to the lifecycle of your environment, and cannot be removed from your environment once added\. If you terminate the environment, the database instance is terminated as well\. You can configure Elastic Beanstalk to save a snapshot of the database when you terminate your environment, and restore a database from a snapshot when you add a DB instance to an environment\. You may incur charges for storing database snapshots\. For more information, see the *Backup Storage* section of [Amazon RDS Pricing](http://aws.amazon.com/rds/pricing/)\.

For a production environment, you may want to launch a database instance outside of your environment and configure your application to connect to it outside of the functionality provided by Elastic Beanstalk\. Using a database instance external to your environment requires additional security group and connection string configuration, but it also lets you connect to the database from multiple environments, use database types not supported with integrated databases, perform blue/green deployments, and tear down your environment without affecting the database instance\.


+ [Adding an Amazon RDS DB Instance to Your Environment](#environments-cfg-rds-create)
+ [Connecting to the database](#environments-cfg-rds-connect)
+ [Configuring an Integrated RDS DB Instance](#using-features.managing.db.CON)

## Adding an Amazon RDS DB Instance to Your Environment<a name="environments-cfg-rds-create"></a>

You can add a DB instance to your environment in the Elastic Beanstalk console\.

**To add a DB instance to your environment**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the management page for your environment\.

1. Choose **Configuration**\.

1. Under **Data Tier**, choose **Create a new RDS database**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-config-db.png)

1. Choose a DB engine, enter a user name and password, and then choose **Apply**\.

You can configure the following options:

+ **DB engine** – Choose a database engine\.

+ **Instance Class** – Choose the DB instance class\. For information about the DB instance classes, see [http://aws\.amazon\.com/rds/](http://aws.amazon.com/rds/)\.

+ **Allocated Storage** – Choose the amount of storage to provision for your database\. You can increase allocated storage later, but you cannot decrease it\. For information about storage allocation, see [Features](https://aws.amazon.com/rds/#features)\.

+ **Master Username** – Type a username using alphanumeric characters\.

+ **Master Password** – Type a password containing 8 to 16 printable ASCII characters \(excluding `/`, `\`, and `@`\)\.

+ **Deletion Policy** – Choose **Create snapshot** to create a snapshot of the database when you terminate your environment\.

+ **Snapshot** – To restore a database from an existing snapshot, choose a snapshot\.

+ **Availability** – Choose **Multiple Availability Zones** to run a warm backup in a second AZ for high availability\.

Adding a DB instance takes about 10 minutes\. When the environment update is complete, the DB instance's hostname and other connection information are available to your application through the following environment properties:

+ **RDS\_HOSTNAME** – The hostname of the DB instance\.

  Amazon RDS console label – **Endpoint** is the hostname\.

+ **RDS\_PORT** – The port on which the DB instance accepts connections\. The default value varies between DB engines\.

  Amazon RDS console label – **Port**

+ **RDS\_DB\_NAME** – The database name, `ebdb`\.

  Amazon RDS console label – **DB Name**

+ **RDS\_USERNAME** – The user name that you configured for your database\.

  Amazon RDS console label – **Username**

+ **RDS\_PASSWORD** – The password that you configured for your database\.

## Connecting to the database<a name="environments-cfg-rds-connect"></a>

Use the connectivity information to connect to your DB from inside your application through environment variables\. For more information about using Amazon RDS with your applications, see the following topics\.

+ Java SE – 

+ Java with Tomcat – 

+ Node\.js – 

+ \.NET – 

+ PHP – 

+ Python – 

+ Ruby – 

## Configuring an Integrated RDS DB Instance<a name="using-features.managing.db.CON"></a>

You can view and modify configuration settings for your DB instance in the **Data Tier** section on the environment's **Configuration** page in the environment management console\.

**To configure your environment's DB instance in the Elastic Beanstalk console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the management page for your environment\.

1. Choose **Configuration**\.

1. Under **Data Tier**, choose **RDS**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environment-cfg-rds-card.png)

You can modify the **Master Password**, **Allocated Storage**, **Instance Class**, **Deletion Policy**, and **Availability** after database creation\. Changing the instance class requires Elastic Beanstalk to reprovision the DB instance\.

Do not modify settings on the DB instance outside of the functionality provided by Elastic Beanstalk \(for example, in the Amazon RDS console\)\.