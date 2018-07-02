# Managing Elastic Beanstalk Instance Profiles<a name="iam-instanceprofile"></a>

An instance profile is a container for an AWS Identity and Access Management \(IAM\) role that you can use to pass role information to an Amazon EC2 instance when the instance starts\. When you launch an environment in the AWS Elastic Beanstalk environment management console, the console creates a default instance profile, called `aws-elasticbeanstalk-ec2-role`, and assigns managed policies with default permissions to it\. 

Elastic Beanstalk provides three managed policies: one for the web server tier, one for the worker tier, and one with additional permissions required for multicontainer Docker environments\. The console assigns all of these policies to the role attached to the default instance profile\. The policies follow\.

**Managed Instance Profile Policies**
+ **AWSElasticBeanstalkWebTier** – Grants permissions for the application to upload logs to Amazon S3 and debugging information to AWS X\-Ray\.

  ```
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Sid": "BucketAccess",
        "Action": [
          "s3:Get*",
          "s3:List*",
          "s3:PutObject"
        ],
        "Effect": "Allow",
        "Resource": [
          "arn:aws:s3:::elasticbeanstalk-*",
          "arn:aws:s3:::elasticbeanstalk-*/*"
        ]
      },
      {
        "Sid": "XRayAccess",
        "Action": [
          "xray:PutTraceSegments",
          "xray:PutTelemetryRecords"
        ],
        "Effect": "Allow",
        "Resource": "*"
      },
      {
        "Sid": "CloudWatchLogsAccess",
        "Action": [
          "logs:PutLogEvents",
          "logs:CreateLogStream",
          "logs:DescribeLogStreams",
          "logs:DescribeLogGroups"
        ],
        "Effect": "Allow",
        "Resource": [
          "arn:aws:logs:*:*:log-group:/aws/elasticbeanstalk*"
        ]
      }
    ]
  }
  ```
+ **AWSElasticBeanstalkWorkerTier** – Grants permissions for log uploads, debugging, metric publication, and worker instance tasks, including queue management, leader election, and periodic tasks\.

  ```
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Sid": "MetricsAccess",
        "Action": [
          "cloudwatch:PutMetricData"
        ],
        "Effect": "Allow",
        "Resource": "*"
      },
      {
        "Sid": "CloudWatchLogsAccess",
        "Action": [
            "logs:PutLogEvents",
            "logs:CreateLogStream"
        ],
        "Effect": "Allow",
        "Resource": [
            "arn:aws:logs:*:*:log-group:/aws/elasticbeanstalk*"
        ]
      },
      {
        "Sid": "XRayAccess",
        "Action":[
          "xray:PutTraceSegments",
          "xray:PutTelemetryRecords"
        ],
        "Effect": "Allow",
        "Resource": "*"
      },
      {
        "Sid": "QueueAccess",
        "Action": [
          "sqs:ChangeMessageVisibility",
          "sqs:DeleteMessage",
          "sqs:ReceiveMessage",
          "sqs:SendMessage"
        ],
        "Effect": "Allow",
        "Resource": "*"
      },
      {
        "Sid": "BucketAccess",
        "Action": [
          "s3:Get*",
          "s3:List*",
          "s3:PutObject"
        ],
        "Effect": "Allow",
        "Resource": [
          "s3:::elasticbeanstalk-*",
          "s3:::elasticbeanstalk-*/*"
        ]
      },
      {
        "Sid": "DynamoPeriodicTasks",
        "Action": [
          "dynamodb:BatchGetItem",
          "dynamodb:BatchWriteItem",
          "dynamodb:DeleteItem",
          "dynamodb:GetItem",
          "dynamodb:PutItem",
          "dynamodb:Query",
          "dynamodb:Scan",
          "dynamodb:UpdateItem"
        ],
        "Effect": "Allow",
        "Resource": [
          "dynamodb:*:*:table/*-stack-AWSEBWorkerCronLeaderRegistry*"
        ]
      }
    ]
  }
  ```
+ **AWSElasticBeanstalkMulticontainerDocker** – Grants permissions for the Amazon Elastic Container Service to coordinate cluster tasks\.

  ```
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Sid": "ECSAccess",
        "Effect": "Allow",
        "Action": [
          "ecs:Poll",
          "ecs:StartTask",
          "ecs:StopTask",
          "ecs:DiscoverPollEndpoint",
          "ecs:StartTelemetrySession",
          "ecs:RegisterContainerInstance",
          "ecs:DeregisterContainerInstance",
          "ecs:DescribeContainerInstances",
          "ecs:Submit*"
        ],
        "Resource": "*"
      }
    ]
  }
  ```

To allow the EC2 instances in your environment to assume the `aws-elasticbeanstalk-ec2-role` role, the instance profile specifies Amazon EC2 as a trusted entity in the trust relationship policy, as follows\.

```
{
  "Version": "2008-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "ec2.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

To customize permissions, you can add policies to the role attached to the default instance profile or create your own instance profile with a restricted set of permissions\.

**Topics**
+ [Verifying the Permissions Assigned to the Default Instance Profile](#iam-instanceprofile-verify)
+ [Updating an Out\-of\-Date Default Instance Profile](#iam-instanceprofile-update)
+ [Adding Permissions to the Default Instance Profile](#iam-instanceprofile-addperms)
+ [Creating an Instance Profile](#iam-instanceprofile-create)

## Verifying the Permissions Assigned to the Default Instance Profile<a name="iam-instanceprofile-verify"></a>

The permissions assigned to your default instance profile can vary depending on when it was created, the last time you launched an environment, and which client you used\. You can verify the permissions on the default instance profile in the IAM console\.

**To verify the default instance profile's permissions**

1. Open the [**Roles** page](https://console.aws.amazon.com/iam/home#roles) in the IAM console\.

1. Choose **aws\-elasticbeanstalk\-ec2\-role**\.

1. Choose the **Permissions** tab and review the **Managed Policies** and **Inline Policies** sections to see the policies attached to the role\.

1. To see the permissions that a policy grants, choose **Show Policy** next to the policy\.

## Updating an Out\-of\-Date Default Instance Profile<a name="iam-instanceprofile-update"></a>

If the default instance profile lacks the required permissions, you can update it by [creating a new environment](using-features.environments.md) in the Elastic Beanstalk environment management console\.

Alternatively, you can add the managed policies to the role attached to the default instance profile manually\.

**To add managed policies to the role attached to the default instance profile**

1. Open the [**Roles** page](https://console.aws.amazon.com/iam/home#roles) in the IAM console\.

1. Choose **aws\-elasticbeanstalk\-ec2\-role**\.

1. On the **Permissions** tab, under **Managed Policies**, choose **Attach Policy**\.

1. Type **AWSElasticBeanstalk** to filter the policies\.

1. Select the following policies, and then choose **Attach Policies**:
   + `AWSElasticBeanstalkWebTier`
   + `AWSElasticBeanstalkWorkerTier`
   + `AWSElasticBeanstalkMulticontainerDocker`

## Adding Permissions to the Default Instance Profile<a name="iam-instanceprofile-addperms"></a>

If your application accesses AWS APIs or resources to which permissions aren't granted in the default instance profile, add policies that grant permissions in the IAM console\.

**To add policies to the role attached to the default instance profile**

1. Open the [Roles page](https://console.aws.amazon.com/iam/home#roles) in the IAM console\.

1. Choose **aws\-elasticbeanstalk\-ec2\-role**\.

1. On the **Permissions** tab, under **Managed Policies**, choose **Attach Policy**\.

1. Select the managed policy for the additional services that your application uses\. For example, `AmazonS3FullAccess` or `AmazonDynamoDBFullAccess`\.

1. Choose **Attach Policies**\.

## Creating an Instance Profile<a name="iam-instanceprofile-create"></a>

An instance profile is a wrapper around a standard IAM role that allows an EC2 instance to assume the role\. You can create additional instance profiles to customize permissions for different applications or to create an instance profile that doesn't grant permissions for worker tier or multicontainer Docker environments, if you don't use those features\.

**To create an instance profile**

1. Open the [**Roles** page](https://console.aws.amazon.com/iam/home#roles) in the IAM console\.

1. Choose **Create New Role**\.

1. Type a name, and then choose **Next Step**\.

1. Under **AWS Service Roles**, choose **Amazon EC2**\.

1. Attach the appropriate managed policies provided by Elastic Beanstalk and any additional policies that provide permissions that your application needs\.

1. Choose **Next Step**\.

1. Choose **Create Role**\.