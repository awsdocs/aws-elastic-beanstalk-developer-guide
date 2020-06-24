# Adding an Amazon RDS DB instance to your Python application environment<a name="create-deploy-python-rds"></a>

You can use an Amazon Relational Database Service \(Amazon RDS\) DB instance to store data gathered and modified by your application\. The database can be attached to your environment and managed by Elastic Beanstalk, or created and managed externally\.

If you are using Amazon RDS for the first time, [add a DB instance](#python-rds-create) to a test environment with the Elastic Beanstalk Management Console and verify that your application can connect to it\.

To connect to a database, [add the driver](#python-rds-drivers) to your application, load the driver in your code, and [create a connection object](#python-rds-connect) with the environment properties provided by Elastic Beanstalk\. The configuration and connection code vary depending on the database engine and framework that you use\.

**Note**  
For learning purposes or test environments, you can use Elastic Beanstalk to add a DB instance\.  
For production environments, you can create a DB instance outside of your Elastic Beanstalk environment to decouple your environment resources from your database resources\. This way, when you terminate your environment, the DB instance isn't deleted\. An external DB instance also lets you connect to the same database from multiple environments and perform [blue\-green deployments](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.CNAMESwap.html)\. For instructions, see [Using Elastic Beanstalk with Amazon RDS](AWSHowTo.RDS.md)\.

**Topics**
+ [Adding a DB instance to your environment](#python-rds-create)
+ [Downloading a driver](#python-rds-drivers)
+ [Connecting to a database](#python-rds-connect)

## Adding a DB instance to your environment<a name="python-rds-create"></a>

**To add a DB instance to your environment**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Configuration**\.

1. In the **Database** configuration category, choose **Edit**\.

1. Choose a DB engine, and enter a user name and password\.

1. Choose **Apply**\.

Adding a DB instance takes about 10 minutes\. When the environment update is complete, the DB instance's hostname and other connection information are available to your application through the following environment properties:


| Property name | Description | Property value | 
| --- | --- | --- | 
|  `RDS_HOSTNAME`  |  The hostname of the DB instance\.  |  On the **Connectivity & security** tab on the Amazon RDS console: **Endpoint**\.  | 
|  `RDS_PORT`  |  The port on which the DB instance accepts connections\. The default value varies among DB engines\.  |  On the **Connectivity & security** tab on the Amazon RDS console: **Port**\.  | 
|  `RDS_DB_NAME`  |  The database name, **ebdb**\.  |  On the **Configuration** tab on the Amazon RDS console: **DB Name**\.  | 
|  `RDS_USERNAME`  |  The username that you configured for your database\.  |  On the **Configuration** tab on the Amazon RDS console: **Master username**\.  | 
|  `RDS_PASSWORD`  |  The password that you configured for your database\.  |  Not available for reference in the Amazon RDS console\.  | 

For more information about configuring an internal DB instance, see [Adding a database to your Elastic Beanstalk environment](using-features.managing.db.md)\.

## Downloading a driver<a name="python-rds-drivers"></a>

Add the database driver to your project's [requirements file](python-configuration-requirements.md)\.

**Example requirements\.txt – Django with MySQL**  

```
Django==2.0.3
mysqlclient==1.3.12
```

**Common driver packages for Python**
+ **MySQL** – `MySQL-python` \(Python 2\) , `mysqlclient` \(Python 3\)
+ **PostgreSQL** – `psycopg2`
+ **Oracle** – `cx_Oracle`
+ **SQL Server** – `adodbapi`

## Connecting to a database<a name="python-rds-connect"></a>

Elastic Beanstalk provides connection information for attached DB instances in environment properties\. Use `os.environ['VARIABLE']` to read the properties and configure a database connection\.

**Example Django settings file – DATABASES dictionary**  

```
import os

if 'RDS_HOSTNAME' in os.environ:
    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.mysql',
            'NAME': os.environ['RDS_DB_NAME'],
            'USER': os.environ['RDS_USERNAME'],
            'PASSWORD': os.environ['RDS_PASSWORD'],
            'HOST': os.environ['RDS_HOSTNAME'],
            'PORT': os.environ['RDS_PORT'],
        }
    }
```