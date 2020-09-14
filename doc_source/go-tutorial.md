# Deploying a Go application to Elastic Beanstalk<a name="go-tutorial"></a>

This tutorial walks you through the process of creating a Go application and deploying it to an AWS Elastic Beanstalk environment\.

**Topics**
+ [Prerequisites](#go-tutorial-prereq)
+ [Create a Go application](#go-tutorial-create-app)
+ [Deploy your Go application with the EB CLI](#go-tutorial-deploy)
+ [Clean up](#go-tutorial-cleanup)

## Prerequisites<a name="go-tutorial-prereq"></a>

This tutorial assumes you have knowledge of the basic Elastic Beanstalk operations and the Elastic Beanstalk console\. If you haven't already, follow the instructions in [Getting started using Elastic Beanstalk](GettingStarted.md) to launch your first Elastic Beanstalk environment\.

To follow the procedures in this guide, you will need a command line terminal or shell to run commands\. Commands are shown in listings preceded by a prompt symbol \($\) and the name of the current directory, when appropriate\.

```
~/eb-project$ this is a command
this is output
```

On Linux and macOS, use your preferred shell and package manager\. On Windows 10, you can [install the Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/install-win10) to get a Windows\-integrated version of Ubuntu and Bash\.

This tutorial uses the Elastic Beanstalk Command Line Interface \(EB CLI\)\. For details on installing and configuring the EB CLI, see [Install the EB CLI](eb-cli3-install.md) and [Configure the EB CLI](eb-cli3-configuration.md)\.

## Create a Go application<a name="go-tutorial-create-app"></a>

Create a project directory\.

```
~$ mkdir eb-go
~$ cd eb-go
```

Next, create an application that you'll deploy using Elastic Beanstalk\. We'll create a "Hello World" RESTful web service\.

This example prints a customized greeting that varies based on the path used to access the service\.

Create a text file in this directory named `application.go` with the following contents\.

**Example `~/eb-go/application.go`**  

```
package main

import (
	"fmt"
	"net/http"
)

func handler(w http.ResponseWriter, r *http.Request) {
	if r.URL.Path == "/" {
		fmt.Fprintf(w, "Hello World! Append a name to the URL to say hello. For example, use %s/Mary to say hello to Mary.", r.Host)
	} else {
		fmt.Fprintf(w, "Hello, %s!", r.URL.Path[1:])
	}
}

func main() {
	http.HandleFunc("/", handler)
	http.ListenAndServe(":5000", nil)
}
```

## Deploy your Go application with the EB CLI<a name="go-tutorial-deploy"></a>

Next, you create your application environment and deploy your configured application with Elastic Beanstalk\.

**To create an environment and deploy your Go application**

1. Initialize your EB CLI repository with the eb init command\.

   ```
   ~/eb-go$ eb init -p go go-tutorial --region us-east-2
   Application go-tutorial has been created.
   ```

   This command creates an application named `go-tutorial`, and configures your local repository to create environments with the latest Go platform version\.

1. \(Optional\) Run eb init again to configure a default key pair so that you can use SSH to connect to the EC2 instance running your application\.

   ```
   ~/eb-go$ eb init
   Do you want to set up SSH for your instances?
   (y/n): y
   Select a keypair.
   1) my-keypair
   2) [ Create new KeyPair ]
   ```

   Select a key pair if you have one already, or follow the prompts to create one\. If you don't see the prompt or need to change your settings later, run eb init \-i\.

1. Create an environment and deploy your application to it with eb create\. Elastic Beanstalk automatically builds a binary file for your application and starts it on port 5000\.

   ```
   ~/eb-go$ eb create go-env
   ```

Environment creation takes about five minutes and creates the following resources:
+ **EC2 instance** – An Amazon Elastic Compute Cloud \(Amazon EC2\) virtual machine configured to run web apps on the platform that you choose\.

  Each platform runs a specific set of software, configuration files, and scripts to support a specific language version, framework, web container, or combination of these\. Most platforms use either Apache or NGINX as a reverse proxy that sits in front of your web app, forwards requests to it, serves static assets, and generates access and error logs\.
+ **Instance security group** – An Amazon EC2 security group configured to allow inbound traffic on port 80\. This resource lets HTTP traffic from the load balancer reach the EC2 instance running your web app\. By default, traffic isn't allowed on other ports\.
+ **Load balancer** – An Elastic Load Balancing load balancer configured to distribute requests to the instances running your application\. A load balancer also eliminates the need to expose your instances directly to the internet\.
+ **Load balancer security group** – An Amazon EC2 security group configured to allow inbound traffic on port 80\. This resource lets HTTP traffic from the internet reach the load balancer\. By default, traffic isn't allowed on other ports\.
+ **Auto Scaling group** – An Auto Scaling group configured to replace an instance if it is terminated or becomes unavailable\.
+ **Amazon S3 bucket** – A storage location for your source code, logs, and other artifacts that are created when you use Elastic Beanstalk\.
+ **Amazon CloudWatch alarms** – Two CloudWatch alarms that monitor the load on the instances in your environment and that are triggered if the load is too high or too low\. When an alarm is triggered, your Auto Scaling group scales up or down in response\.
+ **AWS CloudFormation stack** – Elastic Beanstalk uses AWS CloudFormation to launch the resources in your environment and propagate configuration changes\. The resources are defined in a template that you can view in the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation)\.
+ **Domain name** – A domain name that routes to your web app in the form **subdomain*\.*region*\.elasticbeanstalk\.com*\.

Elastic Beanstalk manages all of these resources\. When you terminate your environment, Elastic Beanstalk terminates all the resources that it contains\.

**Note**  
The Amazon S3 bucket that Elastic Beanstalk creates is shared between environments and is not deleted during environment termination\. For more information, see [Using Elastic Beanstalk with Amazon S3](AWSHowTo.S3.md)\.

When the environment creation process completes, open your website with eb open\.

```
~/eb-go$ eb open
```

This opens a browser window using the domain name created for your application\.

If you don't see your application running, or get an error message, see [Troubleshooting Deployments](troubleshooting-deployments.md) for help with how to determine the cause of the error\.

If you *do* see your application running, then congratulations, you've deployed a Go application with Elastic Beanstalk\!

## Clean up<a name="go-tutorial-cleanup"></a>

When you finish working with Elastic Beanstalk, you can terminate your environment\. Elastic Beanstalk terminates all AWS resources associated with your environment, such as [Amazon EC2 instances](using-features.managing.ec2.md), [database instances](using-features.managing.db.md), [load balancers](using-features.managing.elb.md), security groups, and [alarms](using-features.alarms.md#using-features.alarms.title)\. 

**To terminate your Elastic Beanstalk environment**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. Choose **Environment actions**, and then choose **Terminate environment**\.

1. Use the on\-screen dialog box to confirm environment termination\.

With Elastic Beanstalk, you can easily create a new environment for your application at any time\.

Or, with the EB CLI, do the following\.

```
~/eb-go$ eb terminate
```