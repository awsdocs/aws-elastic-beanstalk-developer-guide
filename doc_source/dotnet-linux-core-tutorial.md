# Tutorial: Deploying an ASP\.NET core application with Elastic Beanstalk on Linux<a name="dotnet-linux-core-tutorial"></a>

This tutorial describes the process of building a new ASP\.NET Core application and deploying it to an Amazon Linux 2 environment with Elastic Beanstalk\.

In this tutorial, you first use the \.NET Core SDK's `dotnet` command line tool to do the following: 
+ Generate an application that serves HTTP requests with ASP\.NET\.
+ Install runtime dependencies\.
+ Compile and run your web application locally\.
+ Publish your application artifacts to an output directory\. The artifacts include the compiled source code, runtime dependencies and configuration files\.

Next, you do the following with your newly created application:
+ Create an application source bundle that contains your published artifacts\.
+ Create an Amazon Linux 2 environment and deploy your application to it with Elastic Beanstalk\.
+ Open the site URL created by Elastic Beanstalk to run your application\.

The application source code is available here: [dotnet\-core\-linux\-tutorial\-source\.zip](samples/dotnet-core-linux-tutorial-source.zip)\.

The deployable source bundle is available here: [dotnet\-core\-linux\-tutorial\-bundle](samples/dotnet-core-linux-tutorial-bundle.zip)\.

**Topics**
+ [Prerequisites](#dotnet-linux-core-tutorial-prereqs)
+ [Generate a \.NET core project as a web application](#dotnet-linux-core-tutorial-generate)
+ [Launch an Elastic Beanstalk environment and deploy your application](#dotnet-linux-core-tutorial-deploy)
+ [Cleanup](#dotnet-linux-core-tutorial-cleanup)
+ [Next steps](#dotnet-linux-core-tutorial-nextsteps)

## Prerequisites<a name="dotnet-linux-core-tutorial-prereqs"></a>

This tutorial uses the \.NET Core SDK to generate a basic \.NET Core web application, run it locally, and build a deployable package\.

**Requirements**
+ \.NET Core \(x64\) 2\.1\.19 or later 

**To install the \.NET core SDK**

1. Download the installer from [microsoft\.com/net/core](https://www.microsoft.com/net/core#windows)\. Choose your development platform\. Choose **Download \.NET Core SDK**\.

1. Run the installer and follow the instructions\.

**Note**  
Although the examples in this tutorial are listings from the **Windows** command line, the \.NET Core SDK supports development platforms on several operating systems\. The `dotnet` commands shown in this tutorial are consistent across different development platforms\.

This tutorial uses a command line ZIP utility to create a source bundle that you can deploy to Elastic Beanstalk\. To use the `zip` command in Windows, you can install `UnxUtils`\. \(UnxUtils is a lightweight collection of useful command line utilities like `zip` and `ls`\.\) Alternatively, you can [use Windows Explorer](applications-sourcebundle.md#using-features.deployment.source.gui) or any other ZIP utility to create source bundle archives\.

**To install UnxUtils**

1. Download `[UnxUtils](https://sourceforge.net/projects/unxutils/)`\.

1. Extract the archive to a local directory\. For example, `C:\Program Files (x86)`\.

1. Add the path to the binaries to your Windows PATH user variable\. For example, `C:\Program Files (x86)\UnxUtils\usr\local\wbin`\.

   1. Press the Windows key, and then enter **environment variables**\.

   1. Choose **Edit environment variables for your account**\.

   1. Choose **PATH**, and then choose **Edit**\.

   1. Add paths to the **Variable value** field, separated by semicolons\. For example: `C:\item1\path;C:\item2\path`

   1. Choose **OK** twice to apply the new settings\.

   1. Close any running Command Prompt windows, and then reopen a Command Prompt window\.

1. Open a new command prompt window and run the `zip` command to verify that it works\.

   ```
   > zip -h
   Copyright (C) 1990-1999 Info-ZIP
   Type 'zip "-L"' for software license.
   ...
   ```

## Generate a \.NET core project as a web application<a name="dotnet-linux-core-tutorial-generate"></a>

Use the `dotnet` command line tool to generate a new C\# \.NET Core web application project and run it locally\. The default \.NET Core web application displays `Hello World!`\.

**To generate a new \.NET core project**

1. Open a new command prompt window and navigate to your user folder\.

   ```
   > cd %USERPROFILE%
   ```

1. Use the `dotnet new` command to generate a new \.NET Core project\.

   ```
   C:\Users\username> dotnet new web -o dotnet-core-tutorial
   The template "ASP.NET Core Empty" was created successfully.
   
   Processing post-creation actions...
   Running 'dotnet restore' on dotnet-core-tutorial\dotnet-core-tutorial.csproj...
     Determining projects to restore...
     Restored C:\Users\username\dotnet-core-tutorial\dotnet-core-tutorial.csproj (in 154 ms).
   
   Restore succeeded.
   ```

**To run the website locally**

1. Use the `dotnet restore` command to install dependencies\.

   ```
   C:\Users\username> cd dotnet-core-tutorial
   C:\Users\username\dotnet-core-tutorial> dotnet restore
     Determining projects to restore...
     All projects are up-to-date for restore.
   ```

1. Use the `dotnet run` command to build and start the application locally\.

   ```
   C:\Users\username\dotnet-core-tutorial> dotnet run
   info: Microsoft.Hosting.Lifetime[0]
         Now listening on: https://localhost:5001
   info: Microsoft.Hosting.Lifetime[0]
         Now listening on: http://localhost:5000
   info: Microsoft.Hosting.Lifetime[0]
         Application started. Press Ctrl+C to shut down.
   info: Microsoft.Hosting.Lifetime[0]
         Hosting environment: Development
   info: Microsoft.Hosting.Lifetime[0]
         Content root path: C:\Users\username\dotnet-core-tutorial
   ```

1. Open [localhost:5000](http://localhost:5000) to view the site from your default web browser\.

   The application returns `Hello World!`, which is displayed on your web browser\.

To run the application on a web server, you must bundle the compiled source code with a `web.config` configuration file and runtime dependencies\. The `dotnet` tool provides a `publish` command that gathers these files in a directory based on the configuration in `dotnet-core-tutorial.csproj`\.

**To build your website**
+ Use the `dotnet publish` command to output compiled code and dependencies to a folder named `site`\.

  ```
  C:\users\username\dotnet-core-tutorial> dotnet publish -o site
  Microsoft (R) Build Engine version 16.7.0-preview-20360-03+188921e2f for .NET
  Copyright (C) Microsoft Corporation. All rights reserved.
  
    Determining projects to restore...
    All projects are up-to-date for restore.
    dotnet-core-tutorial -> C:\Users\username\dotnet-core-tutorial\bin\Debug\netcoreapp3.1\dotnet-core-tutorial.dll
    dotnet-core-tutorial -> C:\Users\username\dotnet-core-tutorial\site\
  ```

**To create a source bundle**
+ Use the `zip` command to create a source bundle named `dotnet-core-tutorial.zip`\.

  The source bundle contains all of the files published to the site folder\.
**Note**  
If you use a different ZIP utility, be sure to add all files to the root folder of the resulting ZIP archive\. This is required for a successful deployment of the application to your Elastic Beanstalk environment\.

  ```
  C:\users\username\dotnet-core-tutorial> cd site
  C:\users\username\dotnet-core-tutorial\site>zip ../dotnet-core-tutorial.zip *
    adding: appsettings.Development.json (164 bytes security) (deflated 38%)
    adding: appsettings.json (164 bytes security) (deflated 39%)
    adding: dotnet-core-tutorial.deps.json (164 bytes security) (deflated 93%)
    adding: dotnet-core-tutorial.dll (164 bytes security) (deflated 58%)
    adding: dotnet-core-tutorial.exe (164 bytes security) (deflated 57%)
    adding: dotnet-core-tutorial.pdb (164 bytes security) (deflated 48%)
    adding: dotnet-core-tutorial.runtimeconfig.json (164 bytes security) (deflated 33%)
    adding: web.config (164 bytes security) (deflated 41%)
  ```
**Note**  
In this tutorial, you are only running one application on the web server, so a Procfile is not required in your source bundle\. However, to deploy multiple applications on the same web server, you must include a Procfile\. For more information, see [Using a Procfile to configure your \.NET Core on Linux environment](dotnet-linux-procfile.md)\.

## Launch an Elastic Beanstalk environment and deploy your application<a name="dotnet-linux-core-tutorial-deploy"></a>

Use the Elastic Beanstalk console to launch an Elastic Beanstalk environment and deploy the source bundle\.

You can download the source bundle here: [dotnet\-core\-linux\-tutorial\-bundle](samples/dotnet-core-linux-tutorial-bundle.zip)

**To launch an environment and deploy your code \(console\)**

1. Open the Elastic Beanstalk console with this preconfigured link: [console\.aws\.amazon\.com/elasticbeanstalk/home\#/newApplication?applicationName=tutorials&environmentType=LoadBalanced](https://console.aws.amazon.com/elasticbeanstalk/home#/newApplication?applicationName=tutorials&environmentType=LoadBalanced)

1. For **Platform**, select *\.NET Core on Linux*\.

1. Choose **Local file**, choose **Choose file**, and then open the source bundle\.

1. Choose **Review and launch**\.

1. Review the available settings, and then choose **Create app**\. The application writes `Hello World!` to the response and returns\.

It takes about 10 minutes to create the environment and deploy your code\.

Launching an environment creates the following resources:
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
The Amazon S3 bucket that Elastic Beanstalk creates is shared between environments and isn't deleted when you terminate the environment\. For more information, see [Using Elastic Beanstalk with Amazon S3](AWSHowTo.S3.md)\.

## Cleanup<a name="dotnet-linux-core-tutorial-cleanup"></a>

When you finish working with Elastic Beanstalk, you can terminate your environment\. Elastic Beanstalk terminates all AWS resources associated with your environment, such as [Amazon EC2 instances](using-features.managing.ec2.md), [database instances](using-features.managing.db.md), [load balancers](using-features.managing.elb.md), security groups, and [alarms](using-features.alarms.md#using-features.alarms.title)\. 

**To terminate your Elastic Beanstalk environment**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. Choose **Environment actions**, and then choose **Terminate environment**\.

1. Use the on\-screen dialog box to confirm environment termination\.

With Elastic Beanstalk, you can easily create a new environment for your application at any time\.

## Next steps<a name="dotnet-linux-core-tutorial-nextsteps"></a>

As you continue to develop your application, you might want to manage your environments and deploy your application without manually creating a \.zip file and uploading it to the Elastic Beanstalk console\. The [Elastic Beanstalk Command Line Interface](eb-cli3.md) \(EB CLI\) provides easy\-to\-use commands for creating, configuring, and deploying applications to Elastic Beanstalk environments from the command line interface\.

If you use Visual Studio to develop your application, you can also use the AWS Toolkit for Visual Studio to deploy changes in your code, manage your Elastic Beanstalk environments, and manage other AWS resources\. See [The AWS Toolkit for Visual Studio](dotnet-toolkit.md) for more information\.

For developing and testing purposes, you can consider leveraging the deployment functionality ofElastic Beanstalk to add a managed DB instance directly to your environment\. For information on setting up a database inside your environment, see [Adding a database to your Elastic Beanstalk environment](using-features.managing.db.md)\.

Last, if you plan to use your application in a production environment, we recommend that you [configure a custom domain name](customdomains.md) for your environment and [enable HTTPS](configuring-https.md) for secure connections\.