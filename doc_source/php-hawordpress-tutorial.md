# Deploying a High\-Availability WordPress Website with an External Amazon RDS Database to Elastic Beanstalk<a name="php-hawordpress-tutorial"></a>

This tutorial describes how you [launch an Amazon RDS DB instance](AWSHowTo.RDS.md) that is external to AWS Elastic Beanstalk\. Then it describes how to configure a high\-availability environment running a WordPress website to connect to it\. Running a DB instance external to Elastic Beanstalk decouples the database from the lifecycle of your environment\. This lets you connect to the same database from multiple environments, swap out one database for another, or perform a blue/green deployment without affecting your database\.

**Topics**
+ [Step 1: Launch a DB Instance in Amazon RDS](#php-hawordpress-tutorial-database)
+ [Step 2: Download WordPress](#php-hawordpress-tutorial-download)
+ [Step 3: Launch an Elastic Beanstalk Environment](#php-hawordpress-tutorial-launch)
+ [Step 4: Configure Environment Properties](#php-hawordpress-tutorial-configure)
+ [Step 5: Install WordPress](#php-hawordpress-tutorial-install)
+ [Step 6: Updating Keys and Salts](#php-hawordpress-tutorial-updatesalts)
+ [Step 7: Update the Environment](#php-hawordpress-tutorial-updateenv)
+ [Step 8: Configure Your Auto Scaling Group](#php-hawordpress-tutorial-autoscaling)
+ [Review](#php-hawordpress-tutorial-review)
+ [Clean Up](#w3ab1c43c25c50)
+ [Next Steps](#php-hawordpress-tutorial-nextsteps)

## Step 1: Launch a DB Instance in Amazon RDS<a name="php-hawordpress-tutorial-database"></a>

To use an external database with an application running in Elastic Beanstalk, first launch a DB instance with Amazon RDS\. This DB instance is completely independent of Elastic Beanstalk and your Elastic Beanstalk environments, and will not be terminated or monitored by Elastic Beanstalk\.

Use the Amazon RDS console to launch a Multi\-AZ **MySQL** DB instance\.

Under **Engine options**, choose **MySQL**\. Under **Use case**, choose **Production \- MySQL** to ensure that your database will failover and continue to be available if the master DB instance goes out of service\.

For **DB instance identifier**, type **wordpress\-beanstalk**, and for **Master username**, type **not\_wp\_admin**\. 

**To launch an RDS DB instance in a default [Amazon Virtual Private Cloud](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/) \(Amazon VPC\)**

1. Open the [RDS console](https://console.aws.amazon.com/rds/home)\.

1. Choose **Instances** in the navigation pane\.

1. Choose **Launch DB instance**\.

1. Under **Engine options**, choose the engine that will meet your needs, and then choose **Next**\.

   If prompted to select **Use case**, choose **Production** for Multi\-AZ deployment or choose **Dev/Test** to consume only Free Tier resources\. Then choose **Next**\.

1. Under **Specify DB details**, for **Instance specifications**, choose the following and then keep the default settings for the remaining options:
   + **DB instance class** – Computation and memory capacity \(if unsure, [learn which option is right for you ](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide//Concepts.DBInstanceClass.html)\) 
   + **Multi\-AZ deployment** – For high availability, set to **Create replica in different zone**\.

1. Under **Estimated monthly costs**, review the total\. Adjust **Use case** and **DB instance class** to fit your budget as needed\.

1. Under **Settings**, enter values for **DB instance identifier**, **Master username**, **Master password**, and **Confirm password**\. Make a note of these settings because you'll use them later\. 

1. Choose **Next**\.

1. Under **Configure advanced settings**, for **Network & Security**, choose the following:
   + **Virtual Private Cloud \(VPC\)** – Keep default value 
   + **Subnet group** – **Default**
   + **Public accessibility** – **No**
   + **Availability Zone** – ** No Preference**
   + **VPC security groups** – **Create new VPC Security Group**

1. Under **Database options**, for **Database name**, type **ebdb**, and verify the default settings for the remaining options\. Make a note of the **Database port** value for use later\.

1. Verify the default settings for the remaining options, and choose **Launch DB instance**\.

Next, you modify the security group that is attached to your DB instance to allow inbound traffic on the appropriate port\. This is the same security group that you will attach to your Elastic Beanstalk environment later, so the rule that you add will grant ingress permission to other resources in the same security group\.

**To modify the ingress rules on your RDS instance's security group**

1. Open the [ Amazon RDS console](https://console.aws.amazon.com/rds/home)\.

1. Choose **Instances**\.

1. Choose the name of your recently created DB instance to view its details\.

1. Go to the **Details** section\.

1. In the **Details** section, note the **Subnets**, **Security groups**, and **Endpoint** shown on this page so you can use this information later\.
**Note**  
The security group name is the first value of the link text shown in **Security groups**, before the value in parentheses\. This second value, in parentheses, is the security group ID\.

1. Under **Security and network**, you can see the security group associated with the DB instance\. Open the link to view the security group in the Amazon EC2 console\.  
![\[Details section of a DB instance page in the Amazon RDS console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/rds-securitygroup.png)

1. In the security group details, choose the **Inbound** view\.

1. Choose **Edit**\.

1. Choose **Add Rule**\.

1. For **Type**, choose the DB engine that your application uses\.

1. For **Source**, choose **Custom**, and then type the group ID of the security group\. This allows resources in the security group to receive traffic on the database port from other resources in the same group\.  
![\[Edit the inbound rules for a security group in the Amazon EC2 console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/ec2-securitygroup-rds.png)

1. Choose **Save**\.

Creating a DB instance takes about 10 minutes\. In the meantime, download WordPress and launch your Elastic Beanstalk environment\.

## Step 2: Download WordPress<a name="php-hawordpress-tutorial-download"></a>

To prepare to deploy WordPress using AWS Elastic Beanstalk, you must copy the WordPress files to your computer and provide some configuration information\. AWS Elastic Beanstalk requires a source bundle, in the format of a \.zip or \.war file\.

**To download WordPress and create a source bundle**

1. Open [http://wordpress\.org/download/](http://wordpress.org/download/)\.

1. Download the latest release\.

1. Extract the WordPress files from the download to a folder on your local computer, and rename the root folder to `wordpress-beanstalk`\. 

1. Download the configuration files from the following repository:

   ```
   [https://github\.com/awslabs/eb\-php\-wordpress/releases/download/v1\.0/eb\-php\-wordpress\-v1\.zip](https://github.com/awslabs/eb-php-wordpress/releases/download/v1.0/eb-php-wordpress-v1.zip)
   ```

1. Extract the configuration files into your `wordpress-beanstalk` folder\.

1. Verify that the structure of your `wordpress-beanstalk` folder is correct, as shown\.

   ```
   ├── .ebextensions
   ├── wp-admin
   │ ├── css
   │ ├── images
   │ ├── includes
   │ ├── js
   │ ├── maint
   │ ├── network
   │ └── user
   ├── wp-content
   │ ├── plugins
   │ └── themes
   ├── wp-includes
   │ ├── certificates
   │ ├── css
   │ ├── customize
   │ ├── fonts
   │ ├── ID3
   │ ├── images
   │ ├── js
   │ ├── pomo
   │ ├── random_compat
   │ ├── Requests
   │ ├── rest-api
   │ ├── SimplePie
   │ ├── Text
   │ ├── theme-compat
   │ └── widgets
   ```

1. Modify the configuration files in the `.ebextensions` folder with the IDs of your default [Amazon Virtual Private Cloud](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/) \(Amazon VPC\) and subnets, and your public IP address\.

   Open the `.ebextensions/efs-create.config` file\. This file creates an Amazon EFS file system and mount points in each Availability Zone or subnet in your Amazon VPC\. Identify your default VPC and subnet IDs in the Amazon VPC console\.

   1. Open the [ Amazon RDS console](https://console.aws.amazon.com/rds/home)\.

   1. Choose **Instances**\.

   1. Choose the name of your recently created DB instance to view details\.

   1. Go to **Details**\.

   1. Under **Security and network**, copy the value for the **VPC** and the values for **Subnets** to the `.ebextensions/efs-create.config` file\.

   The `.ebextensions/dev.config` file restricts access to your environment to your IP address to protect it during the WordPress installation process\. Replace the placeholder IP address near the top of the file with your public IP address, followed by **/32**\.

1. Create a \.zip file from the files and folders in the `wordpress-beanstalk` folder \(not the parent directory\), using one of the following methods, depending on your operating system:
   + Windows – In File Explorer, select the files and folders, right\-click, and then choose **Send to**, **Compressed \(zipped\) folder**\. Name the file `wordpress-x.y.z.zip`, where `x.y.z` is the version of WordPress\.
   + Mac OS X and Linux – Use the following command, where `x.y.z` is the version of WordPress\.

     ```
     zip -r ../wordpress-x.y.z.zip .
     ```

1. Verify that the root structure of your \.zip file is the same as in step 6\.

## Step 3: Launch an Elastic Beanstalk Environment<a name="php-hawordpress-tutorial-launch"></a>

Use the AWS Management Console to launch an Elastic Beanstalk environment\.

1. Open the Elastic Beanstalk console with this preconfigured link: [console\.aws\.amazon\.com/elasticbeanstalk/home\#/newApplication?applicationName=wordpress\-beanstalk&environmentType=LoadBalanced](https://console.aws.amazon.com/elasticbeanstalk/home#/newApplication?applicationName=wordpress-beanstalk&environmentType=LoadBalanced)

1. Under **Application Information**, for **Application name**, type **wordpress\-beanstalk**, and then choose **Next**\. 

1. For **New Environment**, choose **Create web server**\.

1. Under **Environment Type**, for **Predefined configuration**, choose **PHP**\.
**Note**  
If you need to use a version of WordPress earlier than 4\.0, your Elastic Beanstalk environment must use an earlier version of the PHP platform\. Choose **Change platform version**, and then select the latest PHP 5\.6 version from the drop\-down menu\.

1. For **Environment type**, choose **Load balancing, auto scaling**, and then choose **Next**\.

1. Under **Application Version**, for **Source**, choose **Upload your own**\.

1. Choose **Choose File** and navigate to the \.zip file you created for your WordPress files\.

1. For **Deployment Preferences**, leave the defaults for everything, and then choose **Next**\. 

1. For **Environment Information**, customize if you want, or use defaults\. Then choose **Next**\. 

1. Under **Additional Resources**, choose **Create this environment inside a VPC**, and then choose **Next**\. 

1. Under **Configuration Details**, review the options, and then choose **Next**\. 

1. For **Environment Tags**, specify tags if you want, and then choose **Next**\. 

1.  Under **VPC Configuration**, select all of the subnets for both **ELB** and **EC2**\. 

1. Under **VPC Configuration**, for **VPC security group**, select the security group for your RDS instance\. Then choose **Next**\. 

1. For **Permissions**, leave the default selected, and then choose **Next**\. 

1. Review all the options you selected, and when you are satisfied with them, choose **Launch**\.

Environment creation takes about 5 minutes\.

## Step 4: Configure Environment Properties<a name="php-hawordpress-tutorial-configure"></a>

Next, pass the connection information to your environment by using environment properties\. The sample application uses a default set of properties that match the ones that Elastic Beanstalk configures when you provision a database within your environment\.

**To configure environment properties for an Amazon RDS DB instance**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. Choose **Configuration**\.

1. On the **Software** configuration card, choose **Modify**\.

1. In the **Environment Properties** section, define the variables that your application reads to construct a connection string\. For compatibility with environments that have an integrated RDS DB instance, use the following\. Choose the plus symbol \(\+\) to add more properties\.
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

1. Choose **Save**, and then choose **Apply**\.

## Step 5: Install WordPress<a name="php-hawordpress-tutorial-install"></a>

**To complete your WordPress installation**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. Choose the environment URL to open your site in a browser\. You are redirected to a WordPress installation wizard because you haven't configured the site yet\.

1. Perform a standard installation\. The `wp-config.php` file is already present in the source code and configured to read the database connection information from the environment\. You shouldn't be prompted to configure the connection\.

Installation takes about a minute to complete\.

## Step 6: Updating Keys and Salts<a name="php-hawordpress-tutorial-updatesalts"></a>

The WordPress configuration file `wp-config.php` also reads values for keys and salts from environment properties\. Currently, these properties are all set to `test` by the `wordpress.config` file in the `.ebextensions` folder\.

The hash salt can be any value, but you should not store it in source control\. Use the Elastic Beanstalk console to set these properties directly on the environment\.

**To add environment properties**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. On the navigation pane, choose `Configuration`\.

1. For `Software Configuration`, choose the gear icon\.

1. For `Environment Properties`, define the following authentication settings:
   + **AUTH\_KEY** – The value chosen for AUTH\_KEY\.
   + **SECURE\_AUTH\_KEY** – The value chosen for SECURE\_AUTH\_KEY\.
   + **LOGGED\_IN\_KEY** – The value chosen for LOGGED\_IN\_KEY\.
   + **NONCE\_KEY** – The value chosen for NONCE\_KEY\.
   + **AUTH\_SALT** – The value chosen for AUTH\_SALT\.
   + **SECURE\_AUTH\_SALT** – The value chosen for SECURE\_AUTH\_SALT\.
   + **LOGGED\_IN\_SALT** – The value chosen for LOGGED\_IN\_SALT\.
   + **NONCE\_SALT** — The value chosen for NONCE\_SALT\.

   Setting the properties on the environment directly overrides the values in `wordpress.config`\.

## Step 7: Update the Environment<a name="php-hawordpress-tutorial-updateenv"></a>

If the security group uses the IP address, then this tutorial includes a configuration file \(`loadbalancer-sg.config`\) that creates a security group and assigns it to the environment's load balancer\. The security group uses the IP address that you configured in `dev.config`\. It restricts HTTP access over port 80 to connections from your network\. This prevents an outside party from potentially connecting to your site before you complete your WordPress installation and configure your admin account\. To remove this restriction from your load balancer configuration and open the site to the internet, you can use the following steps\.

**To remove the restriction and update your environment**

1. On your local computer, delete the `.ebextensions/loadbalancer-sg-config` file from the `wordpress-beanstalk` folder\.

1. Create a \.zip file from the files and folders in the `wordpress-beanstalk` folder \(not the parent directory\), using one of the following methods, depending on your operating system:
   + Windows – In Windows Explorer, select the files and folders, right\-click, and then choose **Send to**, **Compressed \(zipped\) Folder**\. Name the file using the following format, where `x.y.z` is the version of WordPress\.

     ```
     wordpress-x.y.z-v2.zip
     ```
   + Mac OS X and Linux – Use the following command, where `x.y.z` is the version of WordPress\.

     ```
     zip -r ../wordpress-x.y.z-v2.zip .
     ```

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. Choose **Upload and Deploy**\.

1. Choose **Choose File**, and then navigate to the \.zip file you created for your WordPress files\.

1. Enter a **Version label** that distinguishes this updated version from your previous version\.

1. Choose **Deploy**\.

## Step 8: Configure Your Auto Scaling Group<a name="php-hawordpress-tutorial-autoscaling"></a>

Finally, configure your environment's Auto Scaling group with a higher minimum instance count\. Run at least two instances at all times to prevent the web servers in your environment from being a single point of failure\. This also allows you to deploy changes without taking your site out of service\.

**To configure your environment's Auto Scaling group for high availability**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. Choose **Configuration**\.

1. On the **Capacity** configuration card, choose **Modify**\.

1. In the **Auto Scaling Group** section, set **Min instances** to **2** and **Max instances** to a value greater than **2**\.

1. Choose **Save**, and then choose **Apply**\.

## Review<a name="php-hawordpress-tutorial-review"></a>

Launching an environment creates the following resources:
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

Elastic Beanstalk manages all of these resources\. When you terminate your environment, Elastic Beanstalk terminates all the resources that it contains\. The RDS DB instance that you launched is outside of your environment, so you are responsible for managing its lifecycle\.

**Note**  
The S3 bucket that Elastic Beanstalk creates is shared between environments and is not deleted during environment termination\. For more information, see [Using Elastic Beanstalk with Amazon Simple Storage Service](AWSHowTo.S3.md)\.

## Clean Up<a name="w3ab1c43c25c50"></a>

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

## Next Steps<a name="php-hawordpress-tutorial-nextsteps"></a>

As you continue to develop your application, you'll probably want to manage environments and deploy your application without manually creating a \.zip file and uploading it to the Elastic Beanstalk console\. The [Elastic Beanstalk Command Line Interface](eb-cli3.md) \(EB CLI\) provides easy\-to\-use commands for creating, configuring, and deploying applications to Elastic Beanstalk environments from the command line\.

The sample application uses configuration files to configure PHP settings and create a table in the database, if it doesn't already exist\. You can also use a configuration file to configure your instances' security group settings during environment creation to avoid time\-consuming configuration updates\. See [Advanced Environment Customization with Configuration Files \(`.ebextensions`\)](ebextensions.md) for more information\.

For development and testing, you might want to use the Elastic Beanstalk functionality for adding a managed DB instance directly to your environment\. For instructions on setting up a database inside your environment, see [Adding a Database to Your Elastic Beanstalk Environment](using-features.managing.db.md)\.

If you need a high\-performance database, consider using [Amazon Aurora](https://aws.amazon.com/rds/aurora/)\. Amazon Aurora is a MySQL\-compatible database engine that offers commercial database features at low cost\. To connect your application to a different database, repeat the [security group configuration](#php-hawordpress-tutorial-database) steps and [update the RDS\-related environment properties](#php-hawordpress-tutorial-configure)\. 

If you plan on using your application in a production environment, [configure a custom domain name](customdomains.md) for your environment\.

If you want to [enable HTTPS](configuring-https.md) for secure connections, there are WordPress plugins available to assist\. One example is the [Really Simple SSL](https://wordpress.org/plugins/really-simple-ssl/) plugin\.