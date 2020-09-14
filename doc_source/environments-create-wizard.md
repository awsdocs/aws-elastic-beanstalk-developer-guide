# The create new environment wizard<a name="environments-create-wizard"></a>

In [Creating an Elastic Beanstalk environment](using-features.environments.md) we show how to open the **Create new environment** wizard and quickly create an environment\. Choose **Create environment** to launch an environment with a default environment name, automatically generated domain, sample application code, and recommended settings\.

This topic describes the **Create new environment** wizard and all the ways you can use it to configure the environment you want to create\.

## Wizard main page<a name="environments-create-wizard-mainpage"></a>

The **Create New Environment** wizard main page starts with naming information for the new environment\. Set the environment's name and subdomain, and create a description for your environment\. Be aware that these environment settings cannot change after the environment is created\.

![\[Main page in the create new environment wizard\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/wizard-environment-names.png)
+ **Name** – Enter a name for the environment\. The form provides a generated name\.
+ **Domain** – \(web server environments\) Enter a unique domain name for your environment\. The default name is the environment's name\. You can enter a different domain name\. Elastic Beanstalk uses this name to create a unique CNAME for the environment\. To check whether the domain name you want is available, choose **Check Availability**\.
+ **Description** – Enter a description for this environment\.

### Select a platform for the new environment<a name="environments-create-wizard-platform"></a>

You can create a new environment from two types of platforms:
+ Managed platform
+ Custom platform

**Managed platform**

In most cases you use an Elastic Beanstalk managed platform for your new environment\. When the new environment wizard starts, it selects the **Managed platform** option by default, as shown in the following screenshot\.

![\[Managed platform option in the create new environment wizard\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/wizard-defaultenvironment.png)

Select a platform, a platform branch within that platform, and a specific platfotrm version in the branch\. When you select a platform branch, the recommended version within the branch is selected by default\. In addition, you can select any platform version you've used before\.

**Note**  
For a production environment, we recommend that you choose a platform version in a supported platform branch\. For details about platform branch states, see the *Platform Branch* definition in the [Elastic Beanstalk platforms glossary](platforms-glossary.md)\.

**Custom platform**

If an off\-the\-shelf platform doesn't meet your needs, you can create a new environment from a custom platform\. To specify a custom platform, choose the **Custom platform** option, and then select one of the available custom platforms\. If there are no custom platforms available, this option is dimmed\.

### Provide application code<a name="environments-create-wizard-app-code"></a>

Now that you have selected the platform to use, the next step is to provide your application code\.

![\[Providing application code in the create new environment wizard of the Elastic Beanstalk console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/wizard-environment-appcode.png)

You have several options:
+ You can use the sample application that Elastic Beanstalk provides for each platform\.
+ You can use code that you already deployed to Elastic Beanstalk\. Choose **Existing version** and your application in the **Application code** section\.
+ You can upload new code\. Choose **Upload your code**, and then choose **Upload**\. You can upload new application code from a local file, or you can specify the URL for the Amazon S3 bucket that contains your application code\.
**Note**  
Depending on the platform version you selected, you can upload your application in a ZIP [source bundle](applications-sourcebundle.md), a [WAR file](java-tomcat-platform.md), or a [plaintext Docker configuration](single-container-docker.md)\. The file size limit is 512 MB\.

  When you choose to upload new code, you can also provide tags to associate with your new code\. For more information about tagging an application version, see [Tagging application versions](applications-versions-tagging.md)\.  
![\[Uploading new application code in the create new environment wizard of the Elastic Beanstalk console\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/wizard-environment-appcode-upload.png)

For quick environment creation using default configuration options, you can now choose **Create environment**\. Choose **Configure more options** to make additional configuration changes, as described in the following sections\.

## Wizard configuration page<a name="environments-create-wizard-configure"></a>

When you choose **Configure more options**, the wizard shows the **Configure** page\. On this page you can select a configuration preset, change the platform version you want your environment to use, or make specific configuration choices for the new environment\.

### Choose a preset configuration<a name="environments-create-wizard-presets"></a>

On the **Presets** section of the page, Elastic Beanstalk provides several configuration presets for different use cases\. Each preset includes recommended values for several [configuration options](command-options.md)\.

![\[Configuration presets section in the configuration page of the create new environment wizard\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/wizard-presets.png)

The **High availability** presets include a load balancer, and are recommended for production environments\. Choose them if you want an environment that can run multiple instances for high availability and scale in response to load\. The **Single instance** presets are primarily recommended for development\. Two of the presets enable Spot Instance requests\. For details about Elastic Beanstalk capacity configuration, see [Auto Scaling group](using-features.managing.as.md)\.

The last preset, **Custom configuration**, removes all recommended values except role settings and uses the API defaults\. Choose this option if you are deploying a source bundle with [configuration files](ebextensions.md) that set configuration options\. **Custom configuration** is also selected automatically if you modify either the **Low cost** or **High availability** configuration presets\.

### Change the platform version<a name="environments-create-wizard-platform-version"></a>

On the **Platform** section of the page, you can change the platform version that your new environment will use\. You can choose the recommended version in any platform branch, or any platform version that you've used in the past\.

![\[Platforms section in the configuration page of the create new environment wizard\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/wizard-platform-version.png)

### Customize your configuration<a name="environments-create-wizard-customize"></a>

In addition to \(or instead of\) choosing a configuration preset, you can fine\-tune [configuration options](command-options.md) in your environment\. The **Configure** wizard wizard shows several configuration categories\. Each configuration category displays a summary of values for a group of configuration settings\. Choose **Edit** to edit this group of settings\.

**Topics**
+ [Software settings](#environments-create-wizard-software)
+ [Instances](#environments-create-wizard-instances)
+ [Capacity](#environments-create-wizard-capacity)
+ [Load balancer](#environments-create-wizard-loadbalancer)
+ [Rolling updates and deployments](#environments-create-wizard-deployment-settings)
+ [Security](#environments-create-wizard-security)
+ [Monitoring](#environments-create-wizard-monitoring)
+ [Managed updates](#environments-create-wizard-managed)
+ [Notifications](#environments-create-wizard-notifications)
+ [Network](#environments-create-wizard-network)
+ [Database](#environments-create-wizard-database)
+ [Tags](#environments-create-wizard-tags)
+ [Worker environment](#environments-create-wizard-worker)

#### Software settings<a name="environments-create-wizard-software"></a>

Use the **Modify software** configuration page to configure the software on the Amazon Elastic Compute Cloud \(Amazon EC2\) instances that run your application\. You can configure environment properties, AWS X\-Ray debugging, instance log storing and streaming, and platform\-specific settings\. For details, see [Environment properties and other software settings](environments-cfg-softwaresettings.md)\.

![\[Modify software configuration page\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/wizard-software.png)

#### Instances<a name="environments-create-wizard-instances"></a>

Use the **Modify instances** configuration page to configure the Amazon EC2 instances that run your application\. For details, see [Your Elastic Beanstalk environment's Amazon EC2 instances](using-features.managing.ec2.md)\.

![\[Modify instances configuration page\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/wizard-instances.png)

#### Capacity<a name="environments-create-wizard-capacity"></a>

Use the **Modify capacity** configuration page to configure the compute capacity of your environment and **Auto Scaling group** settings to optimize the number and type of instances you're using\. You can also change your environment capacity based on triggers or on a schedule\.

A load\-balanced environment can run multiple instances for high availability and prevent downtime during configuration updates and deployments\. In a load\-balanced environment, the domain name maps to the load balancer\. In a single\-instance environment, it maps to an elastic IP address on the instance\.

**Warning**  
A single\-instance environment isn't production ready\. If the instance becomes unstable during deployment, or Elastic Beanstalk terminates and restarts the instance during a configuration update, your application can be unavailable for a period of time\. Use single\-instance environments for development, testing, or staging\. Use load\-balanced environments for production\.

For more information about environment capacity settings, see [Auto Scaling group for your Elastic Beanstalk environment](using-features.managing.as.md)\.

![\[Modify capacity configuration page\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/wizard-capacity.png)

#### Load balancer<a name="environments-create-wizard-loadbalancer"></a>

Use the **Modify load balancer** configuration page to select a load balancer type and to configure settings for it\. In a load\-balanced environment, your environment's load balancer is the entry point for all traffic headed for your application\. Elastic Beanstalk supports several types of load balancer\. By default, the Elastic Beanstalk console creates an Application Load Balancer and configures it to serve HTTP traffic on port 80\.

**Note**  
You can only select your environment's load balancer type during environment creation\.

For more information about load balancer types and settings, see [Load balancer for your Elastic Beanstalk environment](using-features.managing.elb.md) and [Configuring HTTPS for your Elastic Beanstalk environment](configuring-https.md)\.

![\[Load balancer configuration during environment creation\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/wizard-elb.png)

#### Rolling updates and deployments<a name="environments-create-wizard-deployment-settings"></a>

Use the **Modify rolling updates and deployments** configuration page to configure how Elastic Beanstalk processes application deployments and configuration updates for your environment\.

Application deployments happen when you upload an updated application source bundle and deploy it to your environment\. For more information about configuring deployments, see [Deployment policies and settings](using-features.rolling-version-deploy.md)\.

![\[Application deployments section in the modify rolling updates and deployments configuration page\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environment-cfg-rollingdeployments.png)

Configuration changes that modify the [launch configuration](command-options-general.md#command-options-general-autoscalinglaunchconfiguration) or [VPC settings](command-options-general.md#command-options-general-ec2vpc) require terminating all instances in your environment and replacing them\. For more information about setting the update type and other options, see [Configuration changes](environments-updating.md)\.

![\[Configuration updates section in the modify rolling updates and deployments configuration page\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-config-rolling-updates-health.png)

#### Security<a name="environments-create-wizard-security"></a>

Use the **Modify security** configuration page to configure service and instance security settings\.

For a description of Elastic Beanstalk security concepts, see [Service roles, instance profiles, and user policies](concepts-roles.md)\. For more information about configuring environment security settings, see [Your AWS Elastic Beanstalk environment security](using-features.managing.security.md)\.

![\[Modify security configuration page\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/wizard-security.png)

#### Monitoring<a name="environments-create-wizard-monitoring"></a>

Use the **Modify monitoring** configuration page to configure health reporting, monitoring rules, and health event streaming\. For details, see [Enabling Elastic Beanstalk enhanced health reporting](health-enhanced-enable.md), [Configuring enhanced health rules for an environment](health-enhanced-rules.md), and [Streaming Elastic Beanstalk environment health information to Amazon CloudWatch Logs](AWSHowTo.cloudwatchlogs.envhealth.md)\.

![\[Modify monitoring configuration page\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/wizard-monitoring.png)

#### Managed updates<a name="environments-create-wizard-managed"></a>

Use the **Modify managed updates** configuration page to configure managed platform updates\. You can decide if you want them enabled, set the schedule, and configure other properties\. For details, see [Managed platform updates](environment-platform-update-managed.md)\.

![\[Modify managed updates configuration page\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/wizard-managed-updates.png)

#### Notifications<a name="environments-create-wizard-notifications"></a>

Use the **Modify notifications** configuration page to specify an email address to receive [email notifications](using-features.managing.sns.md) for important events from your environment\.

![\[Modify notifications configuration page\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/wizard-notifications.png)

#### Network<a name="environments-create-wizard-network"></a>

If you have created a [custom VPC](using-features.managing.vpc.md), the **Modify network** configuration page to configure your environment to use it\. If you don't choose a VPC, Elastic Beanstalk uses the default VPC and subnets\.

![\[Modify network configuration page\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/wizard-network.png)

#### Database<a name="environments-create-wizard-database"></a>

Use the **Modify database** configuration page to add an Amazon Relational Database Service \(Amazon RDS\) database to your environment for development and testing\. Elastic Beanstalk provides connection information to your instances by setting environment properties for the database hostname, user name, password, table name, and port\.

For details, see [Adding a database to your Elastic Beanstalk environment](using-features.managing.db.md)\.

![\[Modify database configuration page\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/wizard-database.png)

#### Tags<a name="environments-create-wizard-tags"></a>

Use the **Modify tags** configuration page to add [tags](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html) to the resources in your environment\. For more information about environment tagging, see [Tagging resources in your Elastic Beanstalk environments](using-features.tagging.md)\.

![\[Modify tags configuration page\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/environment-create-tags.png)

#### Worker environment<a name="environments-create-wizard-worker"></a>

If you're creating a *worker tier environment*, use the **Modify worker** configuration page to configure the worker environment\. The worker daemon on the instances in your environment pulls items from an Amazon Simple Queue Service \(Amazon SQS\) queue and relays them as post messages to your worker application\. You can choose the Amazon SQS queue that the worker daemon reads from \(auto\-generated or existing\)\. You can also configure the messages that the worker daemon sends to your application\.

For more information, see [Elastic Beanstalk worker environments](using-features-managing-env-tiers.md)\.

![\[Modify worker configuration page\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/wizard-worker.png)