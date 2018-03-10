# Getting Started with Node\.js on Elastic Beanstalk<a name="nodejs-getstarted"></a>

To get started with Node\.js applications on AWS Elastic Beanstalk, all you need is an application [source bundle](applications-sourcebundle.md) to upload as your first application version and to deploy to an environment\. When you create an environment, Elastic Beanstalk allocates all of the AWS resources needed to run a highly scalable web application\.

## Launching an Environment with a Sample Node\.js Application<a name="nodejs-getstarted-samples"></a>

Elastic Beanstalk provides single page sample applications for each platform as well as more complex examples that show the use of additional AWS resources such as Amazon RDS and language or platform\-specific features and APIs\.


**Samples**  

|  Supported Configurations  |  Environment Type  |  Source Bundle  |  Description  | 
| --- | --- | --- | --- | 
|  Node\.js  |  Web Server  |   [nodejs\-v1\.zip](samples/nodejs-v1.zip)   |  Single page express application\.  | 
|  Node\.js  |  Web Server with Amazon RDS  |  [nodejs\-express\-hiking\-v1\.zip](samples/nodejs-express-hiking-v1.zip)  |  Hiking log application that uses the Express framework and an RDS database\. [Tutorial](create_deploy_nodejs_express.md)  | 
|  Node\.js  |  Web Server with DynamoDB, Amazon SNS and Amazon SQS  |  [eb\-node\-express\-sample\-v1\.0\.zip](https://github.com/awslabs/eb-node-express-sample/releases/download/v1.0/eb-node-express-sample-v1.0.zip) [Clone the repo at GitHub\.com](https://github.com/awslabs/eb-node-express-sample)  |  Express web site that collects user contact information for a new company's marketing campaign\. Uses the AWS SDK for JavaScript in Node\.js to write entries to a DynamoDB table, and Elastic Beanstalk configuration files to create resources in DynamoDB, Amazon SNS and Amazon SQS\. [Tutorial](nodejs-dynamodb-tutorial.md)  | 

Download any of the sample applications and deploy it to Elastic Beanstalk by following these steps:

**To launch an environment with a sample application \(console\)**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Choose an application or [create a new one](applications.md)\.

1. From the **Actions** menu in the upper right corner, choose **Create environment**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/application-actions-createnewenvironment.png)

1. Choose either the **Web server environment** or **Worker environment** [environment tier](concepts.md#concepts-tier)\. You cannot change an environment's tier after creation\.
**Note**  
The [\.NET on Windows Server platform](create_deploy_NET.md) doesn't support the worker environment tier\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/wizard-choosetier.png)

1. Choose a **Platform** that matches the language used by your application\.
**Note**  
Elastic Beanstalk supports multiple [configurations](concepts.platforms.md) for most platforms listed\. By default, the console selects the latest version of the language, web container, or framework [supported by Elastic Beanstalk](concepts.platforms.md)\. If your application requires an older version, choose **Configure more options**, as described below\.

1. For **App code**, choose **Sample application**\.

1. If you want to further customize your environment, choose **Configure more options**\. The following options can be set only during environment creation:

   + Environment name

   + Domain name

   + Platform configuration

   + VPC

   + Tier

   The following settings can be changed after environment creation, but require new instances or other resources to be provisioned and can take a long time to apply:

   + Instance type, root volume, key pair, and IAM role

   + Internal Amazon RDS database

   + Load balancer

   For details on all available settings, see [The Create New Environment Wizard](environments-create-wizard.md)\.

1. Choose **Create environment**\.

## Next Steps<a name="nodejs-getstarted-next"></a>

After you have an environment running an application, you can deploy a new version of the application or a completely different application at any time\. Deploying a new application version is very quick because it doesn't require provisioning or restarting EC2 instances\.

After you've deployed a sample application or two and are ready to start developing and running Node\.js applications locally, see [the next section](nodejs-devenv.md) to set up a Node\.js development environment with all of the tools that you will need\.