# Functions<a name="ebextensions-functions"></a>

You can use functions in your configuration files to populate values for resource properties with information from other resources or from Elastic Beanstalk configuration option settings\. Elastic Beanstalk supports AWS CloudFormation functions \(`Ref`, `Fn::GetAtt`, `Fn::Join`\), and one Elastic Beanstalk\-specific function, `Fn::GetOptionSetting`\.

**Topics**
+ [Ref](#ebextensions-functions-ref)
+ [Fn::GetAtt](#ebextensions-functions-getatt)
+ [Fn::Join](#ebextensions-functions-join)
+ [Fn::GetOptionSetting](#ebextensions-functions-getoptionsetting)

## Ref<a name="ebextensions-functions-ref"></a>

Use `Ref` to retrieve the default string representation of an AWS resource\. The value returned by `Ref` depends on the resource type, and sometimes depends on other factors as well\. For example, a security group \([AWS::EC2::SecurityGroup](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-ec2-security-group.html)\) returns either the name or ID of the security group, depending on if the security group is in a default [Amazon Virtual Private Cloud](https://docs.aws.amazon.com/vpc/latest/userguide/) \(Amazon VPC\), EC2 classic, or a custom VPC\.

```
{ "Ref" : "resource name" }
```

**Note**  
For details on each resource type, including the return value\(s\) of `Ref`, see [AWS Resource Types Reference](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html) in the *AWS CloudFormation User Guide*\.

From the sample [Auto Scaling lifecycle hook](environment-resources.md):

```
Resources:
  lifecyclehook:
    Type: AWS::AutoScaling::LifecycleHook
    Properties:
      AutoScalingGroupName: { "Ref" : "AWSEBAutoScalingGroup" }
```

You can also use `Ref` to retrieve the value of a AWS CloudFormation parameter defined elsewhere in the same file or in a different configuration file\.

## Fn::GetAtt<a name="ebextensions-functions-getatt"></a>

Use `Fn::GetAtt` to retrieve the value of an attribute on an AWS resource\.

```
{ "Fn::GetAtt" : [ "resource name", "attribute name"] }
```

From the sample [Auto Scaling lifecycle hook](environment-resources.md):

```
Resources:
  lifecyclehook:
    Type: AWS::AutoScaling::LifecycleHook
    Properties:
      RoleARN: { "Fn::GetAtt" : [ "hookrole", "Arn"] }
```

See [Fn::GetAtt](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-getatt.html) for more information\.

## Fn::Join<a name="ebextensions-functions-join"></a>

Use `Fn::Join` to combine strings with a delimiter\. The strings can be hard\-coded or use the output from `Fn::GetAtt` or `Ref`\.

```
{ "Fn::Join" : [ "delimiter", [ "string1", "string2" ] ] }
```

See [Fn::Join](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-join.html) for more information\.

## Fn::GetOptionSetting<a name="ebextensions-functions-getoptionsetting"></a>

Use `Fn::GetOptionSetting` to retrieve the value of a [configuration option](command-options.md) setting applied to the environment\. 

```
"Fn::GetOptionSetting":
  Namespace: "namespace"
  OptionName: "option name"
  DefaultValue: "default value"
```

From the [storing private keys](https-storingprivatekeys.md) example:

```
Resources:
  AWSEBAutoScalingGroup:
    Metadata:
      AWS::CloudFormation::Authentication:
        S3Auth:
          type: "s3"
          buckets: ["elasticbeanstalk-us-west-2-123456789012"]
          roleName: 
            "Fn::GetOptionSetting": 
              Namespace: "aws:autoscaling:launchconfiguration"
              OptionName: "IamInstanceProfile"
              DefaultValue: "aws-elasticbeanstalk-ec2-role"
```