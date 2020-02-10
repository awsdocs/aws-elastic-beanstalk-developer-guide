# Finding and tracking Elastic Beanstalk resources with AWS Config<a name="AWSHowTo.config"></a>

[AWS Config](https://aws.amazon.com/config/) provides a detailed view of the configuration of AWS resources in your AWS account\. You can see how resources are related, get a history of configuration changes, and see how relationships and configurations change over time\. You can use AWS Config to define rules that evaluate resource configurations for data compliance\.

Several Elastic Beanstalk resource types are integrated with AWS Config:
+ Applications
+ Application Versions
+ Environments

The following section shows how to configure AWS Config to record resources of these types\.

For more information about AWS Config, see the [AWS Config Developer Guide](https://docs.aws.amazon.com/config/latest/developerguide/)\. For pricing information, see the [AWS Config pricing information page](https://aws.amazon.com/config/pricing/)\.

## Setting up AWS Config<a name="AWSHowTo.config.setup"></a>

To initially set up AWS Config, see the following topics in the [AWS Config Developer Guide](https://docs.aws.amazon.com/config/latest/developerguide/)\.
+ [Setting up AWS Config with the Console](https://docs.aws.amazon.com/config/latest/developerguide/gs-console.html)
+ [Setting up AWS Config with the AWS CLI](https://docs.aws.amazon.com/config/latest/developerguide/gs-cli.html)

## Configuring AWS Config to record Elastic Beanstalk resources<a name="AWSHowTo.config.record"></a>

By default, AWS Config records configuration changes for all supported types of *regional resources* that it discovers in the region in which your environment is running\. You can customize AWS Config to record changes only for specific resource types, or changes to *global resources*\. For example, you can configure AWS Config to record changes for Elastic Beanstalk resources and a subset of other AWS resources that Elastic Beanstalk starts for you\.

The following figure shows the AWS Config **Settings** page, with Elastic Beanstalk resource types that you can choose to record: **Application**, **ApplicationVersion**, and **Environment**\.

![\[AWS Config settings page showing a list of resource types to choose to record\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/cc-settings-resource-types.png)

After you select a few resource types, this is how the **Specific types** list appears\.

![\[AWS Config settings page showing selected resource types to record\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/cc-settings-resource-types-selected.png)

To learn about *regional* vs\. *global* resources, and for the full customization procedure, see [Selecting which Resources AWS Config Records](https://docs.aws.amazon.com/config/latest/developerguide/select-resources.html)\.

## Viewing Elastic Beanstalk configuration details in the AWS Config console<a name="AWSHowTo.config.viewing"></a>

You can use the AWS Config console to look for Elastic Beanstalk resources, and get current and historical details about their configurations\. The following example shows how to find information about an Elastic Beanstalk environment\.

**To find an Elastic Beanstalk environment in the AWS Config console**

1. Open the [AWS Config console](https://console.aws.amazon.com/config)\.

1. Choose **Resources**\.

1. On the **Resource inventory** page, choose **Resources**\.

1. Open the **Resource type** menu, scroll to **ElasticBeanstalk**, and then choose one or more of the Elastic Beanstalk resource types\. See **1** in the following figure\.
**Note**  
To view configuration details for other resources that Elastic Beanstalk created for your application, choose additional resource types\. For example, you can choose **Instance** under **EC2**\.

1. Choose **Look up**\. See **2** in the following figure\.  
![\[AWS Config resource inventory page showing a list of resource types to look up\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/cc-resources-dropdown.png)

1. Choose a resource ID in the list of resources that AWS Config displays\.  
![\[AWS Config resource inventory page showing a list of resources\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/cc-resources-list.png)

AWS Config displays configuration details and other information about the resource you selected\.

![\[AWS Config resource details page showing configuration details for an Elastic Beanstalk environment\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/cc-resources-resource-details.png)

To see the full details of the recorded configuration, choose **View Details**\.

![\[AWS Config resource details page showing configuration details for an Elastic Beanstalk environment\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/cc-resources-view-details.png)

To learn more ways to find a resource and view information on this page, see [Viewing AWS Resource Configurations and History](https://docs.aws.amazon.com/config/latest/developerguide/view-manage-resource.html) in the *AWS Config Developer Guide*\.

## Evaluating Elastic Beanstalk resources using AWS Config rules<a name="AWSHowTo.config.rules"></a>

You can create AWS Config rules, which represent the ideal configuration settings for your Elastic Beanstalk resources\. You can use predefined *AWS Managed Config Rules*, or define custom rules\. AWS Config continuously tracks changes to the configuration of your resources to determine whether those changes violate any of the conditions in your rules\. The AWS Config console shows the compliance status of your rules and resources\.

If a resource violates a rule and is flagged as *noncompliant*, AWS Config can alert you using an [Amazon Simple Notification Service \(Amazon SNS\)](https://aws.amazon.com/sns/) topic\. To programmatically consume the data in these AWS Config alerts, use an [Amazon Simple Queue Service \(Amazon SQS\)](https://aws.amazon.com/sqs/) queue as the notification endpoint for the Amazon SNS topic\. For example, you might want to write code that starts a workflow when someone modifies your environment's Auto Scaling group configuration\.

To learn more about setting up and using rules, see [Evaluating Resources with AWS Config Rules](https://docs.aws.amazon.com/config/latest/developerguide/evaluate-config.html) in the *AWS Config Developer Guide*\.