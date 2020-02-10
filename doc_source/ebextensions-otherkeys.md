# Other AWS CloudFormation template keys<a name="ebextensions-otherkeys"></a>

We've already introduced configuration file keys from AWS CloudFormation such as `Resources`, `files`, and `packages`\. Elastic Beanstalk adds the contents of configurations files to the AWS CloudFormation template that supports your environment, so you can use other AWS CloudFormation sections to perform advanced tasks in your configuration files\.

**Topics**
+ [Parameters](#ebextensions-otherkeys-parameters)
+ [Outputs](#ebextensions-otherkeys-outputs)
+ [Mappings](#ebextensions-otherkeys-mappings)

## Parameters<a name="ebextensions-otherkeys-parameters"></a>

Parameters are an alternative to Elastic Beanstalk's own [custom options](configuration-options-custom.md) that you can use to define values that you use in other places in your configuration files\. Like custom options, you can use parameters to gather user configurable values in one place\. Unlike custom options, you can not use Elastic Beanstalk's API to set parameter values, and the number of parameters you can define in a template is limited by AWS CloudFormation\.

One reason you might want to use parameters is to make your configuration files double as AWS CloudFormation templates\. If you use parameters instead of custom options, you can use the configuration file to create the same resource in AWS CloudFormation as its own stack\. For example, you could have a configuration file that adds an Amazon EFS file system to your environment for testing, and then use the same file to create an independent file system that isn't tied to your environment's lifecycle for production use\.

The following example shows the use of parameters to gather user\-configurable values at the top of a configuration file\.

**Example [Loadbalancer\-accesslogs\-existingbucket\.config](https://github.com/awsdocs/elastic-beanstalk-samples/tree/master/configuration-files/aws-provided/resource-configuration/loadbalancer-accesslogs-existingbucket.config) – Parameters**  

```
Parameters:
  bucket:
    Type: String
    Description: "Name of the Amazon S3 bucket in which to store load balancer logs"
    Default: "my-bucket"
  bucketprefix:
    Type: String
    Description: "Optional prefix. Can't start or end with a /, or contain the word AWSLogs"
    Default: ""
```

## Outputs<a name="ebextensions-otherkeys-outputs"></a>

You can use an `Outputs` block to export information about created resources to AWS CloudFormation\. You can then use the `Fn::ImportValue` function to pull the value into a AWS CloudFormation template outside of Elastic Beanstalk\.

The following example creates an Amazon SNS topic and exports its ARN to AWS CloudFormation with the name `NotificationTopicArn`\.

**Example [sns\-topic\.config](https://github.com/awsdocs/elastic-beanstalk-samples/tree/master/configuration-files/aws-provided/resource-configuration/sns-topic.config)**  

```
Resources:
  NotificationTopic:
    Type: AWS::SNS::Topic

Outputs:
  NotificationTopicArn:
    Description: Notification topic ARN
    Value: { "Ref" : "NotificationTopic" }
    Export:
      Name: NotificationTopicArn
```

In a configuration file for a different environment, or a AWS CloudFormation template outside of Elastic Beanstalk, you can use the `Fn::ImportValue` function to get the exported ARN\. This example assigns the exported value to an environment property named `TOPIC_ARN`\.

**Example env\.config**  

```
option_settings:
  aws:elasticbeanstalk:application:environment:
    TOPIC_ARN: '`{ "Fn::ImportValue" : "NotificationTopicArn" }`'
```

## Mappings<a name="ebextensions-otherkeys-mappings"></a>

You can use a mapping to store key\-value pairs organized by namespace\. A mapping can help you organize values that you use throughout your configs, or change a parameter value depending on another value\. For example, the following configuration sets the value of an account ID parameter based on the current region\.

**Example [Loadbalancer\-accesslogs\-newbucket\.config](https://github.com/awsdocs/elastic-beanstalk-samples/tree/master/configuration-files/aws-provided/resource-configuration/loadbalancer-accesslogs-newbucket.config) – Mappings**  

```
Mappings: 
  Region2ELBAccountId: 
    us-east-1: 
      AccountId: "111122223333"
    us-west-2: 
      AccountId: "444455556666"
    us-west-1: 
      AccountId: "123456789012"
    eu-west-1: 
      AccountId: "777788889999"
...
            Principal: 
              AWS: 
                ? "Fn::FindInMap"
                : 
                  - Region2ELBAccountId
                  - 
                    Ref: "AWS::Region"
                  - AccountId
```