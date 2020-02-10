# Adding and customizing Elastic Beanstalk environment resources<a name="environment-resources"></a>

You may also want to customize your environment resources that are part of your Elastic Beanstalk environment\. For example, you may want to add an Amazon SQS queue and an alarm on queue depth, or you might want to add an Amazon ElastiCache cluster\. You can easily customize your environment at the same time that you deploy your application version by including a configuration file with your source bundle\.

You can use the `Resources` key in a [configuration file](ebextensions.md) to create and customize AWS resources in your environment\. Resources defined in configuration files are added to the AWS CloudFormation template used to launch your environment\. All AWS CloudFormation [resources types](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html) are supported\.

For example, the following configuration file adds an Auto Scaling lifecycle hook to the default Auto Scaling group created by Elastic Beanstalk:

**`~/my-app/.ebextensions/as-hook.config`**

```
Resources:
  hookrole:
    Type: AWS::IAM::Role
    Properties:
      AssumeRolePolicyDocument: {
               "Version" : "2012-10-17",
               "Statement": [ {
                  "Effect": "Allow",
                  "Principal": {
                     "Service": [ "autoscaling.amazonaws.com" ]
                  },
                  "Action": [ "sts:AssumeRole" ]
               } ]
            }
      Policies: [ {
               "PolicyName": "SNS",
               "PolicyDocument": {
                      "Version": "2012-10-17",
                      "Statement": [{
                          "Effect": "Allow",
                          "Resource": "*",
                          "Action": [
                              "sqs:SendMessage",
                              "sqs:GetQueueUrl",
                              "sns:Publish"
                          ]
                        }
                      ]
                  }
               } ]
  hooktopic:
    Type: AWS::SNS::Topic
    Properties:
      Subscription:
        - Endpoint: "my-email@example.com"
          Protocol: email
  lifecyclehook:
    Type: AWS::AutoScaling::LifecycleHook
    Properties:
      AutoScalingGroupName: { "Ref" : "AWSEBAutoScalingGroup" }
      LifecycleTransition: autoscaling:EC2_INSTANCE_TERMINATING
      NotificationTargetARN: { "Ref" : "hooktopic" }
      RoleARN: { "Fn::GetAtt" : [ "hookrole", "Arn"] }
```

This example defines three resources, `hookrole`, `hooktopic` and `lifecyclehook`\. The first two resources are an IAM role, which grants Amazon EC2 Auto Scaling permission to publish messages to Amazon SNS, and an SNS topic, which relays messages from the Auto Scaling group to an email address\. Elastic Beanstalk creates these resources with the specified properties and types\.

The final resource, `lifecyclehook`, is the lifecycle hook itself:

```
  lifecyclehook:
    Type: AWS::AutoScaling::LifecycleHook
    Properties:
      AutoScalingGroupName: { "Ref" : "AWSEBAutoScalingGroup" }
      LifecycleTransition: autoscaling:EC2_INSTANCE_TERMINATING
      NotificationTargetARN: { "Ref" : "hooktopic" }
      RoleARN: { "Fn::GetAtt" : [ "hookrole", "Arn"] }
```

The lifecycle hook definition uses two [functions](ebextensions-functions.md) to populate values for the hook's properties\. `{ "Ref" : "AWSEBAutoScalingGroup" }` retrieves the name of the Auto Scaling group created by Elastic Beanstalk for the environment\. `AWSEBAutoScalingGroup` is one of the standard [resource names](customize-containers-format-resources-eb.md) provided by Elastic Beanstalk\.

For `[AWS::IAM::Role](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-iam-role.html#d0e48356)`, `Ref` only returns the name of the role, not the ARN\. To get the ARN for the `RoleARN` parameter, you use another intrinsic function, `Fn::GetAtt` instead, which can get any attribute from a resource\. `RoleARN: { "Fn::GetAtt" : [ "hookrole", "Arn"] }` gets the `Arn` attribute from the `hookrole` resource\.

`{ "Ref" : "hooktopic" }` gets the ARN of the Amazon SNS topic created earlier in the configuration file\. The value returned by `Ref` varies per resource type and can be found in the AWS CloudFormation User Guide [topic for the AWS::SNS::Topic resource type](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-sns-topic.html#d0e62250)\.