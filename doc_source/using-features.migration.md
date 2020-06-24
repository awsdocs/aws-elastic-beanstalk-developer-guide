# Migrating your application from a legacy platform version<a name="using-features.migration"></a>

If you have deployed an Elastic Beanstalk application that uses a legacy platform version, you should migrate your application to a new environment using a non\-legacy platform version so that you can get access to new features\. If you are unsure whether you are running your application using a legacy platform version, you can check in the Elastic Beanstalk console\. For instructions, see [To check if you are using a legacy platform version](#using-features.migration-proc)\.

## What new features are legacy platform versions missing?<a name="using-features.migration.missing"></a>

Legacy platforms do not support the following features:
+ Configuration files, as described in the [Advanced environment customization with configuration files \(`.ebextensions`\)](ebextensions.md) topic
+ ELB health checks, as described in the [Basic health reporting](using-features.healthstatus.md) topic
+ Instance Profiles, as described in the [Managing Elastic Beanstalk instance profiles](iam-instanceprofile.md) topic
+ VPCs, as described in the [Using Elastic Beanstalk with Amazon VPC](vpc.md) topic
+ Data Tiers, as described in the [Adding a database to your Elastic Beanstalk environment](using-features.managing.db.md) topic
+ Worker Tiers, as described in the [Worker environments](concepts-worker.md) topic
+ Single Instance Environments, as described in the [Environment types](using-features-managing-env-types.md) topic
+ Tags, as described in the [Tagging resources in your Elastic Beanstalk environments](using-features.tagging.md) topic
+ Rolling Updates, as described in the [Elastic Beanstalk rolling environment configuration updates](using-features.rollingupdates.md) topic

## Why are some platform versions marked legacy?<a name="using-features.migration.why"></a>

Some older platform versions do not support the latest Elastic Beanstalk features\. These versions are marked **\(legacy\)** on the environment overview page in the Elastic Beanstalk console\. <a name="using-features.migration-proc"></a>

**To check if you are using a legacy platform version**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. On the environment overview page, view the **Platform** name\.

   Your application is using a legacy platform version if you see **\(legacy\)** next to the platform's name\.

**To migrate your application**

1. Deploy your application to a new environment\. For instructions, go to [Creating an Elastic Beanstalk environment](using-features.environments.md)\.

1. If you have an Amazon RDS DB Instance, update your database security group to allow access to your EC2 security group for your new environment\. For instructions on how to find the name of your EC2 security group using the AWS Management Console, see [Security groups](using-features.managing.ec2.md#using-features.managing.ec2.securitygroups)\. For more information about configuring your EC2 security group, go to the "Authorizing Network Access to an Amazon EC2 Security Group" section of [Working with DB Security Groups](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_WorkingWithSecurityGroups.html) in the *Amazon Relational Database Service User Guide*\.

1. Swap your environment URL\. For instructions, go to [Blue/Green deployments with Elastic Beanstalk](using-features.CNAMESwap.md)\.

1. Terminate your old environment\. For instructions, go to [Terminate an Elastic Beanstalk environment](using-features.terminating.md)\.

**Note**  
If you use AWS Identity and Access Management \(IAM\) then you will need to update your policies to include AWS CloudFormation and Amazon RDS \(if applicable\)\. For more information, see [Using Elastic Beanstalk with AWS Identity and Access Management](AWSHowTo.iam.md)\.