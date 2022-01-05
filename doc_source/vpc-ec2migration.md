# Migrating Elastic Beanstalk environments from EC2\-Classic to a VPC<a name="vpc-ec2migration"></a>

This topic describes different options for how to migrate your Elastic Beanstalk environments from an EC2\-Classic network platform to an [Amazon Virtual Private Cloud](https://docs.aws.amazon.com/vpc/latest/userguide/) \(Amazon VPC\) network\. 

If you created your AWS account before December 4, 2013, you might have environments that use the EC2\-Classic network configuration in some AWS Regions\. All AWS accounts created on or after December 4, 2013 are already VPC\-only in every AWS Region\. The only exemptions are if Amazon EC2\-Classic was enabled as a result of a support request\.

**Note**  
You can view the network configuration settings for your environment in the **Network configuration** category on the [Configuration overview](environments-cfg-console.md#environments-cfg-console.overview) page of the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

## Why you should migrate<a name="vpc-ec2migration.benefits"></a>

Amazon EC2\-Classic will reach its end of standard support on August 15, 2022\. To avoid interruptions to your workloads, we recommend that you migrate from Amazon EC2\-Classic to a VPC before August 15, 2022\. We also request that you don't launch any AWS resources on Amazon EC2\-Classic in the future and use Amazon VPC instead\. 

When you migrate your Elastic Beanstalk environments from Amazon EC2\-Classic to Amazon VPC, you must create a new AWS account\. You must also re\-create your AWS EC2\-Classic environments in your new AWS account\. No additional configuration work for your environments is required to use the default VPC\. If the default VPC doesn't meet your requirements, manually create a custom VPC and associate it with your environments\.



Alternatively, if your existing AWS account has resources that you can't migrate to a new AWS account, add a VPC into your current account\. Then, configure your environments to use the VPC\.

For more information, see the [EC2\-Classic Networking is Retiring \- Here's How to Prepare](http://aws.amazon.com/blogs/aws/ec2-classic-is-retiring-heres-how-to-prepare/) blog post\. 

## Migrate an environment from EC2\-Classic into a new AWS account \(recommended\)<a name="vpc-ec2migration.newaccount"></a>

If you don't already have an AWS account that was created on or after December 4, 2013, create a new account\. You will migrate your environments into this new account\. 

1. Your new AWS account provides a default VPC to its environments\. If you don't need to create a custom VPC, skip to step 2\.

   You can create a custom VPC in one of the following ways:
   + Create a VPC quickly using the Amazon VPC console wizard with one of the available configuration options\. For more information, see [Amazon VPC console wizard configurations](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_wizard.html)\. 
   + Create a custom VPC on the Amazon VPC console if you have more specific requirements for your VPC\. We recommend you do this, for example, if your use case requires a specific number of subnets\. For more information, see [VPCs and subnets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html)\.
   + Create a VPC using the [elastic\-beanstalk\-samples](https://github.com/awsdocs/elastic-beanstalk-samples/) repository on the GitHub website if you prefer to use AWS CloudFormation templates with your Elastic Beanstalk environments\. This repository includes AWS CloudFormation templates\. For more information, see [Using Elastic Beanstalk with Amazon VPC](vpc.md)\.
**Note**  
You can also create a custom VPC at the same time you recreate the environment in your new AWS account using the [create new environment wizard](environments-create-wizard.md)\. If you use the wizard and choose to create a custom VPC, the wizard redirects you to the Amazon VPC console\.

1. In your new AWS account, create a new environment\. We recommend that the environment includes the same configuration as your existing environment in the AWS account that you're migrating from\. You can do this by using one of the following approaches\. 
**Note**  
If your new environment must use the same CNAME after you migrate, terminate the original environment on the EC2\-Classic platform\. This releases the CNAME for use\. However, doing so can result in downtime for that environment and can also risk that another customer might select your CNAME between you terminating your EC2\-Classic environment and creating the new one\. For more information, see [Terminate an Elastic Beanstalk environment](using-features.terminating.md)\.  
For environments that have their own proprietary domain name, the CNAME doesn't have this issue\. You can just update your Domain Name System \(DNS\) to forward requests to your new CNAME\.
   +  Use the [create new environment wizard](environments-create-wizard.md) on the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\. The wizard provides an option to create a custom VPC\. If you don't choose to create a custom VPC, a default VPC is assigned\. 
   + Use the Elastic Beanstalk Command Line Interface \(EB CLI\) to re\-create your environment in your new AWS account\. One of the [examples](eb3-create.md#eb3-createexample1) in the eb create command description demonstrates the creation of an environment in a custom VPC\. If you don't provide a VPC ID, the environment uses the default VPC\.

     By using this approach, you can use a saved configurations file across the two AWS accounts\. As a result, you don't need to manually enter all the configuration information\. However, you must save the configuration settings for the EC2\-Classic environment that you're migrating with the [eb config save](eb3-config.md) command\. Copy the saved configuration file to a new directory for the new account environment\.
**Note**  
You must edit some of the data in the saved configuration file before you can use it in the new account\. You must also update information that pertains to your previous account with the correct data for your new account\. For example, you must replace the Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role with the IAM role ARN for the new account\. 

     If you use the [eb create](eb3-create.md) command with the `cfg`, the new environment is created using the specified saved configuration file\. For more information, see [Using Elastic Beanstalk saved configurations](environment-configuration-savedconfig.md)\. 

## Migrate an environment from EC2\-Classic within your same AWS account<a name="vpc-ec2migration.classicaccount"></a>

Your existing AWS account might have resources that you can't migrate to a new AWS account\. In this case you must re\-create your environments and manually configure a VPC for every environment you create\.

### Migrate your environments to a custom VPC<a name="vpc-ec2migration.classicaccount.expandable"></a>

**Prerequisites**  
Before you begin, you must have a VPC\. You can create a non\-default \(custom\) VPC in one of the following ways:
+ Create a VPC quickly using the Amazon VPC console wizard with one of the available configuration options\. For more information, see [Amazon VPC console wizard configurations](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_wizard.html)\. 
+ Create a custom VPC on the Amazon VPC console if you have more specific requirements for your VPC\. We recommend you do this, for example, if your use case requires a specific number of subnets\. For more information, see [VPCs and subnets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html)\.
+ Create a VPC using the [elastic\-beanstalk\-samples](https://github.com/awsdocs/elastic-beanstalk-samples/) repository on the GitHub website if you prefer to use AWS CloudFormation templates with your Elastic Beanstalk environments\. This repository includes AWS CloudFormation templates\. For more information, see [Using Elastic Beanstalk with Amazon VPC](vpc.md)\.

In the following steps, you use the generated VPC ID and subnet IDs when you configure the VPC in the new environment\.

1. Create a new environment that includes the same configuration as your existing environment\. You can do this by using one of the following approaches\. 
**Note**  
The Saved Configurations feature can help you re\-create your environments in the new account\. This feature can save an environment’s configuration, so you can apply it when you create or update other environments\. For more information, see [Using Elastic Beanstalk saved configurations](environment-configuration-savedconfig.md)\. 
   + Using the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), apply a saved configuration from your EC2\-Classic environment when you configure the new environment\. This configuration will use the VPC\. For more information, see [Using Elastic Beanstalk saved configurations](environment-configuration-savedconfig.md)\.
   + Using Elastic Beanstalk Command Line Interface \(EB CLI\), run the [eb create](eb3-create.md) command to re\-create your environment\. Provide the parameters of your original environment and the VPC identifier\. One of the [examples](eb3-create.md#eb3-createexample1) in the eb create command description shows how to create an environment in a custom VPC\. 
   + Use the AWS Command Line Interface \(AWS CLI\), and re\-create your environment using the elasticbeanstalk create\-environment command\. Provide the parameters of your original environment with the VPC identifier\. For instructions, see [Creating Elastic Beanstalk environments with the AWS CLI](environments-create-awscli.md)\. 

1. Swap the CNAMEs of the existing environment with the new environment\. This way, the new environment that you created can be referenced with the familiar address\. You can use the EB CLI or the AWS CLI\. 
   + Using the EB CLI, swap the environment CNAMEs by running the eb swap command\. For more information, see [Using the Elastic Beanstalk command line interface \(EB CLI\)](eb-cli3.md)\. 
   + Using the AWS CLI, swap the environment CNAMEs with the [ elasticbeanstalk swap\-environment\-cnames ](https://docs.aws.amazon.com/cli/latest/reference/elasticbeanstalk/swap-environment-cnames.html) command\. For more information, see the [AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/)\. 