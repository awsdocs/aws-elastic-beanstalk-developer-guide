# The Create New Environment Wizard<a name="environments-create-wizard"></a>

In [Creating an AWS Elastic Beanstalk Environment](using-features.environments.md) we show how to open the **Create New Environment** wizard and quickly create an environment\. Choose **Create environment** to launch an environment with a default environment name, automatically generated domain, sample application code, and recommended settings\.

This topic describes the **Create New Environment** wizard and all the ways you can use it to configure the environment you want to create\.

## Main Page in the Wizard<a name="environments-create-wizard-mainpage"></a>

The **Create New Environment** wizard main page starts with naming information for the new environment\. Set the environment's name and subdomain, and create a description for your environment\. Be aware that these environment settings cannot change after the environment is created\.

![\[Main page in the Create New Environment wizard\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/wizard-environment-names.png)
+ **Name** – Enter a name for the environment\. The form provides a default name\.
+ **Domain** – \(web server environments\) Enter a unique domain name for your environment\. The form populates the domain name with the environment's name\. You can enter a different domain name\. Elastic Beanstalk uses this name to create a unique CNAME for the environment\. To check whether the domain name you want is available, choose **Check Availability**\.
+ **Description** – Enter a description for this environment\.

### Select a Platform for the New Environment<a name="environments-create-wizard-platform"></a>

You can create a new environment from two types of platforms:
+ Supported platforms
+ Custom platforms

**Supported Platform**

In most cases you will use an Elastic Beanstalk supported platform for your new environment\. When the new environment wizard starts, it selects the **Preconfigured platform** option by default, as shown in the following screenshot\.

![\[Preconfigured platform option in the Create New Environment wizard\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/wizard-defaultenvironment.png)

Scroll through the list, select the supported platform to base your environment on, and then choose **Create environment**\.

**Custom platform**

If an off\-the\-shelf platform doesn't meet your needs, you can create a new environment from a custom platform\. To specify a custom platform, choose the **Custom platform** option\. If there are no custom platforms available, this option is dimmed\.

![\[Custom platform option in the Create New Environment wizard\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/wizard-customenvironment.png)

Select one of the available custom platforms\.

![\[Selecting a custom platform in the Create New Environment wizard\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/wizard-selectcustomenvironment.png)

After selecting the platform from which the new environment is created, you can also change the platform version\. Choose **Configure more options** and then **Change platform configuration**\. When the **Change a platform version** page appears, select the version to use for your new environment, and then choose **Save**\.

![\[Changing the platform version in the Create New Environment wizard\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/wizard-selectnewversion.png)

### Provide Application Code<a name="environments-create-wizard-app-code"></a>

Now that you have selected the platform to use, the next step is to provide your application code\. You have several options:
+ You can use the sample application that Elastic Beanstalk provides for each platform\.
+ You can use code that you already deployed to Elastic Beanstalk\. Choose **Existing version** and your application in the **Application code** section\.
+ You can upload new code\. Select **Upload your code**, and then choose **Upload**\. You can upload new application code from a local file, or you can specify the URL for the Amazon S3 bucket that contains your application code\.
**Note**  
Depending on the platform configuration you selected, you can upload your application in a ZIP [source bundle](applications-sourcebundle.md), a [WAR file](java-tomcat-platform.md), or a [plaintext Docker configuration](docker-singlecontainer-deploy.md)\. The file size limit is 512 MB\.

![\[Providing application code in the Create New Environment wizard\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/wizard-environment-appcode.png)

Now choose **Create environment** to create your new environment\. Choose **Configure more options** to make additional configuration changes, as described in the following sections\.

![\[The new environment's dashboard\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/wizard-newenvironment.png)

## Configuration Presets<a name="environments-create-wizard-presets"></a>

Elastic Beanstalk provides configuration presets for low\-cost development and high\-availability use cases\. Each preset includes recommended values for several [configuration options](command-options.md)\.

![\[Configuration presets in the configuration page of the Create New Environment wizard\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/wizard-presets.png)

The **High availability** preset includes a load balancer; the **Low cost** preset does not\. Choose this option if you want a load\-balanced environment that can run multiple instances for high availability and scale in response to load\.

The third preset, **Custom configuration**, removes all recommended values except role settings and uses the API defaults\. Choose this option if you are deploying a source bundle with [configuration files](ebextensions.md) that set configuration options\. **Custom configuration** is also selected automatically if you modify either the **Low cost** or **High availability** configuration presets\.

## Customize Your Configuration<a name="environments-create-wizard-customize"></a>

In addition to \(or instead of\) choosing a configuration preset, you can fine\-tune [configuration options](command-options.md) in your environment\. When you choose **Configure more options**, the wizard shows several configuration cards\. Each configuration card displays a summary of values for a group of configuration settings\. Choose **Modify** to edit this group of settings\. The following example shows the **Capacity** configuration card\.

![\[Capacity configuration card\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-env-config-capacity.png)

**Topics**
+ [Software Settings](#environments-create-wizard-software)
+ [Instances](#environments-create-wizard-instances)
+ [Capacity](#environments-create-wizard-capacity)
+ [Load Balancer](#environments-create-wizard-loadbalancer)
+ [Rolling Updates and Deployments](#environments-create-wizard-deployment-settings)
+ [Security](#environments-create-wizard-security)
+ [Monitoring](#environments-create-wizard-monitoring)
+ [Notifications](#environments-create-wizard-notifications)
+ [Network](#environments-create-wizard-network)
+ [Database](#environments-create-wizard-database)
+ [Tags](#environments-create-wizard-tags)
+ [Worker Details](#environments-create-wizard-worker)

### Software Settings<a name="environments-create-wizard-software"></a>

Configure the instances in your environment to run the AWS X\-Ray daemon for debugging, upload, or stream logs, and set environment properties to pass information to your application\. Platform\-specific settings are also available on the **Configuration** page\. In the following example, you can see settings for the Node\.js platform\.

![\[Modify software configuration page\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/wizard-software.png)
+ **AWS X\-Ray** – Enable **X\-Ray daemon** to run [the AWS X\-Ray daemon](environment-configuration-debugging.md) for debugging\.
+ **S3 log storage** – Enable **Rotate logs** to upload rotated logs from the instances in your environment to your Elastic Beanstalk storage bucket in Amazon S3\.
+ **Instance log streaming to CloudWatch Logs** – Enable **Log streaming** to stream logs from the instances in your environment to [Amazon CloudWatch Logs](AWSHowTo.cloudwatchlogs.md)\.
+ **Environment properties** – Set [environment properties](environments-cfg-softwaresettings.md) that are passed to the application on\-instance as environment variables\.

The way that properties are passed to applications varies by platform\. In general, properties are *not* visible if you connect to an instance and run `env`\.

### Instances<a name="environments-create-wizard-instances"></a>

Configure the Amazon EC2 instances that serve requests in your environment\.

![\[Modify instances configuration page\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/wizard-instances.png)
+ **Instance type** – Select a server with the characteristics \(including memory size and CPU power\) that are most appropriate to your application\. 

  For more information about the Amazon EC2 instance types available for your Elastic Beanstalk environment, see [Instance Families and Types](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-types.html) in the *Amazon EC2 User Guide for Linux Instances*\. 
+ **AMI ID** – If you created a [custom AMI](using-features.customenv.md), specify the AMI ID to use on your instances\.
+ **Root volume** – Specify the type, size, and input/output operations per second \(IOPS\) for your root volume\.
  + **Root volume type** – From the list of storage volumes types provided by Amazon EBS, choose the type to attach to the Amazon EC2 instances in your Elastic Beanstalk environment\. Select the volume type that meets your performance and price requirements\. For more information, see [Amazon EBS Volume Types](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSVolumeTypes.html) and [Amazon EBS Product Details](https://aws.amazon.com/ebs/details/)\. 
  + **Size** – Set the size of your storage volume\. The size for magnetic volumes can be between 8 GiB and 1,024 GiB; SSD volumes can be between 10 GiB and 16,384 GiB\. If you choose **Provisioned IOPS \(SSD\)** as the root volume type for your instances, you must specify the value you want for root volume size\. For other root volumes, if you don't specify your own value, Elastic Beanstalk uses the default volume size for that storage volume type\.
  + **IOPS** – Specify the input/output operations per second that you want\. If you selected **Provisioned IOPS \(SSD\)** as your root volume type, you must specify the IOPS\. The minimum is 100 and the maximum is 4,000\. The maximum ratio of IOPS to your volume size is 30 to 1\. For example, a volume with 3,000 IOPS must be at least 100 GiB\.

### Capacity<a name="environments-create-wizard-capacity"></a>

Configure the compute capacity of your environment and **Auto Scaling Group** settings to optimize the number of instances you're using\.

![\[Auto Scaling Group section in the Modify capacity configuration page\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/wizard-capacity.png)
+ **Environment type** – Choose **Load balanced** to run the Amazon EC2 instances in your environment behind a load balancer, or **Single instance** to run one instance without a load balancer\.
**Warning**  
A single\-instance environment isn't production ready\. If the instance becomes unstable during deployment, or Elastic Beanstalk terminates and restarts the instance during a configuration update, your application can be unavailable for a period of time\. Use single\-instance environments for development, testing, or staging\. Use load\-balanced environments for production\.
+ **Availability Zones** – Restrict the number of Availability Zones to use for instances\.
+ **Instances** – Set the minimum and maximum number of instances to run\.
+ **Placement** – Choose Availability Zones that must have instances at all times\. If you assign Availability Zones here, the minimum number of instances must be at least the number of Availability Zones that you choose\.

A load\-balanced environment can run multiple instances for high availability and prevent downtime during configuration updates and deployments\. In a load\-balanced environment, the domain name maps to the load balancer\. In a single\-instance environment, it maps to an elastic IP address on the instance\.

A **scaling trigger** is an Amazon CloudWatch alarm that lets Amazon EC2 Auto Scaling know when to scale the number of instances in your environment\. By default, your environment includes two triggers: a high trigger to scale up, and a low trigger to scale down\.

![\[Scaling triggers section in the Modify capacity configuration page\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/wizard-capacity-triggers.png)
+ **Metric** – Choose the metric that the alarm monitors to identify times when you have too few or too many running instances for the amount of traffic that your application receives\.
+ **Statistic** – Choose how to interpret the metric\. Metrics can be measured as an average across all instances, the maximum or minimum value seen, or a sum value from the numbers submitted by all instances\.
+ **Unit** – Specify the unit of measurement for the values of upper and lower thresholds\.
+ **Period** – Specify the amount of time between each metric evaluation\.
+ **Breach duration** – Specify the amount of time that a metric can meet or exceed a threshold before triggering the alarm\. This value must be a multiple of the value for **Period**\. For example, with a period of 1 minute and a breach duration of 10 minutes, the threshold must be exceeded on 10 consecutive evaluations to trigger a scaling operation\.
+ **Upper threshold** – Specify the minimum value that a statistic can match to be considered in breach\.
+ **Lower threshold** – Specify the maximum value that a statistic can match to be considered in breach\.

For more information on CloudWatch metrics and alarms, see [Amazon CloudWatch Concepts](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html) in the *Amazon CloudWatch User Guide*\.

### Load Balancer<a name="environments-create-wizard-loadbalancer"></a>

In a load\-balanced environment, your environment's load balancer is the entry point for all traffic headed for your application\. Elastic Beanstalk supports several types of load balancer\. Use the **Modify load balancer** configuration page to select a load balancer type and to configure settings for it\. By default, the Elastic Beanstalk console creates a Classic Load Balancer and configures it to serve HTTP traffic on port 80\.

For more details about load balancer types and settings, see [Load Balancer for Your AWS Elastic Beanstalk Environment](using-features.managing.elb.md) and [Configuring HTTPS for your Elastic Beanstalk Environment](configuring-https.md)\.

![\[Load balancer configuration during environment creation\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-config-elb-type-chooser.png)

### Rolling Updates and Deployments<a name="environments-create-wizard-deployment-settings"></a>

 For single\-instance environments, choose a **Deployment policy** to configure how to deploy new application versions and changes to the software configuration for instances\. **All at once** completes deployments as quickly as possible, but can result in downtime\. **Immutable** deployments ensure the new instance passes health checks before switching over to the new version; otherwise, the old version remains untouched\. See [Deployment Policies and Settings](using-features.rolling-version-deploy.md) for more information\.

For load\-balanced environments, choose a **Deployment policy** to configure how to deploy new application versions and changes to the software configuration for instances\. **All at once** completes deployments as quickly as possible, but can result in downtime\. Rolling deployments ensure that some instances remain in service during the entire deployment process\. See [Deployment Policies and Settings](using-features.rolling-version-deploy.md) for more information\.

![\[Application deployments section in the Modify rolling updates and deployments configuration page\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/wizard-rolling.png)
+ **Deployment policy** – **Rolling** deployments take one batch of instances out of service at a time to deploy a new version\. **Rolling with additional batch** launches a new batch first to ensure that capacity is not affected during the deployment\. **Immutable** performs an [immutable update](environmentmgmt-updates-immutable.md) when you deploy\. 
+ **Batch size** – The number or percentage of instances to update in each batch\.

[Rolling updates](using-features.rollingupdates.md) occur when you change instance launch configuration settings or [Amazon Virtual Private Cloud](https://docs.aws.amazon.com/vpc/latest/userguide/) \(Amazon VPC\) settings, which require terminating and replacing the instances in your environment\. Other configuration changes are made in place without affecting capacity\. For more information, see [Configuration Changes](environments-updating.md)\.

![\[Configuration updates section in the Modify rolling updates and deployments configuration page\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/wizard-rolling-updates.png)
+ **Rolling update type** – Time based, where AWS CloudFormation waits for the specified amount of time after new instances are registered before moving on to the next batch, or health based, where AWS CloudFormation waits for instances to pass health checks\. **Immutable** performs an [immutable update](environmentmgmt-updates-immutable.md) when a configuration change would normally trigger a rolling update\.
+ **Batch size** – The number of instances to replace in each batch\.
+ **Minimum capacity** – The minimum number of instances to keep in service at any given time\.
+ **Pause time** – For time\-based rolling updates, the amount of time to wait for new instances to come up to speed after they are registered to the load balancer\.

The remaining options customize health checks and timeouts\.

![\[Deployment preferences section in the Modify rolling updates and deployments configuration page\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/wizard-rolling-deploys.png)
+ **Ignore health check** – Prevents a deployment from rolling back when a batch fails to become healthy within the **Command timeout**\.
+ **Healthy threshold** – Lowers the threshold at which an instance is considered healthy during rolling deployments, rolling updates, and immutable updates\.
+ **Command timeout** – The number of seconds to wait for an instance to become healthy before canceling the deployment or, if **Ignore health check** is set, to continue to the next batch\.

### Security<a name="environments-create-wizard-security"></a>

Choose an Amazon EC2 key pair to enable SSH or RDP access to the instances in your environment\. If you have created a custom service role and instance profile, select them from the lists\. If not, use the default roles, `aws-elasticbeanstalk-service-role` and `aws-elasticbeanstalk-ec2-role`\.

![\[Modify security configuration page\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/wizard-security.png)
+ **Service role** – A [service role](concepts-roles-service.md) grants Elastic Beanstalk permission to monitor the resources in your environment\.
+ **EC2 key pair** – Assign an SSH key to the instances in your environment to allow you to connect to them remotely for debugging\. For more information about Amazon EC2 key pairs, see [Using Credentials](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-credentials.html) in the *Amazon EC2 User Guide for Linux Instances*\.
**Note**  
When you create a key pair, Amazon EC2 stores a copy of your public key\. If you no longer need it to connect to any Amazon EC2 instances, you can delete it from Amazon EC2\. For details, see [Deleting Your Key Pair](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html#delete-key-pair) in the *Amazon EC2 User Guide for Linux Instances*\.
+ **IAM instance profile** – An [instance profile](concepts-roles-instance.md) grants the Amazon EC2 instances in your environment permissions to access AWS resources\. 

The Elastic Beanstalk console looks for an instance profile named `aws-elasticbeanstalk-ec2-role` and a service role named `aws-elasticbeanstalk-service-role`\. If you don't have these roles, the console creates them for you\. For more information, see [Service Roles, Instance Profiles, and User Policies](concepts-roles.md)\.

### Monitoring<a name="environments-create-wizard-monitoring"></a>

Configure health checks for your load\-balanced environment\.

![\[Modify monitoring configuration page\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/wizard-monitoring.png)
+ **Health check** – The path to send health check requests to\. If not set, the load balancer attempts to make a TCP connection on port 80 to verify health\. Set to another path to send an HTTP GET request to that path\. The path must start with `/` and is relative to the root of your application\. You can also include a protocol \(HTTP, HTTPS, TCP, or SSL\) and port before the path to check HTTPS connectivity or use a non\-default port\. For example, `HTTPS:443/health`\.
+ **Health reporting** – [Enhanced health reporting](health-enhanced.md) provides additional health information about the resources in your environment\. Select **Enhanced** to activate enhanced health reporting\. The system provides the **EnvironmentHealth** metric free of charge\. Additional charges apply if you select more metrics from the list\.
+ **Health event streaming to CloudWatch Logs** – Enable **Log streaming** to stream log events about your environment health to [Amazon CloudWatch Logs](AWSHowTo.cloudwatchlogs.envhealth.md)\.

### Notifications<a name="environments-create-wizard-notifications"></a>

Specify an email address to receive [email notifications](using-features.managing.sns.md) for important events from your environment\.

![\[Modify notification configuration page\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/wizard-notifications.png)
+ **Email** – An email address for notifications\.

### Network<a name="environments-create-wizard-network"></a>

If you have created a [custom VPC](using-features.managing.vpc.md), use these settings to configure your environment to use it\. If you don't choose a VPC, Elastic Beanstalk uses the default VPC and subnets\.

![\[Modify network configuration page\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/wizard-network.png)
+ **VPC** – The VPC in which to launch your environment's resources\. 
+ **Load balancer visibility** – Makes your load balancer internal to prevent connections from the internet\. This option is for applications that serve traffic only from networks connected to the VPC\.
+ **Load balancer subnets** – Choose public subnets for your load balancer if your site serves traffic from the internet\.

![\[Instance settings section in the Modify network configuration page\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/wizard-network-instances.png)
+ **Public IP address** – Choose this option if you run your instances and load balancer in the same public subnets\.
+ **Instance subnets** – Choose private subnets for your instances\.
+ **Instance security groups** – Choose security groups to assign to your instances, in addition to standard security groups that Elastic Beanstalk creates\.

For more information about Amazon VPC, see [Amazon Virtual Private Cloud \(Amazon VPC\)](https://aws.amazon.com/vpc/)\.

### Database<a name="environments-create-wizard-database"></a>

Add an Amazon RDS SQL database to your environment for development and testing\. Elastic Beanstalk provides connection information to your instances by setting environment properties for the database hostname, user name, password, table name, and port\.

You can restore a database snapshot you've taken before on a running environment, or you can create a new Amazon RDS database\.

When you add a database to your environment using this configuration page, its lifecycle is tied to your environment's lifecycle\. If you terminate your environment, the database is deleted and you lose your data\. For production environments, consider configuring your instances to connect to a database created outside of Elastic Beanstalk\.

![\[Modify database configuration page\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/wizard-database.png)
+ **Snapshot** – Choose an existing database snapshot\. Elastic Beanstalk restores the snapshot and adds it to your environment\. The default value is **None**, which lets you configure a new database using the other settings on this page\.
+ **Engine** – Choose the database engine used by your application\.
+ **Engine version** – Choose the version of the database engine\.
+ **Instance class** – Choose a database instance class\. For information about the DB instance classes, see [https://aws.amazon.com/rds/](https://aws.amazon.com/rds/)\.
+ **Storage** – Specify the amount of storage space, in gigabytes, to allocate for your database\. For information about storage allocation, see [Features](https://aws.amazon.com/rds/#features)\.
+ **Username** – The user name for the database administrator\. User name requirements vary per database engine\.
+ **Password** – The password for the database administrator\. Password requirements vary per database engine\.
+ **Retention** – You can use a snapshot to restore data by launching a new DB instance\. Choose **Create snapshot** to save a snapshot of the database automatically when you terminate your environment\.
+ **Availability** – Choose **High \(Multi\-AZ\)** to run a second DB instance in a different Availability Zone for high availability\.

For more information about Amazon RDS, see [Amazon Relational Database Service \(Amazon RDS\)](https://aws.amazon.com/rds/)\.

### Tags<a name="environments-create-wizard-tags"></a>

Add [tags](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html) to the resources in your environment\. For more information about environment tagging, see [Tagging Resources in Your Elastic Beanstalk Environment](using-features.tagging.md)\.

![\[Tags page in the Elastic Beanstalk console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environment-create-tags.png)

### Worker Details<a name="environments-create-wizard-worker"></a>

**\(worker environments\)** You can create an Amazon SQS queue for your worker application or pull work items from an existing queue\. The worker daemon on the instances in your environment pulls an item from the queue and relays it in the body of a POST request to a local HTTP path relative to the local host\.

![\[Worker environment details page in the Elastic Beanstalk console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/wizard-worker.png)
+ **Worker queue** – Choose the queue from which the worker environment tier reads messages that it will process\. If you don't provide a value, Elastic Beanstalk automatically creates one for you\.
+ **HTTP path** – Specify the relative path on the local host to which messages from the queue are forwarded in the form of HTTP POST requests\.
+ **MIME type** – Choose The MIME type of the message sent in the HTTP POST request\.
+ **HTTP connections** – The maximum number of concurrent connections to the application\. Set this to the number of processes or thread messages that your application can process in parallel\.
+ **Visibility timeout** – The amount of time that an incoming message is locked for processing before being returned to the queue\. Set this to the potentially longest amount of time that might be required to process a message\.