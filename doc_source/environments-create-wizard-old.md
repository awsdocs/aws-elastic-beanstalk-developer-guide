# The Old New Environment Wizard<a name="environments-create-wizard-old"></a>

**To launch a new environment**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. From the Elastic Beanstalk console applications page, choose **Actions** for the application in which you want to launch a new environment\.  
![\[Actions drop-down menu on the Applications page of the Elastic Beanstalk console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-app-page-action.png)

1. Choose **Launch New Environment**\.

1. Follow the instructions shown to launch an environment\.

See the following sections for details on each page of the wizard\.

**Topics**
+ [New Environment](#environments-create-wizard-old-newenvironment)
+ [Environment Type](#environments-create-wizard-old-environmenttype)
+ [Application Version](#environments-create-wizard-old-applicationversion)
+ [Environment Info](#environments-create-wizard-old-environmentinfo)
+ [Additional Resources](#environments-create-wizard-old-additionalresources)
+ [Configuration Details](#environments-create-wizard-old-configurationdetails)
+ [Environment Tags](#environments-create-wizard-old-environmenttags)
+ [Worker Details](#environments-create-wizard-old-workerdetails)
+ [RDS Configuration](#environments-create-wizard-old-rdsconfiguration)
+ [VPC Configuration](#environments-create-wizard-old-vpcconfiguration)
+ [Permissions](#environments-create-wizard-old-permissions)
+ [Review Information](#environments-create-wizard-old-review)

## New Environment<a name="environments-create-wizard-old-newenvironment"></a>

On the **New Environment** page, select an environment tier\. The environment tier setting specifies whether you want a **Web Server** or **Worker** environment\. For more information, see [Environment Tier](concepts.md#concepts-tier)\.

**Note**  
After you launch an environment, you cannot change the environment tier\. If your application requires a different environment tier, you must launch a new environment\.

## Environment Type<a name="environments-create-wizard-old-environmenttype"></a>

On the **Environment Type** page, select a platform and environment type, and then choose **Next**\.
+ The **Predefined configuration** setting specifies the platform and version that is used for the environment\. For more information, see [Elastic Beanstalk Supported Platforms](concepts.platforms.md)\.
**Note**  
After you launch an environment with a specific configuration, you cannot change the configuration\. If your application requires a different configuration, you must launch a new environment\.
+ The **Saved configuration** setting lists all environment configurations that you previously saved for this application, if any\. If you have no saved configurations for this application, Elastic Beanstalk does not display this option in the console\.
+ The **Environment type** specifies whether the environment is load balancing and automatically scaling or is only a single Amazon EC2 instance\. For more information, see [Environment Types](using-features-managing-env-types.md)\.

## Application Version<a name="environments-create-wizard-old-applicationversion"></a>

On the **Application Version** page, you can use the sample application, upload your own, or specify the URL for the Amazon S3 bucket that contains your application code\.

**Note**  
Depending on the platform configuration you selected, you can upload your application in a ZIP [source bundle](applications-sourcebundle.md), a WAR file, or a plaintext Docker configuration\. You can include multiple WAR files inside a ZIP file to deploy multiple Tomcat applications to each instance in your environment\. The file size limit is 512 MB\.

For load\-balancing, automatically scaling environments, choose a **Deployment policy** to configure how new application versions and changes to software configurations for instances are deployed\. **All at once** completes deployments as quickly as possible, but can result in downtime\. Rolling deployments ensure that some instances remain in service during the entire deployment process\. The **Healthy threshold** option lets you lower the minimum status at which instances can pass health checks during rolling deployments and configuration updates\. See [Deployment Policies and Settings](using-features.rolling-version-deploy.md) for more information\.

## Environment Info<a name="environments-create-wizard-old-environmentinfo"></a>

On the **Environment Information** page, enter the details of your environment and choose **Next**\.
+ Enter a name for the environment\.
+ \(Web server environments\) Enter a unique environment URL\. Although the environment URL is populated with the environment name, you can enter a different name for the URL\. Elastic Beanstalk uses this name to create a unique CNAME for the environment\. You can check the availability of the URL by choosing **Check Availability**\.
+ \(Optional\) Enter a description for this environment\.

## Additional Resources<a name="environments-create-wizard-old-additionalresources"></a>

\(Optional\) On the **Additional Resources** page, select more resources for the environment, and then choose **Next**\. Note the following:
+ If you want to add an Amazon RDS DB to the environment, select **Create an RDS Database with this environment**\. For more information about Amazon RDS, see [Amazon Relational Database Service \(Amazon RDS\)](http://aws.amazon.com/rds/)\.
+ To create your environment in a custom VPC, select **Create this environment inside a VPC**\. For more information about Amazon VPC, see [Amazon Virtual Private Cloud \(Amazon VPC\)](https://aws.amazon.com/vpc/)\.

## Configuration Details<a name="environments-create-wizard-old-configurationdetails"></a>

Set configuration details for the environment, and then choose **Next**\.

![\[Elastic Beanstalk Create New Application Wizard: Configuration Details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-env-create-config.png)
+ **Instance type** displays the instance types available to your Elastic Beanstalk environment\. Select a server with the characteristics \(including memory size and CPU power\) that are most appropriate to your application\. 
**Note**  
Elastic Beanstalk is free, but the AWS resources that it provisions might not be\. For information on Amazon EC2 usage fees, see [Amazon EC2 Pricing](https://aws.amazon.com/ec2/pricing/)\.

  For more information about the Amazon EC2 instance types that are available for your Elastic Beanstalk environment, see [Instance Families and Types](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-types.html) in the *Amazon EC2 User Guide for Linux Instances*\. 
+ Select an **EC2 key pair** to enable SSH or RDP access to the instances in your environment\. For more information about Amazon EC2 key pairs, see [Using Credentials](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-credentials.html) in the *Amazon EC2 User Guide for Linux Instances*\.
+ Specify an **Email address** to receive notifications about important events emitted by your environment\. For more information, see [Elastic Beanstalk Environment Notifications with Amazon SNS](using-features.managing.sns.md)\.
+ For load\-balancing, automatically scaling environments, **Application health check URL**, **Cross\-zone load balancing**, **Connection draining**, and **Connection draining timeout** let you configure the load balancer's behavior\. For more information, see [Load Balancer for Your AWS Elastic Beanstalk Environment](using-features.managing.elb.md)\. 
+ **Rolling updates type** provides options for managing how instances are replaced when you change settings on the Auto Scaling group or VPC\. For more information, see [Elastic Beanstalk Rolling Environment Configuration Updates](using-features.rollingupdates.md)\. 
+ **Root volume type** displays the types of storage volumes provided by Amazon EBS that you can attach to Amazon EC2 instances in your Elastic Beanstalk environment\. Select the volume type that meets your performance and price requirements\. For more information, see [Amazon EBS Volume Types](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSVolumeTypes.html) and [Amazon EBS Product Details](https://aws.amazon.com/ebs/details/)\. The size of magnetic volumes can be between 8 GiB and 1,024 GiB, and SSD volumes can be between 10 GiB and 16,384 GiB\.
+ With **Root volume size**, you can specify the size of the storage volume that you selected\. You must specify the root volume size you want if you choose **Provisioned IOPS \(SSD\)** as the root volume type that your instances will use\. For other root volumes, if you do not specify your own value, Elastic Beanstalk uses the default volume size for the storage volume type\. 
+ If you selected **Provisioned IOPS \(SSD\)** as your root volume type, you must specify the input/output operations per second \(IOPS\) that you want\. The minimum is 100 and the maximum is 4,000\. The maximum ratio of IOPS to your volume size is 30 to 1\. For example, a volume with 3,000 IOPS must be at least 100 GiB\.

## Environment Tags<a name="environments-create-wizard-old-environmenttags"></a>

\(Optional\) On the **Environment Tags** page, create tags for the environment, and then choose **Next**\. Restrictions on tag keys and tag values include the following: 
+ Keys and values can contain any alphabetic character in any language, any numeric character, white space, invisible separator, and the following symbols: \_ \. : / = \+ \\ \- @
+ Keys and values are case sensitive
+ Values cannot match the environment name
+ Values cannot include either **aws:** or **elasticbeanstalk:**

 For more information about using tags, see [Tagging Your Amazon EC2 Resources](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html) in the *Amazon EC2 User Guide for Linux Instances*\. 

![\[Environment Tags configuration section\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-env-create-tags.png)

## Worker Details<a name="environments-create-wizard-old-workerdetails"></a>

**\(worker environments\)** On the **Worker Details** page, set the following preliminary worker environment tier details\. Then, choose **Next**\.

![\[Elastic Beanstalk Create New Application Wizard: Configure Worker Details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-env-create-worker.png)
+ **Worker queue** specifies the queue from which the worker environment tier reads messages that it will process\. If you do not provide a value, then Elastic Beanstalk automatically creates one for you\.
+ **HTTP path** specifies the relative path on the local host to which messages from the queue are forwarded in the form of HTTP POST requests\.
+ **MIME type** specifies the MIME type of the message sent in the HTTP POST request\.
+ **HTTP connections** specifies the maximum number of concurrent connections to the application\. Set this to the number of process or thread messages your application can process in parallel\.
+ **Visibility timeout** specifies how long an incoming message is locked for processing before being returned to the queue\. Set this to the potentially longest amount of time that might be required to process a message\.

## RDS Configuration<a name="environments-create-wizard-old-rdsconfiguration"></a>

If you chose to associate an Amazon RDS DB earlier in the environment configuration process, on the **RDS Configuration** page, set the Amazon RDS configuration settings, and then choose **Next**\.

![\[Elastic Beanstalk Create New Application Wizard: RDS Configuration\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-env-create-rds.png)
+ \(Optional\) For **Snapshot**, select whether to create an Amazon RDS DB from an existing snapshot\.
+ \(Optional\) For **DB engine**, select a database engine\.
+ \(Optional\) For **Instance Class**, select a database instance class\. For information about the DB instance classes, see [https://aws\.amazon\.com/rds/](http://aws.amazon.com/rds/)\.
+ For **Allocated Storage**, type the space needed for your database\. You can allocate between 5 GB and 1,024 GB\. You cannot update the allocated storage for a database to a lower amount after you set it\. In some cases, allocating a larger amount of storage for your DB instance than the size of your database can improve I/O performance\. For information about storage allocation, see [Features](https://aws.amazon.com/rds/#features)\.
+ For **Master Username**, type a name using alphanumeric characters to use to log in to your DB instance with all database privileges\.
+ For **Master Password**, type a password containing 8–16 printable ASCII characters \(excluding /, \\, and @\)\.
+ For **Deletion Policy**, select **Create snapshot** to create a snapshot that you can use later to create another Amazon RDS database\. Select **Delete** to delete the DB instance when you terminate the environment\. If you select **Delete**, you lose your DB instance and all the data in it when you terminate the Elastic Beanstalk instance associated with it\. By default, Elastic Beanstalk creates and saves a snapshot\. You can use a snapshot to restore data to use in a new environment, but cannot otherwise recover lost data\.
**Note**  
You may incur charges for storing database snapshots\. For more information, see the "Backup Storage" section of [Amazon RDS Pricing](https://aws.amazon.com/rds/pricing/)\.
+ For **Availability**, select one of the following:
  + To configure your database in one Availability Zone, select **Single Availability Zone**\. A database instance launched in one Availability Zone does not have protection from the failure of a single location\.
  + To configure your database across multiple Availability Zones, select **Multiple Availability Zones**\. Running your database instance in multiple Availability Zones helps safeguard your data in the unlikely event of a database instance component failure or service health disruption in one Availability Zone\.

## VPC Configuration<a name="environments-create-wizard-old-vpcconfiguration"></a>

If you chose to create an environment inside a VPC earlier in the environment creation process, set the VPC configuration settings, and then choose **Next**\.

![\[Elastic Beanstalk Create New Application Wizard: VPC Configuration\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-env-create-vpc-apip.png)
+ Select the VPC ID of the VPC in which you want to launch your environment\.
**Note**  
If you do not see the VPC information, then you have not created a VPC in the same region in which you are launching your environment\. To learn how to create a VPC, see [Using Elastic Beanstalk with Amazon Virtual Private Cloud](vpc.md)\.
+ For a load\-balancing, automatically scaling environment, select the subnets for the Elastic Load Balancing load balancer and the Amazon EC2 instances\. If you created a single public subnet, select the **Associate Public IP Address** check box, and then select the check boxes for the load balancer and the Amazon EC2 instances\. If you created public and private subnets, be sure the load balancer \(public subnet\) and the Amazon EC2 instances \(private subnet\) are associated with the correct subnet\. By default, Amazon VPC creates a default public subnet using 10\.0\.0\.0/24 and a private subnet using 10\.0\.1\.0/24\. You can view your existing subnets in the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.
+  For a single\-instance environment, select a public subnet for the Amazon EC2 instance\. By default, Amazon VPC creates a default public subnet using 10\.0\.0\.0/24\. You can view your existing subnets in the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\. 
+ If you are using Amazon RDS, you must select at least two subnets in different Availability Zones\. To learn how to create subnets for your VPC, see [Task 1: Create the VPC and Subnets](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Scenario2.html#Case2_Create_VPC_Subnet) in the *Amazon VPC User Guide*\.
+ If your VPC configuration uses a NAT device, select the security group you created for your instances\.
+ For a load\-balancing, automatically scaling environment, select whether you want to make the load balancer external or internal\. If you do not want your load balancer to be available to the Internet, select **Internal**\.

## Permissions<a name="environments-create-wizard-old-permissions"></a>

For **Permissions** window, select an [instance profile](concepts-roles-instance.md) and [service role](concepts-roles-service.md)\. An instance profile grants the Amazon EC2 instances in your environment permissions to access AWS resources\. A service role grants Elastic Beanstalk permission to monitor the resources in your environment\. For more information, see [Service Roles, Instance Profiles, and User Policies](concepts-roles.md)\.\)

If you've created a custom instance profile and service role, select them from the drop\-down menus\. If not, choose **Next** to use the default roles\.

The Elastic Beanstalk console looks for an instance profile named `aws-elasticbeanstalk-ec2-role` and a service role named `aws-elasticbeanstalk-service-role`\. If you don't have these roles, the console creates them for you\.

## Review Information<a name="environments-create-wizard-old-review"></a>

On the **Review Information** page, review your application and environment information, and then choose **Launch**\.

Elastic Beanstalk launches your application in a new environment\. It can take several minutes for the new environment to start while Elastic Beanstalk is provisioning AWS resources\. You can view the status of your deployment on the environment's dashboard\. While Elastic Beanstalk creates your AWS resources and launches your application, the environment displays a gray state\. Status messages about launch events appear in the environment's dashboard\. When the deployment is complete, Elastic Beanstalk performs an application health check\. The environment status becomes green when the application responds to the health check\.