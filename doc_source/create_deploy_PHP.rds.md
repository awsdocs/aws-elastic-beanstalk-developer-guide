# Adding an Amazon RDS DB instance to your PHP application environment<a name="create_deploy_PHP.rds"></a>

You can use an Amazon Relational Database Service \(Amazon RDS\) DB instance to store data gathered and modified by your application\. The database can be attached to your environment and managed by Elastic Beanstalk, or created and managed externally\.

If you are using Amazon RDS for the first time, [add a DB instance](#php-rds-create) to a test environment with the Elastic Beanstalk console and verify that your application can connect to it\.

To connect to a database, [add the driver](#php-rds-drivers) to your application, load the driver class in your code, and [create a connection object](#php-rds-connect) with the environment properties provided by Elastic Beanstalk\. The configuration and connection code vary depending on the database engine and framework that you use\.

**Note**  
For learning purposes or test environments, you can use Elastic Beanstalk to add a DB instance\.  
For production environments, you can create a DB instance outside of your Elastic Beanstalk environment to decouple your environment resources from your database resources\. This way, when you terminate your environment, the DB instance isn't deleted\. An external DB instance also lets you connect to the same database from multiple environments and perform [blue\-green deployments](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.CNAMESwap.html)\. For instructions, see [Using Elastic Beanstalk with Amazon RDS](AWSHowTo.RDS.md)\.

**Topics**
+ [Adding a DB instance to your environment](#php-rds-create)
+ [Downloading a driver](#php-rds-drivers)
+ [Connecting to a database with a PDO or MySQLi](#php-rds-connect)
+ [Connecting to a database with Symfony](#php-rds-symfony)

## Adding a DB instance to your environment<a name="php-rds-create"></a>

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

## Downloading a driver<a name="php-rds-drivers"></a>

To use PHP Data Objects \(PDO\) to connect to the database, install the driver that matches the database engine that you chose\.
+ **MySQL** – [http://php.net/manual/en/ref.pdo-mysql.php](http://php.net/manual/en/ref.pdo-mysql.php)
+ **PostgreSQL** – [http://php.net/manual/en/ref.pdo-pgsql.php](http://php.net/manual/en/ref.pdo-pgsql.php)
+ **Oracle** – [http://php.net/manual/en/ref.pdo-oci.php](http://php.net/manual/en/ref.pdo-oci.php)
+ **SQL Server** – [http://php.net/manual/en/ref.pdo-sqlsrv.php](http://php.net/manual/en/ref.pdo-sqlsrv.php)

For more information, see [http://php.net/manual/en/pdo.installation.php](http://php.net/manual/en/pdo.installation.php)\.

## Connecting to a database with a PDO or MySQLi<a name="php-rds-connect"></a>

You can use `$_SERVER[`VARIABLE`]` to read connection information from the environment\.

For a PDO, create a Data Source Name \(DSN\) from the host, port, and name\. Pass the DSN to the [constructor for the PDO](https://php.net/manual/en/pdo.construct.php) with the database user name and password\.

**Example Connect to an RDS database with PDO \- MySQL**  

```
<?php
$dbhost = $_SERVER['RDS_HOSTNAME'];
$dbport = $_SERVER['RDS_PORT'];
$dbname = $_SERVER['RDS_DB_NAME'];
$charset = 'utf8' ;

$dsn = "mysql:host={$dbhost};port={$dbport};dbname={$dbname};charset={$charset}";
$username = $_SERVER['RDS_USERNAME'];
$password = $_SERVER['RDS_PASSWORD'];

$pdo = new PDO($dsn, $username, $password);
?>
```

For other drivers, replace `mysql` with the name of your driver – `pgsql`, `oci`, or `sqlsrv`\.

For MySQLi, pass the hostname, user name, password, database name, and port to the `mysqli` constructor\.

**Example Connect to an RDS database with mysqli\_connect\(\)**  

```
$link = new mysqli($_SERVER['RDS_HOSTNAME'], $_SERVER['RDS_USERNAME'], $_SERVER['RDS_PASSWORD'], $_SERVER['RDS_DB_NAME'], $_SERVER['RDS_PORT']);
```

## Connecting to a database with Symfony<a name="php-rds-symfony"></a>

For Symfony version 3\.2 and newer, you can use `%env(PROPERTY_NAME)%` to set database parameters in a configuration file based on the environment properties set by Elastic Beanstalk\.

**Example app/config/parameters\.yml**  

```
parameters:
    database_driver:   pdo_mysql
    database_host:     '%env(RDS_HOSTNAME)%'
    database_port:     '%env(RDS_PORT)%'
    database_name:     '%env(RDS_DB_NAME)%'
    database_user:     '%env(RDS_USERNAME)%'
    database_password: '%env(RDS_PASSWORD)%'
```

See [External Parameters \(Symfony 3\.4\)](http://symfony.com/doc/3.4/configuration/external_parameters.html) for more information\.

For earlier versions of Symfony, environment variables are only accessible if they start with `SYMFONY__`\. This means that the Elastic Beanstalk\-defined environment properties are not accessible, and you must define your own environment properties to pass the connection information to Symfony\.

To connect to a database with Symfony 2, [create an environment property](create_deploy_PHP.container.md#php-console-properties) for each parameter\. Then, use `%property.name%` to access the Symfony\-transformed variable in a configuration file\. For example, an environment property named `SYMFONY__DATABASE__USER` is accessible as `database.user`\.

```
    database_user:     "%database.user%"
```

See [External Parameters \(Symfony 2\.8\)](http://symfony.com/doc/2.8/configuration/external_parameters.html) for more information\.