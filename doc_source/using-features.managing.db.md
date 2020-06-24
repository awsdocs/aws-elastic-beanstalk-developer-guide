# Adding a database to your Elastic Beanstalk environment<a name="using-features.managing.db"></a>

Elastic Beanstalk provides integration with [Amazon Relational Database Service \(Amazon RDS\)](https://aws.amazon.com/rds/) to help you add a database instance to your Elastic Beanstalk environment\. You can use Elastic Beanstalk to add a MySQL, PostgreSQL, Oracle, or SQL Server database to your environment during or after environment creation\. When you add a database instance to your environment, Elastic Beanstalk provides connection information to your application by setting environment properties for the database hostname, port, user name, password, and database name\.

A database instance that is part of your environment is tied to the lifecycle of your environment\. You can't remove it from your environment once added\. If you terminate the environment, the database instance is terminated as well\. You can configure Elastic Beanstalk to save a snapshot of the database when you terminate your environment, and restore a database from a snapshot when you add a DB instance to an environment\. You might incur charges for storing database snapshots\. For more information, see the *Backup Storage* section of [Amazon RDS Pricing](https://aws.amazon.com/rds/pricing/)\.

For a production environment, you can [launch a database instance outside of your environment](AWSHowTo.RDS.md) and configure your application to connect to it outside of the functionality provided by Elastic Beanstalk\. Using a database instance that is external to your environment requires additional security group and connection string configuration\. However, it also lets you connect to the database from multiple environments, use database types not supported with integrated databases, perform blue/green deployments, and tear down your environment without affecting the database instance\.

**Topics**
+ [Adding an Amazon RDS DB instance to your environment](#environments-cfg-rds-create)
+ [Connecting to the database](#environments-cfg-rds-connect)
+ [Configuring an integrated RDS DB Instance using the console](#using-features.managing.db.CON)
+ [Configuring an integrated RDS DB Instance using configuration files](#using-features.managing.db.namespace)

## Adding an Amazon RDS DB instance to your environment<a name="environments-cfg-rds-create"></a>

You can add a DB instance to your environment by using the Elastic Beanstalk console\.

**To add a DB instance to your environment**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Configuration**\.

1. In the **Database** configuration category, choose **Edit**\.

1. Choose a DB engine, and enter a user name and password\.

1. Choose **Apply**\.

You can configure the following options:
+ **Snapshot** – Choose an existing database snapshot\. Elastic Beanstalk restores the snapshot and adds it to your environment\. The default value is **None**, which lets you configure a new database using the other settings on this page\.
+ **Engine** – Choose a database engine\.
+ **Engine version** – Choose a specific version of the database engine\.
+ **Instance class** – Choose the DB instance class\. For information about the DB instance classes, see [https://aws\.amazon\.com/rds/](http://aws.amazon.com/rds/)\.
+ **Storage** – Choose the amount of storage to provision for your database\. You can increase allocated storage later, but you cannot decrease it\. For information about storage allocation, see [Features](https://aws.amazon.com/rds/#features)\.
+ **Username** – Enter a user name of your choice using alphanumeric characters\.
+ **Password** – Enter a password of your choice containing 8–16 printable ASCII characters \(excluding `/`, `\`, and `@`\)\.
+ **Retention** – Choose **Create snapshot** to create a snapshot of the database when you terminate your environment\.
+ **Availability** – Choose **High \(Multi\-AZ\)** to run a warm backup in a second Availability Zone for high availability\.

**Note**  
Elastic Beanstalk creates a master user for the database using the user name and password you provide\. To learn more about the master user and its privileges, see [Master User Account Privileges](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/UsingWithRDS.MasterAccounts.html)\.

![\[Elastic Beanstalk Auto Scaling configuration window\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environments-cfg-rds-create.png)

Adding a DB instance takes about 10 minutes\. When the environment update is complete, the DB instance's hostname and other connection information are available to your application through the following environment properties:


| Property name | Description | Property value | 
| --- | --- | --- | 
|  `RDS_HOSTNAME`  |  The hostname of the DB instance\.  |  On the **Connectivity & security** tab on the Amazon RDS console: **Endpoint**\.  | 
|  `RDS_PORT`  |  The port on which the DB instance accepts connections\. The default value varies among DB engines\.  |  On the **Connectivity & security** tab on the Amazon RDS console: **Port**\.  | 
|  `RDS_DB_NAME`  |  The database name, **ebdb**\.  |  On the **Configuration** tab on the Amazon RDS console: **DB Name**\.  | 
|  `RDS_USERNAME`  |  The username that you configured for your database\.  |  On the **Configuration** tab on the Amazon RDS console: **Master username**\.  | 
|  `RDS_PASSWORD`  |  The password that you configured for your database\.  |  Not available for reference in the Amazon RDS console\.  | 

## Connecting to the database<a name="environments-cfg-rds-connect"></a>

Use the connectivity information to connect to your DB from inside your application through environment variables\. For more information about using Amazon RDS with your applications, see the following topics\.
+ Java SE – [Connecting to a database \(Java SE platforms\)](java-rds.md#java-rds-javase)
+ Java with Tomcat – [Connecting to a database \(Tomcat platforms\)](java-rds.md#java-rds-tomcat)
+ Node\.js – [Connecting to a database](create-deploy-nodejs.rds.md#nodejs-rds-connect)
+ \.NET – [Connecting to a database](create_deploy_NET.rds.md#dotnet-rds-connect)
+ PHP – [Connecting to a database with a PDO or MySQLi](create_deploy_PHP.rds.md#php-rds-connect)
+ Python – [Connecting to a database](create-deploy-python-rds.md#python-rds-connect)
+ Ruby – [Connecting to a database](create_deploy_Ruby.rds.md#ruby-rds-connect)

## Configuring an integrated RDS DB Instance using the console<a name="using-features.managing.db.CON"></a>

You can view and modify configuration settings for your DB instance in the **Database** section on the environment's **Configuration** page in the [Elastic Beanstalk console](environments-console.md)\.

**To configure your environment's DB instance in the Elastic Beanstalk console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Configuration**\.

1. In the **Database** configuration category, choose **Edit**\.

You can modify the **Instance class**, ****Storage**, Password**, **Retention**, and **Availability** settings after database creation\. If you change the instance class, Elastic Beanstalk re\-provisions the DB instance\.

**Warning**  
Don't modify settings on the DB instance outside of the functionality provided by Elastic Beanstalk \(for example, in the Amazon RDS console\)\. If you do, your Amazon RDS DB configuration might be out of sync with your environment's definition\. When you update or restart your environment, the settings specified in the environment override any settings you made outside of Elastic Beanstalk\.  
If you need to modify settings that Elastic Beanstalk doesn't directly support, use Elastic Beanstalk [configuration files](#using-features.managing.db.namespace)\.

## Configuring an integrated RDS DB Instance using configuration files<a name="using-features.managing.db.namespace"></a>

You can configure your environment's DB instance using [configuration files](ebextensions.md)\. Use the options in the [`aws:rds:dbinstance`](command-options-general.md#command-options-general-rdsdbinstance) namespace\. The following example modifies the allocated database storage size to 100 GB\.

**Example \.ebextensions/db\-instance\-options\.config**  

```
option_settings:
  aws:rds:dbinstance:
    DBAllocatedStorage: 100
```

If you need to configure DB instance properties that Elastic Beanstalk doesn't support, you can still use a configuration file, and specify your settings using the `resources` key\. The following example sets values to the `StorageType` and `Iops` Amazon RDS properties\.

**Example \.ebextensions/db\-instance\-properties\.config**  

```
Resources:
  AWSEBRDSDatabase:
    Type: AWS::RDS::DBInstance
    Properties:
      StorageType:io1
      Iops: 1000
```