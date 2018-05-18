# Controlling Access to Elastic Beanstalk<a name="AWSHowTo.iam.managed-policies"></a>

AWS Elastic Beanstalk provides two managed policies that enable you to assign full access or read\-only access to all Elastic Beanstalk resources\. You can attach the policies to AWS Identity and Access Management \(IAM\) users or groups\.

**Managed User Policies**
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
+ **AWSElasticBeanstalkReadOnlyAccess** – Allows the user to view applications and environments, but not to perform operations on them\. It provides read\-only access to all Elastic Beanstalk resources\. Note that read\-only access does not enable actions such as downloading Elastic Beanstalk logs so that you can read them\. See the example at the end of this topic for information on how to enable read\-only access to Elastic Beanstalk logs\.

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

## Controlling Access with Managed Policies<a name="iam-userpolicies-managed"></a>

You can use managed policies to grant full access or read\-only access to Elastic Beanstalk\. Elastic Beanstalk updates these policies automatically when additional permissions are required to access new features\.

**To apply a managed policy to an IAM user**

1. Open the [**Users** page](https://console.aws.amazon.com/iam/home#users) in the IAM console\.

1. In the navigation pane, choose **Permissions**\.

1. Choose **Attach Policy**\.

1. Type **AWSElasticBeanstalk** to filter the policies\.

1. Select **AWSElasticBeanstalkReadOnlyAccess** or **AWSElasticBeanstalkFullAccess**, and then choose **Attach Policy**\.

## Creating a Custom User Policy<a name="AWSHowTo.iam.policies"></a>

You can create your own IAM policy to allow or deny specific Elastic Beanstalk API actions on specific Elastic Beanstalk resources\. For more information about attaching a policy to a user or group, see [Working with Policies](http://docs.aws.amazon.com/IAM/latest/UserGuide/ManagingPolicies.html) in *Using AWS Identity and Access Management*\.

**Note**  
While you can restrict how a user interacts with Elastic Beanstalk APIs, there is not currently an effective way to prevent users who have permission to create the necessary underlying resources from creating other resources in Amazon EC2 and other services\.  
Think of these policies as an effective way to distribute Elastic Beanstalk responsibilities, not as a way to secure all underlying resources\.

An IAM policy contains policy statements that describe the permissions that you want to grant\. When you create a policy statement for Elastic Beanstalk, you need to understand how to use the following four parts of a policy statement:
+ **Effect** specifies whether to allow or deny the actions in the statement\.
+ **Action** specifies the [API operations](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_Operations.html) that you want to control\. For example, use `elasticbeanstalk:CreateEnvironment` to specify the `CreateEnvironment` operation\. Certain operations, such as creating an environment, require additional permissions to perform those actions\. For more information, see [Resources and Conditions for Elastic Beanstalk Actions](AWSHowTo.iam.policies.actions.md)\. 
**Note**  
To use the [http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_UpdateTagsForResource.html](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_UpdateTagsForResource.html) API operation, specify one of the following two virtual actions \(or both\) instead of the API operation name:  

`elasticbeanstalk:AddTags`  
Controls permission to call `UpdateTagsForResource` and pass a list of tags to add in the `TagsToAdd` parameter\.

`elasticbeanstalk:RemoveTags`  
Controls permission to call `UpdateTagsForResource` and pass a list of tag keys to remove in the `TagsToRemove` parameter\.
+ **Resource** specifies the resources that you want to control access to\. To specify Elastic Beanstalk resources, list the [Amazon Resource Name](AWSHowTo.iam.policies.arn.md) \(ARN\) of each resource\.
+ \(optional\) **Condition** specifies restrictions on the permission granted in the statement\. For more information, see [Resources and Conditions for Elastic Beanstalk Actions](AWSHowTo.iam.policies.actions.md)\.

The following sections demonstrate a few cases in which you might consider a custom user policy\.

### Enabling Limited Elastic Beanstalk Environment Creation<a name="AWSHowTo.iam.policy.env-creation"></a>

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

### Enabling Read\-Only Access to Elastic Beanstalk Logs Stored in Amazon S3<a name="AWSHowTo.iam.policy.view-s3-logs"></a>

The policy in the following example enables a user to view and pull Elastic Beanstalk logs that were stored in Amazon S3\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Stmt1491295324000",
      "Effect": "Allow",
      "Action": [
        "s3:GetObjectAcl"
      ],
      "Resource": [
        "arn:aws:s3:::elasticbeanstalk-*/*"
      ]
    },
    {
      "Sid": "Stmt1491295472000",
      "Effect": "Allow",
      "Action": [
        "s3:GetObjectAcl"
      ],
      "Resource": [
        "arn:aws:s3:::elasticbeanstalk-*"
      ]
    }
  ]
}
```

### Enabling Management of a Specific Elastic Beanstalk Application<a name="AWSHowTo.iam.policy.manage-app"></a>

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