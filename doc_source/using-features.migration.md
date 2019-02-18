# Migrating Your Application from a Legacy Container Type<a name="using-features.migration"></a>

If you have deployed an Elastic Beanstalk application that uses a legacy container type, you should migrate your application to a new environment using a non\-legacy container type so that you can get access to new features\. If you are unsure whether you are running your application using a legacy container type, you can check in the Elastic Beanstalk console\. For instructions, see [To check if you are using a legacy container type](#using-features.migration-proc)\.

## What new features are legacy containers missing?<a name="using-features.migration.missing"></a>

Legacy platforms do not support the following features:
+ Configuration files, as described in the [Advanced Environment Customization with Configuration Files \(`.ebextensions`\)](ebextensions.md) topic
+ ELB health checks, as described in the [Basic Health Reporting](using-features.healthstatus.md) topic
+ Instance Profiles, as described in the [Managing Elastic Beanstalk Instance Profiles](iam-instanceprofile.md) topic
+ VPCs, as described in the [Using Elastic Beanstalk with Amazon Virtual Private Cloud](vpc.md) topic
+ Data Tiers, as described in the [Adding a Database to Your Elastic Beanstalk Environment](using-features.managing.db.md) topic
+ Worker Tiers, as described in the [Worker Environments](concepts-worker.md) topic
+ Single Instance Environments, as described in the [Environment Types](using-features-managing-env-types.md) topic
+ Tags, as described in the [Tagging Resources in Your Elastic Beanstalk Environment](using-features.tagging.md) topic
+ Rolling Updates, as described in the [Elastic Beanstalk Rolling Environment Configuration Updates](using-features.rollingupdates.md) topic

## Why are some container types marked legacy?<a name="using-features.migration.why"></a>

Some older platform versions do not support the latest Elastic Beanstalk features\. These versions are marked **\(legacy\)** on the environment configuration page in the AWS Management Console\. <a name="using-features.migration-proc"></a>

**To check if you are using a legacy container type**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. From the Elastic Beanstalk console applications page, click the environment that you want to verify\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-app-page-env.png)

1. In the **Overview** section of the environment dashboard, view the **Configuration** name\.

   Your application is using a legacy container type if you see **\(legacy\)** next to the configuration\.

**To migrate your application**

1. Deploy your application to a new environment\. For instructions, go to [Creating an AWS Elastic Beanstalk Environment](using-features.environments.md)\.

1. If you have an Amazon RDS DB Instance, update your database security group to allow access to your EC2 security group for your new environment\. For instructions on how to find the name of your EC2 security group using the AWS Management Console, see [Security Groups](using-features.managing.ec2.md#using-features.managing.ec2.securitygroups)\. For more information about configuring your EC2 security group, go to the "Authorizing Network Access to an Amazon EC2 Security Group" section of [Working with DB Security Groups](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_WorkingWithSecurityGroups.html) in the *Amazon Relational Database Service User Guide*\.

1. Swap your environment URL\. For instructions, go to [Blue/Green Deployments with AWS Elastic Beanstalk](using-features.CNAMESwap.md)\.

1. Terminate your old environment\. For instructions, go to [Terminate an Elastic Beanstalk Environment](using-features.terminating.md)\.

**Note**  
If you use AWS Identity and Access Management \(IAM\) then you will need to update your policies to include AWS CloudFormation and Amazon RDS \(if applicable\)\. For more information, see [Using Elastic Beanstalk with AWS Identity and Access Management](AWSHowTo.iam.md)\.