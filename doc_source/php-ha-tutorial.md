# Deploying a High\-Availability PHP Application with an External Amazon RDS Database to Elastic Beanstalk<a name="php-ha-tutorial"></a>

This tutorial walks you through the process of [launching an RDS DB instance](AWSHowTo.RDS.md) external to AWS Elastic Beanstalk, and configuring a high\-availability environment running a PHP application to connect to it\. Running a DB instance external to Elastic Beanstalk decouples the database from the lifecycle of your environment\. This lets you connect to the same database from multiple environments, swap out one database for another, or perform a blue/green deployment without affecting your database\.

The tutorial uses a [sample PHP application](https://github.com/awslabs/eb-demo-php-simple-app) that uses a MySQL database to store user\-provided text data\. The sample application uses [configuration files](ebextensions.md) to configure [PHP settings](create_deploy_PHP.container.md#php-namespaces) and to create a table in the database for the application to use\. It also shows how to use a [Composer file](php-configuration-composer.md) to install packages during deployment\.


+ [Prerequisites](#php-hawrds-tutorial-prereqs)
+ [Launch a DB Instance in Amazon RDS](#php-hawrds-tutorial-database)
+ [Launch an Elastic Beanstalk Environment](#php-hawrds-tutorial-launch)
+ [Configure Security Groups, Environment Properties, and Scaling](#php-hawrds-tutorial-configure)
+ [Deploy the Sample Application](#php-hawrds-tutorial-deploy)
+ [Clean Up](#w3ab1c43c19c38)
+ [Next Steps](#php-hawrds-tutorial-nextsteps)

## Prerequisites<a name="php-hawrds-tutorial-prereqs"></a>

Before you start, download the sample application source bundle from GitHub: [eb\-demo\-php\-simple\-app\-1\.1\.zip](https://github.com/awslabs/eb-demo-php-simple-app/releases/download/v1.1/eb-demo-php-simple-app-v1.1.zip)

The procedures in this tutorial for Amazon Relational Database Service \(Amazon RDS\) tasks assume that you are launching resources in a default VPC\. All new accounts include a default VPC in each region\. If you don't have a default VPC, the procedures will vary\. See [Using Elastic Beanstalk with Amazon Relational Database Service](AWSHowTo.RDS.md) for instructions for EC2\-Classic and custom VPC platforms\.

## Launch a DB Instance in Amazon RDS<a name="php-hawrds-tutorial-database"></a>

To use an external database with an application running in Elastic Beanstalk, first launch a DB instance with Amazon RDS\. When you launch an instance with Amazon RDS, it is completely independent of Elastic Beanstalk and your Elastic Beanstalk environments, and will not be terminated or monitored by Elastic Beanstalk\.

Use the Amazon RDS console to launch a Multi\-AZ **MySQL** DB instance\. Choosing a Multi\-AZ deployment ensures that your database will fail over and continue to be available if the master DB instance goes out of service\.

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
![\[Details tab of a DB instance page in the Amazon RDS console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/rds-securitygroup.png)
**Note**  
While you have the **Details** tab open, note the **Endpoint** and security group name shown on this page for use later\.  
The security group name is the first value of the link shown in **Security Groups**, before the parentheses\. The second value, in parentheses, is the security group ID\.

1. In the security group details, choose the **Inbound** tab\.

1. Choose **Edit**\.

1. Choose **Add Rule**\.

1. For **Type**, choose the DB engine that your application uses\.

1. For **Source**, choose **Custom**, and then type the group ID of the security group\. This allows resources in the security group to receive traffic on the database port from other resources in the same group\.  
![\[Edit inbound rules page for a security group in the Amazon EC2 console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/ec2-securitygroup-rds.png)

1. Choose **Save**\.

Creating a DB instance takes about 10 minutes\. In the meantime, launch your Elastic Beanstalk environment\.

## Launch an Elastic Beanstalk Environment<a name="php-hawrds-tutorial-launch"></a>

Use the AWS Management Console to launch an Elastic Beanstalk environment\. Choose the **PHP 5\.6** platform configuration and accept the default settings and sample code\. After you configure the environment to connect to the database, you deploy the sample application that you downloaded from GitHub\.

**To launch an environment \(console\)**

1. Open the Elastic Beanstalk console using this preconfigured link: [console\.aws\.amazon\.com/elasticbeanstalk/home\#/newApplication?applicationName=tutorials&environmentType=LoadBalanced](https://console.aws.amazon.com/elasticbeanstalk/home#/newApplication?applicationName=tutorials&environmentType=LoadBalanced)

1. For **Platform**, choose the platform that matches the language used by your application\.

1. For **Application code**, choose **Sample application**\.

1. Choose **Review and launch**\.

1. Review all options\. When you're satisfied with them, choose **Create app**\.

Environment creation takes about 5 minutes\.

## Configure Security Groups, Environment Properties, and Scaling<a name="php-hawrds-tutorial-configure"></a>

Next, add the security group of your DB instance to your running environment\. This procedure causes Elastic Beanstalk to reprovision all instances in your environment with the additional security group attached\.

**To add a security group to your environment**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. Choose **Configuration**\.

1. On the **Security** configuration card, choose **Modify**\.

1. For **EC2 security groups**, type a comma after the name of the autogenerated security group, followed by the name of the Amazon RDS DB instance's security group\. It's the name you noted while configuring the security group earlier\.

1. Choose **Save**, and then choose **Apply**\.

1. Read the warning, and then choose **Save**\.

Next, use environment properties to pass the connection information to your environment\. The sample application uses a default set of properties that match the ones that Elastic Beanstalk configures when you provision a database within your environment\.

**To configure environment properties for an Amazon RDS DB instance**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. Choose **Configuration**\.

1. On the **Software** configuration card, choose **Modify**\.

1. In the **Environment Properties** section, define the variables that your application reads to construct a connection string\. For compatibility with environments that have an integrated RDS DB instance, use the following:

   + **RDS\_HOSTNAME** – The hostname of the DB instance\.

     Amazon RDS console label – **Endpoint** \(this is the hostname\)

   + **RDS\_PORT** – The port on which the DB instance accepts connections\. The default value varies between DB engines\.

     Amazon RDS console label – **Port**

   + **RDS\_DB\_NAME** – The database name, `ebdb`\.

     Amazon RDS console label – **DB Name**

   + **RDS\_USERNAME** – The user name that you configured for your database\.

     Amazon RDS console label – **Username**

   + **RDS\_PASSWORD** – The password that you configured for your database\.

   Choose the plus symbol \(\+\) to add more properties\.  
![\[Environment Properties section\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environment-cfg-envprops-rds.png)

1. Choose **Save**, and then choose **Apply**\.

Finally, configure your environment's Auto Scaling group with a higher minimum instance count\. Run at least two instances at all times to prevent the web servers in your environment from being a single point of failure, and to allow you to deploy changes without taking your site out of service\.

**To configure your environment's Auto Scaling group for high availability**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. Choose **Configuration**\.

1. On the **Capacity** configuration card, choose **Modify**\.

1. In the **Auto Scaling Group** section, set **Min instances** to **2** and the **Max instances** to a value greater than **2**\.

1. Choose **Save**, and then choose **Apply**\.

## Deploy the Sample Application<a name="php-hawrds-tutorial-deploy"></a>

Now your environment is ready to run the sample application and connect to Amazon RDS\. Deploy the sample application to your environment\.

**Note**  
Download the source bundle from GitHub, if you haven't already: [eb\-demo\-php\-simple\-app\-1\.1\.zip](https://github.com/awslabs/eb-demo-php-simple-app/releases/download/v1.1/eb-demo-php-simple-app-v1.1.zip)

**To deploy a source bundle**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. Choose **Upload and Deploy**\.

1. Choose **Choose File** and use the dialog box to select the source bundle\.

1. Choose **Deploy**\.

1. When the deployment completes, choose the site URL to open your website in a new tab\.

The site collects user comments and uses a MySQL database to store the data\. To add a comment, choose **Share Your Thought**, enter a comment, and then choose **Submit Your Thought**\. The web app writes the comment to the database so that any instance in the environment can read it, and it won't be lost if instances go out of service\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/php-ha-tutorial-app.png)

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

All of these resources are managed by Elastic Beanstalk\. When you terminate your environment, Elastic Beanstalk terminates all the resources that it contains\. The RDS DB instance that you launched is outside of your environment, so you are responsible for managing its lifecycle\.

**Note**  
The Amazon S3 bucket that Elastic Beanstalk creates is shared between environments and is not deleted during environment termination\. For more information, see [Using Elastic Beanstalk with Amazon Simple Storage Service](AWSHowTo.S3.md)\.

## Clean Up<a name="w3ab1c43c19c38"></a>

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

## Next Steps<a name="php-hawrds-tutorial-nextsteps"></a>

As you continue to develop your application, you'll probably want to manage environments and deploy your application without manually creating a \.zip file and uploading it to the Elastic Beanstalk console\. The [Elastic Beanstalk Command Line Interface](eb-cli3.md) \(EB CLI\) provides easy\-to\-use commands for creating, configuring, and deploying applications to Elastic Beanstalk environments from the command line\.

The sample application uses configuration files to configure PHP settings and create a table in the database if it doesn't already exist\. You can also use a configuration file to configure the security group settings of your instances during environment creation to avoid time\-consuming configuration updates\. See [Advanced Environment Customization with Configuration Files \(`.ebextensions`\)](ebextensions.md) for more information\.

For development and testing, you might want to use the Elastic Beanstalk functionality for adding a managed DB instance directly to your environment\. For instructions on setting up a database inside your environment, see [Adding a Database to Your Elastic Beanstalk Environment](using-features.managing.db.md)\.

If you need a high\-performance database, consider using [Amazon Aurora](https://aws.amazon.com/rds/aurora/)\. Amazon Aurora is a MySQL\-compatible database engine that offers commercial database features at low cost\. To connect your application to a different database, repeat the [security group configuration](#php-hawrds-tutorial-database) steps and [update the RDS\-related environment properties](#php-hawrds-tutorial-configure)\. 

Finally, if you plan on using your application in a production environment, [configure a custom domain name](customdomains.md) for your environment and [enable HTTPS](configuring-https.md) for secure connections\.