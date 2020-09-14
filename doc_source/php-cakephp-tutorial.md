# Deploying a CakePHP application to Elastic Beanstalk<a name="php-cakephp-tutorial"></a>

CakePHP is an open source, MVC framework for PHP\. This tutorial walks you through the process of generating a CakePHP project, deploying it to an Elastic Beanstalk environment, and configuring it to connect to an Amazon RDS database instance\.

**Topics**
+ [Prerequisites](#php-cakephp-tutorial-prereqs)
+ [Launch an Elastic Beanstalk environment](#php-cakephp-tutorial-launch)
+ [Install CakePHP and generate a website](#php-cakephp-tutorial-generate)
+ [Deploy your application](#php-cakephp-tutorial-deploy)
+ [Add a database to your environment](#php-cakephp-tutorial-database)
+ [Cleanup](#php-cakephp-tutorial-cleanup)
+ [Next steps](#php-cakephp-tutorial-nextsteps)

## Prerequisites<a name="php-cakephp-tutorial-prereqs"></a>

This tutorial assumes you have knowledge of the basic Elastic Beanstalk operations and the Elastic Beanstalk console\. If you haven't already, follow the instructions in [Getting started using Elastic Beanstalk](GettingStarted.md) to launch your first Elastic Beanstalk environment\.

To follow the procedures in this guide, you will need a command line terminal or shell to run commands\. Commands are shown in listings preceded by a prompt symbol \($\) and the name of the current directory, when appropriate\.

```
~/eb-project$ this is a command
this is output
```

On Linux and macOS, use your preferred shell and package manager\. On Windows 10, you can [install the Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/install-win10) to get a Windows\-integrated version of Ubuntu and Bash\.

CakePHP requires PHP 5\.5\.9 or newer, and the `mbstring` and `intl` extensions for PHP\. In this tutorial we use PHP 7\.0 and the corresponding Elastic Beanstalk platform version\. Install PHP and Composer by following the instructions at [Setting up your PHP development environment](php-development-environment.md)\.

## Launch an Elastic Beanstalk environment<a name="php-cakephp-tutorial-launch"></a>

Use the Elastic Beanstalk console to create an Elastic Beanstalk environment\. Choose the **PHP** platform and accept the default settings and sample code\.

**To launch an environment \(console\)**

1. Open the Elastic Beanstalk console using this preconfigured link: [console\.aws\.amazon\.com/elasticbeanstalk/home\#/newApplication?applicationName=tutorials&environmentType=LoadBalanced](https://console.aws.amazon.com/elasticbeanstalk/home#/newApplication?applicationName=tutorials&environmentType=LoadBalanced)

1. For **Platform**, select the platform and platform branch that match the language used by your application\.

1. For **Application code**, choose **Sample application**\.

1. Choose **Review and launch**\.

1. Review the available options\. Choose the available option you want to use, and when you're ready, choose **Create app**\.

Environment creation takes about 5 minutes and creates the following resources:
+ **EC2 instance** – An Amazon Elastic Compute Cloud \(Amazon EC2\) virtual machine configured to run web apps on the platform that you choose\.

  Each platform runs a specific set of software, configuration files, and scripts to support a specific language version, framework, web container, or combination of these\. Most platforms use either Apache or NGINX as a reverse proxy that sits in front of your web app, forwards requests to it, serves static assets, and generates access and error logs\.
+ **Instance security group** – An Amazon EC2 security group configured to allow inbound traffic on port 80\. This resource lets HTTP traffic from the load balancer reach the EC2 instance running your web app\. By default, traffic isn't allowed on other ports\.
+ **Load balancer** – An Elastic Load Balancing load balancer configured to distribute requests to the instances running your application\. A load balancer also eliminates the need to expose your instances directly to the internet\.
+ **Load balancer security group** – An Amazon EC2 security group configured to allow inbound traffic on port 80\. This resource lets HTTP traffic from the internet reach the load balancer\. By default, traffic isn't allowed on other ports\.
+ **Auto Scaling group** – An Auto Scaling group configured to replace an instance if it is terminated or becomes unavailable\.
+ **Amazon S3 bucket** – A storage location for your source code, logs, and other artifacts that are created when you use Elastic Beanstalk\.
+ **Amazon CloudWatch alarms** – Two CloudWatch alarms that monitor the load on the instances in your environment and that are triggered if the load is too high or too low\. When an alarm is triggered, your Auto Scaling group scales up or down in response\.
+ **AWS CloudFormation stack** – Elastic Beanstalk uses AWS CloudFormation to launch the resources in your environment and propagate configuration changes\. The resources are defined in a template that you can view in the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation)\.
+ **Domain name** – A domain name that routes to your web app in the form **subdomain*\.*region*\.elasticbeanstalk\.com*\.

All of these resources are managed by Elastic Beanstalk\. When you terminate your environment, Elastic Beanstalk terminates all the resources that it contains\.

**Note**  
The Amazon S3 bucket that Elastic Beanstalk creates is shared between environments and is not deleted during environment termination\. For more information, see [Using Elastic Beanstalk with Amazon S3](AWSHowTo.S3.md)\.

## Install CakePHP and generate a website<a name="php-cakephp-tutorial-generate"></a>

Composer can install CakePHP and create a working project with one command:

```
~$ composer create-project --prefer-dist cakephp/app eb-cake
Installing cakephp/app (3.6.0)
  - Installing cakephp/app (3.6.0): Loading from cache
Created project in eb-cake2
Loading composer repositories with package information
Updating dependencies (including require-dev)

Package operations: 46 installs, 0 updates, 0 removals
  - Installing cakephp/plugin-installer (1.1.0): Loading from cache
  - Installing aura/intl (3.0.0): Loading from cache
...
```

Composer installs CakePHP and around 20 dependencies, and generates a default project\.

If you run into any issues installing CakePHP, visit the installation topic in the official documentation: [http://book\.cakephp\.org/3\.0/en/installation\.html](http://book.cakephp.org/3.0/en/installation.html)

## Deploy your application<a name="php-cakephp-tutorial-deploy"></a>

Create a [source bundle](applications-sourcebundle.md) containing the files created by Composer\. The following command creates a source bundle named `cake-default.zip`\. It excludes files in the `vendor` folder, which take up a lot of space and are not necessary for deploying your application to Elastic Beanstalk\.

```
eb-cake zip ../cake-default.zip -r * .[^.]* -x "vendor/*"
```

Upload the source bundle to Elastic Beanstalk to deploy CakePHP to your environment\.

**To deploy a source bundle**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. On the environment overview page, choose **Upload and deploy**\.

1. Use the on\-screen dialog box to upload the source bundle\.

1. Choose **Deploy**\.

1. When the deployment completes, you can choose the site URL to open your website in a new tab\.

**Note**  
To optimize the source bundle further, initialize a Git repository and use the [`git archive` command](applications-sourcebundle.md#using-features.deployment.source.git) to create the source bundle\. The default Symfony project includes a `.gitignore` file that tells Git to exclude the `vendor` folder and other files that are not required for deployment\.

When the process completes, click the URL to open your CakePHP application in the browser:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/php-cakephp-defaultnodb.png)

So far, so good\. Next you'll add a database to your environment and configure CakePHP to connect to it\.

## Add a database to your environment<a name="php-cakephp-tutorial-database"></a>

Launch an Amazon RDS database instance in your Elastic Beanstalk environment\. You can use MySQL, SQLServer, or PostgreSQL databases with CakePHP on Elastic Beanstalk\. For this example, we'll use PostgreSQL\.

**To add an Amazon RDS DB instance to your Elastic Beanstalk environment**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Configuration**\.

1. Under **Database**, choose **Edit**\.

1. For **DB engine**, choose **postgres**\.

1. Type a master **username** and **password**\. Elastic Beanstalk will provide these values to your application using environment properties\.

1. Choose **Apply**\.

Creating a database instance takes about 10 minutes\. In the meantime, you can update your source code to read connection information from the environment\. Elastic Beanstalk provides connection details using environment variables such as `RDS_HOSTNAME` that you can access from your application\.

CakePHP's database configuration is in a file named `app.php` in the `config` folder in your project code\. Open this file and add some code that reads the environment variables from `$_SERVER` and assigns them to local variables\. Insert the highlighted lines in the below example after the first line \(`<?php`\):

**Example \~/Eb\-cake/config/app\.php**  

```
<?php
if (!defined('RDS_HOSTNAME')) {
  define('RDS_HOSTNAME', $_SERVER['RDS_HOSTNAME']);
  define('RDS_USERNAME', $_SERVER['RDS_USERNAME']);
  define('RDS_PASSWORD', $_SERVER['RDS_PASSWORD']);
  define('RDS_DB_NAME', $_SERVER['RDS_DB_NAME']);
}
return [
...
```

The database connection is configured further down in `app.php`\. Find the following section and modify the default datasources configuration with the name of the driver that matches your database engine \(`Mysql`, `Sqlserver`, or `Postgres`\), and set the `host`, `username`, `password` and `database` variables to read the corresponding values from Elastic Beanstalk:

**Example \~/Eb\-cake/config/app\.php**  

```
...
     /**
     * Connection information used by the ORM to connect
     * to your application's datastores.
     * Drivers include Mysql Postgres Sqlite Sqlserver
     * See vendor\cakephp\cakephp\src\Database\Driver for complete list
     */
    'Datasources' => [
        'default' => [
            'className' => 'Cake\Database\Connection',
            'driver' => 'Cake\Database\Driver\Postgres',
            'persistent' => false,
            'host' => RDS_HOSTNAME,
            /*
             * CakePHP will use the default DB port based on the driver selected
             * MySQL on MAMP uses port 8889, MAMP users will want to uncomment
             * the following line and set the port accordingly
             */
            //'port' => 'non_standard_port_number',
            'username' => RDS_USERNAME,
            'password' => RDS_PASSWORD,
            'database' => RDS_DB_NAME,
            /*
             * You do not need to set this flag to use full utf-8 encoding (internal default since CakePHP 3.6).
             */
            //'encoding' => 'utf8mb4',
            'timezone' => 'UTC',
            'flags' => [],
            'cacheMetadata' => true,
            'log' => false,
...
```

When the DB instance has finished launching, bundle up and deploy the updated application to your environment:

**To update your Elastic Beanstalk environment**

1. Create a new source bundle:

   ```
   ~/eb-cake$ zip ../cake-v2-rds.zip -r * .[^.]* -x "vendor/*"
   ```

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. Choose **Upload and Deploy**\.

1. Choose **Browse** and upload `cake-v2-rds.zip`\.

1. Choose **Deploy**\.

Deploying a new version of your application takes less than a minute\. When the deployment is complete, refresh the web page again to verify that the database connection succeeded:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/php-cakephp-defaultwdb.png)

## Cleanup<a name="php-cakephp-tutorial-cleanup"></a>

When you finish working with Elastic Beanstalk, you can terminate your environment\. Elastic Beanstalk terminates all AWS resources associated with your environment, such as [Amazon EC2 instances](using-features.managing.ec2.md), [database instances](using-features.managing.db.md), [load balancers](using-features.managing.elb.md), security groups, and [alarms](using-features.alarms.md#using-features.alarms.title)\. 

**To terminate your Elastic Beanstalk environment**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. Choose **Environment actions**, and then choose **Terminate environment**\.

1. Use the on\-screen dialog box to confirm environment termination\.

With Elastic Beanstalk, you can easily create a new environment for your application at any time\.

In addition, you can terminate database resources that you created outside of your Elastic Beanstalk environment\. When you terminate an Amazon RDS DB instance, you can take a snapshot and restore the data to another instance later\.

**To terminate your RDS DB instance**

1. Open the [Amazon RDS console](https://console.aws.amazon.com/rds)\.

1. Choose **Databases**\.

1. Choose your DB instance\.

1. Choose **Actions**, and then choose **Delete**\.

1. Choose whether to create a snapshot, and then choose **Delete**\.

## Next steps<a name="php-cakephp-tutorial-nextsteps"></a>

For more information about CakePHP, read the book at [book\.cakephp\.org](http://book.cakephp.org/3.0/en/index.html)\.

As you continue to develop your application, you'll probably want a way to manage environments and deploy your application without manually creating a \.zip file and uploading it to the Elastic Beanstalk console\. The [Elastic Beanstalk Command Line Interface](eb-cli3.md) \(EB CLI\) provides easy\-to\-use commands for creating, configuring, and deploying applications to Elastic Beanstalk environments from the command line\.

Running an Amazon RDS DB instance in your Elastic Beanstalk environment is great for development and testing, but it ties the lifecycle of your database to your environment\. See [Adding an Amazon RDS DB instance to your PHP application environment](create_deploy_PHP.rds.md) for instructions on connecting to a database running outside of your environment\.

Finally, if you plan on using your application in a production environment, you will want to [configure a custom domain name](customdomains.md) for your environment and [enable HTTPS](configuring-https.md) for secure connections\.