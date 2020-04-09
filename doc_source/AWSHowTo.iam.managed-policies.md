# Managing Elastic Beanstalk user policies<a name="AWSHowTo.iam.managed-policies"></a>

AWS Elastic Beanstalk provides two managed policies that enable you to assign full access or read\-only access to all Elastic Beanstalk resources\. You can attach the policies to AWS Identity and Access Management \(IAM\) users or groups\.

**Managed user policies**
+ **AWSElasticBeanstalkFullAccess** – Allows the user to create, modify, and delete Elastic Beanstalk applications, application versions, configuration settings, environments, and their underlying resources\.

  ```
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action": [
          "elasticbeanstalk:*",
          "ec2:*",
          "ecs:*",
          "ecr:*",
          "elasticloadbalancing:*",
          "autoscaling:*",
          "cloudwatch:*",
          "s3:*",
          "sns:*",
          "cloudformation:*",
          "dynamodb:*",
          "rds:*",
          "sqs:*",
          "logs:*",
          "iam:GetPolicyVersion",
          "iam:GetRole",
          "iam:PassRole",
          "iam:ListRolePolicies",
          "iam:ListAttachedRolePolicies",
          "iam:ListInstanceProfiles",
          "iam:ListRoles",
          "iam:ListServerCertificates",
          "acm:DescribeCertificate",
          "acm:ListCertificates",
          "codebuild:CreateProject",
          "codebuild:DeleteProject",
          "codebuild:BatchGetBuilds",
          "codebuild:StartBuild"
        ],
        "Resource": "*"
      },
      {
        "Effect": "Allow",
        "Action": [
          "iam:AddRoleToInstanceProfile",
          "iam:CreateInstanceProfile",
          "iam:CreateRole"
        ],
        "Resource": [
          "arn:aws:iam::*:role/aws-elasticbeanstalk*",
          "arn:aws:iam::*:instance-profile/aws-elasticbeanstalk*"
        ]
      },
      {
        "Effect": "Allow",
        "Action": [
          "iam:CreateServiceLinkedRole"
        ],
        "Resource": [
          "arn:aws:iam::*:role/aws-service-role/autoscaling.amazonaws.com/AWSServiceRoleForAutoScaling*"
        ],
        "Condition": {
          "StringLike": {
            "iam:AWSServiceName": "autoscaling.amazonaws.com"
          }
        }
      },
      {
        "Effect": "Allow",
        "Action": [
          "iam:CreateServiceLinkedRole"
        ],
        "Resource": [
          "arn:aws:iam::*:role/aws-service-role/elasticbeanstalk.amazonaws.com/AWSServiceRoleForElasticBeanstalk*"
        ],
        "Condition": {
          "StringLike": {
            "iam:AWSServiceName": "elasticbeanstalk.amazonaws.com"
          }
        }
      },
      {
        "Effect": "Allow",
        "Action": [
          "iam:AttachRolePolicy"
        ],
        "Resource": "*",
        "Condition": {
          "StringLike": {
            "iam:PolicyArn": [
              "arn:aws:iam::aws:policy/AWSElasticBeanstalk*",
              "arn:aws:iam::aws:policy/service-role/AWSElasticBeanstalk*"
            ]
          }
        }
      }
    ]
  }
  ```
+ **AWSElasticBeanstalkReadOnlyAccess** – Allows the user to view applications and environments, but not to perform operations on them\. It provides read\-only access to all Elastic Beanstalk resources\. Note that read\-only access does not enable actions such as downloading Elastic Beanstalk logs so that you can read them\. This is because the logs are staged in the Amazon S3 bucket, where Elastic Beanstalk would require write permission\. See the example at the end of this topic for information on how to enable access to Elastic Beanstalk logs\.

  ```
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action": [
          "elasticbeanstalk:Check*",
          "elasticbeanstalk:Describe*",
          "elasticbeanstalk:List*",
          "elasticbeanstalk:RequestEnvironmentInfo",
          "elasticbeanstalk:RetrieveEnvironmentInfo",
          "ec2:Describe*",
          "elasticloadbalancing:Describe*",
          "autoscaling:Describe*",
          "cloudwatch:Describe*",
          "cloudwatch:List*",
          "cloudwatch:Get*",
          "s3:Get*",
          "s3:List*",
          "sns:Get*",
          "sns:List*",
          "cloudformation:Describe*",
          "cloudformation:Get*",
          "cloudformation:List*",
          "cloudformation:Validate*",
          "cloudformation:Estimate*",
          "rds:Describe*",
          "sqs:Get*",
          "sqs:List*"
        ],
        "Resource": "*"
      }
    ]
  }
  ```

## Controlling access with managed policies<a name="iam-userpolicies-managed"></a>

You can use managed policies to grant full access or read\-only access to Elastic Beanstalk\. Elastic Beanstalk updates these policies automatically when additional permissions are required to access new features\.

**To apply a managed policy to IAM users or groups**

1. Open the [**Policies** page](https://console.aws.amazon.com/iam/home#policies) in the IAM console\.

1. In the search box, type **AWSElasticBeanstalk** to filter the policies\.

1. In the list of policies, select the check box next to **AWSElasticBeanstalkReadOnlyAccess** or **AWSElasticBeanstalkFullAccess**\.

1. Choose **Policy actions**, and then choose **Attach**\.

1. Select one or more users and groups to attach the policy to\. You can use the **Filter** menu and the search box to filter the list of principal entities\.

1. Choose **Attach policy**\.

## Creating a custom user policy<a name="AWSHowTo.iam.policies"></a>

You can create your own IAM policy to allow or deny specific Elastic Beanstalk API actions on specific Elastic Beanstalk resources\. For more information about attaching a policy to a user or group, see [Working with Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/ManagingPolicies.html) in *Using AWS Identity and Access Management*\.

**Note**  
While you can restrict how a user interacts with Elastic Beanstalk APIs, there is not currently an effective way to prevent users who have permission to create the necessary underlying resources from creating other resources in Amazon EC2 and other services\.  
Think of these policies as an effective way to distribute Elastic Beanstalk responsibilities, not as a way to secure all underlying resources\.

In November 2019, Elastic Beanstalk released support for [Amazon EC2 launch templates](https://docs.aws.amazon.com/elasticbeanstalk/latest/relnotes/release-2019-11-25-launchtemplates.html)\. This is a new resource type that your environment's Auto Scaling group can use to launch Amazon EC2 instances, and it requires new permissions\. Most customers shouldn't be affected, because environments can still use the legacy resource, launch configurations, if your user policy lacks the required permissions\. However, if you're trying to use a new feature that requires Amazon EC2 launch templates, and you have a custom policy, your environment creation or update might fail\. In this case, ensure that your custom policy has the following permissions\.

**Required permissions for Amazon EC2 launch templates**
+ `EC2:CreateLauchTemplate`
+ `EC2:CreateLauchTemplateVersions`
+ `EC2:DeleteLaunchTemplate`
+ `EC2:DeleteLaunchTemplateVersions`
+ `EC2:DescribeLaunchTemplate`
+ `EC2:DescribeLaunchTemplateVersions`

An IAM policy contains policy statements that describe the permissions that you want to grant\. When you create a policy statement for Elastic Beanstalk, you need to understand how to use the following four parts of a policy statement:
+ **Effect** specifies whether to allow or deny the actions in the statement\.
+ **Action** specifies the [API operations](https://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_Operations.html) that you want to control\. For example, use `elasticbeanstalk:CreateEnvironment` to specify the `CreateEnvironment` operation\. Certain operations, such as creating an environment, require additional permissions to perform those actions\. For more information, see [Resources and conditions for Elastic Beanstalk actions](AWSHowTo.iam.policies.actions.md)\. 
**Note**  
To use the [https://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_UpdateTagsForResource.html](https://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_UpdateTagsForResource.html) API operation, specify one of the following two virtual actions \(or both\) instead of the API operation name:  

`elasticbeanstalk:AddTags`  
Controls permission to call `UpdateTagsForResource` and pass a list of tags to add in the `TagsToAdd` parameter\.

`elasticbeanstalk:RemoveTags`  
Controls permission to call `UpdateTagsForResource` and pass a list of tag keys to remove in the `TagsToRemove` parameter\.
+ **Resource** specifies the resources that you want to control access to\. To specify Elastic Beanstalk resources, list the [Amazon Resource Name](AWSHowTo.iam.policies.arn.md) \(ARN\) of each resource\.
+ \(optional\) **Condition** specifies restrictions on the permission granted in the statement\. For more information, see [Resources and conditions for Elastic Beanstalk actions](AWSHowTo.iam.policies.actions.md)\.

The following sections demonstrate a few cases in which you might consider a custom user policy\.

### Enabling limited Elastic Beanstalk environment creation<a name="AWSHowTo.iam.policy.env-creation"></a>

The policy in the following example enables a user to call the `CreateEnvironment` action to create an environment whose name begins with **Test** with the specified application and application version\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid":"CreateEnvironmentPerm",
      "Action": [
        "elasticbeanstalk:CreateEnvironment"
      ],
      "Effect": "Allow",
      "Resource": [
        "arn:aws:elasticbeanstalk:us-east-2:123456789012:environment/My First Elastic Beanstalk Application/Test*"
      ],
      "Condition": {
        "StringEquals": {
          "elasticbeanstalk:InApplication": ["arn:aws:elasticbeanstalk:us-east-2:123456789012:application/My First Elastic Beanstalk Application"],
          "elasticbeanstalk:FromApplicationVersion": ["arn:aws:elasticbeanstalk:us-east-2:123456789012:applicationversion/My First Elastic Beanstalk Application/First Release"]
        }
      }
    },
    {
      "Sid":"AllNonResourceCalls",
      "Action":[
        "elasticbeanstalk:CheckDNSAvailability",
        "elasticbeanstalk:CreateStorageLocation"
      ],
      "Effect":"Allow",
      "Resource":[
        "*"
      ]
    }
  ]
}
```

The above policy shows how to grant limited access to Elastic Beanstalk operations\. In order to actually launch an environment, the user must have permission to create the AWS resources that power the environment as well\. For example, the following policy grants access to the default set of resources for a web server environment:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:*",
        "ecs:*",
        "elasticloadbalancing:*",
        "autoscaling:*",
        "cloudwatch:*",
        "s3:*",
        "sns:*",
        "cloudformation:*",
        "sqs:*"
        ],
      "Resource": "*"
    }
  ]
}
```

### Enabling access to Elastic Beanstalk logs stored in Amazon S3<a name="AWSHowTo.iam.policy.view-s3-logs"></a>

The policy in the following example enables a user to pull Elastic Beanstalk logs, stage them in Amazon S3, and retrieve them\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "s3:DeleteObject",
        "s3:GetObjectAcl",
        "s3:PutObjectAcl"
      ],
      "Effect": "Allow",
      "Resource": "arn:aws:s3:::elasticbeanstalk-*"
    }
  ]
}
```

**Note**  
To restrict these permissions to only the logs path, use the following resource format\.   

```
"arn:aws:s3:::elasticbeanstalk-us-east-2-123456789012/resources/environments/logs/*"
```

### Enabling management of a specific Elastic Beanstalk application<a name="AWSHowTo.iam.policy.manage-app"></a>

The policy in the following example enables a user to manage environments and other resources within one specific Elastic Beanstalk application\. The policy denies Elastic Beanstalk actions on resources of other applications, and also denies creation and deletion of Elastic Beanstalk applications\.

**Note**  
The policy doesn't deny access to any resources through other services\. It demonstrates an effective way to distribute responsibilities for managing Elastic Beanstalk applications among different users, not as a way to secure the underlying resources\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": [
        "elasticbeanstalk:CreateApplication",
        "elasticbeanstalk:DeleteApplication"
      ],
      "Resource": [
        "*"
      ]
    },
    {
      "Effect": "Deny",
      "Action": [
        "elasticbeanstalk:CreateApplicationVersion",
        "elasticbeanstalk:CreateConfigurationTemplate",
        "elasticbeanstalk:CreateEnvironment",
        "elasticbeanstalk:DeleteApplicationVersion",
        "elasticbeanstalk:DeleteConfigurationTemplate",
        "elasticbeanstalk:DeleteEnvironmentConfiguration",
        "elasticbeanstalk:DescribeApplicationVersions",
        "elasticbeanstalk:DescribeConfigurationOptions",
        "elasticbeanstalk:DescribeConfigurationSettings",
        "elasticbeanstalk:DescribeEnvironmentResources",
        "elasticbeanstalk:DescribeEnvironments",
        "elasticbeanstalk:DescribeEvents",
        "elasticbeanstalk:DeleteEnvironmentConfiguration",
        "elasticbeanstalk:RebuildEnvironment",
        "elasticbeanstalk:RequestEnvironmentInfo",
        "elasticbeanstalk:RestartAppServer",
        "elasticbeanstalk:RetrieveEnvironmentInfo",
        "elasticbeanstalk:SwapEnvironmentCNAMEs",
        "elasticbeanstalk:TerminateEnvironment",
        "elasticbeanstalk:UpdateApplicationVersion",
        "elasticbeanstalk:UpdateConfigurationTemplate",
        "elasticbeanstalk:UpdateEnvironment",
        "elasticbeanstalk:RetrieveEnvironmentInfo",
        "elasticbeanstalk:ValidateConfigurationSettings"
      ],
      "Resource": [
        "*"
      ],
      "Condition": {
        "StringNotEquals": {
          "elasticbeanstalk:InApplication": [
            "arn:aws:elasticbeanstalk:us-east-2:123456789012:application/myapplication"
          ]
        }
      }
    }
  ]
}
```