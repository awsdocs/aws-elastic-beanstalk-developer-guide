# Step 1: Create an example application<a name="GettingStarted.CreateApp"></a>

In this step, you create a new application starting from a preexisting example application\. Elastic Beanstalk supports platforms for different programming languages, application servers, and Docker containers\. You choose a platform when you create the application\.

## Create an application and an environment<a name="GettingStarted.CreateApp.Create"></a>

To create your example application, you'll use the **Create a web app** console wizard\. It creates an Elastic Beanstalk application and launches an environment within it\. An environment is the collection of AWS resources required to run your application code\.

**To create an example application**

1. Open the Elastic Beanstalk console using this link: [https://console.aws.amazon.com/elasticbeanstalk/home#/gettingStarted?applicationName=getting-started-app](https://console.aws.amazon.com/elasticbeanstalk/home#/gettingStarted?applicationName=getting-started-app)

1. Optionally add [application tags](applications-tagging.md)\.

1. For **Platform**, choose a platform, and then choose **Create application**\.

To run the example application on AWS resources, Elastic Beanstalk takes the following actions\. They take about five minutes to complete\.

1. Creates an Elastic Beanstalk application named **getting\-started\-app**\.

1. Launches an environment named **GettingStartedApp\-env** with these AWS resources:
   + An Amazon Elastic Compute Cloud \(Amazon EC2\) instance \(virtual machine\)
   + An Amazon EC2 security group
   + An Amazon Simple Storage Service \(Amazon S3\) bucket 
   + Amazon CloudWatch alarms 
   + An AWS CloudFormation stack 
   + A domain name

   For details about these AWS resources, see [AWS resources created for the example application](#GettingStarted.CreateApp.AWSresources)\.

1. Creates a new application version named **Sample Application**\. This is the default Elastic Beanstalk example application file\.

1. Deploys the code for the example application to the **GettingStartedApp\-env** environment\.

During the environment creation process, the console tracks progress and displays events\.

![\[Elastic Beanstalk console showing the events that occur when it creates an environment\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/gettingstarted-events.png)

When all of the resources are launched and the EC2 instances running the application pass health checks, the environment's health changes to `Ok`\. You can now use your web application's website\.

## AWS resources created for the example application<a name="GettingStarted.CreateApp.AWSresources"></a>

When you create the example application, Elastic Beanstalk creates the following AWS resources:
+ **EC2 instance** – An Amazon EC2 virtual machine configured to run web apps on the platform you choose\.

  Each platform runs a different set of software, configuration files, and scripts to support a specific language version, framework, web container, or combination thereof\. Most platforms use either Apache or nginx as a reverse proxy that processes web traffic in front of your web app, forwards requests to it, serves static assets, and generates access and error logs\.
+ **Instance security group** – An Amazon EC2 security group configured to allow incoming traffic on port 80\. This resource lets HTTP traffic from the load balancer reach the EC2 instance running your web app\. By default, traffic is not allowed on other ports\.
+ **Amazon S3 bucket** – A storage location for your source code, logs, and other artifacts that are created when you use Elastic Beanstalk\.
+ **Amazon CloudWatch alarms** – Two CloudWatch alarms that monitor the load on the instances in your environment and are triggered if the load is too high or too low\. When an alarm is triggered, your Auto Scaling group scales up or down in response\.
+ **AWS CloudFormation stack** – Elastic Beanstalk uses AWS CloudFormation to launch the resources in your environment and propagate configuration changes\. The resources are defined in a template that you can view in the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation)\.
+ **Domain name** – A domain name that routes to your web app in the form **subdomain*\.*region*\.elasticbeanstalk\.com*\.