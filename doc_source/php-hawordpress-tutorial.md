# Deploying a High\-Availability WordPress Website with an External Amazon RDS Database to Elastic Beanstalk<a name="php-hawordpress-tutorial"></a>

This tutorial describes how you [launch an Amazon RDS DB instance](AWSHowTo.RDS.md) that is external to AWS Elastic Beanstalk\. Then it describes how to configure a high\-availability environment running a WordPress website to connect to it\. Running a DB instance external to Elastic Beanstalk decouples the database from the lifecycle of your environment\. This lets you connect to the same database from multiple environments, swap out one database for another, or perform a blue/green deployment without affecting your database\.

**Topics**
+ [Prerequisites](#php-wordpress-tutorial-prereqs)
+ [Launch a DB Instance in Amazon RDS](#php-hawordpress-tutorial-database)
+ [Download WordPress](#php-hawordpress-tutorial-download)
+ [Launch an Elastic Beanstalk Environment](#php-hawordpress-tutorial-launch)
+ [Configure Security Groups and Environment Properties](#php-wordpress-tutorial-configure)
+ [Configure and Deploy Your Application](#php-wordpress-tutorial-deploy)
+ [Install WordPress](#php-hawordpress-tutorial-install)
+ [Update Keys and Salts](#php-hawordpress-tutorial-updatesalts)
+ [Remove Access Restrictions](#php-hawordpress-tutorial-updateenv)
+ [Configure Your Auto Scaling Group](#php-hawordpress-tutorial-autoscaling)
+ [Upgrade WordPress](#php-hawordpress-tutorial-upgrade)
+ [Cleanup](#php-hawordpress-tutorial-cleanup)
+ [Next Steps](#php-hawordpress-tutorial-nextsteps)

## Prerequisites<a name="php-wordpress-tutorial-prereqs"></a>

This tutorial assumes that you have some knowledge of basic Elastic Beanstalk operations and the Elastic Beanstalk console\. If you haven't already, follow the instructions in [Getting Started Using Elastic Beanstalk](GettingStarted.md) to launch your first Elastic Beanstalk environment\.

To follow the procedures in this guide, you will need a command line terminal or shell to run commands\. Commands are shown in listings preceded by a prompt symbol \($\) and the name of the current directory, when appropriate:

```
~/eb-project$ this is a command
this is output
```

On Linux and macOS, use your preferred shell and package manager\. On Windows 10, you can [install the Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/install-win10) to get a Windows\-integrated version of Ubuntu and Bash\.

The procedures in this tutorial for Amazon Relational Database Service \(Amazon RDS\) tasks assume that you are launching resources in a default [Amazon Virtual Private Cloud](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/) \(Amazon VPC\)\. All new accounts include a default VPC in each region\. If you don't have a default VPC, the procedures will vary\. See [Using Elastic Beanstalk with Amazon Relational Database Service](AWSHowTo.RDS.md) for instructions for EC2\-Classic and custom VPC platforms\.

This tutorial was developed with WordPress version 4\.9\.5 and PHP 7\.0\.

## Launch a DB Instance in Amazon RDS<a name="php-hawordpress-tutorial-database"></a>

To use an external database with an application running in Elastic Beanstalk, first launch a DB instance with Amazon RDS\. When you launch an instance with Amazon RDS, it is completely independent of Elastic Beanstalk and your Elastic Beanstalk environments, and will not be terminated or monitored by Elastic Beanstalk\.

Use the Amazon RDS console to launch a Multi\-AZ **MySQL** DB instance\. Choosing a Multi\-AZ deployment ensures that your database will fail over and continue to be available if the master DB instance goes out of service\.

**To launch an RDS DB instance in a default VPC**

1. Open the [RDS console](https://console.aws.amazon.com/rds/home)\.

1. Choose **Instances** in the navigation pane\.

1. Choose **Launch DB instance**\.

1. Choose a database engine\. Choose **Next**\.

1. Choose a use case, if prompted\.

1. Under **Specify DB details**, review the default settings and adjust as necessary\. Pay attention to the following options:
   + **DB instance class** – Choose an instance size that has an appropriate amount of memory and CPU power for your workload\.
   + **Multi\-AZ deployment** – For high availability, set to **Create replica in different zone**\.
   + **Master username** and **Master password** – The database username and password\. Make a note of these settings because you'll use them later\.

1. Choose **Next**\.

1. Under **Database options**, for **Database name**, type **ebdb**\. Make a note of the **Database port** value for use later\.

1. Verify the default settings for the remaining options, and choose **Launch DB instance**\.

Next, modify the security group attached to your DB instance to allow inbound traffic on the appropriate port\. This is the same security group that you will attach to your Elastic Beanstalk environment later, so the rule that you add will grant ingress permission to other resources in the same security group\.

**To modify the ingress rules on your RDS instance's security group**

1. Open the [ Amazon RDS console](https://console.aws.amazon.com/rds/home)\.

1. Choose **Instances**\.

1. Choose the name of your DB instance to view its details\.

1. Under **Details** section, note the **Subnets**, **Security groups**, and **Endpoint** shown on this page so you can use this information later\.

1. Under **Security and network**, you can see the security group associated with the DB instance\. Open the link to view the security group in the Amazon EC2 console\.  
![\[Details section of a DB instance page in the Amazon RDS console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/rds-securitygroup.png)

1. In the security group details, choose **Inbound**\.

1. Choose **Edit**\.

1. Choose **Add Rule**\.

1. For **Type**, choose the DB engine that your application uses\.

1. For **Source**, type **sg\-** to view a list of available security groups\. Choose the current security group to allow resources in the security group to receive traffic on the database port from other resources in the same group\.  
![\[Edit the inbound rules for a security group in the Amazon EC2 console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/ec2-securitygroup-rds.png)

1. Choose **Save**\.

Creating a DB instance takes about 10 minutes\. In the meantime, download WordPress and create your Elastic Beanstalk environment\.

## Download WordPress<a name="php-hawordpress-tutorial-download"></a>

To prepare to deploy WordPress using AWS Elastic Beanstalk, you must copy the WordPress files to your computer and provide some configuration information\.

**To create a WordPress project**

1. Download WordPress from [wordpress\.org](https://wordpress.org/download/)\.

   ```
   ~$ curl https://wordpress.org/wordpress-4.9.5.tar.gz -o wordpress.tar.gz
   ```

1. Download the configuration files from the sample repository:

   ```
   ~$ wget https://github.com/aws-samples/eb-php-wordpress/releases/download/v1.1/eb-php-wordpress-v1.zip
   ```

1. Extract WordPress and change the name of the folder\.

   ```
    ~$ tar -xvf wordpress.tar.gz
    ~$ mv wordpress wordpress-beanstalk
    ~$ cd wordpress-beanstalk
   ```

1. Extract the configuration files over the WordPress installation\.

   ```
    ~/wordpress-beanstalk$ unzip ../eb-php-wordpress-v1.zip
     creating: .ebextensions/
    inflating: .ebextensions/dev.config
    inflating: .ebextensions/efs-create.config
    inflating: .ebextensions/efs-mount.config
    inflating: .ebextensions/loadbalancer-sg.config
    inflating: .ebextensions/wordpress.config
    inflating: LICENSE
    inflating: README.md
    inflating: wp-config.php
   ```

## Launch an Elastic Beanstalk Environment<a name="php-hawordpress-tutorial-launch"></a>

Use the AWS Management Console to create an Elastic Beanstalk environment\. Choose the **PHP** platform and accept the default settings and sample code\. After you launch the environment, you can configure the environment to connect to the database, then deploy the WordPress code to the environment\.

**To launch an environment \(console\)**

1. Open the Elastic Beanstalk console using this preconfigured link: [console\.aws\.amazon\.com/elasticbeanstalk/home\#/newApplication?applicationName=tutorials&environmentType=LoadBalanced&instanceType=t2\.micro](https://console.aws.amazon.com/elasticbeanstalk/home#/newApplication?applicationName=tutorials&environmentType=LoadBalanced&instanceType=t2.micro)

1. For **Platform**, choose the platform that matches the language used by your application\.

1. For **Application code**, choose **Sample application**\.

1. Choose **Review and launch**\.

1. Review the available options\. When you're satisfied with them, choose **Create app**\.

Environment creation takes about 5 minutes and creates the following resources:
+ **EC2 instance** – An Amazon Elastic Compute Cloud \(Amazon EC2\) virtual machine configured to run web apps on the platform that you choose\.

  Each platform runs a specific set of software, configuration files, and scripts to support a specific language version, framework, web container, or combination thereof\. Most platforms use either Apache or nginx as a reverse proxy that sits in front of your web app, forwards requests to it, serves static assets, and generates access and error logs\.
+ **Instance security group** – An Amazon EC2 security group configured to allow ingress on port 80\. This resource lets HTTP traffic from the load balancer reach the EC2 instance running your web app\. By default, traffic isn't allowed on other ports\.
+ **Load balancer** – An Elastic Load Balancing load balancer configured to distribute requests to the instances running your application\. A load balancer also eliminates the need to expose your instances directly to the internet\.
+ **Load balancer security group** – An Amazon EC2 security group configured to allow ingress on port 80\. This resource lets HTTP traffic from the internet reach the load balancer\. By default, traffic isn't allowed on other ports\.
+ **Auto Scaling group** – An Auto Scaling group configured to replace an instance if it is terminated or becomes unavailable\.
+ **Amazon S3 bucket** – A storage location for your source code, logs, and other artifacts that are created when you use Elastic Beanstalk\.
+ **Amazon CloudWatch alarms** – Two CloudWatch alarms that monitor the load on the instances in your environment and are triggered if the load is too high or too low\. When an alarm is triggered, your Auto Scaling group scales up or down in response\.
+ **AWS CloudFormation stack** – Elastic Beanstalk uses AWS CloudFormation to launch the resources in your environment and propagate configuration changes\. The resources are defined in a template that you can view in the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation)\.
+ **Domain name** – A domain name that routes to your web app in the form **subdomain*\.*region*\.elasticbeanstalk\.com*\.

All of these resources are managed by Elastic Beanstalk\. When you terminate your environment, Elastic Beanstalk terminates all the resources that it contains\. The RDS DB instance that you launched is outside of your environment, so you are responsible for managing its lifecycle\.

**Note**  
The Amazon S3 bucket that Elastic Beanstalk creates is shared between environments and is not deleted during environment termination\. For more information, see [Using Elastic Beanstalk with Amazon S3](AWSHowTo.S3.md)\.

## Configure Security Groups and Environment Properties<a name="php-wordpress-tutorial-configure"></a>

Add the security group of your DB instance to your running environment\. This procedure causes Elastic Beanstalk to reprovision all instances in your environment with the additional security group attached\.

**To add a security group to your environment**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. Choose **Configuration**\.

1. On the **Instances** configuration card, choose **Modify**\.

1. Under **EC2 security groups**, choose the security group to attach to the instances, in addition to the instance security group that Elastic Beanstalk creates\.

1. Choose **Apply**\.

1. Read the warning, and then choose **Confirm**\.

Next, use environment properties to pass the connection information to your environment\. The sample application uses a default set of properties that match the ones that Elastic Beanstalk configures when you provision a database within your environment\.

**To configure environment properties for an Amazon RDS DB instance**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. Choose **Configuration**\.

1. On the **Software** configuration card, choose **Modify**\.

1. In the **Environment properties** section, define the variables that your application reads to construct a connection string\. For compatibility with environments that have an integrated RDS DB instance, use the following\.
   + **RDS\_HOSTNAME** – The hostname of the DB instance\.

     Amazon RDS console label – **Endpoint** \(this is the hostname\)
   + **RDS\_PORT** – The port on which the DB instance accepts connections\. The default value varies among DB engines\.

     Amazon RDS console label – **Port**
   + **RDS\_DB\_NAME** – The database name, `ebdb`\.

     Amazon RDS console label – **DB Name**
   + **RDS\_USERNAME** – The user name that you configured for your database\.

     Amazon RDS console label – **Username**
   + **RDS\_PASSWORD** – The password that you configured for your database\.  
![\[Environment Properties section with RDS properties added\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environment-cfg-envprops-rds.png)

1. Choose **Apply**\.

## Configure and Deploy Your Application<a name="php-wordpress-tutorial-deploy"></a>

Verify that the structure of your `wordpress-beanstalk` folder is correct, as shown\.

```
wordpress-beanstalk$ tree -aL 1
.
├── .ebextensions
├── index.php
├── LICENSE
├── license.txt
├── readme.html
├── README.md
├── wp-activate.php
├── wp-admin
├── wp-blog-header.php
├── wp-comments-post.php
├── wp-config.php
├── wp-config-sample.php
├── wp-content
├── wp-cron.php
├── wp-includes
├── wp-links-opml.php
├── wp-load.php
├── wp-login.php
├── wp-mail.php
├── wp-settings.php
├── wp-signup.php
├── wp-trackback.php
└── xmlrpc.php
```

The customized `wp-config.php` file from the project repo uses the environment variables that you defined in the previous step to configure the database connection\. The `.ebextensions` folder contains configuration files that create additional resources within your Elastic Beanstalk environment\.

The configuration files require modification to work with your account\. Replace the placeholder values in the files with the appropriate IDs and create a source bundle\.

**To update configuration files and create a source bundle\.**

1. Modify the configuration files as follows\.
   + `.ebextensions/dev.config` – restricts access to your environment to your IP address to protect it during the WordPress installation process\. Replace the placeholder IP address near the top of the file with the public IP address of the computer you'll use to access your environment's web site to [complete your WordPress installation](#php-hawordpress-tutorial-install)\. Depending on your network topology, you might need to use an IP address block\.
   + `.ebextensions/efs-create.config` – creates an EFS file system and mount points in each Availability Zone / subnet in your VPC\. Identify your default VPC and subnet IDs in the [Amazon VPC console](https://console.aws.amazon.com/vpc/home#subnets:filter=default)\.

1. Create a [source bundle](applications-sourcebundle.md) containing the files in your project folder\. The following command creates a source bundle named `wordpress-beanstalk.zip`\.

   ```
   ~/eb-wordpress$ zip ../wordpress-beanstalk.zip -r * .[^.]*
   ```

Upload the source bundle to Elastic Beanstalk to deploy WordPress to your environment\.

**To deploy a source bundle**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. Choose **Upload and Deploy**\.

1. Choose **Choose File** and use the dialog box to select the source bundle\.

1. Choose **Deploy**\.

1. When the deployment completes, choose the site URL to open your website in a new tab\.

## Install WordPress<a name="php-hawordpress-tutorial-install"></a>

**To complete your WordPress installation**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. Choose the environment URL to open your site in a browser\. You are redirected to a WordPress installation wizard because you haven't configured the site yet\.

1. Perform a standard installation\. The `wp-config.php` file is already present in the source code and configured to read the database connection information from the environment\. You shouldn't be prompted to configure the connection\.

Installation takes about a minute to complete\.

## Update Keys and Salts<a name="php-hawordpress-tutorial-updatesalts"></a>

The WordPress configuration file `wp-config.php` also reads values for keys and salts from environment properties\. Currently, these properties are all set to `test` by the `wordpress.config` file in the `.ebextensions` folder\.

The hash salt can be any value that meets [environment property requirements](environments-cfg-softwaresettings.md#environments-cfg-softwaresettings-console), but you should not store it in source control\. Use the Elastic Beanstalk console to set these properties directly on the environment\.

**To update environment properties**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. On the navigation pane, choose **Configuration**\.

1. Under **Software**, choose **Modify**\.

1. For `Environment properties`, modify the following properties:
   + **AUTH\_KEY** – The value chosen for AUTH\_KEY\.
   + **SECURE\_AUTH\_KEY** – The value chosen for SECURE\_AUTH\_KEY\.
   + **LOGGED\_IN\_KEY** – The value chosen for LOGGED\_IN\_KEY\.
   + **NONCE\_KEY** – The value chosen for NONCE\_KEY\.
   + **AUTH\_SALT** – The value chosen for AUTH\_SALT\.
   + **SECURE\_AUTH\_SALT** – The value chosen for SECURE\_AUTH\_SALT\.
   + **LOGGED\_IN\_SALT** – The value chosen for LOGGED\_IN\_SALT\.
   + **NONCE\_SALT** — The value chosen for NONCE\_SALT\.

1. Choose **Apply**\.

Setting the properties on the environment directly overrides the values in `wordpress.config`\.

## Remove Access Restrictions<a name="php-hawordpress-tutorial-updateenv"></a>

The sample project includes a configuration file \(`loadbalancer-sg.config`\) that creates a security group and assigns it to the environment's load balancer, using the IP address that you configured in `dev.config` to restrict HTTP access on port 80 to connections from your network\. Otherwise, an outside party could potentially connect to your site before you have installed WordPress and configured your admin account\.

Now that you've installed WordPress, remove the configuration file to open the site to the world\.

**To remove the restriction and update your environment**

1. Delete the `.ebextensions/loadbalancer-sg.config` file from your project directory\.

   ```
   ~/wordpress-beanstalk$ rm .ebextensions/loadbalancer-sg.config
   ```

1. Create a source bundle\.

   ```
   ~/eb-wordpress$ zip ../wordpress-beanstalk-v2.zip -r * .[^.]*
   ```

Upload the source bundle to Elastic Beanstalk to deploy WordPress to your environment\.

**To deploy a source bundle**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. Choose **Upload and Deploy**\.

1. Choose **Choose File** and use the dialog box to select the source bundle\.

1. Choose **Deploy**\.

1. When the deployment completes, choose the site URL to open your website in a new tab\.

## Configure Your Auto Scaling Group<a name="php-hawordpress-tutorial-autoscaling"></a>

Finally, configure your environment's Auto Scaling group with a higher minimum instance count\. Run at least two instances at all times to prevent the web servers in your environment from being a single point of failure\. This also allows you to deploy changes without taking your site out of service\.

**To configure your environment's Auto Scaling group for high availability**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. Choose **Configuration**\.

1. On the **Capacity** configuration card, choose **Modify**\.

1. In the **Auto Scaling Group** section, set **Min instances** to **2**\.

1. Choose **Apply**\.

To support content uploads across multiple instances, the sample project uses Amazon Elastic File System to create a shared file system\. Create a post on the site and upload content to store it on the shared file system\. View the post and refresh the page multiple times to hit both instances and verify that the shared file system is working\.

## Upgrade WordPress<a name="php-hawordpress-tutorial-upgrade"></a>

To upgrade to a new version of WordPress, back up your site and deploy it to a new environment\. Do not use the update functionality within WordPress or update your source files to use a new version\. Both of these actions can result in your post URLs returning 404 errors even though they are still in the database and file system\.

**To upgrade WordPress**

1. Export your posts to an XML file with the export tool in the WordPress admin console\.

1. Deploy and install the new version of WordPress to Elastic Beanstalk with the same steps that you used to install the previous version\. To avoid downtime, you can create a new environment with the new version\.

1. On the new version, install the WordPress importer tool in the admin console and use it to import the XML file containing your posts\. If the posts were created by the admin user on the old version, assign them to the admin user on the new site instead of trying to import the admin user\.

1. If you deployed the new version to a separate environment, do a [CNAME swap](using-features.CNAMESwap.md) to redirect users from the old site to the new site\.

## Cleanup<a name="php-hawordpress-tutorial-cleanup"></a>

When you finish working with Elastic Beanstalk, you can terminate your environment\. Elastic Beanstalk terminates all AWS resources associated with your environment, such as [Amazon EC2 instances](using-features.managing.ec2.md), [database instances](using-features.managing.db.md), [load balancers](using-features.managing.elb.md), security groups, and [alarms](using-features.alarms.md#using-features.alarms.title)\. 

**To terminate your Elastic Beanstalk environment**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. Choose **Actions**, and then choose **Terminate Environment**\.

1. In the **Confirm Termination** dialog box, type the environment name, and then choose **Terminate**\.

With Elastic Beanstalk, you can easily create a new environment for your application at any time\.

In addition, you can terminate database resources that you created outside of your Elastic Beanstalk environment\. When you terminate an Amazon RDS database instance, you can take a snapshot and restore the data to another instance later\.

**To terminate your RDS DB instance**

1. Open the [Amazon RDS console](https://console.aws.amazon.com/rds)\.

1. Choose **Instances**\.

1. Choose your DB instance\.

1. Choose **Instance actions**, and then choose **Delete**\.

1. Choose whether to create a snapshot, and then choose **Delete**\.

## Next Steps<a name="php-hawordpress-tutorial-nextsteps"></a>

As you continue to develop your application, you'll probably want a way to manage environments and deploy your application without manually creating a \.zip file and uploading it to the Elastic Beanstalk console\. The [Elastic Beanstalk Command Line Interface](eb-cli3.md) \(EB CLI\) provides easy\-to\-use commands for creating, configuring, and deploying applications to Elastic Beanstalk environments from the command line\.

The sample application uses configuration files to configure PHP settings and create a table in the database, if it doesn't already exist\. You can also use a configuration file to configure your instances' security group settings during environment creation to avoid time\-consuming configuration updates\. See [Advanced Environment Customization with Configuration Files \(`.ebextensions`\)](ebextensions.md) for more information\.

For development and testing, you might want to use the Elastic Beanstalk functionality for adding a managed DB instance directly to your environment\. For instructions on setting up a database inside your environment, see [Adding a Database to Your Elastic Beanstalk Environment](using-features.managing.db.md)\.

If you need a high\-performance database, consider using [Amazon Aurora](https://aws.amazon.com/rds/aurora/)\. Amazon Aurora is a MySQL\-compatible database engine that offers commercial database features at low cost\. To connect your application to a different database, repeat the [security group configuration](php-ha-tutorial.md#php-hawrds-tutorial-database) steps and [update the RDS\-related environment properties](php-ha-tutorial.md#php-hawrds-tutorial-configure)\. 

Finally, if you plan on using your application in a production environment, you will want to [configure a custom domain name](customdomains.md) for your environment and [enable HTTPS](configuring-https.md) for secure connections\.