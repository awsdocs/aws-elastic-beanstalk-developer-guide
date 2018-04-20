# Deploying a Laravel Application to Elastic Beanstalk<a name="php-laravel-tutorial"></a>

Laravel is an open source, model\-view\-controller \(MVC\) framework for PHP\. This tutorial walks you through the process of generating a Laravel application, deploying it to an AWS Elastic Beanstalk environment, and configuring it to connect to an Amazon Relational Database Service \(Amazon RDS\) database instance\.

**Topics**
+ [Prerequisites](#php-laravel-tutorial-prereqs)
+ [Install Composer](#php-laravel-tutorial-composer)
+ [Install Laravel and Generate a Website](#php-laravel-tutorial-generate)
+ [Create an Elastic Beanstalk Environment and Deploy Your Application](#php-laravel-tutorial-deploy)
+ [Add a Database to Your Environment](#php-laravel-tutorial-database)
+ [Clean Up](#w3ab1c43c17c32)
+ [Next Steps](#php-laravel-tutorial-nextsteps)

## Prerequisites<a name="php-laravel-tutorial-prereqs"></a>

This tutorial assumes that you have some knowledge of basic Elastic Beanstalk operations and the Elastic Beanstalk console\. If you haven't already, follow the instructions in [Getting Started Using Elastic Beanstalk](GettingStarted.md) to launch your first Elastic Beanstalk environment\.

To follow the procedures in this guide, you will need a command line terminal or shell to run commands\. Commands are shown in listings proceded by a prompt symbol \($\) and the name of the current directory, when appropriate:

```
~/eb-project$ this is a command
this is output
```

On Linux and macOS, use your preferred shell and package manager\. On Windows 10, you can [install the Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/install-win10) to get a Windows\-integrated version of Ubuntu and Bash\.

Laravel requires PHP 5\.5\.9 or later and the `mbstring` extension for PHP\. In this tutorial we use PHP 5\.6 and the corresponding Elastic Beanstalk platform configuration\.

Install PHP 5\.6 and the required extensions\. Depending on your platform and package manager, the steps will vary\.

On Amazon Linux, use yum:

```
$ sudo yum install php56 --skip-broken
$ sudo yum install php56-mbstring
```

On OS X, use Homebrew:

```
$ brew install php56
```

On Windows, go to the download page at [windows\.php\.net](http://windows.php.net/download/) to get PHP, and read the [Windows extensions page](http://php.net/manual/en/install.windows.legacy.index.php#install.windows.legacy.extensions) for information about extensions\.

After installing PHP, reopen your terminal and run `php --version` to ensure that the new version has been installed and is the default\.

## Install Composer<a name="php-laravel-tutorial-composer"></a>

Composer is a dependency management tool for PHP\. It is the preferred tool for installing Laravel and its dependencies and generating a Laravel application\.

Install Composer by downloading the installer and running it with PHP\. The installer generates a Phar file that you can invoke with PHP to generate a Laravel project n the current directory\.

```
~$ curl -s https://getcomposer.org/installer | php
All settings correct for using Composer
Downloading...

Composer successfully installed to: /home/ec2-user/composer.phar
Use it: php composer.phar
```

If you run into issues installing Composer, go to the official documentation: [https://getcomposer\.org/](https://getcomposer.org/)

## Install Laravel and Generate a Website<a name="php-laravel-tutorial-generate"></a>

Composer can install Laravel and create a working project with one command:

```
~$ php composer.phar create-project --prefer-dist laravel/laravel eb-laravel
Installing laravel/laravel (v5.2.15)
  - Installing laravel/laravel (v5.2.15)
    Downloading: 100%

Created project in eb-laravel
> php -r "copy('.env.example', '.env');"
Loading composer repositories with package information
Installing dependencies (including require-dev)
  - Installing vlucas/phpdotenv (v2.2.0)
    Downloading: 100%

  - Installing symfony/polyfill-mbstring (v1.1.0)
    Loading from cache
...
```

Composer installs Laravel and its dependencies, and generates a default project\.

If you run into any issues installing Laravel, go to the installation topic in the official documentation: [https://laravel\.com/docs/5\.2](https://laravel.com/docs/5.2)

## Create an Elastic Beanstalk Environment and Deploy Your Application<a name="php-laravel-tutorial-deploy"></a>

Create a [source bundle](applications-sourcebundle.md) containing the files created by Composer\. You can use any program to create the \.zip file, as long as it allows hidden files\. On the command line, use the `zip` command:

```
~$ cd eb-laravel
~/eb-laravel$ zip ../laravel-default.zip -r * .[^.]*
```

Save the \.zip archive in a location that you can access\. This is the source bundle that you will upload to Elastic Beanstalk when you create an environment\.

**Note**  
If you are working remotely in an Elastic Beanstalk environment, you can upload the archive to your Elastic Beanstalk storage bucket in Amazon Simple Storage Service \(Amazon S3\) with the AWS CLI `aws cp` command:  

```
~$ aws s3 cp laravel-default.zip s3://elasticbeanstalk-us-west-2-123456789012
```
Elastic Beanstalk creates this bucket the first time you create an environment\. To upload files to Amazon S3, you have to give your environment's [instance profile](concepts-roles-instance.md) permission to write to the bucket\.

Use the AWS Management Console to create an Elastic Beanstalk environment running your application\. Choose the **PHP 5\.6** platform configuration and upload your source bundle when prompted:

**To launch an environment \(console\)**

1. Open the Elastic Beanstalk console with this preconfigured link: [console\.aws\.amazon\.com/elasticbeanstalk/home\#/newApplication?applicationName=tutorials&environmentType=LoadBalanced](https://console.aws.amazon.com/elasticbeanstalk/home#/newApplication?applicationName=tutorials&environmentType=LoadBalanced)

1. For **Platform**, choose the platform that matches the language used by your application\.

1. For **App code**, choose **Upload**\.

1. Choose **Local file**, choose **Browse**, and open the source bundle\.

1. Choose **Upload**\.

1. Choose **Review and launch**\.

1. Review the available settings and choose **Create app**\.

Environment creation takes about 5 minutes\. When the process completes, click the URL to open your Laravel application in the browser:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/php-laravel-403.png)

What's this? By default, Elastic Beanstalk serves the root of your project at the root path of the web site\. In this case, though, the default page \(`index.php`\) is one level down in the `public` folder\. You can verify this by adding `/public` to the URL\. For example, `http://laravel.us-east-2.elasticbeanstalk.com/public`\.

To allow access to this folder, use the Elastic Beanstalk console to configure the *document root* for the web site\.

**To configure your web site's document root**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. Choose **Configuration**\.

1. On the **Software** configuration card, choose **Modify**\.

1. For **Document Root**, type **/public**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/php-laravel-docroot.png)

1. Choose **Apply**\.

1. When the update is complete, click the URL to reopen your site in the browser\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/php-laravel-defaultnodb.png)

So far, so good\. Next you'll add a database to your environment and configure Laravel to connect to it\.

## Add a Database to Your Environment<a name="php-laravel-tutorial-database"></a>

Launch an RDS DB instance in your Elastic Beanstalk environment\. You can use MySQL, SQLServer, or PostgreSQL databases with Laravel on Elastic Beanstalk\. For this example, we'll use MySQL\.

**To add an RDS DB instance to your Elastic Beanstalk environment**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. Choose **Configuration**\.

1. In the **Data Tier** section, choose **create a new RDS database**\.

1. For **DB engine**, choose **mysql**\.

1. Type a master **username** and **password**\. Elastic Beanstalk will provide these values to your application using environment properties\.

1. Choose **Apply**\.

Creating a database instance takes about 10 minutes\. In the meantime, you can update your source code to read connection information from the environment\. Elastic Beanstalk provides connection details using environment variables, such as `RDS_HOSTNAME`, that you can access from your application\.

Laravel's database configuration is stored in a file named `database.php` in the `config` folder in your project code\. Open this file and add code that reads the environment variables from `$_SERVER` and assigns them to local variables by inserting the highlighted lines in the following example after the first line \(`<?php`\):

**Example \~/eb\-laravel/config/database\.php**  

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

The database connection is configured further down in `database.php` file\. Find the following section and modify the default datasources configuration with the name of the driver that matches your database engine \(`Mysql`, `Sqlserver`, or `Postgres`\), and set the `host`, `database`, `username`, `and password` variables to read the corresponding values from Elastic Beanstalk:

**Example \~/eb\-laravel/config/database\.php**  

```
...
    'connections' => [

        'sqlite' => [
            'driver'   => 'sqlite',
            'database' => database_path('database.sqlite'),
            'prefix'   => '',
        ],

        'mysql' => [
            'driver'    => 'mysql',
            'host'      => RDS_HOSTNAME,
            'database'  => RDS_DB_NAME,
            'username'  => RDS_USERNAME,
            'password'  => RDS_PASSWORD,
            'charset'   => 'utf8',
            'collation' => 'utf8_unicode_ci',
            'prefix'    => '',
            'strict'    => false,
            'engine'    => null,
        ],
...
```

To verify that the database connection is configured correctly, add code to `index.php` to connect to the database and add some code to the default response:

**Example \~/eb\-laravel/public/index\.php**  

```
...
if(DB::connection()->getDatabaseName())
{
   echo "Connected to database ".DB::connection()->getDatabaseName();
}
$response->send();
...
```

When the DB instance has finished launching, bundle and deploy the updated application to your environment\.

**To update your Elastic Beanstalk environment**

1. Create a new source bundle:

   ```
   ~/eb-laravel$ zip ../laravel-v2-rds.zip -r * .[^.]*
   ```

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. Choose **Upload and Deploy**\.

1. Choose **Browse**, and upload `laravel-v2-rds.zip`\.

1. Choose **Deploy**\.

Deploying a new version of your application takes less than a minute\. When the deployment is complete, refresh the web page again to verify that the database connection succeeded:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/php-laravel-defaultwdb.png)

## Clean Up<a name="w3ab1c43c17c32"></a>

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

1. Choose **Instance actions**, and then choose **Delete**\.

1. Choose whether to create a snapshot, and then choose **Delete**\.

**To delete a DynamoDB table**

1. Open the [Tables page](https://console.aws.amazon.com/dynamodb/home?#tables:) in the DynamoDB console\.

1. Select a table\.

1. Choose **Actions**, and then choose **Delete table**\.

1. Choose **Delete**\.

## Next Steps<a name="php-laravel-tutorial-nextsteps"></a>

As you continue to develop your application, you'll probably want a way to manage environments and deploy your application without manually creating a \.zip file and uploading it to the Elastic Beanstalk console\. The [Elastic Beanstalk Command Line Interface](eb-cli3.md) \(EB CLI\) provides easy\-to\-use commands for creating, configuring, and deploying applications to Elastic Beanstalk environments from the command line\.

In this tutorial, you configured a document root for your application\. When you launch more environments, it's impractical to manually configure this setting on each environment\. You can use [configuration files](ebextensions.md) to store this and other settings in your source code, so that they are applied automatically\.

Running an RDS DB instance in your Elastic Beanstalk environment is great for development and testing, but it ties the life cycle of your database to your environment\. For instructions on connecting to a database running outside of your environment, see [Adding an Amazon RDS DB Instance to Your PHP Application Environment](create_deploy_PHP.rds.md) \.

Finally, if you plan on using your application in a production environment, you will want to [configure a custom domain name](customdomains.md) for your environment and [enable HTTPS](configuring-https.md) for secure connections\.

For more information about Laravel, go to the tutorial at [laravel\.com](https://laravel.com/docs/5.2/quickstart)\.