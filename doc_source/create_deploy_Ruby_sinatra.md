# Deploying a Sinatra Application to AWS Elastic Beanstalk<a name="create_deploy_Ruby_sinatra"></a>

This walkthrough shows how to deploy a simple [Sinatra](http://www.sinatrarb.com/) web application to AWS Elastic Beanstalk using the Elastic Beanstalk Command Line Interface \(EB CLI\)\.

**Note**  
Creating environments with the EB CLI requires a [service role](concepts-roles-service.md)\. You can create a service role by creating an environment in the Elastic Beanstalk console\. If you don't have a service role, the EB CLI attempts to create one when you run `eb create`\.


+ [Prerequisites](#create_deploy_Ruby_sinatra-prereq)
+ [Step 1: Set Up Your Project](#create_deploy_Ruby_sinatra-gitinit)
+ [Step 2: Create an Application](#create_deploy_Ruby_eb_init)
+ [Step 3: Create an Environment](#create_deploy_Ruby_eb_env)
+ [Step 4: Deploy a Simple Sinatra Application](#create_deploy_Ruby_sinatra_update)
+ [Step 5: Clean Up](#create_deploy_Ruby_sinatra_delete)
+ [Related Resources](#create_deploy_Ruby_sinatra_resources)

## Prerequisites<a name="create_deploy_Ruby_sinatra-prereq"></a>

This walkthrough requires a Linux, Windows or OS X workstation\. Performing the walkthrough will modify your workstation's Git and EB CLI configuration\. If you do not want to modify your workstation's configuration for the walkthrough, you can use one of the following:

+ An instance running in a virtual machine on your workstation\.

  This walkthrough was prepared using [Vagrant](https://www.vagrantup.com/) to run a Ubuntu 14\.04 LTS instance in [VirtualBox](https://www.virtualbox.org/)\.

+ An Amazon Elastic Compute Cloud \(Amazon EC2\) instance\.

  [Use SSH](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html) to log in to the instance\. You can perform the entire walkthrough from the command line\. When you have finished, you can terminate the instance\.

The following tools are required to complete this walkthrough:

+ The EB CLI, installed as described in [Install the Elastic Beanstalk Command Line Interface \(EB CLI\)](eb-cli3-install.md)\. 

  This topic also describes how to sign up for an AWS account, if you do not have one\.

+ AWS credentials that have permissions to create the AWS resources that make up the application's environment on your system\.

  These credentials allow the EB CLI to act on your behalf to create the environment's resources\. If you do not have stored credentials, the EB CLI prompts you for credentials when you create the application\. For more information on how to store credentials and how the EB CLI handles stored credentials, see [Configuration Settings and Precedence](eb-cli3-configuration.md#eb-cli3-credentials)\. For more information on the required permissions, see [Using Elastic Beanstalk with AWS Identity and Access Management](AWSHowTo.iam.md)\.

+ Git

  For Linux systems, you can use the package manager to install Git\. For example, the following command installs Git on Debian\-family Linux systems, such as Ubuntu\.

  ```
  $ sudo apt-get install git
  ```

  For Red Hat\-family systems, you can use the same command, but you use the package manager name `yum`\. For more information, including directions for installing Git on OS X systems, see [git](http://git-scm.com/)\.

## Step 1: Set Up Your Project<a name="create_deploy_Ruby_sinatra-gitinit"></a>

With the EB CLI, you can quickly create an Elastic Beanstalk [environment](using-features.managing.md) and deploy applications to that environment from a [Git](http://git-scm.com/) repository\. Before starting your first project, set up the example project for the walkthrough\.

**To set up the example project**

1. Open a terminal window and create a directory for your project in a convenient location on your system\. This walkthrough assumes that the directory is named `sinatraapp`\.

   ```
   ~$ mkdir sinatraapp
   ```

1. Move to the `sinatraapp` directory and initialize a Git repository\.

   ```
   ~$ cd sinatraapp
   ~sinatraapp$ git init .
   ```
**Note**  
You do not need to have access to a remote repository, such as GitHub, for this walkthrough\. The walkthrough uses a local Git repository\.

1. If this is your first time using Git, add your user name and email address to the Git configuration so you can commit changes\.

   ```
   $ git config --global user.email "you@example.com"
   $ git config --global user.name "Username"
   ```

## Step 2: Create an Application<a name="create_deploy_Ruby_eb_init"></a>

 Now create an application and its associated [environment](using-features.managing.md)\.

**To create an application**

1. In the `sinatraapp` directory, create the application by running the following command\.

   ```
   ~/sinatraapp$ eb init
   ```

1. Choose the default AWS region\. For this walkthrough, choose **US\-West\-2**\.

   ```
   Select a default region
   1) us-east-1 : US East (N. Virginia)
   2) us-west-1 : US West (N. California)
   3) us-west-2 : US West (Oregon)
   4) eu-west-1 : EU (Ireland)
   5) eu-central-1 : EU (Frankfurt)
   6) ap-south-1 : Asia Pacific (Mumbai)
   7) ap-southeast-1 : Asia Pacific (Singapore)
   8) ap-southeast-2 : Asia Pacific (Sydney)
   9) ap-northeast-1 : Asia Pacific (Tokyo)
   10) ap-northeast-2 : Asia Pacific (Seoul)
   11) sa-east-1 : South America (São Paulo)
   12) cn-north-1 : China (Beijing)
   13) cn-northwest-1 : China (Ningxia)
   14) us-east-2 : US East (Ohio)
   15) ca-central-1 : Canada (Central)
   16) eu-west-2 : EU (London)
   17) eu-west-3 : EU (Paris)
   (default is 3): 3
   ```
**Note**  
If you have previously configured a default region with the EB CLI, the AWS CLI or an SDK, the EB CLI skips this step and creates the application in the default region unless you explicitly specify a region with the [`--region`](eb3-cmd-options.md) option\.

1. Provide a set of AWS credentials with [appropriate permissions](AWSHowTo.iam.md)\. If you have an [appropriate set of stored credentials](eb-cli3-configuration.md#eb-cli3-credentials), the EB CLI uses them automatically and skips this step\. 
**Important**  
We strongly recommend that you do not provide your account's root credentials to Elastic Beanstalk\. Instead, create an AWS Identity and Access Management \(IAM\) user with appropriate permissions and provide those credentials\. For more information on managing AWS credentials, see [Best Practices for Managing AWS Access Keys](http://docs.aws.amazon.com/general/latest/gr/aws-access-keys-best-practices.html)\.

   If you do not have stored credentials, or your stored credentials do not grant the correct permissions, `eb init` prompts you for credentials, as follows:

   ```
   You have not yet set up your credentials or your credentials are incorrect
   You must provide your credentials.
   (aws-access-id): AKIAIOSFODNN7EXAMPLE
   (aws-secret-key): wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
   ```

   Elastic Beanstalk then stores these credentials in the AWS CLI `config` file with a profile name of `eb-cli`\. 

1. Enter your application name\. For this walkthrough, use the default value, which is the application's root directory name\.

   ```
   Enter Application Name
   (default is "sinatraapp"): sinatraapp
   ```

1. Specify your platform\. For this walkthrough, use **Ruby**\.

   ```
   Select a platform.
   1) Node.js
   2) PHP
   3) Python
   4) Ruby
   5) Tomcat
   6) IIS
   7) Docker
   8) Multi-container Docker
   9) GlassFish
   10) Go
   11) Java
   (default is 1): 6
   ```

1. Specify the platform version\. For this example, use **Ruby 2\.1 \(Puma\)**\. 

   ```
   Select a platform version.1
   1) Ruby 2.1 (Puma)
   2) Ruby 2.1 (Passenger Standalone)
   3) Ruby 2.0 (Puma)
   4) Ruby 2.0 (Passenger Standalone)
   5) Ruby 1.9.3
   (default is 1): 1
   ```

1. Specify whether you want to use SSH to log in to your instances\. You won't need to log in to your instances for this walkthrough, so enter `n`\.

   ```
   Do you want to set up SSH for your instances?
   (y/n): n
   ```

## Step 3: Create an Environment<a name="create_deploy_Ruby_eb_env"></a>

Next, create an Elastic Beanstalk environment and deploy a sample application to it with `eb create`:

```
~/sinatraapp$ eb create sinatraapp-dev --sample
```

**Note**  
If you see a "service role required" error message, run `eb create` interactively \(without specifying an environment name\) and the EB CLI creates the role for you\.

 With just one command, the EB CLI sets up all of the resources our application needs to run in AWS, including the following: 

+ An Amazon S3 bucket to store environment data

+ A load balancer to distribute traffic to the web server\(s\)

+ A security group to allow incoming web traffic

+ An Auto Scaling group to adjust the number of servers in response to load changes

+ Amazon CloudWatch alarms that notify the Auto Scaling group when load is low or high

+ An Amazon EC2 instance hosting our application

 When the process is complete, the EB CLI outputs the public DNS name of the application server\. Use `eb open` to open the website in the default browser:

```
~/sinatraapp$ eb open
```

## Step 4: Deploy a Simple Sinatra Application<a name="create_deploy_Ruby_sinatra_update"></a>

You can now create and deploy a Sinatra application\. This step describes how to implement a simple Sinatra application and deploy it to the environment that you created in the preceding step\. Our example implements a classic application that prints a simple text string, `Hello World!`\. You can easily extend this example to implement more complex classic applications or [modular applications](http://www.sinatrarb.com/intro.html#Sinatra::Base%20-%20Middleware,%20Libraries,%20and%20Modular%20Apps)\.

**Note**  
Create all of the application files in the following procedure in the application's root directory, `sinatraapp`\.

**To create and deploy a Sinatra application**

1. Create a configuration file named **config\.ru** with the following contents\.

   ```
   require './helloworld'
   run Sinatra::Application
   ```

1. Create a Ruby code file named **helloworld\.rb** with the following contents\.

   ```
   require 'sinatra'
   get '/' do
     "Hello World!"
   end
   ```

1. Create a **Gemfile** with the following contents\.

   ```
   source 'https://rubygems.org'
   gem 'sinatra'
   ```

1. Add your files to the Git repository and then commit your changes, as follows:

   ```
   ~/sinatraapp$ git add .
   ~/sinatraapp$ git commit -m "Add a simple Sinatra application"
   ```

   You should see output similar to the following indicating that your files were successfully committed\. 

   ```
   [master (root-commit) dcdfe6c] Add a simple Sinatra application
    4 files changed, 13 insertions(+)
    create mode 100644 .gitignore
    create mode 100644 Gemfile
    create mode 100644 config.ru
    create mode 100644 helloworld.rb
   ```

   The files are committed to the current Git branch\. Because you have not yet explicitly created any branches, the files are committed to the default branch, which is named `master`\. If the repository has multiple branches, you can configure Git to push each branch to a different environment\. For more information, see [Managing Elastic Beanstalk Environments with the EB CLI](eb-cli3-getting-started.md)\. 

1. Deploy the new Sinatra application to the environment\. 

   ```
   ~/sinatraapp$ eb deploy
   ```

   The `eb deploy` command creates a [bundle](applications-sourcebundle.md) of the application code in the master branch and deploys it to the environment, replacing the default application\. The second deployment should be much faster than the first because you have already created the environment's AWS resources\. 

1. Run the `eb status --verbose` command to check your environment status\. You should see output similar to the following\.

   ```
   ~/sinatraapp$ eb status --verbose
   Environment details for: sinatraapp-dev
     Application name: sinatraapp
     Region: us-west-2
     Deployed Version: dcdf
     Environment ID: e-kn7feaqre2
     Platform: 64bit Amazon Linux 2014.09 v1.2.0 running Ruby 2.1 (Puma)
     Tier: WebServer-Standard
     CNAME: sinatraapp-dev.elasticbeanstalk.com
     Updated: 2015-03-03 23:15:19.183000+00:00
     Status: Ready
     Health: Green
     Running instances: 1
         i-c2e712cf: InService
   ```

   Repeat the command until **Status** is **Ready** and **Health** is **Green**\. Then, refresh your browser or run `eb open` again to view the updated application, which should display `Hello World!`\.

**Note**  
For a detailed description of the deployment, you can display the deployment log by running `eb logs`\.

## Step 5: Clean Up<a name="create_deploy_Ruby_sinatra_delete"></a>

When you have finished, you can terminate the application's environment by running the following command from the root directory, `sinatraapp`\.

```
~/sinatraapp$ eb terminate
```

This command shuts down all of the environment's AWS resources, so you do not incur further charges\. It typically takes a few minutes\. When the process is complete, Elastic Beanstalk displays the following message\. 

```
INFO: terminateEnvironment completed successfully.
```

**Note**  
If you have attached an Amazon Relational Database Service \(Amazon RDS\) database instance to your environment, termination deletes the instance\. If you want to save your data, create a snapshot before terminating the environment\. For more information, see [Creating a DB Snapshot](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_CreateSnapshot.html)\. For more information about using Amazon RDS with Elastic Beanstalk, see [Using Amazon RDS with Ruby](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/create_deploy_Ruby.rds.html)\.

## Related Resources<a name="create_deploy_Ruby_sinatra_resources"></a>

For more information about Git commands, see [Git \- Fast Version Control System](http://git-scm.com/)\.

This walkthrough uses only a few of the EB CLI commands\. For a complete list run `eb --help` or see [EB CLI Command Reference](eb3-cmd-commands.md)\.