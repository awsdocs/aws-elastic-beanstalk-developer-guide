# Tutorial: Deploying an ASP\.NET core application with Elastic Beanstalk<a name="dotnet-core-tutorial"></a>

In this tutorial, you will walk through the process of building a new ASP\.NET Core application and deploying it to AWS Elastic Beanstalk\.

 First, you will use the \.NET Core SDK's `dotnet` command line tool to generate a basic \.NET Core command line application, install dependencies, compile code, and run applications locally\.  Next, you will create the default `Program.cs` class, and add an ASP\.NET `Startup.cs` class and configuration files to make an application that serves HTTP requests with ASP\.NET and IIS\. 

Finally, Elastic Beanstalk uses a [deployment manifest](dotnet-manifest.md) to configure deployments for \.NET Core applications, custom applications, and multiple \.NET Core or MSBuild applications on a single server\. To deploy a \.NET Core application to a Windows Server environment, you add a site archive to an application source bundle with a deployment manifest\. The `dotnet publish` command generates compiled classes and dependencies that you can bundle with a `web.config` file to create a site archive\. The deployment manifest tells Elastic Beanstalk the path at which the site should run and can be used to configure application pools and run multiple applications at different paths\.

The application source code is available here: [dotnet\-core\-tutorial\-source\.zip](samples/dotnet-core-tutorial-source.zip)

The deployable source bundle is available here: [dotnet\-core\-tutorial\-bundle\.zip](samples/dotnet-core-tutorial-bundle.zip)

**Topics**
+ [Prerequisites](#dotnet-core-tutorial-prereqs)
+ [Generate a \.NET core project](#dotnet-core-tutorial-generate)
+ [Launch an Elastic Beanstalk environment](#dotnet-core-tutorial-launch)
+ [Update the source code](#dotnet-core-tutorial-update)
+ [Deploy your application](#dotnet-core-tutorial-deploy)
+ [Cleanup](#dotnet-core-tutorial-cleanup)
+ [Next steps](#dotnet-core-tutorial-nextsteps)

## Prerequisites<a name="dotnet-core-tutorial-prereqs"></a>

This tutorial uses the \.NET Core SDK to generate a basic \.NET Core application, run it locally, and build a deployable package\.

**Requirements**
+ \.NET Core \(x64\) 1\.0\.1, 2\.0\.0, or later 

**To install the \.NET core SDK**

1. Download the installer from [microsoft\.com/net/core](https://www.microsoft.com/net/core#windows)\. Choose **Windows**\. Choose **Download \.NET SDK**\.

1. Run the installer and follow the instructions\.

This tutorial uses a command line ZIP utility to create a source bundle that you can deploy to Elastic Beanstalk\. To use the `zip` command in Windows, you can install `UnxUtils`, a lightweight collection of useful command line utilities like `zip` and `ls`\. Alternatively, you can [use Windows Explorer](applications-sourcebundle.md#using-features.deployment.source.gui) or any other ZIP utility to create source bundle archives\.

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

## Generate a \.NET core project<a name="dotnet-core-tutorial-generate"></a>

Use the `dotnet` command line tool to generate a new C\# \.NET Core project and run it locally\. The default \.NET Core application is a command line utility that prints `Hello World!` and then exits\. 

**To generate a new \.NET core project**

1. Open a new command prompt window and navigate to your user folder\.

   ```
   > cd %USERPROFILE%
   ```

1. Use the `dotnet new` command to generate a new \.NET Core project\.

   ```
   C:\Users\username> dotnet new console -o dotnet-core-tutorial
   Content generation time: 65.0152 ms
   The template "Console Application" created successfully.
   C:\Users\username> cd dotnet-core-tutorial
   ```

1. Use the `dotnet restore` command to install dependencies\.

   ```
   C:\Users\username\dotnet-core-tutorial> dotnet restore
   Restoring packages for C:\Users\username\dotnet-core-tutorial\dotnet-core-tutorial.csproj...
   Generating MSBuild file C:\Users\username\dotnet-core-tutorial\obj\dotnet-core-tutorial.csproj.nuget.g.props.
   Generating MSBuild file C:\Users\username\dotnet-core-tutorial\obj\dotnet-core-tutorial.csproj.nuget.g.targets.
   Writing lock file to disk. Path: C:\Users\username\dotnet-core-tutorial\obj\project.assets.json
   Restore completed in 1.25 sec for C:\Users\username\dotnet-core-tutorial\dotnet-core-tutorial.csproj.
   
   NuGet Config files used:
       C:\Users\username\AppData\Roaming\NuGet\NuGet.Config
       C:\Program Files (x86)\NuGet\Config\Microsoft.VisualStudio.Offline.config
   Feeds used:
       https://api.nuget.org/v3/index.json
       C:\Program Files (x86)\Microsoft SDKs\NuGetPackages\
   ```

1. Use the `dotnet run` command to build and run the application locally\.

   ```
   C:\Users\username\dotnet-core-tutorial> dotnet run
   Hello World!
   ```

## Launch an Elastic Beanstalk environment<a name="dotnet-core-tutorial-launch"></a>

Use the Elastic Beanstalk console to launch an Elastic Beanstalk environment\. For this example, you will launch with a \.NET platform\. After you launch and configure your environment, you can deploy new source code at any time\.

**To launch an environment \(console\)**

1. Open the Elastic Beanstalk console using this preconfigured link: [console\.aws\.amazon\.com/elasticbeanstalk/home\#/newApplication?applicationName=tutorials&environmentType=LoadBalanced](https://console.aws.amazon.com/elasticbeanstalk/home#/newApplication?applicationName=tutorials&environmentType=LoadBalanced)

1. For **Platform**, select the platform and platform branch that match the language used by your application\.

1. For **Application code**, choose **Sample application**\.

1. Choose **Review and launch**\.

1. Review the available options\. Choose the available option you want to use, and when you're ready, choose **Create app**\.

Environment creation takes about 10 minutes\. During this time you can update your source code\.

## Update the source code<a name="dotnet-core-tutorial-update"></a>

Modify the default application into a web application that uses ASP\.NET and IIS\. 
+ ASP\.NET is the website framework for \.NET\. 
+ IIS is the web server that runs the application on the Amazon EC2 instances in your Elastic Beanstalk environment\.

The source code examples to follow are available here: [dotnet\-core\-tutorial\-source\.zip](samples/dotnet-core-tutorial-source.zip)

**Note**  
The following procedure shows how to convert the project code into a web application\. To simplify the process, you can generate the project as a web application right from the start\. In the previous section [Generate a \.NET core project](#dotnet-core-tutorial-generate), modify the `dotnet new` step's command with the following command\.  

```
C:\Users\username> dotnet new web -o dotnet-core-tutorial
```

**To add ASP\.NET and IIS support to your code**

1. Copy `Program.cs` to your application directory to run as a web host builder\.  
**Example c:\\users\\username\\dotnet\-core\-tutorial\\Program\.cs**  

   ```
   using System;
   using Microsoft.AspNetCore.Hosting;
   using System.IO;
   
   namespace aspnetcoreapp
   {
       public class Program
       {
           public static void Main(string[] args)
           {
               var host = new WebHostBuilder()
                 .UseKestrel()
                 .UseContentRoot(Directory.GetCurrentDirectory())
                 .UseIISIntegration()
                 .UseStartup<Startup>()
                 .Build();
   
               host.Run();
           }
       }
   }
   ```

1. Add `Startup.cs` to run an ASP\.NET website\.  
**Example c:\\users\\username\\dotnet\-core\-tutorial\\Startup\.cs**  

   ```
   using System;
   using Microsoft.AspNetCore.Builder;
   using Microsoft.AspNetCore.Hosting;
   using Microsoft.AspNetCore.Http;
   
   namespace aspnetcoreapp
   {
       public class Startup
       {
           public void Configure(IApplicationBuilder app)
           {
               app.Run(context =>
               {
                   return context.Response.WriteAsync("Hello from ASP.NET Core!");
               });
           }
       }
   }
   ```

1. Add the `web.config` file to configure the IIS server\.  
**Example c:\\users\\username\\dotnet\-core\-tutorial\\web\.config**  

   ```
   <?xml version="1.0" encoding="utf-8"?>
   <configuration>
     <system.webServer>
       <handlers>
         <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
       </handlers>
       <aspNetCore processPath="dotnet" arguments=".\dotnet-core-tutorial.dll" stdoutLogEnabled="false" stdoutLogFile=".\logs\stdout" forwardWindowsAuthToken="false" />
     </system.webServer>
   </configuration>
   ```

1. Add `dotnet-core-tutorial.csproj`, which includes IIS middleware and includes the `web.config` file from the output of `dotnet publish`\.
**Note**  
The following example was developed using \.NET Core Runtime 2\.2\.1\. You might need to modify the `TargetFramework` or the `Version` attribute values in the `PackageReference` elements to match the version of \.NET Core Runtime that you are using in your custom projects\.  
**Example c:\\users\\username\\dotnet\-core\-tutorial\\dotnet\-core\-tutorial\.csproj**  

   ```
   <Project Sdk="Microsoft.NET.Sdk">
   
       <PropertyGroup>
           <OutputType>Exe</OutputType>
           <TargetFramework>netcoreapp2.2</TargetFramework>
       </PropertyGroup>
   
       <ItemGroup>
           <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel" Version="2.2.0" />
       </ItemGroup>
   
       <ItemGroup>
           <PackageReference Include="Microsoft.AspNetCore.Server.IISIntegration" Version="2.2.0" />
       </ItemGroup>
   
       <ItemGroup>
           <None Include="web.config" CopyToPublishDirectory="Always" />
       </ItemGroup>
   
   </Project>
   ```

Next, install the new dependencies and run the ASP\.NET website locally\.

**To run the website locally**

1. Use the `dotnet restore` command to install dependencies\.

1. Use the `dotnet run` command to build and run the app locally\.

1. Open [localhost:5000](http://localhost:5000) to view the site\.

To run the application on a web server, you need to bundle the compiled source code with a `web.config` configuration file and runtime dependencies\. The `dotnet` tool provides a `publish` command that gathers these files in a directory based on the configuration in `dotnet-core-tutorial.csproj`\.

**To build your website**
+ Use the `dotnet publish` command to output compiled code and dependencies to a folder named `site`\.

  ```
  C:\users\username\dotnet-core-tutorial> dotnet publish -o site
  ```

To deploy the application to Elastic Beanstalk, bundle the site archive with a [deployment manifest](dotnet-manifest.md)\. This tells Elastic Beanstalk how to run it\.

**To create a source bundle**

1. Add the files in the site folder to a ZIP archive\.
**Note**  
If you use a different ZIP utility, be sure to add all files to the root folder of the resulting ZIP archive\. This is required for a successful deployment of the application to your Elastic Beanstalk environment\.

   ```
   C:\users\username\dotnet-core-tutorial> cd site
   C:\users\username\dotnet-core-tutorial\site> zip ../site.zip *
     adding: dotnet-core-tutorial.deps.json (164 bytes security) (deflated 84%)
     adding: dotnet-core-tutorial.dll (164 bytes security) (deflated 59%)
     adding: dotnet-core-tutorial.pdb (164 bytes security) (deflated 28%)
     adding: dotnet-core-tutorial.runtimeconfig.json (164 bytes security) (deflated 26%)
     adding: Microsoft.AspNetCore.Authentication.Abstractions.dll (164 bytes security) (deflated 49%)
     adding: Microsoft.AspNetCore.Authentication.Core.dll (164 bytes security) (deflated 57%)
     adding: Microsoft.AspNetCore.Connections.Abstractions.dll (164 bytes security) (deflated 51%)
     adding: Microsoft.AspNetCore.Hosting.Abstractions.dll (164 bytes security) (deflated 49%)
     adding: Microsoft.AspNetCore.Hosting.dll (164 bytes security) (deflated 60%)
     adding: Microsoft.AspNetCore.Hosting.Server.Abstractions.dll (164 bytes security) (deflated 44%)
     adding: Microsoft.AspNetCore.Http.Abstractions.dll (164 bytes security) (deflated 54%)
     adding: Microsoft.AspNetCore.Http.dll (164 bytes security) (deflated 55%)
     adding: Microsoft.AspNetCore.Http.Extensions.dll (164 bytes security) (deflated 50%)
     adding: Microsoft.AspNetCore.Http.Features.dll (164 bytes security) (deflated 50%)
     adding: Microsoft.AspNetCore.HttpOverrides.dll (164 bytes security) (deflated 49%)
     adding: Microsoft.AspNetCore.Server.IISIntegration.dll (164 bytes security) (deflated 46%)
     adding: Microsoft.AspNetCore.Server.Kestrel.Core.dll (164 bytes security) (deflated 63%)
     adding: Microsoft.AspNetCore.Server.Kestrel.dll (164 bytes security) (deflated 46%)
     adding: Microsoft.AspNetCore.Server.Kestrel.Https.dll (164 bytes security) (deflated 44%)
     adding: Microsoft.AspNetCore.Server.Kestrel.Transport.Abstractions.dll (164 bytes security) (deflated 56%)
     adding: Microsoft.AspNetCore.Server.Kestrel.Transport.Sockets.dll (164 bytes security) (deflated 51%)
     adding: Microsoft.AspNetCore.WebUtilities.dll (164 bytes security) (deflated 55%)
     adding: Microsoft.Extensions.Configuration.Abstractions.dll (164 bytes security) (deflated 48%)
     adding: Microsoft.Extensions.Configuration.Binder.dll (164 bytes security) (deflated 47%)
     adding: Microsoft.Extensions.Configuration.dll (164 bytes security) (deflated 46%)
     adding: Microsoft.Extensions.Configuration.EnvironmentVariables.dll (164 bytes security) (deflated 46%)
     adding: Microsoft.Extensions.Configuration.FileExtensions.dll (164 bytes security) (deflated 47%)
     adding: Microsoft.Extensions.DependencyInjection.Abstractions.dll (164 bytes security) (deflated 54%)
     adding: Microsoft.Extensions.DependencyInjection.dll (164 bytes security) (deflated 53%)
     adding: Microsoft.Extensions.FileProviders.Abstractions.dll (164 bytes security) (deflated 46%)
     adding: Microsoft.Extensions.FileProviders.Physical.dll (164 bytes security) (deflated 47%)
     adding: Microsoft.Extensions.FileSystemGlobbing.dll (164 bytes security) (deflated 49%)
     adding: Microsoft.Extensions.Hosting.Abstractions.dll (164 bytes security) (deflated 47%)
     adding: Microsoft.Extensions.Logging.Abstractions.dll (164 bytes security) (deflated 54%)
     adding: Microsoft.Extensions.Logging.dll (164 bytes security) (deflated 48%)
     adding: Microsoft.Extensions.ObjectPool.dll (164 bytes security) (deflated 45%)
     adding: Microsoft.Extensions.Options.dll (164 bytes security) (deflated 53%)
     adding: Microsoft.Extensions.Primitives.dll (164 bytes security) (deflated 50%)
     adding: Microsoft.Net.Http.Headers.dll (164 bytes security) (deflated 53%)
     adding: System.IO.Pipelines.dll (164 bytes security) (deflated 50%)
     adding: System.Runtime.CompilerServices.Unsafe.dll (164 bytes security) (deflated 43%)
     adding: System.Text.Encodings.Web.dll (164 bytes security) (deflated 57%)
     adding: web.config (164 bytes security) (deflated 39%)
   C:\users\username\dotnet-core-tutorial\site> cd ../
   ```

1. Add a deployment manifest that points to the site archive\.  
**Example c:\\users\\username\\dotnet\-core\-tutorial\\aws\-windows\-deployment\-manifest\.json**  

   ```
   {
       "manifestVersion": 1,
       "deployments": {
           "aspNetCoreWeb": [
           {
               "name": "test-dotnet-core",
               "parameters": {
                   "appBundle": "site.zip",
                   "iisPath": "/",
                   "iisWebSite": "Default Web Site"
               }
           }
           ]
       }
   }
   ```

1. Use the `zip` command to create a source bundle named `dotnet-core-tutorial.zip`\.

   ```
   C:\users\username\dotnet-core-tutorial> zip dotnet-core-tutorial.zip site.zip aws-windows-deployment-manifest.json
     adding: site.zip (164 bytes security) (stored 0%)
     adding: aws-windows-deployment-manifest.json (164 bytes security) (deflated 50%)
   ```

## Deploy your application<a name="dotnet-core-tutorial-deploy"></a>

Deploy the source bundle to the Elastic Beanstalk environment that you created\.

You can download the source bundle here: [dotnet\-core\-tutorial\-bundle\.zip](samples/dotnet-core-tutorial-bundle.zip)

**To deploy a source bundle**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. On the environment overview page, choose **Upload and deploy**\.

1. Use the on\-screen dialog box to upload the source bundle\.

1. Choose **Deploy**\.

1. When the deployment completes, you can choose the site URL to open your website in a new tab\.

The application simply writes `Hello from ASP.NET Core!` to the response and returns\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/dotnet-core-tutorial-site.png)

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

All of these resources are managed by Elastic Beanstalk\. When you terminate your environment, Elastic Beanstalk terminates all the resources that it contains\.

**Note**  
The Amazon S3 bucket that Elastic Beanstalk creates is shared between environments and isn't deleted during environment termination\. For more information, see [Using Elastic Beanstalk with Amazon S3](AWSHowTo.S3.md)\.

## Cleanup<a name="dotnet-core-tutorial-cleanup"></a>

When you finish working with Elastic Beanstalk, you can terminate your environment\. Elastic Beanstalk terminates all AWS resources associated with your environment, such as [Amazon EC2 instances](using-features.managing.ec2.md), [database instances](using-features.managing.db.md), [load balancers](using-features.managing.elb.md), security groups, and [alarms](using-features.alarms.md#using-features.alarms.title)\. 

**To terminate your Elastic Beanstalk environment**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. Choose **Environment actions**, and then choose **Terminate environment**\.

1. Use the on\-screen dialog box to confirm environment termination\.

With Elastic Beanstalk, you can easily create a new environment for your application at any time\.

## Next steps<a name="dotnet-core-tutorial-nextsteps"></a>

As you continue to develop your application, you'll probably want to manage environments and deploy your application without manually creating a \.zip file and uploading it to the Elastic Beanstalk console\. The [Elastic Beanstalk Command Line Interface](eb-cli3.md) \(EB CLI\) provides easy\-to\-use commands for creating, configuring, and deploying applications to Elastic Beanstalk environments from the command line\.

If you use Visual Studio to develop your application, you can also use the AWS Toolkit for Visual Studio to deploy changed, manage your Elastic Beanstalk environments, and manage other AWS resources\. See [The AWS Toolkit for Visual Studio](dotnet-toolkit.md) for more information\.

For developing and testing, you might want to use the Elastic Beanstalk functionality for adding a managed DB instance directly to your environment\. For instructions on setting up a database inside your environment, see [Adding a database to your Elastic Beanstalk environment](using-features.managing.db.md)\.

Finally, if you plan to use your application in a production environment, [configure a custom domain name](customdomains.md) for your environment and [enable HTTPS](configuring-https.md) for secure connections\.