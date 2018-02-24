# Getting Started Using Elastic Beanstalk<a name="GettingStarted"></a>

The following tasks help you get started with AWS Elastic Beanstalk to create, view, deploy, and update your application, and edit and terminate your environment\. You use the AWS Management Console, a point\-and\-click web\-based interface, to complete these tasks\. 


+ [Step 1: Sign up for the Service](#GettingStarted.Walkthrough.Signup)
+ [Step 2: Create an Application](#GettingStarted.Walkthrough.CreateApp)
+ [Step 3: View Information about Your Environment](#GettingStarted.Walkthrough.ViewApp)
+ [Step 4: Deploy a New Application Version](#GettingStarted.Walkthrough.DeployApp)
+ [Step 5: Change Configuration](#GettingStarted.Walkthrough.EditConfig)
+ [Step 6: Clean Up](#GettingStarted.Walkthrough.Cleanup)
+ [Where to Go Next](#GettingStarted.Next)

## Step 1: Sign up for the Service<a name="GettingStarted.Walkthrough.Signup"></a>

If you're not already an AWS customer, you need to sign up\. Signing up enables you to access Elastic Beanstalk and other AWS services that you need, such as Amazon Elastic Compute Cloud \(Amazon EC2\), Amazon Simple Storage Service \(Amazon S3\), and Amazon Simple Notification Service \(Amazon SNS\)\.

**To sign up for an AWS account**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Follow the instructions shown\.

## Step 2: Create an Application<a name="GettingStarted.Walkthrough.CreateApp"></a>

Next, you create and deploy a sample application\. For this step, you use a sample application that is already prepared\.

Elastic Beanstalk is free to use, but the AWS resources that it provides are live \(and not running in a sandbox\)\. You incur the standard usage fees for these resources until you terminate them in the last task in this tutorial\. The total charges are minimal \(typically less than a dollar\)\. For information about how you might minimize any charges, see [AWS Free Tier](https://aws.amazon.com/free)\.

**To create a sample application**

1. Open the Elastic Beanstalk console with this preconfigured link: [https://console.aws.amazon.com/elasticbeanstalk/home#/gettingStarted?applicationName=getting-started-app](https://console.aws.amazon.com/elasticbeanstalk/home#/gettingStarted?applicationName=getting-started-app)

1. Choose a platform, and then choose **Create application**\.  
![\[New environment wizard\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/gettingstarted-wizard.png)

To run a sample application on AWS resources, Elastic Beanstalk takes the following actions\. These take about five minutes to complete:

+ Creates an Elastic Beanstalk application named **getting\-started\-app**\.

+ Launches an environment named **GettingStartedApp\-env** with the following AWS resources:

  + **EC2 instance** – An Amazon Elastic Compute Cloud \(Amazon EC2\) virtual machine configured to run web apps on the platform you choose\.

    Each platform runs a different set of software, configuration files, and scripts to support a specific language version, framework, web container, or combination thereof\. Most platforms use either Apache or nginx as a reverse proxy that sits in front of your web app, forwards requests to it, serves static assets, and generates access and error logs\.

  + **Instance security group** – An Amazon EC2 security group configured to allow ingress on port 80\. This resource lets HTTP traffic from the load balancer reach the EC2 instance running your web app\. By default, traffic is not allowed on other ports\.

  + **Amazon S3 bucket** – A storage location for your source code, logs, and other artifacts that are created when you use Elastic Beanstalk\.

  + **Amazon CloudWatch alarms** – Two CloudWatch alarms that monitor the load on the instances in your environment and are triggered if the load is too high or too low\. When an alarm is triggered, your Auto Scaling group scales up or down in response\.

  + **AWS CloudFormation stack** – Elastic Beanstalk uses AWS CloudFormation to launch the resources in your environment and propagate configuration changes\. The resources are defined in a template that you can view in the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation)\.

  + **Domain name** – A domain name that routes to your web app in the form **subdomain*\.*region*\.elasticbeanstalk\.com*\.

+ Creates a new application version named **Sample Application**, which refers to the default Elastic Beanstalk sample application file\.

+ Deploys the sample application code to **GettingStartedApp\-env**\.

During the environment creation process, the console tracks its progress and displays events, as shown\.

![\[Console showing environment creation event output.\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/gettingstarted-events.png)

When all of the resources finish launching and the EC2 instances running the application pass health checks, the environment's health changes to `Ok` and the website becomes ready to use\.

## Step 3: View Information about Your Environment<a name="GettingStarted.Walkthrough.ViewApp"></a>

After you create the Elastic Beanstalk application, you can view information about the application you deployed and its provisioned resources by going to the environment dashboard in the AWS Management Console\. The dashboard shows the health of your application's environment, the running version, and the environment configuration\.

While Elastic Beanstalk creates your AWS resources and launches your application, the environment is in a `Pending` state\. Status messages about launch events are displayed in the environment's dashboard\.

If you are not currently viewing the dashboard, return to it now\.

**To view the dashboard**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Choose **GettingStartedApp\-env**\.  
![\[Elastic beanstalk console showing list of applications and environments.\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/gettingstarted-chooseenvironment.png)

The dashboard shows a subset of useful information about your environment\. This includes its current health status, the name of the currently deployed application version, its five most recent events, and the platform configuration on which the application runs\.

![\[Environment dashboard.\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/gettingstarted-dashboard.png)

On the left side of the console is a navigation pane that links to other pages, which contain more detailed information about your environment and provide access to additional features\. Explore the following pages to see the current state of your environment:

+ The **Configuration** page shows the resources provisioned for this environment, such as Amazon EC2 instances that host your application\. This page also lets you configure some of the provisioned resources\.

+ The **Health** page shows the status and detailed health information about the EC2 instances running your application\.

+ The **Monitoring** page shows the statistics for the environment, such as average latency and CPU utilization\. You also use this page to create alarms for the metrics that you are monitoring\.

+ The **Events** page shows any informational or error messages from services that this environment is using\.

+ The **Tags** page shows tags — key\-value pairs that are applied to resources in the environment\. You use this page to manage your environment's tags\.

## Step 4: Deploy a New Application Version<a name="GettingStarted.Walkthrough.DeployApp"></a>

You can deploy a new version of your application at any time, as long as no other update operations are currently in progress on your environment\.

The application version you are running now is labeled **Sample Application**\.

**To update your application version**

1. Download one of the following sample applications that match the configuration for your environment:

   + **Single Container Docker** – [docker\-singlecontainer\-v1\.zip](samples/docker-singlecontainer-v1.zip)

   + **Multicontainer Docker** – [docker\-multicontainer\-v2\.zip](samples/docker-multicontainer-v2.zip)

   + **Preconfigured Docker \(Glassfish\)** – [docker\-glassfish\-v1\.zip](samples/docker-glassfish-v1.zip)

   + **Preconfigured Docker \(Python 3\)** – [docker\-python\-v1\.zip](samples/docker-python-v1.zip)

   + **Preconfigured Docker \(Go\)** – [docker\-golang\-v1\.zip](samples/docker-golang-v1.zip)

   + **Go** – [go\-v1\.zip](samples/go-v1.zip)

   + **Java SE** – [java\-se\-jetty\-gradle\-v3\.zip](samples/java-se-jetty-gradle-v3.zip)

   + **Tomcat** – [java\-tomcat\-v3\.zip](samples/java-tomcat-v3.zip)

   + **\.NET** – [dotnet\-asp\-v1\.zip](samples/dotnet-asp-v1.zip)

   + **Node\.js** – [nodejs\-v1\.zip](samples/nodejs-v1.zip) 

   + **PHP** – [php\-v1\.zip](samples/php-v1.zip)

   + **Python** – [python\-v1\.zip](samples/python-v1.zip)

   + **Ruby \(Passenger Standalone\)** – [ruby\-passenger\-v2\.zip](samples/ruby-passenger-v2.zip)

   + **Ruby \(Puma\)** – [ruby\-puma\-v2\.zip](samples/ruby-puma-v2.zip)

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. From the Elastic Beanstalk applications page, choose **getting\-started\-app**, and then choose **GettingStartedApp\-env**\.

1. In the **Overview** section, choose **Upload and Deploy**\.

1. Select **Choose File** and upload the sample source bundle that you downloaded\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-app-version-upload.png)

1. The console automatically fills in the **Version label** based on the name of the archive that you uploaded\. For future deployments, you must type a unique version label if you use a source bundle with the same name\.

1. Choose **Deploy**\.

 Elastic Beanstalk now deploys your file to your Amazon EC2 instances\. You can view the status of your deployment on the environment's dashboard\. The **Environment Health** status turns gray while the application version is updated\. When the deployment is complete, Elastic Beanstalk performs an application health check\. The status returns to green when the application responds to the health check\. The environment dashboard will show the new **Running Version** as **Sample Application Second Version** \(or whatever you provided as the **Version label**\)\.

Your new application version is also uploaded and added to the table of application versions\. To view the table, choose **My First Elastic Beanstalk Application**, and then choose **Application Versions**\.

## Step 5: Change Configuration<a name="GettingStarted.Walkthrough.EditConfig"></a>

You can customize your environment to better suit your application\. For example, if you have a compute\-intensive application, you can change the type of Amazon EC2 instance that is running your application\.

Some configuration changes are simple and happen quickly\. Some changes require Elastic Beanstalk to delete and recreate AWS resources, which can take several minutes\. Elastic Beanstalk will warn you about possible application downtime when changing configuration settings\. 

In this task, you edit your environment's capacity settings\. You configure a load\-balanced, automatically scaling environment that has between two and four instances in its Auto Scaling group, and then verify that the change occurred\. Two Amazon EC2 instances get created and are associated with the environment's load balancer\. These instances replace the single instance that Elastic Beanstalk created initially\.

**To change your environment configuration**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. Choose **Configuration**\.

1. On the **Capacity** configuration card, choose **Modify**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-env-config-capacity.png)

1. In the **Auto Scaling Group** section, change **Environment type** to **Load balanced**\.

1. At the **Instances** row, change **Max** to **4**, and then change **Min** to **2**\.

1. At the bottom of the **Modify capacity** page, choose **Save**\.

1. At the bottom of the **Configuration overview** page, choose **Apply**\.

1. A warning appears\. It tells you that the migration replaces all your current instances\. Choose **Confirm**\.

The environment update might take a few minutes\. When the environment is ready, you can go to the next task to verify your changes\.

**To verify changes to load balancers**

1. In the navigation pane, choose **Events**\.

   You will see the event **Successfully deployed new configuration to environment** in the events list\. This confirms that the Auto Scaling minimum instance count has been set to 2\. A second instance is launched automatically\. 

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, under **LOAD BALANCING**, choose **Load Balancers**\. 

1. Repeat the next two steps until you identify the load balancer with the instance name you want\.

1. Choose a load balancer in the list of load balancers\.

1. Choose the **Instances** tab in the **Load balancer:** *<load balancer name>* pane, and then look at the **Name** in the **Instances** table\.   
![\[Load balancer pane\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-lb-as.png)

   The information shows that two instances are associated with this load balancer, corresponding to the increase in EC2 instances\.

## Step 6: Clean Up<a name="GettingStarted.Walkthrough.Cleanup"></a>

Congratulations\! You have successfully deployed a sample application to the cloud, uploaded a new version, and modified its configuration to add a second Auto Scaling instance\. To ensure that you're not charged for any services you don't need, delete any unwanted applications and environments from Elastic Beanstalk and AWS services\. 

**To completely delete the application**

1. Delete all application versions\.

   1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

   1. From the Elastic Beanstalk applications page, choose the **getting\-started\-app** application\.

   1. On the navigation pane, choose **Application versions**\.

   1. On the **Application Versions** page, select all application versions that you want to delete, and then choose **Delete**\.

   1. Confirm the versions that you are deleting, and then choose **Delete**\.

   1. Choose **Done**\.

1. Terminate the environment\.

   1. To go back to the environment dashboard, click **getting\-started\-app**, and then click **GettingStartedApp\-env**\.

   1. Choose **Actions**, and then choose **Terminate Environment**\.

   1. Confirm that you are terminating **GettingStartedApp\-env**, and then choose **Terminate**\.

1. Delete the **getting\-started\-app** Elastic Beanstalk application\.

   1. Choose **Elastic Beanstalk** at the upper left to return to the main dashboard\.

   1. From the Elastic Beanstalk applications page, choose **Actions** for the **getting\-started\-app** application, and then choose **Delete application**\.

   1. Confirm that you want to delete this Elastic Beanstalk application by choosing **Delete**\.

## Where to Go Next<a name="GettingStarted.Next"></a>

Now that you have learned about Elastic Beanstalk and how to access it, we recommend that you read [AWS Elastic Beanstalk Concepts](concepts.md)\. This topic provides information about the Elastic Beanstalk components, architecture, and important design considerations for your Elastic Beanstalk application\. 

In addition to the AWS Management Console, you can also use the following tools to create and manage Elastic Beanstalk environments\.


+ [The EB CLI](#GettingStarted.UsingAEB.cli)
+ [AWS SDK for Java](#GettingStarted.UsingAEB.JavaSDK)
+ [AWS Toolkit for Eclipse](#GettingStarted.UsingAEB.eclipse)
+ [AWS SDK for \.NET](#GettingStarted.UsingAEB.NETSDK)
+ [AWS Toolkit for Visual Studio](#GettingStarted.UsingAEB.vs)
+ [AWS SDK for JavaScript in Node\.js](#GettingStarted.UsingAEB.NodejsSDK)
+ [AWS SDK for PHP](#GettingStarted.UsingAEB.PHPSDK)
+ [AWS SDK for Python \(Boto\)](#GettingStarted.UsingAEB.PythonSDK)
+ [AWS SDK for Ruby](#GettingStarted.UsingAEB.RubySDK)

### The EB CLI<a name="GettingStarted.UsingAEB.cli"></a>

The EB CLI is a command line tool for creating and managing environments\. See [The Elastic Beanstalk Command Line Interface \(EB CLI\)](eb-cli3.md) for details\.

### AWS SDK for Java<a name="GettingStarted.UsingAEB.JavaSDK"></a>

 The AWS SDK for Java provides a Java API you can use to build applications that use AWS infrastructure services\. With the AWS SDK for Java, you can get started in minutes with a single, downloadable package that includes the AWS Java library, code samples, and documentation\. 

 The AWS SDK for Java requires the J2SE Development Kit 5\.0 or later\. You can download the latest Java software from [http://developers.sun.com/downloads/](http://developers.sun.com/downloads/)\. The SDK also requires Apache Commons \(Codec, HTTPClient, and Logging\) and Saxon\-HE third\-party packages, which are included in the third\-party directory of the SDK\. 

 For more information, see [AWS SDK for Java](https://aws.amazon.com/sdk-for-java/)\.

### AWS Toolkit for Eclipse<a name="GettingStarted.UsingAEB.eclipse"></a>

With the AWS Toolkit for Eclipse plug\-in, you can create new AWS Java web projects that are preconfigured with the AWS SDK for Java, and then deploy the web applications to Elastic Beanstalk\. The Elastic Beanstalk plug\-in builds on top of the Eclipse Web Tools Platform \(WTP\)\. The toolkit provides a Travel Log sample web application template that demonstrates the use of Amazon S3 and Amazon SNS\. 

To ensure that you have all the WTP dependencies, we recommend that you start with the Java EE distribution of Eclipse, which you can download from [http://eclipse\.org/downloads/](http://eclipse.org/downloads/)\. 

For more information about using the Elastic Beanstalk plug\-in for Eclipse, see [AWS Toolkit for Eclipse](https://aws.amazon.com/eclipse/)\. To get started creating your Elastic Beanstalk application using Eclipse, see [Creating and Deploying Java Applications on AWS Elastic Beanstalk](create_deploy_Java.md)\.

### AWS SDK for \.NET<a name="GettingStarted.UsingAEB.NETSDK"></a>

The AWS SDK for \.NET enables you to build applications that use AWS infrastructure services\. With the AWS SDK for \.NET, you can get started in minutes with a single, downloadable package that includes the AWS \.NET library, code samples, and documentation\. 

For more information, see [AWS SDK for \.NET](https://aws.amazon.com/sdk-for-net/)\. For supported \.NET Framework and Visual studio versions, see [AWS SDK for \.NET Developer Guide](http://docs.aws.amazon.com/sdk-for-net/latest/developer-guide/)\.

### AWS Toolkit for Visual Studio<a name="GettingStarted.UsingAEB.vs"></a>

With the AWS Toolkit for Visual Studio plug\-in, you can deploy an existing \.NET application to Elastic Beanstalk\. You can also create new projects using the AWS templates that are preconfigured with the AWS SDK for \.NET\. For prerequisite and installation information, see [AWS Toolkit for Visual Studio](https://aws.amazon.com/visualstudio/)\. To get started creating your Elastic Beanstalk application using Visual Studio, see [Creating and Deploying Elastic Beanstalk Applications in \.NET Using AWS Toolkit for Visual Studio](create_deploy_NET.md)\.

### AWS SDK for JavaScript in Node\.js<a name="GettingStarted.UsingAEB.NodejsSDK"></a>

The AWS SDK for JavaScript in Node\.js enables you to build applications on top of AWS infrastructure services\. With the AWS SDK for JavaScript in Node\.js, you can get started in minutes with a single, downloadable package that includes the AWS Node\.js library, code samples, and documentation\. 

For more information, see [AWS SDK for JavaScript in Node\.js](https://aws.amazon.com/sdk-for-node-js/)\. 

### AWS SDK for PHP<a name="GettingStarted.UsingAEB.PHPSDK"></a>

The AWS SDK for PHP enables you to build applications on top of AWS infrastructure services\. With the AWS SDK for PHP, you can get started in minutes with a single, downloadable package that includes the AWS PHP library, code samples, and documentation\. 

The AWS SDK for PHP requires [PHP 5\.2 or later](http://www.php.net/)\. 

For more information, see [AWS SDK for PHP](https://aws.amazon.com/sdk-for-php/)\. 

### AWS SDK for Python \(Boto\)<a name="GettingStarted.UsingAEB.PythonSDK"></a>

With the AWS SDK for Python \(Boto\), you can get started in minutes with a single, downloadable package complete with the AWS Python library, code samples, and documentation\. You can build Python applications on top of APIs that take the complexity out of coding directly against web service interfaces\. The all\-in\-one library provides Python developer\-friendly APIs that hide many of the lower\-level tasks associated with programming for the AWS Cloud, including authentication, request retries, and error handling\. The SDK provides practical examples in Python for how to use the libraries to build applications\. For information about Boto, sample code, documentation, tools, and additional resources, see [Python Developer Center](https://aws.amazon.com/python/)\. 

### AWS SDK for Ruby<a name="GettingStarted.UsingAEB.RubySDK"></a>

You can get started in minutes with a single, downloadable package complete with the AWS Ruby library, code samples, and documentation\. You can build Ruby applications on top of APIs that take the complexity out of coding directly against web services interfaces\. The all\-in\-one library provides Ruby developer\-friendly APIs that hide many of the lower\-level tasks associated with programming for the AWS Cloud, including authentication, request retries, and error handling\. The SDK provides practical examples in Ruby for how to use the libraries to build applications\. For information about the SDK, sample code, documentation, tools, and additional resources, see [Ruby Developer Center](https://aws.amazon.com/ruby/)\. 