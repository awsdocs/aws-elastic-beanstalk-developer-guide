# Adding an Amazon RDS DB Instance to Your \.NET Application Environment<a name="create_deploy_NET.rds"></a>

You can use an Amazon Relational Database Service \(Amazon RDS\) DB instance to store data gathered and modified by your application\. The database can be attached to your environment and managed by Elastic Beanstalk, or created and managed externally\.

If you are using Amazon RDS for the first time, [add a DB instance](#dotnet-rds-create) to a test environment with the Elastic Beanstalk console and verify that your application can connect to it\.

To connect to a database, [add the driver](#dotnet-rds-drivers) to your application, load the driver class in your code, and [create a connection string](#dotnet-rds-connect) with the environment properties provided by Elastic Beanstalk\. The configuration and connection code vary depending on the database engine and framework that you use\.

For production environments, create a DB instance outside of your Elastic Beanstalk environment to decouple your environment resources from your database resources\. Using an external DB instance lets you connect to the same database from multiple environments and perform blue\-green deployments\. For instructions, see [Using Elastic Beanstalk with Amazon Relational Database Service](AWSHowTo.RDS.md)\.

**Topics**
+ [Adding a DB Instance to Your Environment](#dotnet-rds-create)
+ [Downloading a Driver](#dotnet-rds-drivers)
+ [Connecting to a Database](#dotnet-rds-connect)

## Adding a DB Instance to Your Environment<a name="dotnet-rds-create"></a>

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

## Downloading a Driver<a name="dotnet-rds-drivers"></a>

Download and install the `EntityFramework` package and a database driver for your development environment with `NuGet`\.

**Common Entity Framework Database Providers for \.NET**
+ **SQL Server** – `Microsoft.EntityFrameworkCore.SqlServer`
+ **MySQL** – `Pomelo.EntityFrameworkCore.MySql`
+ **PostgreSQL** – `Npgsql.EntityFrameworkCore.PostgreSQL`

## Connecting to a Database<a name="dotnet-rds-connect"></a>

Elastic Beanstalk provides connection information for attached DB instances in environment properties\. Use `ConfigurationManager.AppSettings` to read the properties and configure a database connection\.

**Example Helpers\.cs \- Connection String Method**  

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