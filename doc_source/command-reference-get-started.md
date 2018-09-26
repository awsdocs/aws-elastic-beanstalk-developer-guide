# Getting Started with Eb<a name="command-reference-get-started"></a>

**Note**  
 This version of the EB CLI and its documentation have been replaced with version 3 \(in this section, EB CLI 3 represents version 3 and later of the EB CLI\)\. For information on the new version, see [The Elastic Beanstalk Command Line Interface \(EB CLI\)](eb-cli3.md)\. 

Eb is a command line interface \(CLI\) tool that asks you a series of questions and uses your answers to deploy and manage Elastic Beanstalk applications\. This section provides an end\-to\-end walkthrough using eb to launch a sample application, view it, update it, and then delete it\.

To complete this walkthrough, you will need to download the command line tools at the [AWS Sample Code & Libraries](https://aws.amazon.com/code/6752709412171743) website\. For a complete CLI reference for more advanced scenarios, see [Operations](OperationList-cmd.md), and see [Getting Set Up](usingCLI.md) for instructions on how to get set up\.

## Step 1: Initialize Your Git Repository<a name="command-reference-get-started-getinit"></a>

Eb is a command line interface that you can use with Git to deploy applications quickly and more easily\. Eb is available as part of the Elastic Beanstalk command line tools package\. Follow the steps below to install eb and initialize your Git repository\.

**To install eb, its prerequisite software, and initialize your Git repository**

1. Install the following software onto your local computer:

   1. Linux/Unix/Mac
      + Download and unzip the Elastic Beanstalk command line tools package at the [AWS Sample Code & Libraries](https://aws.amazon.com/code/6752709412171743) website\.
      + Git 1\.6\.6 or later\. To download Git, go to [http://git\-scm\.com/](http://git-scm.com/)\.
      + Python 2\.7 or 3\.0\.

   1. Windows
      + Download and unzip the Elastic Beanstalk command line tools package at the [AWS Sample Code & Libraries](https://aws.amazon.com/code/6752709412171743) website\.
      + Git 1\.6\.6 or later\. To download Git, go to [http://git\-scm\.com/](http://git-scm.com/)\. 
      + PowerShell 2\.0\. 
**Note**  
Windows 7 and Windows Server 2008 R2 come with PowerShell 2\.0\. If you are running an earlier version of Windows, you can download PowerShell 2\.0\. Visit [http://technet\.microsoft\.com/en\-us/scriptcenter/dd742419\.aspx](http://technet.microsoft.com/en-us/scriptcenter/dd742419.aspx) for more details\.

1. Initialize your Git repository\. 

   ```
   git init .
   ```

## Step 2: Configure Elastic Beanstalk<a name="command-reference-get-started-init"></a>

Elastic Beanstalk needs the following information to deploy an application: 
+ AWS access key ID
+ AWS secret key
+ Service region
+ Application name
+ Environment name
+ Solution stack

When you use the `init` command, Elastic Beanstalk will prompt you to enter this information\. If a default value or current setting is available, and you want to use it, press `Enter`\.

Before you use eb, set your PATH to the location of eb\. The following table shows an example for Linux/UNIX and Windows\.


****  

| In Linux and UNIX | In Windows | 
| --- | --- | 
|  $ export PATH=$PATH:<path to unzipped eb CLI package>/eb/linux/python2\.7/ If you are using Python 3\.0, the path will include `python3` rather than `python2.7`\.  |  C:\\> set PATH=%PATH%;<path to unzipped eb CLI package>\\eb\\windows\\  | 

**To configure Elastic Beanstalk**
**Note**  
EB CLI stores your credentials in a file named `credentials` in a folder named `.aws` in your user directory\.

1. From the directory where you created your local repository, type the following command:

   ```
   eb init
   ```

1. When you are prompted for the access key ID, type your access key ID\. To get your access key ID, see [How Do I Get Security Credentials?](https://docs.aws.amazon.com/general/latest/gr/getting-aws-sec-creds.html) in the *AWS General Reference*\.

   ```
   Enter your AWS Access Key ID (current value is "AKIAIOSFODNN7EXAMPLE"): 
   ```

1. When you are prompted for the secret access key, type your secret access key\. To get your secret access key, see [How Do I Get Security Credentials?](https://docs.aws.amazon.com/general/latest/gr/getting-aws-sec-creds.html) in the *AWS General Reference*\.

   ```
   Enter your AWS Secret Access Key (current value is "wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY"): 
   ```

1. When you are prompted for the Elastic Beanstalk region, type the number of the region\. For information about this product's regions, go to [Regions and Endpoints](http://docs.aws.amazon.com/general/latest/gr/rande.html?r=1166) in the Amazon Web Services General Reference\. For this example, we'll use **US West \(Oregon\)**\.

1. When you are prompted for the Elastic Beanstalk application name, type the name of the application\. Elastic Beanstalk generates an application name based on the current directory name if an application name has not been previously configured\. In this example, we use HelloWorld\. 

   ```
   Enter an AWS Elastic Beanstalk application name (auto-generated value is "windows"): HelloWorld
   ```
**Note**  
If you have a space in your application name, be sure you do not use quotation marks\. 

1. When you are prompted for the Elastic Beanstalk environment name, type the name of the environment\. Elastic Beanstalk automatically creates an environment name based on your application name\. If you want to accept the default, press **Enter**\.

   ```
   Enter an AWS Elastic Beanstalk environment name (current value is "HelloWorld-env"): 
   ```
**Note**  
If you have a space in your application name, be sure you do not have a space in your environment name\. 

1. When you are prompted, choose an environment tier\. For more information about environment tiers, see [Environment Tier](concepts.md#concepts-tier)\. For this example, we'll use **1**\.

   ```
   Available environment tiers are:
   1) WebServer::Standard::1.0
   2) Worker::SQS/HTTP::1.0
   ```

1. When you are prompted for the solution stack, type the number of the solution stack you want\. For more information about solution stacks, see [Elastic Beanstalk Supported Platforms](concepts.platforms.md)\. For this example, we'll use **64bit Amazon Linux running PHP 5\.4**\.

1. When you are prompted, choose an environment type\. For this example, use **2**\.

   ```
   Available environment types are: 
   1) LoadBalanced
   2) SingleInstance
   ```

1. When you are prompted to create an Amazon RDS DB instance, type **y** or **n**\. For more information about using Amazon RDS, see [Using Elastic Beanstalk with Amazon Relational Database Service](AWSHowTo.RDS.md)\. For this example, we'll type **y**\.

   ```
   Create an Amazon RDS DB Instance? [y/n]:
   ```

1. When you are prompted to create the database from scratch or a snapshot, type your selection\. For this example, we'll use **No snapshot**\.

1. When you are prompted to enter your RDS user master password, type your password containing 8 to 16 printable ASCII characters \(excluding /, \\, and @\)\.

   ```
   Enter an Amazon RDS DB master password: 
   ```

   ```
   Retype password to confirm: 
   ```

1. When you are prompted to create a snapshot if you delete the Amazon RDS DB instance, type **y** or **n**\. For this example, we'll type **n**\. If you type **n**, then your RDS DB instance will be deleted and your data will be lost if you terminate your environment\.

   By default, eb sets the following default values for Amazon RDS\.
   + **Database engine** — MySQL
   + **Default version: ** — 5\.5
   + **Database name: ** — ebdb
   + **Allocated storage** — 5GB
   + **Instance class** — db\.t2\.micro \(db\.m1\.large for an environment not running in an Amazon VPC\)
   + **Deletion policy** — delete
   + **Master username** — ebroot

1. When you are prompted to enter your instance profile name, you can choose to create a default instance profile or use an existing instance profile\. Using an instance profile enables IAM users and AWS services to gain access to temporary security credentials to make AWS API calls\. Using instance profiles prevents you from having to store long\-term security credentials on the EC2 instance\. For more information about instance profiles, see [Service Roles, Instance Profiles, and User Policies](concepts-roles.md)\. For this example, we'll use **Create a default instance profile**\.

   You should see a confirmation that your AWS Credential file was successfully updated\.

After configuring Elastic Beanstalk, you are ready to deploy a sample application\. 

If you want to update your Elastic Beanstalk configuration, you can use the **init** command again\. When prompted, you can update your configuration options\. If you want to keep any previous settings, press the **Enter** key\. If you want to update your Amazon RDS DB configuration settings, you can update your `optionsettings` file in the `.elasticbeanstalk` directory, and then use the `eb update` command to update your Elastic Beanstalk environment\.

**Note**  
You can set up multiple directories for use with eb—each with its own Elastic Beanstalk configuration—by repeating the preceding two steps in each directory: first initialize a Git repository, and then use init to configure eb\.

## Step 3: Create the Application<a name="command-reference-get-started-create"></a>

Next, you need to create and deploy a sample application\. For this step, you use a sample application that is already prepared\. Elastic Beanstalk uses the configuration information you specified in the previous step to do the following: 
+ Create an application using the application name you specified\.
+ Launch an environment using the environment name you specified that provisions the AWS resources to host the application\.
+ Deploy the application into the newly created environment\.

Use the `start` command to create and deploy a sample application\.

**To create the application**
+ From the directory where you created your local repository, type the following command:

  ```
  eb start
  ```

It may take several minutes to complete this process\. Elastic Beanstalk provides status updates during the process\. If at any time you want to stop polling for status updates, press **Ctrl\+C**\. When the environment status is Green, Elastic Beanstalk outputs a URL for the application\.

## Step 4: View the Application<a name="command-reference-get-started-view"></a>

In the previous step, you created an application and deployed it to Elastic Beanstalk\. After the environment is ready and its status is Green, Elastic Beanstalk provides a URL to view the application\. In this step, you can check the status of the environment to make sure it is set to Green and then copy and paste the URL to view the application\.

Use the `status` command to check the environment status, and then use the URL to view the application\. 

**To view the application**

1. From the directory where you created your local repository, type the following command:

   ```
   eb status --verbose
   ```

   Elastic Beanstalk displays the environment status\. If the environment is set to Green, Elastic Beanstalk displays the URL for the application\. If you attached an Amazon RDS DB instance to your environment, your Amazon RDS DB information is displayed\.

1. Copy and paste the URL into your web browser to view your application\.

## Step 5: Update the Application<a name="command-reference-get-started-update"></a>

After you have deployed a sample application, you can update the sample application with your own application\. In this step, we'll update the sample PHP application with a simple HelloWorld application\.

**To update the sample application**

1. Create a simple PHP file that displays "Hello World" and name it `index.php`\.

   ```
   <html>
     <head>
       <title>PHP Test</title>
     </head>
     <body>
       <?php echo '<p>Hello World</p>'; ?> 
     </body>
   </html>
   ```

   Next, add your new program to your local Git repository, and then commit your change\.

   ```
   git add index.php
   git commit -m "initial check-in"
   ```
**Note**  
For information about Git commands, go to [Git \- Fast Version Control System](http://git-scm.com/)\.

1. Deploy to Elastic Beanstalk\. 

   ```
   eb push
   ```

1. View your updated application\. Copy and paste the same URL in your web browser as you did in [Step 4: View the Application](#command-reference-get-started-view)\.

## Step 6: Clean Up<a name="command-reference-get-started-delete"></a>

If you no longer want to run your application, you can clean up by terminating your environment and deleting your application\. 

Use the `stop` command to terminate your environment and the `delete` command to delete your application\. 

**To terminate your environment and delete the application**

1. From the directory where you created your local repository, type the following command:

   ```
   eb stop
   ```

   This process may take a few minutes\. Elastic Beanstalk displays a message once the environment has been successfully terminated\. 
**Note**  
If you attached an Amazon RDS DB instance to your environment, your Amazon RDS DB will be deleted, and you will lose your data\. To save your data, create a snapshot before you delete the application\. For instructions on how to create a snapshot, go to [Creating a DB Snapshot](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_CreateSnapshot.html) in the *Amazon Relational Database Service User Guide*\.

1. From the directory where you installed the command line interface, type the following command:

   ```
   eb delete
   ```

   Elastic Beanstalk displays a message once it has successfully deleted the application\. 