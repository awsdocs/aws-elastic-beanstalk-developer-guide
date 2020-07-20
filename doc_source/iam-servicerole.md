# Managing Elastic Beanstalk service roles<a name="iam-servicerole"></a>

To manage and monitor your environment, AWS Elastic Beanstalk performs actions on the environment's resources on your behalf\. Elastic Beanstalk needs certain permissions to perform these actions, and it assumes AWS Identity and Access Management \(IAM\) service roles to get these permissions\.

Elastic Beanstalk needs to use temporary security credentials whenever it assumes a service role\. To get these credentials, Elastic Beanstalk sends a request to AWS Security Token Service \(AWS STS\) on a region\-specific endpoint\. For more information, see [Temporary Security Credentials](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp.html) in the *IAM User Guide*\.

**Note**  
If the AWS STS endpoint of your environment's region is deactivated, Elastic Beanstalk sends the request on an alternative endpoint that can't be deactivated\. This endpoint is associated with a different region, and the request is therefore a cross\-region request\. For more information, see [Activating and Deactivating AWS STS in an AWS Region](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_enable-regions.html) in the *IAM User Guide*\.

## Managing service roles using the Elastic Beanstalk console and EB CLI<a name="iam-servicerole-console"></a>

The Elastic Beanstalk console and the EB CLI make it easier for you to set up service roles for your environment with a sufficient set of permissions\. They create a default service role and use managed policies in it\.

### Managed service role policies<a name="iam-servicerole-policy"></a>

Elastic Beanstalk provides a managed policy for [enhanced health monitoring](health-enhanced.md), and one with additional permissions required for [managed platform updates](environment-platform-update-managed.md)\. The console and EB CLI assign both of these policies to the default service role that they create for you\.

#### `AWSElasticBeanstalkEnhancedHealth`<a name="iam-servicerole-policy.health"></a>

This policy grants permissions for Elastic Beanstalk to monitor instance and environment health\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "elasticloadbalancing:DescribeInstanceHealth",
                "elasticloadbalancing:DescribeLoadBalancers",
                "elasticloadbalancing:DescribeTargetHealth",
                "ec2:DescribeInstances",
                "ec2:DescribeInstanceStatus",
                "ec2:GetConsoleOutput",
                "ec2:AssociateAddress",
                "ec2:DescribeAddresses",
                "ec2:DescribeSecurityGroups",
                "sqs:GetQueueAttributes",
                "sqs:GetQueueUrl",
                "autoscaling:DescribeAutoScalingGroups",
                "autoscaling:DescribeAutoScalingInstances",
                "autoscaling:DescribeScalingActivities",
                "autoscaling:DescribeNotificationConfigurations",
                "sns:Publish"
            ],
            "Resource": [
                "*"
            ]
        }
    ]
}
```

#### `AWSElasticBeanstalkService`<a name="iam-servicerole-policy.service"></a>

This policy grants permissions for Elastic Beanstalk to update environments on your behalf to perform managed platform updates\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowCloudformationOperationsOnElasticBeanstalkStacks",
            "Effect": "Allow",
            "Action": [
                "cloudformation:*"
            ],
            "Resource": [
                "arn:aws:cloudformation:*:*:stack/awseb-*",
                "arn:aws:cloudformation:*:*:stack/eb-*"
            ]
        },
        {
            "Sid": "AllowS3OperationsOnElasticBeanstalkBuckets",
            "Effect": "Allow",
            "Action": [
                "s3:*"
            ],
            "Resource": [
                "arn:aws:s3:::elasticbeanstalk-*",
                "arn:aws:s3:::elasticbeanstalk-*/*"
            ]
        },
        {
            "Sid": "AllowOperations",
            "Effect": "Allow",
            "Action": [
                "autoscaling:AttachInstances",
                "autoscaling:CreateAutoScalingGroup",
                "autoscaling:CreateLaunchConfiguration",
                "autoscaling:DeleteLaunchConfiguration",
                "autoscaling:DeleteAutoScalingGroup",
                "autoscaling:DeletePolicy",
                "autoscaling:DeleteScheduledAction",
                "autoscaling:DescribeAccountLimits",
                "autoscaling:DescribeAutoScalingGroups",
                "autoscaling:DescribeAutoScalingInstances",
                "autoscaling:DescribeLaunchConfigurations",
                "autoscaling:DescribeLoadBalancers",
                "autoscaling:DescribeNotificationConfigurations",
                "autoscaling:DescribeScalingActivities",
                "autoscaling:DescribeScheduledActions",
                "autoscaling:DetachInstances",
                "autoscaling:PutNotificationConfiguration",
                "autoscaling:PutScalingPolicy",
                "autoscaling:PutScheduledUpdateGroupAction",
                "autoscaling:ResumeProcesses",
                "autoscaling:SetDesiredCapacity",
                "autoscaling:SuspendProcesses",
                "autoscaling:TerminateInstanceInAutoScalingGroup",
                "autoscaling:UpdateAutoScalingGroup",
                "cloudwatch:PutMetricAlarm",
                "ec2:AuthorizeSecurityGroupEgress",
                "ec2:AuthorizeSecurityGroupIngress",
                "ec2:CreateLaunchTemplate",
                "ec2:CreateLaunchTemplateVersion",
                "ec2:CreateSecurityGroup",
                "ec2:DeleteLaunchTemplate",
                "ec2:DeleteLaunchTemplateVersions",
                "ec2:DeleteSecurityGroup",
                "ec2:DescribeAccountAttributes",
                "ec2:DescribeImages",
                "ec2:DescribeInstances",
                "ec2:DescribeKeyPairs",
                "ec2:DescribeLaunchTemplates",
                "ec2:DescribeLaunchTemplateVersions",
                "ec2:DescribeSecurityGroups",
                "ec2:DescribeSubnets",
                "ec2:DescribeVpcs",
                "ec2:RevokeSecurityGroupEgress",
                "ec2:RevokeSecurityGroupIngress",
                "ec2:RunInstances",
                "ec2:TerminateInstances",
                "ecs:CreateCluster",
                "ecs:DeleteCluster",
                "ecs:DescribeClusters",
                "ecs:RegisterTaskDefinition",
                "elasticbeanstalk:*",
                "elasticloadbalancing:ApplySecurityGroupsToLoadBalancer",
                "elasticloadbalancing:ConfigureHealthCheck",
                "elasticloadbalancing:CreateLoadBalancer",
                "elasticloadbalancing:DeleteLoadBalancer",
                "elasticloadbalancing:DeregisterInstancesFromLoadBalancer",
                "elasticloadbalancing:DescribeInstanceHealth",
                "elasticloadbalancing:DescribeLoadBalancers",
                "elasticloadbalancing:DescribeTargetHealth",
                "elasticloadbalancing:RegisterInstancesWithLoadBalancer",
                "iam:ListRoles",
                "iam:PassRole",
                "logs:CreateLogGroup",
                "logs:PutRetentionPolicy",
                "rds:DescribeDBInstances",
                "rds:DescribeOrderableDBInstanceOptions",
                "s3:CopyObject",
                "s3:GetObject",
                "s3:GetObjectAcl",
                "s3:GetObjectMetadata",
                "s3:ListBucket",
                "s3:listBuckets",
                "sns:CreateTopic",
                "sns:GetTopicAttributes",
                "sns:ListSubscriptionsByTopic",
                "sns:Subscribe",
                "sqs:GetQueueAttributes",
                "sqs:GetQueueUrl"
            ],
            "Resource": [
                "*"
            ]
        }
    ]
}
```

### Using the Elastic Beanstalk console<a name="iam-servicerole-console"></a>

When you launch an environment in the Elastic Beanstalk console, the console creates a default service role, named `aws-elasticbeanstalk-service-role`, and attaches managed policies with default permissions to it\. 

To allow Elastic Beanstalk to assume the `aws-elasticbeanstalk-service-role` role, the service role specifies Elastic Beanstalk as a trusted entity in the trust relationship policy\.

```
{
    "Version": "2012-10-17",
    "Statement": [
      {
        "Sid": "",
        "Effect": "Allow",
        "Principal": {
          "Service": "elasticbeanstalk.amazonaws.com"
        },
        "Action": "sts:AssumeRole",
        "Condition": {
          "StringEquals": {
            "sts:ExternalId": "elasticbeanstalk"
          }
        }
      }
    ]
}
```

When you enable [managed platform updates](environment-platform-update-managed.md) for your environment, Elastic Beanstalk assumes a separate managed\-updates service role to perform managed updates\. By default, the Elastic Beanstalk console uses the same generated service role, `aws-elasticbeanstalk-service-role`, for the managed\-updates service role\. If you change your default service role, the console sets the managed\-updates service role to use the managed\-updates service\-linked role, `AWSServiceRoleForElasticBeanstalkManagedUpdates`\. For details about service\-linked roles, see [Using service\-linked roles](#iam-servicerole-slr)\.

**Note**  
Due to permission issues, the Elastic Beanstalk service doesn't always succeed in creating this service\-linked role for you\. Therefore, the console tries to explicitly create it\. To ensure your account has this service\-linked role, create an environment at least once using the console, and configure managed updates to be enabled before you create the environment\.

### Using the EB CLI<a name="iam-servicerole-ebcli"></a>

When you launch an environment using the [eb create](eb3-create.md) command of the Elastic Beanstalk Command Line Interface \(EB CLI\) and don't specify a service role through the `--service-role` option, Elastic Beanstalk creates the default service role `aws-elasticbeanstalk-service-role`\. If the default service role already exists, Elastic Beanstalk uses it for the new environment\. This is similar to what the Elastic Beanstalk console does\.

As opposed to the console, you can't specify a managed\-updates service role using an EB CLI command option\. If you enable managed updates for your environment, set the managed\-updates service role though configuration options\. The following example enables managed updates and uses the default service role as a managed\-updates service role\.

**Example \.ebextensions/managed\-platform\-update\.config**  

```
option_settings:
  aws:elasticbeanstalk:managedactions:
    ManagedActionsEnabled: true
    PreferredStartTime: "Tue:09:00"
    ServiceRoleForManagedUpdates: "aws-elasticbeanstalk-service-role"
  aws:elasticbeanstalk:managedactions:platformupdate:
    UpdateLevel: patch
    InstanceRefreshEnabled: true
```

## Managing service roles using the Elastic Beanstalk API<a name="iam-servicerole-api"></a>

When you use the `CreateEnvironment` action of the Elastic Beanstalk API to create an environment, specify a service role using the `ServiceRole` configuration option in the `aws:elasticbeanstalk:environment` namespace\. See [Using enhanced health reporting with the Elastic Beanstalk API](health-enhanced-api.md) for details on using enhanced health monitoring with the Elastic Beanstalk API\. 

In addition, if you enable [managed platform updates](environment-platform-update-managed.md) for your environment, you can specify a managed\-updates service role using the `ServiceRoleForManagedUpdates` option of the `aws:elasticbeanstalk:managedactions` namespace\.

## Using service\-linked roles<a name="iam-servicerole-slr"></a>

A service\-linked role is a unique type of service role that is predefined by Elastic Beanstalk to include all the permissions that the service requires to call other AWS services on your behalf\. The service\-linked role is associated with your account\. Elastic Beanstalk creates it once, then reuses it when creating additional environments\. For details about using service\-linked roles with Elastic Beanstalk environments, see [Using service\-linked roles for Elastic Beanstalk](using-service-linked-roles.md)\.

When you create an environment by using the Elastic Beanstalk API, and don't specify a service role, Elastic Beanstalk creates a [monitoring service\-linked role](using-service-linked-roles-monitoring.md) for your account, if it doesn't already exist, and uses it for the new environment\. You can also use IAM to create your account's monitoring service\-linked role in advance\. When your account has a monitoring service\-linked role, you can use it to create an environment by using the Elastic Beanstalk API, the Elastic Beanstalk console, or the EB CLI\.

In addition, if you enable [managed platform updates](environment-platform-update-managed.md) for the environment and specify `AWSServiceRoleForElasticBeanstalkManagedUpdates` as the value for the `ServiceRoleForManagedUpdates` option of the `aws:elasticbeanstalk:managedactions` namespace, Elastic Beanstalk creates a [managed\-updates service\-linked role](using-service-linked-roles-managedupdates.md) for your account, if it doesn't already exist, and uses it to perform managed updates for the new environment\.

**Note**  
When Elastic Beanstalk tries to create the monitoring and managed\-updates service\-linked roles for your account when you create an environment, you must have the `iam:CreateServiceLinkedRole` permission\. If you don't have this permission, environment creation fails, and you see a message explaining the issue\.  
As an alternative, another user with permission to create service\-linked roles can use IAM to create the service linked\-role in advance\. You can then create your environment even without having the `iam:CreateServiceLinkedRole` permission\.

## Verifying the default service role's permissions<a name="iam-servicerole-verify"></a>

The permissions granted by your default service role can vary depending on when it was created, the last time you launched an environment, and which client you used\. You can verify the permissions granted by the default service role in the IAM console\.

**To verify the default service role's permissions**

1. Open the [**Roles** page](https://console.aws.amazon.com/iam/home#roles) in the IAM console\.

1. Choose **aws\-elasticbeanstalk\-service\-role**\.

1. On the **Permissions** tab, review the list of policies attached to the role\.

1. To view the permissions that a policy grants, choose the policy\.

## Updating an out\-of\-date default service role<a name="iam-servicerole-update"></a>

If the default service role lacks the required permissions, you can update it by [creating a new environment](using-features.environments.md) in the Elastic Beanstalk environment management console\.

Alternatively, you can add the managed policies to the default service role manually\.

**To add managed policies to the default service role**

1. Open the [**Roles** page](https://console.aws.amazon.com/iam/home#roles) in the IAM console\.

1. Choose **aws\-elasticbeanstalk\-service\-role**\.

1. On the **Permissions** tab, choose **Attach policies**\.

1. Type **AWSElasticBeanstalk** to filter the policies\.

1. Select the following policies, and then choose **Attach policy**:
   + `AWSElasticBeanstalkEnhancedHealth`
   + `AWSElasticBeanstalkService`

## Adding permissions to the default service role<a name="iam-servicerole-addperms"></a>

If your application includes configuration files that refer to AWS resources for which permissions aren't included in the default service role, Elastic Beanstalk might need additional permissions to resolve these references when it processes the configuration files during a managed update\. If permissions are missing, the update fails and Elastic Beanstalk returns a message indicating which permission it needs\. Add permissions for additional services to the default service role in the IAM console\.

**To add additional policies to the default service role**

1. Open the [**Roles** page](https://console.aws.amazon.com/iam/home#roles) in the IAM console\.

1. Choose **aws\-elasticbeanstalk\-service\-role**\.

1. On the **Permissions** tab, choose **Attach policies**\.

1. Select the managed policy for the additional services that your application uses\. For example, `AmazonAPIGatewayAdministrator` or `AmazonElasticFileSystemFullAccess`\.

1. Choose **Attach policy**\.

## Creating a service role<a name="iam-servicerole-create"></a>

If you can't use the default service role, create a service role\.

**To create a service role**

1. Open the [**Roles** page](https://console.aws.amazon.com/iam/home#roles) in the IAM console\.

1. Choose **Create role**\.

1. Under **AWS service**, choose **AWS Elastic Beanstalk**, and then select your use case\.

1. Choose **Next: Permissions**\.

1. Attach the `AWSElasticBeanstalkService` and `AWSElasticBeanstalkEnhancedHealth` managed policies and any additional policies that provide permissions that your application needs\.

1. Choose **Next: Tags**\.

1. \(Optional\) Add tags to the role\.

1. Choose **Next: Review**\.

1. Enter a name for the role\.

1. Choose **Create role**\.

You can apply your custom service role when you create an environment in the [environment creation wizard](environments-create-wizard.md) or with the `--service-role` option on the `eb create` command\.