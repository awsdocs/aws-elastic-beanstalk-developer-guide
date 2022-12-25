# Configuring Elastic Beanstalk environments<a name="customize-containers"></a>

AWS Elastic Beanstalk provides a wide range of options for customizing the resources in your environment, and Elastic Beanstalk behavior and platform settings\. When you create a web server environment, Elastic Beanstalk creates several resources to support the operation of your application\.
+ **EC2 instance** – An Amazon Elastic Compute Cloud \(Amazon EC2\) virtual machine configured to run web apps on the platform that you choose\.

  Each platform runs a specific set of software, configuration files, and scripts to support a specific language version, framework, web container, or combination of these\. Most platforms use either Apache or NGINX as a reverse proxy that sits in front of your web app, forwards requests to it, serves static assets, and generates access and error logs\.
**Important**  
The *Let's Encrypt* cross\-signed DST Root CA X3 certificate *expired* on *September 30, 2021*\. Due to this, Beanstalk environments running on the Amazon Linux 2 and Amazon Linux AMI operating systems might not be able to connect to servers using *Let's Encrypt* certificates\.  
On October 3, 2021 Elastic Beanstalk released new platform versions for Amazon Linux AMI and Amazon Linux 2 with the updated CA certificates\. To receive these updates and address this issue turn on [Managed Updates](environment-platform-update-managed.md) or [update your platforms manually](using-features.platform.upgrade.md#using-features.platform.upgrade.config)\. For more information, see the [platform update release notes](https://docs.aws.amazon.com/elasticbeanstalk/latest/relnotes/release-2021-10-03-linux.html) in the *AWS Elastic Beanstalk Release Notes*\.  
You can also apply the manual workarounds described in this [AWS Knowledge Center article](https://aws.amazon.com/premiumsupport/knowledge-center/ec2-expired-certificate/)\. Since Elastic Beanstalk provides AMIs with locked GUIDs, we recommend that you use the sudo yum install command in the article\. Alternatively, you can also use the sudo sed command in the article if you prefer to manually modify the system in place\.
+ **Instance security group** – An Amazon EC2 security group configured to allow inbound traffic on port 80\. This resource lets HTTP traffic from the load balancer reach the EC2 instance running your web app\. By default, traffic isn't allowed on other ports\.
+ **Load balancer** – An Elastic Load Balancing load balancer configured to distribute requests to the instances running your application\. A load balancer also eliminates the need to expose your instances directly to the internet\.
+ **Load balancer security group** – An Amazon EC2 security group configured to allow inbound traffic on port 80\. This resource lets HTTP traffic from the internet reach the load balancer\. By default, traffic isn't allowed on other ports\.
+ **Auto Scaling group** – An Auto Scaling group configured to replace an instance if it is terminated or becomes unavailable\.
+ **Amazon S3 bucket** – A storage location for your source code, logs, and other artifacts that are created when you use Elastic Beanstalk\.
+ **Amazon CloudWatch alarms** – Two CloudWatch alarms that monitor the load on the instances in your environment and that are triggered if the load is too high or too low\. When an alarm is triggered, your Auto Scaling group scales up or down in response\.
+ **AWS CloudFormation stack** – Elastic Beanstalk uses AWS CloudFormation to launch the resources in your environment and propagate configuration changes\. The resources are defined in a template that you can view in the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation)\.
+ **Domain name** – A domain name that routes to your web app in the form **subdomain*\.*region*\.elasticbeanstalk\.com*\.

This topic focuses on the resource configuration options available in the Elastic Beanstalk console\. The following topics show how to configure your environment in the console\. They also describe the underlying namespaces that correspond to the console options for use with configuration files or API configuration options\. To learn about advanced configuration methods, see [Configuring Elastic Beanstalk environments \(advanced\)](beanstalk-environment-configuration-advanced.md)\.

**Topics**
+ [Environment configuration using the Elastic Beanstalk console](environments-cfg-console.md)
+ [The Amazon EC2 instances for your Elastic Beanstalk environment](using-features.managing.ec2.md)
+ [Auto Scaling group for your Elastic Beanstalk environment](using-features.managing.as.md)
+ [Load balancer for your Elastic Beanstalk environment](using-features.managing.elb.md)
+ [Adding a database to your Elastic Beanstalk environment](using-features.managing.db.md)
+ [Your AWS Elastic Beanstalk environment security](using-features.managing.security.md)
+ [Tagging resources in your Elastic Beanstalk environments](using-features.tagging.md)
+ [Environment properties and other software settings](environments-cfg-softwaresettings.md)
+ [Elastic Beanstalk environment notifications with Amazon SNS](using-features.managing.sns.md)
+ [Configuring Amazon Virtual Private Cloud \(Amazon VPC\) with Elastic Beanstalk](using-features.managing.vpc.md)
+ [Your Elastic Beanstalk environment's Domain name](customdomains.md)