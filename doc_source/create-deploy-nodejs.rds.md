# Adding an Amazon RDS DB instance to your Node\.js application environment<a name="create-deploy-nodejs.rds"></a>

You can use an Amazon Relational Database Service \(Amazon RDS\) DB instance to store data gathered and modified by your application\. The database can be attached to your environment and managed by Elastic Beanstalk, or created and managed externally\.

If you are using Amazon RDS for the first time, [add a DB instance](#nodejs-rds-create) to a test environment with the Elastic Beanstalk Management Console and verify that your application is able to connect to it\.

To connect to a database, [add the driver](#nodejs-rds-drivers) to your application, load the driver in your code, and [create a connection object](#nodejs-rds-connect) with the environment properties provided by Elastic Beanstalk\. The configuration and connection code vary depending on the database engine and framework that you use\.

**Topics**
+ [Adding a DB instance to your environment](#nodejs-rds-create)
+ [Downloading a driver](#nodejs-rds-drivers)
+ [Connecting to a database](#nodejs-rds-connect)

## Adding a DB instance to your environment<a name="nodejs-rds-create"></a>

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
|  `RDS_PORT`  |  The port where the DB instance accepts connections\. The default value varies among DB engines\.  |  On the **Connectivity & security** tab on the Amazon RDS console: **Port**\.  | 
|  `RDS_DB_NAME`  |  The database name, **ebdb**\.  |  On the **Configuration** tab on the Amazon RDS console: **DB Name**\.  | 
|  `RDS_USERNAME`  |  The username that you configured for your database\.  |  On the **Configuration** tab on the Amazon RDS console: **Master username**\.  | 
|  `RDS_PASSWORD`  |  The password that you configured for your database\.  |  Not available for reference in the Amazon RDS console\.  | 

For more information about configuring an internal DB instance, see [Adding a database to your Elastic Beanstalk environment](using-features.managing.db.md)\.

## Downloading a driver<a name="nodejs-rds-drivers"></a>

Add the database driver to your project's [`package.json` file](nodejs-platform-dependencies.md#nodejs-platform-packagejson) under `dependencies`\.

**Example `package.json` – Express with MySQL**  

```
{
  "name": "my-app",
  "version": "0.0.1",
  "private": true,
  "dependencies": {
    "ejs": "latest",
    "aws-sdk": "latest",
    "express": "latest",
    "body-parser": "latest",
    "mysql": "latest"
  },
  "scripts": {
    "start": "node app.js"
  }
}
```

**Common driver packages for Node\.js**
+ **MySQL** – [mysql](https://www.npmjs.com/package/mysql)
+ **PostgreSQL** – [node\-postgres](https://www.npmjs.com/package/pg)
+ **SQL Server** – [node\-mssql](https://www.npmjs.com/package/mssql)
+ **Oracle** – [node\-oracledb](https://www.npmjs.com/package/oracledb)

## Connecting to a database<a name="nodejs-rds-connect"></a>

Elastic Beanstalk provides connection information for attached DB instances in environment properties\. Use `process.env.VARIABLE` to read the properties and configure a database connection\.

**Example app\.js – MySQL database connection**  

```
var mysql = require('mysql');

var connection = mysql.createConnection({
  host     : process.env.RDS_HOSTNAME,
  user     : process.env.RDS_USERNAME,
  password : process.env.RDS_PASSWORD,
  port     : process.env.RDS_PORT
});

connection.connect(function(err) {
  if (err) {
    console.error('Database connection failed: ' + err.stack);
    return;
  }

  console.log('Connected to database.');
});

connection.end();
```
For more information about constructing a connection string using node\-mysql, see [npmjs\.org/package/mysql](https://npmjs.org/package/mysql)\.