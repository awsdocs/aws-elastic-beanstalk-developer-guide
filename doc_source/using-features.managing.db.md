# Adding a Database to Your Elastic Beanstalk Environment<a name="using-features.managing.db"></a>

Elastic Beanstalk provides integration with Amazon Relational Database Service \(Amazon RDS\) to help you add a database instance to your Elastic Beanstalk environment\. You can use Elastic Beanstalk to add a MySQL, PostgreSQL, Oracle, or SQL Server database to your environment during or after environment creation\. When you add a database instance to your environment, Elastic Beanstalk provides connection information to your application by setting environment properties for the database hostname, port, user name, password, and database name\.

A database instance that is part of your environment is tied to the lifecycle of your environment\. You can't remove it from your environment once added\. If you terminate the environment, the database instance is terminated as well\. You can configure Elastic Beanstalk to save a snapshot of the database when you terminate your environment, and restore a database from a snapshot when you add a DB instance to an environment\. You might incur charges for storing database snapshots\. For more information, see the *Backup Storage* section of [Amazon RDS Pricing](https://aws.amazon.com/rds/pricing/)\.

For a production environment, you can [launch a database instance outside of your environment](AWSHowTo.RDS.md) and configure your application to connect to it outside of the functionality provided by Elastic Beanstalk\. Using a database instance that is external to your environment requires additional security group and connection string configuration\. However, it also lets you connect to the database from multiple environments, use database types not supported with integrated databases, perform blue/green deployments, and tear down your environment without affecting the database instance\.


+ [Adding an Amazon RDS DB Instance to Your Environment](#environments-cfg-rds-create)
+ [Connecting to the database](#environments-cfg-rds-connect)
+ [Configuring an Integrated RDS DB Instance](#using-features.managing.db.CON)

## Adding an Amazon RDS DB Instance to Your Environment<a name="environments-cfg-rds-create"></a>

You can add a DB instance to your environment by using the Elastic Beanstalk console\.

**To add a DB instance to your environment**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. Choose **Configuration**\.

1. On the **Database** configuration card, choose **Modify**\.

1. Choose a DB engine, and enter a user name and password\.

1. Choose **Save**, and then choose **Apply**\.

You can configure the following options:

+ **Snapshot** – Choose an existing database snapshot\. Elastic Beanstalk restores the snapshot and adds it to your environment\. The default value is **None**, which lets you configure a new database using the other settings on this page\.

+ **Engine** – Choose a database engine\.

+ **Engine version** – Choose a specific version of the database engine\.

+ **Instance class** – Choose the DB instance class\. For information about the DB instance classes, see [https://aws\.amazon\.com/rds/](http://aws.amazon.com/rds/)\.

+ **Storage** – Choose the amount of storage to provision for your database\. You can increase allocated storage later, but you cannot decrease it\. For information about storage allocation, see [Features](https://aws.amazon.com/rds/#features)\.

+ **Username** – Type a user name using alphanumeric characters\.

+ **Password** – Type a password containing 8–16 printable ASCII characters \(excluding `/`, `\`, and `@`\)\.

+ **Retention** – Choose **Create snapshot** to create a snapshot of the database when you terminate your environment\.

+ **Availability** – Choose **High \(Multi\-AZ\)** to run a warm backup in a second Availability Zone for high availability\.

![\[Elastic Beanstalk Auto Scaling Configuration Window\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environments-cfg-rds-create.png)

Adding a DB instance takes about 10 minutes\. When the environment update is complete, the DB instance's hostname and other connection information are available to your application through the following environment properties:

+ **RDS\_HOSTNAME** – The hostname of the DB instance\.

  Amazon RDS console label – **Endpoint** \(this is the hostname\)

+ **RDS\_PORT** – The port on which the DB instance accepts connections\. The default value varies between DB engines\.

  Amazon RDS console label – **Port**

+ **RDS\_DB\_NAME** – The database name, `ebdb`\.

  Amazon RDS console label – **DB Name**

+ **RDS\_USERNAME** – The user name that you configured for your database\.

  Amazon RDS console label – **Username**

+ **RDS\_PASSWORD** – The password that you configured for your database\.

## Connecting to the database<a name="environments-cfg-rds-connect"></a>

Use the connectivity information to connect to your DB from inside your application through environment variables\. For more information about using Amazon RDS with your applications, see the following topics\.

+ Java SE – [Connecting to a Database \(Java SE Platforms\)](java-rds.md#java-rds-javase)

+ Java with Tomcat – [Connecting to a Database \(Tomcat Platforms\)](java-rds.md#java-rds-tomcat)

+ Node\.js – [Connecting to a Database](create-deploy-nodejs.rds.md#nodejs-rds-connect)

+ \.NET – [Connecting to a Database](create_deploy_NET.rds.md#dotnet-rds-connect)

+ PHP – [Connecting to a Database with a PDO or MySQLi](create_deploy_PHP.rds.md#php-rds-connect)

+ Python – [Connecting to a Database](create-deploy-python-rds.md#python-rds-connect)

+ Ruby – [Connecting to a Database](create_deploy_Ruby.rds.md#ruby-rds-connect)

## Configuring an Integrated RDS DB Instance<a name="using-features.managing.db.CON"></a>

You can view and modify configuration settings for your DB instance in the **Data Tier** section on the environment's **Configuration** page in the [environment management console](environments-console.md)\.

**To configure your environment's DB instance in the Elastic Beanstalk console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. Choose **Configuration**\.

1. On the **Database** configuration card, choose **Modify**\.

You can modify the **Instance class**, ****Storage**, Password**, **Retention**, and **Availability** settings after database creation\. If you change the instance class, Elastic Beanstalk reprovisions the DB instance\.

Do not modify settings on the DB instance outside of the functionality provided by Elastic Beanstalk \(for example, in the Amazon RDS console\)\.