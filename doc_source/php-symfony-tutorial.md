# Deploying a Symfony application to Elastic Beanstalk<a name="php-symfony-tutorial"></a>

[Symfony](http://symfony.com/) is an open source framework for developing dynamic PHP web applications\. This tutorial walks you through the process of generating a Symfony application and deploying it to an AWS Elastic Beanstalk environment\.

**Topics**
+ [Prerequisites](#php-symfony-tutorial-prereqs)
+ [Launch an Elastic Beanstalk environment](#php-symfony-tutorial-launch)
+ [Install Symfony and generate a website](#php-symfony-tutorial-generate)
+ [Deploy your application](#php-symfony-tutorial-deploy)
+ [Configure Composer settings](#php-symfony-tutorial-configure)
+ [Cleanup](#php-symfony-tutorial-cleanup)
+ [Next steps](#php-symfony-tutorial-nextsteps)

## Prerequisites<a name="php-symfony-tutorial-prereqs"></a>

This tutorial assumes you have knowledge of the basic Elastic Beanstalk operations and the Elastic Beanstalk console\. If you haven't already, follow the instructions in [Getting started using Elastic Beanstalk](GettingStarted.md) to launch your first Elastic Beanstalk environment\.

To follow the procedures in this guide, you will need a command line terminal or shell to run commands\. Commands are shown in listings preceded by a prompt symbol \($\) and the name of the current directory, when appropriate\.

```
~/eb-project$ this is a command
this is output
```

On Linux and macOS, use your preferred shell and package manager\. On Windows 10, you can [install the Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/install-win10) to get a Windows\-integrated version of Ubuntu and Bash\.

Symfony 4\.3 requires PHP 7\.1 or later and the `intl` extension for PHP\. In this tutorial we use PHP 7\.2 and the corresponding Elastic Beanstalk platform version\. Install PHP and Composer by following the instructions at [Setting up your PHP development environment](php-development-environment.md)\.

## Launch an Elastic Beanstalk environment<a name="php-symfony-tutorial-launch"></a>

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

## Install Symfony and generate a website<a name="php-symfony-tutorial-generate"></a>

Composer can install Symfony and create a working project with one command:

```
~$ composer create-project symfony/website-skeleton eb-symfony
Installing symfony/website-skeleton (v4.3.99)
  - Installing symfony/website-skeleton (v4.3.99): Downloading (100%)
Created project in eb-symfony
Loading composer repositories with package information
Updating dependencies (including require-dev)
Package operations: 1 install, 0 updates, 0 removals
  - Installing symfony/flex (v1.4.5): Downloading (100%)
Symfony operations: 1 recipe (539d006017ad5ef71beab4a2e2870e9a)
  - Configuring symfony/flex (>=1.0): From github.com/symfony/recipes:master
Loading composer repositories with package information
Updating dependencies (including require-dev)
Restricting packages listed in "symfony/symfony" to "4.3.*"
Package operations: 103 installs, 0 updates, 0 removals
  - Installing ocramius/package-versions (1.4.0): Loading from cache
...
```

Composer installs Symfony and its dependencies, and generates a default project\.

If you run into any issues installing Symfony, go to the installation topic in the official documentation: [symfony\.com/doc/current/setup\.html](https://symfony.com/doc/current/setup.html)

## Deploy your application<a name="php-symfony-tutorial-deploy"></a>

Go to the project directory\.

```
~$ cd eb-symfony
```

Create a [source bundle](applications-sourcebundle.md) containing the files created by Composer\. The following command creates a source bundle named `symfony-default.zip`\. It excludes files in the `vendor` folder, which take up a lot of space and are not necessary for deploying your application to Elastic Beanstalk\.

```
eb-symfony$ zip ../symfony-default.zip -r * .[^.]* -x "vendor/*"
```

Upload the source bundle to Elastic Beanstalk to deploy Symfony to your environment\.

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

## Configure Composer settings<a name="php-symfony-tutorial-configure"></a>

When the deployment completes, click the URL to open your Symfony application in the browser:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/php-symfony-403.png)

What's this? By default, Elastic Beanstalk serves the root of your project at the root path of the web site\. In this case, though, the default page \(`app.php`\) is one level down in the `web` folder\. You can verify this by adding `/public` to the URL\. For example, `http://symfony.us-east-2.elasticbeanstalk.com/public`\.

To serve the Symfony application at the root path, use the Elastic Beanstalk console to configure the *document root* for the web site\.

**To configure your web site's document root**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Configuration**\.

1. In the **Software** configuration category, choose **Edit**\.

1. For **Document root**, enter **/public**\.

1. Choose **Apply**\.

1. When the update is complete, click the URL to reopen your site in the browser\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/php-symfony-success.png)

## Cleanup<a name="php-symfony-tutorial-cleanup"></a>

When you finish working with Elastic Beanstalk, you can terminate your environment\. Elastic Beanstalk terminates all AWS resources associated with your environment, such as [Amazon EC2 instances](using-features.managing.ec2.md), [database instances](using-features.managing.db.md), [load balancers](using-features.managing.elb.md), security groups, and [alarms](using-features.alarms.md#using-features.alarms.title)\. 

**To terminate your Elastic Beanstalk environment**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. Choose **Environment actions**, and then choose **Terminate environment**\.

1. Use the on\-screen dialog box to confirm environment termination\.

With Elastic Beanstalk, you can easily create a new environment for your application at any time\.

## Next steps<a name="php-symfony-tutorial-nextsteps"></a>

For more information about Symfony, see [What is Symfony?](https://symfony.com/what-is-symfony) at symfony\.com\.

As you continue to develop your application, you'll probably want a way to manage environments and deploy your application without manually creating a \.zip file and uploading it to the Elastic Beanstalk console\. The [Elastic Beanstalk Command Line Interface](eb-cli3.md) \(EB CLI\) provides easy\-to\-use commands for creating, configuring, and deploying applications to Elastic Beanstalk environments from the command line\.

In this tutorial, you used the Elastic Beanstalk console to configure composer options\. To make this configuration part of your application source, you can use a configuration file like the following\.

**Example \.ebextensions/composer\.config**  

```
option_settings:
  aws:elasticbeanstalk:container:php:phpini:
    document_root: /public
```

For more information, see [Advanced environment customization with configuration files \(`.ebextensions`\)](ebextensions.md)\.

Symfony uses its own configuration files to configure database connections\. For instructions on connecting to a database with Symfony, see [Connecting to a database with Symfony](create_deploy_PHP.rds.md#php-rds-symfony)\.

Finally, if you plan on using your application in a production environment, you will want to [configure a custom domain name](customdomains.md) for your environment and [enable HTTPS](configuring-https.md) for secure connections\.