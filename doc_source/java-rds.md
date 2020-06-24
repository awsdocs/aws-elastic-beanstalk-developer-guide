# Adding an Amazon RDS DB instance to your Java application environment<a name="java-rds"></a>

You can use an Amazon Relational Database Service \(Amazon RDS\) DB instance to store data that your application gathers and modifies\. The database can be attached to your environment and managed by Elastic Beanstalk, or created and managed externally\.

If you are using Amazon RDS for the first time, add a DB instance to a test environment by using the Elastic Beanstalk console and verify that your application can connect to it\. 

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

For more information about configuring an internal DB instance, see [Adding a database to your Elastic Beanstalk environment](using-features.managing.db.md)\. For instructions on configuring an external database for use with Elastic Beanstalk, see [Using Elastic Beanstalk with Amazon RDS](AWSHowTo.RDS.md)\.

To connect to the database, add the appropriate driver JAR file to your application, load the driver class in your code, and create a connection object with the environment properties provided by Elastic Beanstalk\.

**Topics**
+ [Downloading the JDBC driver](#java-rds-drivers)
+ [Connecting to a database \(Java SE platforms\)](#java-rds-javase)
+ [Connecting to a database \(Tomcat platforms\)](#java-rds-tomcat)
+ [Troubleshooting database connections](#create_deploy_Java.rds.troubleshooting)

## Downloading the JDBC driver<a name="java-rds-drivers"></a>

You will need the JAR file of the JDBC driver for the DB engine that you choose\. Save the JAR file in your source code and include it in your classpath when you compile the class that creates connections to the database\.

You can find the latest driver for your DB engine in the following locations:
+ **MySQL** – [MySQL Connector/J](https://dev.mysql.com/downloads/connector/j/)
+ **Oracle SE\-1** – [Oracle JDBC Driver](http://www.oracle.com/technetwork/database/features/jdbc/index-091264.html)
+ **Postgres** – [PostgreSQL JDBC Driver](https://jdbc.postgresql.org/)
+ **SQL Server** – [Microsoft JDBC Driver](https://msdn.microsoft.com/en-us/sqlserver/aa937724.aspx)

To use the JDBC driver, call `Class.forName()` to load it before creating the connection with `DriverManager.getConnection()` in your code\.

JDBC uses a connection string in the following format:

```
jdbc:driver://hostname:port/dbName?user=userName&password=password
```

You can retrieve the hostname, port, database name, user name, and password from the environment variables that Elastic Beanstalk provides to your application\. The driver name is specific to your database type and driver version\. The following are example driver names:
+ `mysql` for MySQL
+ `postgresql` for PostgreSQL
+ `oracle:thin` for Oracle Thin
+ `oracle:oci` for Oracle OCI
+ `oracle:oci8` for Oracle OCI 8
+ `oracle:kprb` for Oracle KPRB
+ `sqlserver` for SQL Server

## Connecting to a database \(Java SE platforms\)<a name="java-rds-javase"></a>

In a Java SE environment, use `System.getenv()` to read the connection variables from the environment\. The following example code shows a class that creates a connection to a PostgreSQL database\.

```
private static Connection getRemoteConnection() {
    if (System.getenv("RDS_HOSTNAME") != null) {
      try {
      Class.forName("org.postgresql.Driver");
      String dbName = System.getenv("RDS_DB_NAME");
      String userName = System.getenv("RDS_USERNAME");
      String password = System.getenv("RDS_PASSWORD");
      String hostname = System.getenv("RDS_HOSTNAME");
      String port = System.getenv("RDS_PORT");
      String jdbcUrl = "jdbc:postgresql://" + hostname + ":" + port + "/" + dbName + "?user=" + userName + "&password=" + password;
      logger.trace("Getting remote connection with connection string from environment variables.");
      Connection con = DriverManager.getConnection(jdbcUrl);
      logger.info("Remote connection successful.");
      return con;
    }
    catch (ClassNotFoundException e) { logger.warn(e.toString());}
    catch (SQLException e) { logger.warn(e.toString());}
    }
    return null;
  }
```

## Connecting to a database \(Tomcat platforms\)<a name="java-rds-tomcat"></a>

In a Tomcat environment, environment properties are provided as system properties that are accessible with `System.getProperty()`\.

The following example code shows a class that creates a connection to a PostgreSQL database\.

```
private static Connection getRemoteConnection() {
    if (System.getProperty("RDS_HOSTNAME") != null) {
      try {
      Class.forName("org.postgresql.Driver");
      String dbName = System.getProperty("RDS_DB_NAME");
      String userName = System.getProperty("RDS_USERNAME");
      String password = System.getProperty("RDS_PASSWORD");
      String hostname = System.getProperty("RDS_HOSTNAME");
      String port = System.getProperty("RDS_PORT");
      String jdbcUrl = "jdbc:postgresql://" + hostname + ":" + port + "/" + dbName + "?user=" + userName + "&password=" + password;
      logger.trace("Getting remote connection with connection string from environment variables.");
      Connection con = DriverManager.getConnection(jdbcUrl);
      logger.info("Remote connection successful.");
      return con;
    }
    catch (ClassNotFoundException e) { logger.warn(e.toString());}
    catch (SQLException e) { logger.warn(e.toString());}
    }
    return null;
  }
```

If you have trouble getting a connection or running SQL statements, try placing the following code in a JSP file\. This code connects to a DB instance, creates a table, and writes to it\.

```
<%@ page import="java.sql.*" %>
<%
  // Read RDS connection information from the environment
  String dbName = System.getProperty("RDS_DB_NAME");
  String userName = System.getProperty("RDS_USERNAME");
  String password = System.getProperty("RDS_PASSWORD");
  String hostname = System.getProperty("RDS_HOSTNAME");
  String port = System.getProperty("RDS_PORT");
  String jdbcUrl = "jdbc:mysql://" + hostname + ":" +
    port + "/" + dbName + "?user=" + userName + "&password=" + password;
  
  // Load the JDBC driver
  try {
    System.out.println("Loading driver...");
    Class.forName("com.mysql.jdbc.Driver");
    System.out.println("Driver loaded!");
  } catch (ClassNotFoundException e) {
    throw new RuntimeException("Cannot find the driver in the classpath!", e);
  }

  Connection conn = null;
  Statement setupStatement = null;
  Statement readStatement = null;
  ResultSet resultSet = null;
  String results = "";
  int numresults = 0;
  String statement = null;

  try {
    // Create connection to RDS DB instance
    conn = DriverManager.getConnection(jdbcUrl);
    
    // Create a table and write two rows
    setupStatement = conn.createStatement();
    String createTable = "CREATE TABLE Beanstalk (Resource char(50));";
    String insertRow1 = "INSERT INTO Beanstalk (Resource) VALUES ('EC2 Instance');";
    String insertRow2 = "INSERT INTO Beanstalk (Resource) VALUES ('RDS Instance');";
    
    setupStatement.addBatch(createTable);
    setupStatement.addBatch(insertRow1);
    setupStatement.addBatch(insertRow2);
    setupStatement.executeBatch();
    setupStatement.close();
    
  } catch (SQLException ex) {
    // Handle any errors
    System.out.println("SQLException: " + ex.getMessage());
    System.out.println("SQLState: " + ex.getSQLState());
    System.out.println("VendorError: " + ex.getErrorCode());
  } finally {
    System.out.println("Closing the connection.");
    if (conn != null) try { conn.close(); } catch (SQLException ignore) {}
  }

  try {
    conn = DriverManager.getConnection(jdbcUrl);
    
    readStatement = conn.createStatement();
    resultSet = readStatement.executeQuery("SELECT Resource FROM Beanstalk;");

    resultSet.first();
    results = resultSet.getString("Resource");
    resultSet.next();
    results += ", " + resultSet.getString("Resource");
    
    resultSet.close();
    readStatement.close();
    conn.close();

  } catch (SQLException ex) {
    // Handle any errors
    System.out.println("SQLException: " + ex.getMessage());
    System.out.println("SQLState: " + ex.getSQLState());
    System.out.println("VendorError: " + ex.getErrorCode());
  } finally {
       System.out.println("Closing the connection.");
      if (conn != null) try { conn.close(); } catch (SQLException ignore) {}
  }
%>
```

To display the results, place the following code in the body of the HTML portion of the JSP file\.

```
<p>Established connection to RDS. Read first two rows: <%= results %></p>
```

## Troubleshooting database connections<a name="create_deploy_Java.rds.troubleshooting"></a>

If you run into issues connecting to a database from within your application, review the web container log and database\.

### Reviewing logs<a name="create_deploy_Java.rds.troubleshooting.logs"></a>

You can view all the logs from your Elastic Beanstalk environment from within Eclipse\. If you don't have the AWS Explorer view open, choose the arrow next to the orange AWS icon in the toolbar, and then choose **Show AWS Explorer View**\. Expand **AWS Elastic Beanstalk** and your environment name, and then open the context \(right\-click\) menu for the server\. Choose **Open in WTP Server Editor**\. 

 Choose the **Log** tab of the **Server** view to see the aggregate logs from your environment\. To open the latest logs, choose the **Refresh** button at the upper right corner of the page\. 

 Scroll down to locate the Tomcat logs in `/var/log/tomcat7/catalina.out`\. If you loaded the webpage from our earlier example several times, you might see the following\. 

```
-------------------------------------
/var/log/tomcat7/catalina.out
-------------------------------------
INFO: Server startup in 9285 ms
Loading driver...
Driver loaded!
SQLException: Table 'Beanstalk' already exists
SQLState: 42S01
VendorError: 1050
Closing the connection.
Closing the connection.
```

All information that the web application sends to standard output appears in the web container log\. In the previous example, the application tries to create the table every time the page loads\. This results in catching a SQL exception on every page load after the first one\. 

As an example, the preceding is acceptable\. But in actual applications, keep your database definitions in schema objects, perform transactions from within model classes, and coordinate requests with controller servlets\.

### Connecting to an RDS DB Instance<a name="create_deploy_Java.rds.troubleshooting.connecting"></a>

 You can connect directly to the RDS DB instance in your Elastic Beanstalk environment by using the MySQL client application\. 

 First, open the security group to your RDS DB instance to allow traffic from your computer\. 

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Configuration**\.

1. In the **Database** configuration category, choose **Edit**\.

1. Next to **Endpoint**, choose the Amazon RDS console link\.

1. On the **RDS Dashboard** instance details page, under **Security and Network**, choose the security group starting with *rds\-* next to **Security Groups**\.
**Note**  
The database might have multiple entries labeled **Security Groups**\. Use the first, which starts with *awseb*, only if you have an older account that doesn't have a default [Amazon Virtual Private Cloud](https://docs.aws.amazon.com/vpc/latest/userguide/) \(Amazon VPC\)\.

1. In **Security group details**, choose the **Inbound** tab, and then choose **Edit**\.

1. Add a rule for MySQL \(port 3306\) that allows traffic from your IP address, specified in CIDR format\.

1. Choose **Save**\. The changes take effect immediately\.

 Return to the Elastic Beanstalk configuration details for your environment and note the endpoint\. You will use the domain name to connect to the RDS DB instance\. 

 Install the MySQL client and initiate a connection to the database on port 3306\. On Windows, install MySQL Workbench from the MySQL home page and follow the prompts\. 

 On Linux, install the MySQL client using the package manager for your distribution\. The following example works on Ubuntu and other Debian derivatives\. 

```
// Install MySQL client
$ sudo apt-get install mysql-client-5.5
...
// Connect to database
$ mysql -h aas839jo2vwhwb.cnubrrfwfka8.us-west-2.rds.amazonaws.com -u username -ppassword ebdb
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 117
Server version: 5.5.40-log Source distribution
...
```

After you have connected, you can run SQL commands to see the status of the database, whether your tables and rows were created, and other information\. 

```
mysql> SELECT Resource from Beanstalk;
+--------------+
| Resource     |
+--------------+
| EC2 Instance |
| RDS Instance |
+--------------+
2 rows in set (0.01 sec)
```