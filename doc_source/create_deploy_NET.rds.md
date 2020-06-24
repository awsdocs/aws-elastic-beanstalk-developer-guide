# Adding an Amazon RDS DB instance to your \.NET application environment<a name="create_deploy_NET.rds"></a>

You can use an Amazon Relational Database Service \(Amazon RDS\) DB instance to store data gathered and modified by your application\. The database can be attached to your environment and managed by Elastic Beanstalk, or created and managed externally\.

If you are using Amazon RDS for the first time, [add a DB instance](#dotnet-rds-create) to a test environment with the Elastic Beanstalk console and verify that your application can connect to it\.

To connect to a database, [add the driver](#dotnet-rds-drivers) to your application, load the driver class in your code, and [create a connection string](#dotnet-rds-connect) with the environment properties provided by Elastic Beanstalk\. The configuration and connection code vary depending on the database engine and framework that you use\.

**Note**  
For learning purposes or test environments, you can use Elastic Beanstalk to add a DB instance\.  
For production environments, you can create a DB instance outside of your Elastic Beanstalk environment to decouple your environment resources from your database resources\. This way, when you terminate your environment, the DB instance isn't deleted\. An external DB instance also lets you connect to the same database from multiple environments and perform [blue\-green deployments](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.CNAMESwap.html)\. For instructions, see [Using Elastic Beanstalk with Amazon RDS](AWSHowTo.RDS.md)\.

**Topics**
+ [Adding a DB instance to your environment](#dotnet-rds-create)
+ [Downloading a driver](#dotnet-rds-drivers)
+ [Connecting to a database](#dotnet-rds-connect)

## Adding a DB instance to your environment<a name="dotnet-rds-create"></a>

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

## Downloading a driver<a name="dotnet-rds-drivers"></a>

Download and install the `EntityFramework` package and a database driver for your development environment with `NuGet`\.

**Common entity framework database providers for \.NET**
+ **SQL Server** – `Microsoft.EntityFrameworkCore.SqlServer`
+ **MySQL** – `Pomelo.EntityFrameworkCore.MySql`
+ **PostgreSQL** – `Npgsql.EntityFrameworkCore.PostgreSQL`

## Connecting to a database<a name="dotnet-rds-connect"></a>

Elastic Beanstalk provides connection information for attached DB instances in environment properties\. Use `ConfigurationManager.AppSettings` to read the properties and configure a database connection\.

**Example Helpers\.cs \- connection string method**  

```
using System;
using System.Collections.Generic;
using System.Configuration;
using System.Linq;
using System.Web;

namespace MVC5App.Models
{
  public class Helpers
  {
    public static string GetRDSConnectionString()
    {
      var appConfig = ConfigurationManager.AppSettings;

      string dbname = appConfig["RDS_DB_NAME"];

      if (string.IsNullOrEmpty(dbname)) return null;

      string username = appConfig["RDS_USERNAME"];
      string password = appConfig["RDS_PASSWORD"];
      string hostname = appConfig["RDS_HOSTNAME"];
      string port = appConfig["RDS_PORT"];

      return "Data Source=" + hostname + ";Initial Catalog=" + dbname + ";User ID=" + username + ";Password=" + password + ";";
    }
  }
}
```

Use the connection string to initialize your database context\.

**Example DBContext\.cs**  

```
using System.Data.Entity;
using System.Security.Claims;
using System.Threading.Tasks;
using Microsoft.AspNet.Identity;
using Microsoft.AspNet.Identity.EntityFramework;

namespace MVC5App.Models
{
  public class RDSContext : DbContext
  { 
    public RDSContext()
      : base(GetRDSConnectionString())
    {
    }

    public static RDSContext Create()
    {
      return new RDSContext();
    }
  }
}
```