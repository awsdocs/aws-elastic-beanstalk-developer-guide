# Deploying a Node\.js application with DynamoDB to Elastic Beanstalk<a name="nodejs-dynamodb-tutorial"></a>

This tutorial and [sample application](https://github.com/awslabs/eb-node-express-sample) walks you through the process of deploying a Node\.js application that uses the AWS SDK for JavaScript in Node\.js to interact with Amazon DynamoDB\. You'll create a DynamoDB table that is external to the AWS Elastic Beanstalk environment, and configure the application to use this external table instead of creating one in the environment\. In a production environment, you keep the table independent of the Elastic Beanstalk environment to protect against accidental data loss and enable you to perform [blue/green deployments](using-features.CNAMESwap.md)\.

The tutorial's sample application uses a DynamoDB table to store user\-provided text data\. The sample application uses [configuration files](ebextensions.md) to create the table and an Amazon Simple Notification Service topic\. It also shows how to use a [package\.json file](nodejs-platform-dependencies.md#nodejs-platform-packagejson) to install packages during deployment\.

**Topics**
+ [Prerequisites](#nodejs-dynamodb-tutorial-prereqs)
+ [Launch an Elastic Beanstalk environment](#nodejs-dynamodb-tutorial-launch)
+ [Add permissions to your environment's instances](#nodejs-dynamodb-tutorial-role)
+ [Deploy the sample application](#nodejs-dynamodb-tutorial-deploy)
+ [Create a DynamoDB table](#nodejs-dynamodb-tutorial-database)
+ [Update the application's configuration files](#nodejs-dynamodb-tutorial-update)
+ [Configure your environment for high availability](#nodejs-dynamodb-tutorial-configure)
+ [Cleanup](#nodejs-dynamodb-tutorial-cleanup)
+ [Next steps](#nodejs-dynamodb-tutorial-nextsteps)

## Prerequisites<a name="nodejs-dynamodb-tutorial-prereqs"></a>
+ Before you start, download the sample application source bundle from GitHub: [eb\-node\-express\-sample\-v1\.1\.zip](https://github.com/awslabs/eb-node-express-sample/releases/download/v1.1/eb-node-express-sample-v1.1.zip)\.
+ You will also need a command line terminal or shell to run the commands in the procedures\. Example commands are preceded by a prompt symbol \($\) and the name of the current directory, when appropriate:

  ```
  ~/eb-project$ this is a command
  this is output
  ```
**Note**  
You can run all commands in this tutorial on a Linux virtual machine, an OS X machine, or an Amazon EC2 instance running Amazon Linux\. If you need a development environment, you can launch a single\-instance Elastic Beanstalk environment and connect to it with SSH\.
+ This tutorial uses a command line ZIP utility to create a source bundle that you can deploy to Elastic Beanstalk\. To use the `zip` command in Windows, you can install `UnxUtils`, a lightweight collection of useful command\-line utilities like `zip` and `ls`\. \(Alternatively, you can [use Windows Explorer](applications-sourcebundle.md#using-features.deployment.source.gui) or any other ZIP utility to create source bundle archives\.\)

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

## Launch an Elastic Beanstalk environment<a name="nodejs-dynamodb-tutorial-launch"></a>

You use the Elastic Beanstalk console to launch an Elastic Beanstalk environment\. You'll choose the **Node\.js** platform and accept the default settings and sample code\. After you configure the environment's permissions, you deploy the sample application that you downloaded from GitHub\.

**To launch an environment \(console\)**

1. Open the Elastic Beanstalk console using this preconfigured link: [console\.aws\.amazon\.com/elasticbeanstalk/home\#/newApplication?applicationName=tutorials&environmentType=LoadBalanced](https://console.aws.amazon.com/elasticbeanstalk/home#/newApplication?applicationName=tutorials&environmentType=LoadBalanced)

1. For **Platform**, select the platform and platform branch that match the language used by your application\.

1. For **Application code**, choose **Sample application**\.

1. Choose **Review and launch**\.

1. Review the available options\. Choose the available option you want to use, and when you're ready, choose **Create app**\.

Elastic Beanstalk takes about five minutes to create the environment with the following resources:
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

Elastic Beanstalk manages all of these resources\. When you terminate your environment, Elastic Beanstalk terminates all of the resources that it contains\.

**Note**  
The S3 bucket that Elastic Beanstalk creates is shared between environments and is not deleted during environment termination\. For more information, see [Using Elastic Beanstalk with Amazon S3](AWSHowTo.S3.md)\.

## Add permissions to your environment's instances<a name="nodejs-dynamodb-tutorial-role"></a>

Your application runs on one or more EC2 instances behind a load balancer, serving HTTP requests from the Internet\. When it receives a request that requires it to use AWS services, the application uses the permissions of the instance it runs on to access those services\.

The sample application uses instance permissions to write data to a DynamoDB table, and to send notifications to an Amazon SNS topic with the SDK for JavaScript in Node\.js\. Add the following managed policies to the default [instance profile](concepts-roles-instance.md) to grant the EC2 instances in your environment permission to access DynamoDB and Amazon SNS:
+ **AmazonDynamoDBFullAccess**
+ **AmazonSNSFullAccess**

**To add policies to the default instance profile**

1. Open the [Roles page](https://console.aws.amazon.com/iam/home#roles) in the IAM console\.

1. Choose **aws\-elasticbeanstalk\-ec2\-role**\.

1. On the **Permissions** tab, choose **Attach policies**\.

1. Select the managed policy for the additional services that your application uses\. For example, `AmazonSNSFullAccess` or `AmazonDynamoDBFullAccess`\.

1. Choose **Attach policy**\.

See [Managing Elastic Beanstalk instance profiles](iam-instanceprofile.md) for more on managing instance profiles\.

## Deploy the sample application<a name="nodejs-dynamodb-tutorial-deploy"></a>

Now your environment is ready for you to deploy the sample application to it and then run it\.

**Note**  
Download the source bundle from GitHub if you haven't already: [eb\-node\-express\-sample\-v1\.1\.zip](https://github.com/awslabs/eb-node-express-sample/releases/download/v1.1/eb-node-express-sample-v1.1.zip)\.

**To deploy a source bundle**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. On the environment overview page, choose **Upload and deploy**\.

1. Use the on\-screen dialog box to upload the source bundle\.

1. Choose **Deploy**\.

1. When the deployment completes, you can choose the site URL to open your website in a new tab\.

The site collects user contact information and uses a DynamoDB table to store the data\. To add an entry, choose **Sign up today**, enter a name and email address, and then choose **Sign Up\!**\. The web app writes the form contents to the table and triggers an Amazon SNS email notification\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/nodejs-dynamodb-tutorial-app.png)

Right now, the Amazon SNS topic is configured with a placeholder email for notifications\. You will update the configuration soon, but in the meantime you can verify the DynamoDB table and Amazon SNS topic in the AWS Management Console\.

**To view the table**

1. Open the [Tables page](https://console.aws.amazon.com/dynamodb/home?#tables:) in the DynamoDB console\.

1. Find the table that the application created\. The name starts with **awseb** and contains **StartupSignupsTable**\.

1. Select the table, choose **Items**, and then choose **Start search** to view all items in the table\.

The table contains an entry for every email address submitted on the signup site\. In addition to writing to the table, the application sends a message to an Amazon SNS topic that has two subscriptions, one for email notifications to you, and another for an Amazon Simple Queue Service queue that a worker application can read from to process requests and send emails to interested customers\.

**To view the topic**

1. Open the [Topics page](https://console.aws.amazon.com/sns/v2/home?#/topics) in the Amazon SNS console\.

1. Find the topic that the application created\. The name starts with **awseb** and contains **NewSignupTopic**\.

1. Choose the topic to view its subscriptions\.

The application \(`[app\.js](https://github.com/awslabs/eb-node-express-sample/blob/master/app.js)`\) defines two routes\. The root path \(`/`\) returns a webpage rendered from an Embedded JavaScript \(EJS\) template with a form that the user fills out to register their name and email address\. Submitting the form sends a POST request with the form data to the `/signup` route, which writes an entry to the DynamoDB table and publishes a message to the Amazon SNS topic to notify the owner of the signup\.

The sample application includes [configuration files](ebextensions.md) that create the DynamoDB table, Amazon SNS topic, and Amazon SQS queue used by the application\. This lets you create a new environment and test the functionality immediately, but has the drawback of tying the DynamoDB table to the environment\. For a production environment, you should create the DynamoDB table outside of your environment to avoid losing it when you terminate the environment or update its configuration\.

## Create a DynamoDB table<a name="nodejs-dynamodb-tutorial-database"></a>

To use an external DynamoDB table with an application running in Elastic Beanstalk, first create a table in DynamoDB\. When you create a table outside of Elastic Beanstalk, it is completely independent of Elastic Beanstalk and your Elastic Beanstalk environments, and will not be terminated by Elastic Beanstalk\.

Create a table with the following settings:
+ **Table name** – **nodejs\-tutorial**
+ **Primary key** – **email**
+ Primary key type – **String**

**To create a DynamoDB table**

1. Open the [Tables page](https://console.aws.amazon.com/dynamodb/home?#tables:) in the DynamoDB management console\.

1. Choose **Create table**\.

1. Type a **Table name** and **Primary key**\.

1. Choose the primary key type\.

1. Choose **Create**\.

## Update the application's configuration files<a name="nodejs-dynamodb-tutorial-update"></a>

Update the [configuration files](ebextensions.md) in the application source to use the **nodejs\-tutorial** table instead of creating a new one\.

**To update the sample application for production use**

1. Extract the project files from the source bundle:

   ```
   ~$ mkdir nodejs-tutorial
   ~$ cd nodejs-tutorial
   ~/nodejs-tutorial$ unzip ~/Downloads/eb-node-express-sample-v1.0.zip
   ```

1. Open `.ebextensions/options.config` and change the values of the following settings:
   + **NewSignupEmail** – Your email address\.
   + **STARTUP\_SIGNUP\_TABLE** – **nodejs\-tutorial**  
**Example \.ebextensions/options\.config**  

   ```
   option_settings:
     aws:elasticbeanstalk:customoption:
       NewSignupEmail: you@example.com
     aws:elasticbeanstalk:application:environment:
       THEME: "flatly"
       AWS_REGION: '`{"Ref" : "AWS::Region"}`'
       STARTUP_SIGNUP_TABLE: nodejs-tutorial
       NEW_SIGNUP_TOPIC: '`{"Ref" : "NewSignupTopic"}`'
     aws:elasticbeanstalk:container:nodejs:
       ProxyServer: nginx
     aws:elasticbeanstalk:container:nodejs:staticfiles:
       /static: /static
     aws:autoscaling:asg:
       Cooldown: "120"
     aws:autoscaling:trigger:
       Unit: "Percent"
       Period: "1"
       BreachDuration: "2"
       UpperThreshold: "75"
       LowerThreshold: "30"
       MeasureName: "CPUUtilization"
   ```

   This configures the application to use the **nodejs\-tutorial** table instead of the one created by `.ebextensions/create-dynamodb-table.config`, and sets the email address that the Amazon SNS topic uses for notifications\.

1. Remove `.ebextensions/create-dynamodb-table.config`\.

   ```
   ~/nodejs-tutorial$ rm .ebextensions/create-dynamodb-table.config
   ```

   The next time you deploy the application, the table created by this configuration file will be deleted\.

1. Create a source bundle from the modified code\.

   ```
   ~/nodejs-tutorial$ zip nodejs-tutorial.zip -r * .[^.]*
     adding: LICENSE (deflated 65%)
     adding: README.md (deflated 56%)
     adding: app.js (deflated 63%)
     adding: iam_policy.json (deflated 47%)
     adding: misc/ (stored 0%)
     adding: misc/theme-flow.png (deflated 1%)
     adding: npm-shrinkwrap.json (deflated 87%)
     adding: package.json (deflated 40%)
     adding: static/ (stored 0%)
     adding: static/bootstrap/ (stored 0%)
     adding: static/bootstrap/css/ (stored 0%)
     adding: static/bootstrap/css/jumbotron-narrow.css (deflated 59%)
     adding: static/bootstrap/css/theme/ (stored 0%)
     adding: static/bootstrap/css/theme/united/ (stored 0%)
     adding: static/bootstrap/css/theme/united/bootstrap.css (deflated 86%)
     adding: static/bootstrap/css/theme/amelia/ (stored 0%)
     adding: static/bootstrap/css/theme/amelia/bootstrap.css (deflated 86%)
     adding: static/bootstrap/css/theme/slate/ (stored 0%)
     adding: static/bootstrap/css/theme/slate/bootstrap.css (deflated 87%)
     adding: static/bootstrap/css/theme/default/ (stored 0%)
     adding: static/bootstrap/css/theme/default/bootstrap.css (deflated 86%)
     adding: static/bootstrap/css/theme/flatly/ (stored 0%)
     adding: static/bootstrap/css/theme/flatly/bootstrap.css (deflated 86%)
     adding: static/bootstrap/LICENSE (deflated 65%)
     adding: static/bootstrap/js/ (stored 0%)
     adding: static/bootstrap/js/bootstrap.min.js (deflated 74%)
     adding: static/bootstrap/fonts/ (stored 0%)
     adding: static/bootstrap/fonts/glyphicons-halflings-regular.eot (deflated 1%)
     adding: static/bootstrap/fonts/glyphicons-halflings-regular.svg (deflated 73%)
     adding: static/bootstrap/fonts/glyphicons-halflings-regular.woff (deflated 1%)
     adding: static/bootstrap/fonts/glyphicons-halflings-regular.ttf (deflated 44%)
     adding: static/jquery/ (stored 0%)
     adding: static/jquery/jquery-1.11.3.min.js (deflated 65%)
     adding: static/jquery/MIT-LICENSE.txt (deflated 41%)
     adding: views/ (stored 0%)
     adding: views/index.ejs (deflated 67%)
     adding: .ebextensions/ (stored 0%)
     adding: .ebextensions/options.config (deflated 47%)
     adding: .ebextensions/create-sns-topic.config (deflated 56%)
   ```

Deploy the nodejs\-tutorial\.zip source bundle to your environment\.

**To deploy a source bundle**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. On the environment overview page, choose **Upload and deploy**\.

1. Use the on\-screen dialog box to upload the source bundle\.

1. Choose **Deploy**\.

1. When the deployment completes, you can choose the site URL to open your website in a new tab\.

When you deploy, Elastic Beanstalk updates the configuration of the Amazon SNS topic and deletes the DynamoDB table that it created when you deployed the first version of the application\. 

Now, when you terminate the environment, the **nodejs\-tutorial** table will not be deleted\. This lets you perform blue/green deployments, modify configuration files, or take down your website without risking data loss\.

Open your site in a browser and verify that the form works as you expect\. Create a few entries, and then check the DynamoDB console to verify the table\.

**To view the table**

1. Open the [Tables page](https://console.aws.amazon.com/dynamodb/home?#tables:) in the DynamoDB console\.

1. Find the **nodejs\-tutorial** table\.

1. Select the table, choose **Items**, and then choose **Start search** to view all items in the table\.

You can also see that Elastic Beanstalk deleted the table that it created previously\.

## Configure your environment for high availability<a name="nodejs-dynamodb-tutorial-configure"></a>

Finally, configure your environment's Auto Scaling group with a higher minimum instance count\. Run at least two instances at all times to prevent the web servers in your environment from being a single point of failure, and to allow you to deploy changes without taking your site out of service\.

**To configure your environment's Auto Scaling group for high availability**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Configuration**\.

1. In the **Capacity** configuration category, choose **Edit**\.

1. In the **Auto Scaling group** section, set **Min instances** to **2**\.

1. Choose **Apply**\.

## Cleanup<a name="nodejs-dynamodb-tutorial-cleanup"></a>

When you finish working with Elastic Beanstalk, you can terminate your environment\. Elastic Beanstalk terminates all AWS resources associated with your environment, such as [Amazon EC2 instances](using-features.managing.ec2.md), [database instances](using-features.managing.db.md), [load balancers](using-features.managing.elb.md), security groups, and [alarms](using-features.alarms.md#using-features.alarms.title)\. 

**To terminate your Elastic Beanstalk environment**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. Choose **Environment actions**, and then choose **Terminate environment**\.

1. Use the on\-screen dialog box to confirm environment termination\.

With Elastic Beanstalk, you can easily create a new environment for your application at any time\.

You can also delete the external DynamoDB tables that you created\.

**To delete a DynamoDB table**

1. Open the [Tables page](https://console.aws.amazon.com/dynamodb/home?#tables:) in the DynamoDB console\.

1. Select a table\.

1. Choose **Actions**, and then choose **Delete table**\.

1. Choose **Delete**\.

## Next steps<a name="nodejs-dynamodb-tutorial-nextsteps"></a>

As you continue to develop your application, you'll probably want to manage environments and deploy your application without manually creating a \.zip file and uploading it to the Elastic Beanstalk console\. The [Elastic Beanstalk Command Line Interface](eb-cli3.md) \(EB CLI\) provides easy\-to\-use commands for creating, configuring, and deploying applications to Elastic Beanstalk environments from the command line\.

The sample application uses configuration files to configure software settings and create AWS resources as part of your environment\. See [Advanced environment customization with configuration files \(`.ebextensions`\)](ebextensions.md) for more information about configuration files and their use\.

The sample application for this tutorial uses the Express web framework for Node\.js\. For more information about Express, see the official documentation at [expressjs\.com](https://expressjs.com)\.

Finally, if you plan on using your application in a production environment, [configure a custom domain name](customdomains.md) for your environment and [enable HTTPS](configuring-https.md) for secure connections\.