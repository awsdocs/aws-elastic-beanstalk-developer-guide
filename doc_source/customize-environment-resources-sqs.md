# Example: SQS, CloudWatch, and SNS<a name="customize-environment-resources-sqs"></a>

This example adds an Amazon SQS queue and an alarm on queue depth to the environment\. The properties that you see in this example are the minimum required properties that you must set for each of these resources\. You can download the example at [SQS, SNS, and CloudWatch](https://elasticbeanstalk.s3.amazonaws.com/extensions/SNS.config)\. 

**Note**  
This example creates AWS resources, which you might be charged for\. For more information about AWS pricing, see [https://aws.amazon.com/pricing/](https://aws.amazon.com/pricing/)\. Some services are part of the AWS Free Usage Tier\. If you are a new customer, you can test drive these services for free\. See [https://aws.amazon.com/free/](https://aws.amazon.com/free/) for more information\.

To use this example, do the following:

1. Create an `[\.ebextensions](ebextensions.md)` directory in the top\-level directory of your source bundle\. 

1. Create two configuration files with the `.config` extension and place them in your `.ebextensions` directory\. One configuration file defines the resources, and the other configuration file defines the options\.

1. Deploy your application to Elastic Beanstalk\.

   YAML relies on consistent indentation\. Match the indentation level when replacing content in an example configuration file and ensure that your text editor uses spaces, not tab characters, to indent\.

Create a configuration file \(e\.g\., sqs\.config\) that defines the resources\. In this example, we create an SQS queue and define the `VisbilityTimeout` property in the `MySQSQueue` resource\. Next, we create an SNS `Topic` and specify that email gets sent to `someone@example.com` when the alarm is fired\. Finally, we create a CloudWatch alarm if the queue grows beyond 10 messages\. In the `Dimensions` property, we specify the name of the dimension and the value representing the dimension measurement\. We use `Fn::GetAtt` to return the value of `QueueName` from `MySQSQueue`\.

```
#This sample requires you to create a separate configuration file to define the custom options for the SNS topic and SQS queue.
Resources:
  MySQSQueue:
    Type: AWS::SQS::Queue
    Properties: 
      VisibilityTimeout:
        Fn::GetOptionSetting:
          OptionName: VisibilityTimeout
          DefaultValue: 30
  AlarmTopic:
    Type: AWS::SNS::Topic
    Properties: 
      Subscription:
        - Endpoint:
            Fn::GetOptionSetting:
              OptionName: AlarmEmail
              DefaultValue: "nobody@amazon.com"
          Protocol: email
  QueueDepthAlarm:
    Type: AWS::CloudWatch::Alarm
    Properties:
      AlarmDescription: "Alarm if queue depth grows beyond 10 messages"
      Namespace: "AWS/SQS"
      MetricName: ApproximateNumberOfMessagesVisible
      Dimensions:
        - Name: QueueName
          Value : { "Fn::GetAtt" : [ "MySQSQueue", "QueueName"] }
      Statistic: Sum
      Period: 300
      EvaluationPeriods: 1
      Threshold: 10
      ComparisonOperator: GreaterThanThreshold
      AlarmActions:
        - Ref: AlarmTopic
      InsufficientDataActions:
        - Ref: AlarmTopic

Outputs :
  QueueURL: 
    Description : "URL of newly created SQS Queue"
    Value : { Ref : "MySQSQueue" }
  QueueARN :
    Description : "ARN of newly created SQS Queue"
    Value : { "Fn::GetAtt" : [ "MySQSQueue", "Arn"]}
  QueueName :
    Description : "Name newly created SQS Queue"
    Value : { "Fn::GetAtt" : [ "MySQSQueue", "QueueName"]}
```

For more information about the resources used in this example configuration file, see the following references: 
+ [AWS::SQS::Queue](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-sqs-queues.html)
+ [AWS::SNS::Topic](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-sns-topic.html)
+ [AWS::CloudWatch::Alarm](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-cw-alarm.html)

Create a separate configuration file called `options.config` and define the custom option settings\.

```
option_settings:
  "aws:elasticbeanstalk:customoption":
     VisibilityTimeout : 30
     AlarmEmail : "nobody@example.com"
```

These lines tell Elastic Beanstalk to get the values for the **VisibilityTimeout and Subscription Endpoint** properties from the **VisibilityTimeout and Subscription Endpoint** values in a config file \(options\.config in our example\) that contains an option\_settings section with an **aws:elasticbeanstalk:customoption** section that contains a name\-value pair that contains the actual value to use\. In the example above, this means 30 and "nobody@amazon\.com" would be used for the values\. For more information about `Fn::GetOptionSetting`, see [Functions](ebextensions-functions.md)\.