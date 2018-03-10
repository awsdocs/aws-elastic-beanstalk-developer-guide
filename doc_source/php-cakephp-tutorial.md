# Deploying a CakePHP Application to Elastic Beanstalk<a name="php-cakephp-tutorial"></a>

CakePHP is an open source, MVC framework for PHP\. This tutorial walks you through the process of generating a CakePHP project, deploying it to an Elastic Beanstalk environment, and configuring it to connect to an Amazon RDS database instance\.


+ [Prerequisites](#php-cakephp-tutorial-prereqs)
+ [Install Composer](#php-cakephp-tutorial-composer)
+ [Install CakePHP and Generate a Website](#php-cakephp-tutorial-generate)
+ [Create an Elastic Beanstalk Environment and Deploy Your Application](#php-cakephp-tutorial-deploy)
+ [Add a Database to Your Environment](#php-cakephp-tutorial-database)
+ [Clean Up](#w3ab1c43c15c32)
+ [Next Steps](#php-cakephp-tutorial-nextsteps)

## Prerequisites<a name="php-cakephp-tutorial-prereqs"></a>

To follow the procedures in this guide you will need a command line terminal or shell to run commands\. Commands are shown in listings like the following, preceded by a prompt symbol \('$'\) and the name of the current directory, when appropriate:

```
~/eb-project$ this is a command
this is output
```

**Note**  
All commands in this tutorial can be run on an Amazon Linux EC2 instance\. If you need a development environment, you can launch a single instance Elastic Beanstalk environment and connect to it with SSH\.

CakePHP requires PHP 5\.5\.9 or newer, and the `mbstring` and `intl` extensions for PHP\. In this tutorial we use PHP 5\.6 and the corresponding Elastic Beanstalk platform configuration\.

Install PHP 5\.6 and the required extensions\. Depending on your platform and available package manager, the steps will vary\.

On Amazon Linux, use yum:

```
$ sudo yum install php56 --skip-broken
$ sudo yum install php56-mbstring
$ sudo yum install php56-intl
```

On OS\-X, use brew:

```
$ brew install php56
$ brew install php56-intl
```

On Windows, visit the download page at [windows\.php\.net](http://windows.php.net/download/) to get PHP, and read [this page](http://php.net/manual/en/install.windows.legacy.index.php#install.windows.legacy.extensions) for information about extensions\.

After installing PHP, reopen your terminal and run `php --version` to ensure that the new version has been installed and is the default\.

## Install Composer<a name="php-cakephp-tutorial-composer"></a>

Composer is a dependency management tool for PHP\. It is the preferred method for installing CakePHP and its dependencies and generating the a CakePHP project\.

Install Composer by downloading the installer and running it with PHP\. The installer generates a PHAR file in the current directory that you can invoke with PHP to generate a CakePHP project\.

```
~$ curl -s https://getcomposer.org/installer | php
All settings correct for using Composer
Downloading...

Composer successfully installed to: /home/ec2-user/composer.phar
Use it: php composer.phar
```

If you run into issues installing Composer, visit the official documentation: [https://getcomposer\.org/](https://getcomposer.org/)

## Install CakePHP and Generate a Website<a name="php-cakephp-tutorial-generate"></a>

Composer can install CakePHP and create a working project with one command:

```
~$ php composer.phar create-project --prefer-dist cakephp/app eb-cake

Installing cakephp/app (3.2.0)
  - Installing cakephp/app (3.2.0)
    Downloading: 100%

Created project in eb-cake
Loading composer repositories with package information
Installing dependencies (including require-dev)
  - Installing aura/installer-default (1.0.0)
    Downloading: 100%

  - Installing cakephp/plugin-installer (0.0.12)
    Downloading: 100%

  - Installing psr/log (1.0.0)
    Downloading: 100%
...
```

Composer installs CakePHP and around 20 dependencies, and generates a default project\.

If you run into any issues installing CakePHP, visit the installation topic in the official documentation: [http://book\.cakephp\.org/3\.0/en/installation\.html](http://book.cakephp.org/3.0/en/installation.html)

## Create an Elastic Beanstalk Environment and Deploy Your Application<a name="php-cakephp-tutorial-deploy"></a>

Create a [source bundle](applications-sourcebundle.md) containing the files created by Composer\. You can use any program to create the ZIP file, as long as it includes hidden files\. On the command line, use the `zip` command:

```
~$ cd eb-cake
~/eb-cake$ zip ../cake-default.zip -r * .[^.]*
```

Save the ZIP archive in a location that you can access\. This is the source bundle that you will upload to Elastic Beanstalk when you create an environment\.

**Note**  
If you are working remotely in an Elastic Beanstalk environment, you can upload the archive to your Elastic Beanstalk storage bucket in Amazon S3 with the AWS CLI `aws cp` command:  

```
~$ aws s3 cp cake-default.zip s3://elasticbeanstalk-us-west-2-123456789012
```
Elastic Beanstalk creates this bucket the first time you create an environment\. To upload files to Amazon S3, your environment's [instance profile](concepts-roles-instance.md) must have permission to write to the bucket\.

Use the AWS Management Console to create an Elastic Beanstalk environment running your application\. Choose the **PHP 5\.6** platform configuration and upload your source bundle when prompted:

**To launch an environment \(console\)**

1. Open the Elastic Beanstalk console with this preconfigured link: [console\.aws\.amazon\.com/elasticbeanstalk/home\#/newApplication?applicationName=tutorials&environmentType=LoadBalanced](https://console.aws.amazon.com/elasticbeanstalk/home#/newApplication?applicationName=tutorials&environmentType=LoadBalanced)

1. For **Platform**, choose the platform that matches the language used by your application\.

1. For **App code**, choose **Upload**\.

1. Choose **Local file**, choose **Browse**, and open the source bundle\.

1. Choose **Upload**\.

1. Choose **Review and launch**\.

1. Review the available settings and choose **Create app**\.

Environment creation takes about 5 minutes\. When the process completes, click the URL to open your CakePHP application in the browser:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/php-cakephp-defaultnodb.png)

So far, so good\. Next you'll add a database to your environment and configure CakePHP to connect to it\.

## Add a Database to Your Environment<a name="php-cakephp-tutorial-database"></a>

Launch an Amazon RDS database instance in your Elastic Beanstalk environment\. You can use MySQL, SQLServer, or PostgreSQL databases with CakePHP on Elastic Beanstalk\. For this example, we'll use PostgreSQL\.

**To add an Amazon RDS DB instance to your Elastic Beanstalk environment**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. Choose **Configuration**\.

1. In the **Data Tier** section, choose **create a new RDS database**\.

1. For **DB engine**, choose **postgres**\.

1. Type a master **username** and **password**\. Elastic Beanstalk will provide these values to your application using environment properties\.

1. Choose **Apply**\.

Creating a database instance takes about 10 minutes\. In the meantime, you can update your source code to read connection information from the environment\. Elastic Beanstalk provides connection details using environment variables such as `RDS_HOSTNAME` that you can access from your application\.

CakePHP's database configuration is in a file named `app.php` in the `config` folder in your project code\. Open this file and add some code that reads the environment variables from `$_SERVER` and assigns them to local variables\. Insert the highlighted lines in the below example after the first line \(`<?php`\):

**Example \~/eb\-cake/config/app\.php**  

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

**Example \~/eb\-cake/config/app\.php**  

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
            /**
             * CakePHP will use the default DB port based on the driver selected
             * MySQL on MAMP uses port 8889, MAMP users will want to uncomment
             * the following line and set the port accordingly
             */
            //'port' => 'non_standard_port_number',
            'username' => RDS_USERNAME,
            'password' => RDS_PASSWORD,
            'database' => RDS_DB_NAME,
            'encoding' => 'utf8',
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
   ~/eb-cake$ zip ../cake-v2-rds.zip -r * .[^.]*
   ```

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. Choose **Upload and Deploy**\.

1. Choose **Browse** and upload `cake-v2-rds.zip`\.

1. Choose **Deploy**\.

Deploying a new version of your application takes less than a minute\. When the deployment is complete, refresh the web page again to verify that the database connection succeeded:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/php-cakephp-defaultwdb.png)

## Clean Up<a name="w3ab1c43c15c32"></a>

When you finish working with Elastic Beanstalk, you can terminate your environment\. Elastic Beanstalk terminates all AWS resources associated with your environment, such as [Amazon EC2 instances](using-features.managing.ec2.md), [database instances](using-features.managing.db.md), [load balancers](using-features.managing.elb.md), security groups, and [alarms](using-features.alarms.md#using-features.alarms.title)\. 

**To terminate your Elastic Beanstalk environment**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. Choose **Actions**, and then choose **Terminate Environment**\.

1. In the **Confirm Termination** dialog box, type the environment name, and then choose **Terminate**\.

In addition, you can terminate database resources that you created outside of your Elastic Beanstalk environment\. When you terminate an Amazon RDS database instance, you can take a snapshot and restore the data to another instance later\.

**To terminate your RDS DB instance**

1. Open the [Amazon RDS console](https://console.aws.amazon.com/rds)\.

1. Choose **Instances**\.

1. Choose your DB instance\.

1. Choose **Instance Actions**, and then choose **Delete**\.

1. Choose whether to create a snapshot, and then choose **Delete**\.

**To delete a DynamoDB table**

1. Open the [Tables page](https://console.aws.amazon.com/dynamodb/home?#tables:) in the DynamoDB console\.

1. Select a table\.

1. Choose **Actions**, and then choose **Delete table**\.

1. Choose **Delete**\.

## Next Steps<a name="php-cakephp-tutorial-nextsteps"></a>

For more information about CakePHP, read the book at [book\.cakephp\.org](http://book.cakephp.org/3.0/en/index.html)\.

As you continue to develop your application, you'll probably want a way to manage environments and deploy your application without creating a ZIP file manually and uploading it to the Elastic Beanstalk console\. The [Elastic Beanstalk Command Line Interface](eb-cli3.md) \(EB CLI\) provides easy to use commands for creating, configuring, and deploying to Elastic Beanstalk environments from the command line\.

Running an Amazon RDS DB instance in your Elastic Beanstalk environment is great for development and testing, but it ties the lifecycle of your database to your environment\. See [Adding an Amazon RDS DB Instance to Your PHP Application Environment](create_deploy_PHP.rds.md) for instructions on connecting to a database running outside of your environment\.

Finally, if you plan on using your application in a production environment, you will want to [configure a custom domain name](customdomains.md) for your environment and [enable HTTPS](configuring-https.md) for secure connections\.