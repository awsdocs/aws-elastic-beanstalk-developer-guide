# Adding an Amazon RDS DB Instance to Your Ruby Application Environment<a name="create_deploy_Ruby.rds"></a>

You can use an Amazon Relational Database Service \(Amazon RDS\) DB instance to store data gathered and modified by your application\. The database can be attached to your environment and managed by Elastic Beanstalk, or created and managed externally\.

If you are using Amazon RDS for the first time, [add a DB instance](#ruby-rds-create) to a test environment with the Elastic Beanstalk Management Console and verify that your application can connect to it\.

To connect to a database, [add the adapter](#ruby-rds-drivers) to your application and [configure a connection](#ruby-rds-connect) with the environment properties provided by Elastic Beanstalk\. The configuration and connection code vary depending on the database engine and framework that you use\.

For production environments, create a DB instance outside of your Elastic Beanstalk environment to decouple your environment resources from your database resources\. Using an external DB instance lets you connect to the same database from multiple environments and perform blue\-green deployments\. For instructions, see [Using Elastic Beanstalk with Amazon Relational Database Service](AWSHowTo.RDS.md)\.

**Topics**
+ [Adding a DB Instance to Your Environment](#ruby-rds-create)
+ [Downloading an Adapter](#ruby-rds-drivers)
+ [Connecting to a Database](#ruby-rds-connect)

## Adding a DB Instance to Your Environment<a name="ruby-rds-create"></a>

**To add a DB instance to your environment**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. Choose **Configuration**\.

1. On the **Database** configuration card, choose **Modify**\.

1. Choose a DB engine, and enter a user name and password\.

1. Choose **Apply**\.

Adding a DB instance takes about 10 minutes\. When the environment update is complete, the DB instance's hostname and other connection information are available to your application through the following environment properties:
+ **RDS\_HOSTNAME** – The hostname of the DB instance\.

  Amazon RDS console label – **Endpoint** \(this is the hostname\)
+ **RDS\_PORT** – The port on which the DB instance accepts connections\. The default value varies among DB engines\.

  Amazon RDS console label – **Port**
+ **RDS\_DB\_NAME** – The database name, ebdb\.

  Amazon RDS console label – **DB Name**
+ **RDS\_USERNAME** – The user name that you configured for your database\.

  Amazon RDS console label – **Username**
+ **RDS\_PASSWORD** – The password that you configured for your database\.

For more information about configuring an internal DB instance, see [Adding a Database to Your Elastic Beanstalk Environment](using-features.managing.db.md)\.

## Downloading an Adapter<a name="ruby-rds-drivers"></a>

Add the database adapter to your project's [gem file](ruby-platform-gemfile.md)\.

**Example Gemfile – Rails with MySQL**  

```
source 'https://rubygems.org'
gem 'puma'
gem 'rails', '4.1.8'
gem 'mysql2'
```

**Common Adapter Gems for Ruby**
+ **MySQL** – `mysql2`
+ **PostgreSQL** – `pg`
+ **Oracle** – `activerecord-oracle_enhanced-adapter`
+ **SQL Server** – ` sql_server`

## Connecting to a Database<a name="ruby-rds-connect"></a>

Elastic Beanstalk provides connection information for attached DB instances in environment properties\. Use `ENV['VARIABLE']` to read the properties and configure a database connection\.

**Example config/database\.yml – Ruby on Rails Database Configuration \(MySQL\)**  

```
production:
  adapter: mysql2
  encoding: utf8
  database: <%= ENV['RDS_DB_NAME'] %>
  username: <%= ENV['RDS_USERNAME'] %>
  password: <%= ENV['RDS_PASSWORD'] %>
  host: <%= ENV['RDS_HOSTNAME'] %>
  port: <%= ENV['RDS_PORT'] %>
```