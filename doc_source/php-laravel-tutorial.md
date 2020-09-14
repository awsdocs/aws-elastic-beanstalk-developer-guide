# Deploying a Laravel application to Elastic Beanstalk<a name="php-laravel-tutorial"></a>

Laravel is an open source, model\-view\-controller \(MVC\) framework for PHP\. This tutorial walks you through the process of generating a Laravel application, deploying it to an AWS Elastic Beanstalk environment, and configuring it to connect to an Amazon Relational Database Service \(Amazon RDS\) database instance\.

**Topics**
+ [Prerequisites](#php-laravel-tutorial-prereqs)
+ [Launch an Elastic Beanstalk environment](#php-laravel-tutorial-launch)
+ [Install Laravel and generate a website](#php-laravel-tutorial-generate)
+ [Deploy your application](#php-laravel-tutorial-deploy)
+ [Configure Composer settings](#php-laravel-tutorial-configure)
+ [Add a database to your environment](#php-laravel-tutorial-database)
+ [Cleanup](#php-laravel-tutorial-cleanup)
+ [Next steps](#php-laravel-tutorial-nextsteps)

## Prerequisites<a name="php-laravel-tutorial-prereqs"></a>

This tutorial assumes you have knowledge of the basic Elastic Beanstalk operations and the Elastic Beanstalk console\. If you haven't already, follow the instructions in [Getting started using Elastic Beanstalk](GettingStarted.md) to launch your first Elastic Beanstalk environment\.

To follow the procedures in this guide, you will need a command line terminal or shell to run commands\. Commands are shown in listings preceded by a prompt symbol \($\) and the name of the current directory, when appropriate\.

```
~/eb-project$ this is a command
this is output
```

On Linux and macOS, use your preferred shell and package manager\. On Windows 10, you can [install the Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/install-win10) to get a Windows\-integrated version of Ubuntu and Bash\.

Laravel requires PHP 5\.5\.9 or later and the `mbstring` extension for PHP\. In this tutorial we use PHP 7\.0 and the corresponding Elastic Beanstalk platform version\. Install PHP and Composer by following the instructions at [Setting up your PHP development environment](php-development-environment.md)\.

## Launch an Elastic Beanstalk environment<a name="php-laravel-tutorial-launch"></a>

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

## Install Laravel and generate a website<a name="php-laravel-tutorial-generate"></a>

Composer can install Laravel and create a working project with one command:

```
~$ composer create-project --prefer-dist laravel/laravel eb-laravel
Installing laravel/laravel (v5.5.28)
  - Installing laravel/laravel (v5.5.28): Downloading (100%)
Created project in eb-laravel
> @php -r "file_exists('.env') || copy('.env.example', '.env');"
Loading composer repositories with package information
Updating dependencies (including require-dev)

Package operations: 70 installs, 0 updates, 0 removals
  - Installing symfony/thanks (v1.0.7): Downloading (100%)
  - Installing hamcrest/hamcrest-php (v2.0.0): Downloading (100%)
  - Installing mockery/mockery (1.0): Downloading (100%)
  - Installing vlucas/phpdotenv (v2.4.0): Downloading (100%)
  - Installing symfony/css-selector (v3.4.8): Downloading (100%)
  - Installing tijsverkoyen/css-to-inline-styles (2.2.1): Downloading (100%)
...
```

Composer installs Laravel and its dependencies, and generates a default project\.

If you run into any issues installing Laravel, go to the installation topic in the official documentation: [https://laravel\.com/docs/5\.2](https://laravel.com/docs/5.2)

## Deploy your application<a name="php-laravel-tutorial-deploy"></a>

Create a [source bundle](applications-sourcebundle.md) containing the files created by Composer\. The following command creates a source bundle named `laravel-default.zip`\. It excludes files in the `vendor` folder, which take up a lot of space and are not necessary for deploying your application to Elastic Beanstalk\.

```
~/eb-laravel$ zip ../laravel-default.zip -r * .[^.]* -x "vendor/*"
```

Upload the source bundle to Elastic Beanstalk to deploy Laravel to your environment\.

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
To optimize the source bundle further, initialize a Git repository and use the [`git archive` command](applications-sourcebundle.md#using-features.deployment.source.git) to create the source bundle\. The default Laravel project includes a `.gitignore` file that tells Git to exclude the `vendor` folder and other files that are not required for deployment\.

## Configure Composer settings<a name="php-laravel-tutorial-configure"></a>

When the deployment completes, click the URL to open your Laravel application in the browser:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/php-laravel-403.png)

What's this? By default, Elastic Beanstalk serves the root of your project at the root path of the web site\. In this case, though, the default page \(`index.php`\) is one level down in the `public` folder\. You can verify this by adding `/public` to the URL\. For example, `http://laravel.us-east-2.elasticbeanstalk.com/public`\.

To serve the Laravel application at the root path, use the Elastic Beanstalk console to configure the *document root* for the web site\.

**To configure your web site's document root**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Configuration**\.

1. In the **Software** configuration category, choose **Edit**\.

1. For **Document Root**, enter **/public**\.

1. Choose **Apply**\.

1. When the update is complete, click the URL to reopen your site in the browser\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/php-laravel-defaultnodb.png)

So far, so good\. Next you'll add a database to your environment and configure Laravel to connect to it\.

## Add a database to your environment<a name="php-laravel-tutorial-database"></a>

Launch an RDS DB instance in your Elastic Beanstalk environment\. You can use MySQL, SQLServer, or PostgreSQL databases with Laravel on Elastic Beanstalk\. For this example, we'll use MySQL\.

**Note**  
Running an Amazon RDS instance in your Elastic Beanstalk environment is great for development and testing, but it ties the lifecycle of your database to your environment\. See [Launching and connecting to an external Amazon RDS instance in a default VPC](rds-external-defaultvpc.md) for instructions on connecting to a database running outside of your environment\.

**To add an RDS DB instance to your Elastic Beanstalk environment**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Configuration**\.

1. In the **Database** configuration category, choose **Edit**\.

1. For **Engine**, choose **mysql**\.

1. Type a master **username** and **password**\. Elastic Beanstalk will provide these values to your application using environment properties\.

1. Choose **Apply**\.

Creating a database instance takes about 10 minutes\. In the meantime, you can update your source code to read connection information from the environment\. Elastic Beanstalk provides connection details using environment variables, such as `RDS_HOSTNAME`, that you can access from your application\.

Laravel's database configuration is stored in a file named `database.php` in the `config` folder in your project code\. Find the `mysql` entry and modify the `host`, `database`, `username`, `and password` variables to read the corresponding values from Elastic Beanstalk:

**Example \~/Eb\-laravel/config/database\.php**  

```
...
    'connections' => [

        'sqlite' => [
            'driver' => 'sqlite',
            'database' => env('DB_DATABASE', database_path('database.sqlite')),
            'prefix' => '',
        ],

        'mysql' => [
            'driver' => 'mysql',
            'host' => env('RDS_HOSTNAME', '127.0.0.1'),
            'port' => env('RDS_PORT', '3306'),
            'database' => env('RDS_DB_NAME', 'forge'),
            'username' => env('RDS_USERNAME', 'forge'),
            'password' => env('RDS_PASSWORD', ''),
            'unix_socket' => env('DB_SOCKET', ''),
            'charset' => 'utf8mb4',
            'collation' => 'utf8mb4_unicode_ci',
            'prefix' => '',
            'strict' => true,
            'engine' => null,
        ],
...
```

To verify that the database connection is configured correctly, add code to `index.php` to connect to the database and add some code to the default response:

**Example \~/Eb\-laravel/public/index\.php**  

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
   ~/eb-laravel$ zip ../laravel-v2-rds.zip -r * .[^.]* -x "vendor/*"
   ```

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. Choose **Upload and Deploy**\.

1. Choose **Browse**, and upload `laravel-v2-rds.zip`\.

1. Choose **Deploy**\.

Deploying a new version of your application takes less than a minute\. When the deployment is complete, refresh the web page again to verify that the database connection succeeded:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/php-laravel-defaultwdb.png)

## Cleanup<a name="php-laravel-tutorial-cleanup"></a>

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

## Next steps<a name="php-laravel-tutorial-nextsteps"></a>

For more information about Laravel, go to the tutorial at [laravel\.com](https://laravel.com/docs/5.2/quickstart)\.

As you continue to develop your application, you'll probably want a way to manage environments and deploy your application without manually creating a \.zip file and uploading it to the Elastic Beanstalk console\. The [Elastic Beanstalk Command Line Interface](eb-cli3.md) \(EB CLI\) provides easy\-to\-use commands for creating, configuring, and deploying applications to Elastic Beanstalk environments from the command line\.

In this tutorial, you used the Elastic Beanstalk console to configure composer options\. To make this configuration part of your application source, you can use a configuration file like the following\.

**Example \.ebextensions/composer\.config**  

```
option_settings:
  aws:elasticbeanstalk:container:php:phpini:
    document_root: /public
```

For more information, see [Advanced environment customization with configuration files \(`.ebextensions`\)](ebextensions.md)\.

Running an Amazon RDS DB instance in your Elastic Beanstalk environment is great for development and testing, but it ties the lifecycle of your database to your environment\. See [Adding an Amazon RDS DB instance to your PHP application environment](create_deploy_PHP.rds.md) for instructions on connecting to a database running outside of your environment\.

Finally, if you plan on using your application in a production environment, you will want to [configure a custom domain name](customdomains.md) for your environment and [enable HTTPS](configuring-https.md) for secure connections\.