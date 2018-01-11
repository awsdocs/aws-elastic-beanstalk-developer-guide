# Deploying a High\-Availability Drupal Website with an External Amazon RDS Database to Elastic Beanstalk<a name="php-hadrupal-tutorial"></a>

This tutorial walks you through the process of launching an RDS DB instance external to AWS Elastic Beanstalk, and configuring a high\-availability environment running a Drupal website to connect to it\. Running a DB instance external to Elastic Beanstalk decouples the database from the lifecycle of your environment, and lets you connect to the same database from multiple environments, swap out one database for another, or perform a blue/green deployment without affecting your database\.


+ [Launch a DB Instance in Amazon RDS](#php-hadrupal-tutorial-database)
+ [Download Drupal](#php-hadrupal-tutorial-download)
+ [Launch an Elastic Beanstalk Environment](#php-hadrupal-tutorial-launch)
+ [Configure Security Groups and Environment Properties](#php-hadrupal-tutorial-configure)
+ [Install Drupal](#php-hadrupal-tutorial-install)
+ [Update the Environment](#php-hadrupal-tutorial-updateenv)
+ [Configure Autoscaling](#php-hadrupal-tutorial-autoscaling)
+ [Review](#php-hadrupal-tutorial-review)
+ [Clean Up](#w3ab1c43c23c46)
+ [Next Steps](#php-hadrupal-tutorial-nextsteps)

## Launch a DB Instance in Amazon RDS<a name="php-hadrupal-tutorial-database"></a>

To use an external database with an application running in Elastic Beanstalk, first launch a DB instance with Amazon RDS\. When you launch an instance with Amazon RDS, it is completely independent of Elastic Beanstalk and your Elastic Beanstalk environments, and will not be terminated or monitored by Elastic Beanstalk\.

Use the Amazon RDS console to launch a Multi\-AZ **MySQL** DB instance\. Choosing a Multi\-AZ deployment ensures that your database will failover and continue to be available if the master DB instance goes out of service\.

**To launch an RDS DB instance in a default VPC**

1. Open the [RDS console](https://console.aws.amazon.com/rds/home)\.

1. Choose **Instances** in the navigation pane\.

1. Choose **Launch DB Instance**\.

1. Choose a **DB Engine** and preset configuration\.

1. Under **Specify DB Details**, choose a **DB Instance Class**\. For high availability, set **Multi\-AZ Deployment** to **Yes**\.

1. Under **Settings**, enter values for **DB Instance Identifier**, **Master Username**, and **Master Password** \(and **Confirm Password**\)\. Note the values that you entered for later\.

1. Choose **Next**\.

1. For **Network and Security** settings, choose the following:

   + **VPC** – **Default VPC**

   + **Subnet Group** – **default**

   + **Publicly Accessible** – **No**

   + **Availability Zone** – ** No Preference**

   + **VPC Security Groups** – **Default VPC Security Group**

1. For **Database Name**, type **ebdb**, and verify the default settings for the remaining options\. Note the values of the following options:

   + **Database Name**

   + **Database Port**

1. Choose **Launch DB Instance**\.

Next, modify the security group attached to your DB instance to allow inbound traffic on the appropriate port\. This is the same security group that you will attach to your Elastic Beanstalk environment later, so the rule that you add will grant ingress permission to other resources in the same security group\.

**To modify the ingress rules on your RDS instance's security group**

1. Open the [Amazon RDS console](https://console.aws.amazon.com/rds/home)\.

1. Choose **Instances**\.

1. Choose the arrow next to the entry for your DB instance to expand the view\.

1. Choose the **Details** tab\.

1. In the **Security and Network** section, the security group associated with the DB instance is shown\. Open the link to view the security group in the Amazon EC2 console\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/rds-securitygroup.png)
**Note**  
While you have the **Details** tab open, note the **Endpoint** and security group name shown on this page for use later\.  
The security group name is the first value of the link shown in **Security Groups**, before the parentheses\. The second value, in parentheses, is the security group ID\.

1. In the security group details, choose the **Inbound** tab\.

1. Choose **Edit**\.

1. Choose **Add Rule**\.

1. For **Type**, choose the DB engine that your application uses\.

1. For **Source**, choose **Custom**, and then type the group ID of the security group\. This allows resources in the security group to receive traffic on the database port from other resources in the same group\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/ec2-securitygroup-rds.png)

1. Choose **Save**\.

Creating a DB instance takes about 10 minutes\. In the meantime, download Drupal and launch your Elastic Beanstalk environment\.

## Download Drupal<a name="php-hadrupal-tutorial-download"></a>

To prepare to deploy Drupal using AWS Elastic Beanstalk, you must copy the Drupal files to your computer and provide some configuration information\. AWS Elastic Beanstalk requires a source bundle, in the format of a ZIP or WAR file\.

**To download Drupal and create a source bundle**

1. Open [https://www\.drupal\.org/download](https://www.drupal.org/download)\.

1. Download the latest release\.

1. Extract the Drupal files from the download to a folder on your local computer, which you should rename to `drupal-beanstalk`\.

1. Download the configuration files in the following repository:

   ```
   [https://github\.com/awslabs/eb\-php\-drupal/releases/download/v1\.0/eb\-php\-drupal\-v1\.zip](https://github.com/awslabs/eb-php-drupal/releases/download/v1.0/eb-php-drupal-v1.zip)
   ```

1. Extract the configuration files into your `drupal-beanstalk` folder\.

1. Verify that the structure of your `drupal-beanstalk` folder is correct\.

   ```
   ├── .ebextensions
   ├── core
   │ ├── assets
   │ ├── config
   │ ├── includes
   │ ├── lib
   │ ├── misc
   │ ├── modules
   │ ├── profiles
   │ ├── scripts
   │ ├── tests
   │ └── themes
   ├── modules
   ├── profiles
   ├── sites
   │ └── default
   ├── themes
   ├── vendor
   │ ├── asm89
   │ ├── composer
   │ ├── doctrine
   │ ├── easyrdf
   │ ├── egulias
   │ ├── guzzlehttp
   │ ├── ircmaxwell
   │ ├── masterminds
   │ ├── paragonie
   │ ├── psr
   │ ├── stack
   │ ├── symfony
   │ ├── symfony-cmf
   │ ├── twig
   │ ├── wikimedia
   │ └── zendframework
   ```

1. Modify the configuration files in the `.ebextensions` folder with the IDs of your default VPC and subnets, and your public IP address\.

1. The `.ebextensions/efs-create.config` file creates an EFS file system and mount points in each Availability Zone/subnet in your VPC\. Identify your default VPC and subnet IDs in the Amazon VPC console\.

   \-\-OR\-\-

   The `.ebextensions/dev.config` file restricts access to your environment to your IP address to protect it during the Drupal installation process\. Replace the placeholder IP address near the top of the file with your public IP address\.

1. Create a ZIP file from the files and folders in the `drupal-beanstalk` folder \(not the parent directory\), using one of the following methods, depending on your operating system:

1. Windows — In Windows Explorer, select the files and folders, right\-click, and then choose **Send to**, **Compressed \(zipped\) Folder**\. Name the file `drupal-x.y.z.zip`, where `x.y.z` is the version of Drupal\.

   \-\-OR\-\-

   Mac OS X and Linux — Use the following command, where `x.y.z` is the version of Drupal:

   ```
   zip -r ../drupal-x.y.z.zip .
   ```

## Launch an Elastic Beanstalk Environment<a name="php-hadrupal-tutorial-launch"></a>

Use the AWS Management Console to launch an Elastic Beanstalk environment\.

1. Open the Elastic Beanstalk console with this preconfigured link: [console\.aws\.amazon\.com/elasticbeanstalk/home\#/newApplication?applicationName=drupal\-beanstalk&environmentType=LoadBalanced](https://console.aws.amazon.com/elasticbeanstalk/home#/newApplication?applicationName=drupal-beanstalk&environmentType=LoadBalanced)

1. For **Platform**, choose **PHP**\.

1. For **App code**, choose **Upload your code**\.

1. Choose **Upload** and navigate to the ZIP file you created for your Drupal files\.

1. Choose **Upload** to select your application code\.

1. Choose **Configure more options**\.

1. For **Configuration presets**, select **Custom configuration**\.

1. Choose **Change platform configuration** and select **64bit Amazon Linux 2016\.09 v2\.3\.1 running PHP 5\.6** from the drop down menu and then choose **Save**\.

1. Review all options and once you are satisfied with those options choose **Create app**\.

Environment creation takes about 5 minutes\.

## Configure Security Groups and Environment Properties<a name="php-hadrupal-tutorial-configure"></a>

Next, add the DB instance's security group to your running environment\. This procedure causes Elastic Beanstalk to reprovision all instances in your environment with the additional security group attached\.

**To add a security group to your environment**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the management page for your environment\.

1. Choose **Configuration**\.

1. In the **Instances** section, choose the settings icon \( ![\[Edit\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/cog.png)\)\.

1. For **EC2 security groups**, type a comma after the name of the autogenerated security group followed by the name of the RDS DB instance's security group\. It is the name you noted while configuring the security group earlier\.

1. Choose **Apply**\.

1. Read the warning, and then choose **Save**\.

Next, pass the connection information to your environment by using environment properties\. The sample application uses a default set of properties that match the ones that Elastic Beanstalk configures when you provision a database within your environment\.

**To configure environment properties for an Amazon RDS DB instance**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the management page for your environment\.

1. Choose **Configuration**\.

1. In the **Software Configuration** section, choose the settings icon \( ![\[Edit\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/cog.png)\)\.

1. In the **Environment Properties** section, define the variables that your application reads to construct a connection string\. For compatibility with environments that have an integrated RDS DB instance, use the following:

   + **RDS\_HOSTNAME** – The hostname of the DB instance\.

     Amazon RDS console label – **Endpoint** is the hostname\.

   + **RDS\_PORT** – The port on which the DB instance accepts connections\. The default value varies between DB engines\.

     Amazon RDS console label – **Port**

   + **RDS\_DB\_NAME** – The database name, `ebdb`\.

     Amazon RDS console label – **DB Name**

   + **RDS\_USERNAME** – The user name that you configured for your database\.

     Amazon RDS console label – **Username**

   + **RDS\_PASSWORD** – The password that you configured for your database\.

   Choose the plus symbol \(\+\) to add additional properties\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environment-cfg-envprops-rds.png)

1. Choose **Apply**\.

## Install Drupal<a name="php-hadrupal-tutorial-install"></a>

**To complete your Drupal installation**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the management page for your environment\.

1. Choose the environment URL to open your site in a browser\. You are redirected to a Drupal installation wizard because the site has not been configured yet\.

1. Perform a standard installation with the following settings for the database:

   + **Database name** – The **DB Name** shown in the Amazon RDS console\.

   + **Database username and password** – The **Master Username** and **Master Password** values you entered when creating your database\.

   + **Advanced Options > Host** – The **Endpoint** of the DB instance shown in the Amazon RDS console\.

Installation takes about a minute to complete\.

## Update the Environment<a name="php-hadrupal-tutorial-updateenv"></a>

This tutorial includes a configuration file \(`loadbalancer-sg.config`\) that creates a security group and assigns it to the environment's load balancer, using the IP address that you configured in `dev.config` to restrict HTTP access over port 80 to connections from your network\. This prevents an outside party from potentially connecting to your site before you have completed your Drupal installation and configured your admin account\. To remove this restriction from your load balancer configuration and open the site to the Internet you can use the following steps\.

**To remove the restriction and update your environment**

1. On your local computer, delete the `.ebextensions/loadbalancer-sg-config` file from the `drupal-beanstalk` folder\.

1. Create a ZIP file from the files and folders in the `drupal-beanstalk` folder \(not the parent directory\), using one of the following methods, depending on your operating system:

1. Windows — In Windows Explorer, select the files and folders, right\-click, and then choose **Send to**, **Compressed \(zipped\) Folder**\. Name the file using the following format, where `x.y.z` is the version of Drupal\.

   ```
   drupal-x.y.z-v2.zip
   ```

   \-\-OR\-\-

   Mac OS X and Linux — Use the following command, where `x.y.z` is the version of Drupal:

   ```
   zip -r ../drupal-x.y.z-v2.zip .
   ```

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the management page for your environment\.

1. Choose **Upload and Deploy**\.

1. Choose **Choose File** and navigate to the ZIP file you created for your Drupal files\.

1. Enter a **Version label** that distinguishes this updated version from your previous version\.

1. Choose **Deploy**\.

## Configure Autoscaling<a name="php-hadrupal-tutorial-autoscaling"></a>

Finally, configure your environment's Auto Scaling group with a higher minimum instance count\. Run at least two instances at all times to prevent the web servers in your environment from being a single point of failure, and to allow you to deploy changes without taking your site out of service\.

**To configure your environment's Auto Scaling group for high availability**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the management page for your environment\.

1. Choose **Configuration**\.

1. In the **Scaling** section, choose the settings icon \( ![\[Edit\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/cog.png)\)\.

1. Under **Auto Scaling**, set **Minimum instance count** to **2** and the **Maximum instance count** to a value greater than **2**\.

1. Choose **Apply**\.

## Review<a name="php-hadrupal-tutorial-review"></a>

Launching an environment creates the following resources:

+ **EC2 instance** – An Amazon Elastic Compute Cloud \(Amazon EC2\) virtual machine configured to run web apps on the platform that you choose\.

  Each platform runs a different set of software, configuration files, and scripts to support a specific language version, framework, web container, or combination thereof\. Most platforms use either Apache or nginx as a reverse proxy that sits in front of your web app, forwards requests to it, serves static assets, and generates access and error logs\.

+ **Instance security group** – An Amazon EC2 security group configured to allow ingress on port 80\. This resource lets HTTP traffic from the load balancer reach the EC2 instance running your web app\. By default, traffic is not allowed on other ports\.

+ **Load balancer** – An Elastic Load Balancing load balancer configured to distribute requests to the instances running your application\. A load balancer also eliminates the need to expose your instances directly to the Internet\.

+ **Load balancer security group** – An Amazon EC2 security group configured to allow ingress on port 80\. This resource lets HTTP traffic from the Internet reach the load balancer\. By default, traffic is not allowed on other ports\.

+ **Auto Scaling group** – An Auto Scaling group configured to replace an instance if it is terminated or becomes unavailable\.

+ **Amazon S3 bucket** – A storage location for your source code, logs, and other artifacts that are created when you use Elastic Beanstalk\.

+ **Amazon CloudWatch alarms** – Two CloudWatch alarms that monitor the load on the instances in your environment and are triggered if the load is too high or too low\. When an alarm is triggered, your Auto Scaling group scales up or down in response\.

+ **AWS CloudFormation stack** – Elastic Beanstalk uses AWS CloudFormation to launch the resources in your environment and propagate configuration changes\. The resources are defined in a template that you can view in the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation)\.

+ **Domain name** – A domain name that routes to your web app in the form **subdomain*\.*region*\.elasticbeanstalk\.com*\.

All of these resources are managed by Elastic Beanstalk\. When you terminate your environment, Elastic Beanstalk terminates all of resources that it contains\. The RDS DB instance that you launched is outside of your environment, so you are responsible for managing its lifecycle\.

**Note**  
The S3 bucket that Elastic Beanstalk creates is shared between environments and is not deleted during environment termination\. For more information, see \.

## Clean Up<a name="w3ab1c43c23c46"></a>

When you finish working with Elastic Beanstalk, you can terminate your environment\. Elastic Beanstalk terminates all AWS resources associated with your environment, such as Amazon EC2 instances, database instances, load balancers, security groups, and alarms\. 

**To terminate your Elastic Beanstalk environment**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the management page for your environment\.

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

## Next Steps<a name="php-hadrupal-tutorial-nextsteps"></a>

As you continue to develop your application, you'll probably want to manage environments and deploy your application without manually creating a \.zip file and uploading it to the Elastic Beanstalk console\. The Elastic Beanstalk Command Line Interface \(EB CLI\) provides easy\-to\-use commands for creating, configuring, and deploying applications to Elastic Beanstalk environments from the command line\.

The sample application uses configuration files to configure PHP settings and create a table in the database if it doesn't already exist\. You can also use a configuration file to configure your instances' security group settings during environment creation to avoid time\-consuming configuration updates\. See  for more information\.

For development and testing, you might want to use Elastic Beanstalk's functionality for adding a managed DB instance directly to your environment\. For instructions on setting up a database inside your environment, see \.

If you need a high\-performance database, consider using [Amazon Aurora](https://aws.amazon.com/rds/aurora/)\. Amazon Aurora is a MySQL\-compatible database engine that offers commercial database features at low cost\. To connect your application to a different database, repeat the security group configuration steps and update the RDS\-related environment properties\. 

Finally, if you plan on using your application in a production environment, configure a custom domain name for your environment and enable HTTPS for secure connections\.