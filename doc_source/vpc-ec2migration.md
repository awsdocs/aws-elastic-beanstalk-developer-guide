# Migrating Elastic Beanstalk environments from EC2\-Classic to a VPC<a name="vpc-ec2migration"></a>

This topic describes options for migrating your Elastic Beanstalk environments from an EC2\-Classic network platform to an [Amazon Virtual Private Cloud](https://docs.aws.amazon.com/vpc/latest/userguide/) \(Amazon VPC\) network\. 

If you created your AWS account before December 4, 2013, you might have environments using the EC2\-Classic network configuration in some AWS Regions\.

**Note**  
You can view your environment's network configuration settings in the **Network configuration** category on the [Configuration overview](environments-cfg-console.md#environments-cfg-console.overview) page of the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

## Why you should migrate<a name="vpc-ec2migration.benefits"></a>

In most cases, you should migrate your AWS account on the EC2\-Classic platform to a new AWS account that supports VPCs\. All AWS accounts created on or after December 4, 2013 have a default VPC in every AWS Region\. You will benefit from access to the most recent AWS functionality and supported features\. 

This involves creating a new AWS account and re\-creating your AWS EC2\-Classic environments in your new AWS account\. You don't have to do any additional configuration work for your environments to use the default VPC\. If the default VPC does not meet your requirements, you can manually create a custom VPC and associate it with your environments\.

Alternatively, if your existing AWS account has resources that you can't migrate to a new AWS account, you can add a VPC into your current account and configure your environments to use it\.

## Migrate an environment from EC2\-Classic into a new AWS account \(recommended\)<a name="vpc-ec2migration.newaccount"></a>

If you don't already have an AWS account that was created on or after December 4, 2013, you need to create a new one first\. You will be migrating your environments into this new account\. 

1. Your new AWS account provides a default VPC to its environments\. If you don't need to create a custom VPC, skip to step 2\.

   You can create a custom VPC in one of the following ways:
   + The Amazon VPC console wizard enables you to set up a VPC quickly, using one of the available configuration options\. For more information, see [Amazon VPC console wizard configurations](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_wizard.html)\. 
   + If you have more specific requirements for your VPC, such as a particular number of subnets, you can also use the Amazon VPC console\. For more information, see [VPCs and subnets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html)\.
   + If you prefer to use AWS CloudFormation templates with your Elastic Beanstalk environments, the [elastic\-beanstalk\-samples](https://github.com/awsdocs/elastic-beanstalk-samples/) repository on the GitHub website provides AWS CloudFormation templates that you can use to create a VPC\. For more information, see [Using Elastic Beanstalk with Amazon VPC](vpc.md)\.
**Note**  
You can also create a custom VPC at the same time you re\-create the environment in your new AWS account using the [create new environment wizard](environments-create-wizard.md)\. The wizard will give you the option to create a custom VPC and will navigate you to the Amazon VPC console if you select to do so\.

1. In your new AWS account create a new environment that includes the same configuration as your existing environment in the AWS account that you're migrating from\. You can do this by using one of the following approaches\. 
**Note**  
If your new environment must continue using the same CNAME after you migrate, you must first terminate the original environment on the EC2\-Classic platform to release the CNAME for use\. This will result in downtime for that environment and the risk that another customer might select your CNAME in the time between the termination of your EC2\-Classic environment and the creation of the new one\. For more information, see [Terminate an Elastic Beanstalk environment](using-features.terminating.md)\.  
For environments that have their own proprietary domain name, the CNAME doesn't have this issue\. You can just update your Domain Name System \(DNS\) to forward requests to your new CNAME\.
   +  Use the [create new environment wizard](environments-create-wizard.md) on the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\. The wizard provides an option to create a custom VPC\. If you don't choose to create a custom VPC, a default VPC will be assigned\. 
   + Use the Elastic Beanstalk Command Line Interface \(EB CLI\) to re\-create your environment in your new AWS account\. One of the [examples](eb3-create.md#eb3-createexample1) in the eb create command description demonstrates the creation of an environment in a custom VPC\. If you don't provide a VPC ID, the environment will use the default VPC\.

      This approach offers you the option to use a saved configurations file across the two AWS accounts, so you don't have to enter all the configuration information manually\. You will need to save the configuration settings for the EC2\-Classic environment you are migrating with the [eb config save](eb3-config.md) command\. Copy the saved configuration file to a new directory for the new account environment\.
**Note**  
You need to edit some of the data in the saved configuration file before you can use it in the new account\. You need to update information that pertains to your old account with the correct data for your new account\. For example, you must replace the Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role with the IAM role ARN for the new account\. 

     The [eb create](eb3-create.md) command with the `cfg` option will create the new environment using the specified saved configuration file\. For more information, see [Using Elastic Beanstalk saved configurations](environment-configuration-savedconfig.md)\. 

## Migrate an environment from EC2\-Classic within your same AWS account<a name="vpc-ec2migration.classicaccount"></a>

Your existing AWS account might have resources that you can't migrate to a new AWS account\. You will need to re\-create your environments and manually configure a VPC for every environment you create\.

### Migrate your environments to a custom VPC<a name="vpc-ec2migration.classicaccount.expandable"></a>

**Prerequisites**  
Before you begin, you must have a VPC\. You can create a non\-default \(custom\) VPC in one of the following ways:
+ The Amazon VPC console wizard enables you to set up a VPC quickly, using one of the available configuration options\. For more information, see [Amazon VPC console wizard configurations](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_wizard.html)\. 
+ If you have more specific requirements for your VPC, such as a particular number of subnets, you can also use the Amazon VPC console\. For more information, see [VPCs and subnets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html)\.
+ If you prefer to use AWS CloudFormation templates with your Elastic Beanstalk environments, the [elastic\-beanstalk\-samples](https://github.com/awsdocs/elastic-beanstalk-samples/) repository on the GitHub website provides AWS CloudFormation templates that you can use to create a VPC\. For more information, see [Using Elastic Beanstalk with Amazon VPC](vpc.md)\.

In the following steps, you use the generated VPC ID and subnet IDs when you configure the VPC in the new environment\.

1. Create a new environment that includes the same configuration as your existing environment\. You can do this by using one of the following approaches\. 
**Note**  
The Saved Configurations feature can help you re\-create your environments in the new account\. For more information, see [Using Elastic Beanstalk saved configurations](environment-configuration-savedconfig.md)\. 
   + Using the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), apply a saved configuration from your EC2\-Classic environment during the new environment's that will use the VPC\. For more information, see [Using Elastic Beanstalk saved configurations](environment-configuration-savedconfig.md)\.
   + Using Elastic Beanstalk Command Line Interface \(EB CLI\), run the [eb create](eb3-create.md) command to re\-create your environment\. Provide the parameters of your original environment and the VPC identifier\. One of the [examples](eb3-create.md#eb3-createexample1) in the eb create command description demonstrates the creation of an environment in a custom VPC\. 
   + Use the AWS Command Line Interface \(AWS CLI\)\. Re\-create your environment using the elasticbeanstalk create\-environment command\. Provide the parameters of your original environment with the VPC identifier\. For instructions, see [Creating Elastic Beanstalk environments with the AWS CLI](environments-create-awscli.md)\. 

1. Swap the CNAMEs of the old environment and its corresponding new environment\. This enables your newly created environment to be referenced with the familiar address\. You can use the EB CLI or the AWS CLI\. 
   +  Using the EB CLI, swap the environment CNAMEs by using the eb swap command\. For more information, see [Using the Elastic Beanstalk command line interface \(EB CLI\)](eb-cli3.md)\. 
   +  Using the AWS CLI, swap the environment CNAMEs with the [ elasticbeanstalk swap\-environment\-cnames ](https://docs.aws.amazon.com/cli/latest/reference/elasticbeanstalk/swap-environment-cnames.html) command\. For more information, see the [AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/)\. 