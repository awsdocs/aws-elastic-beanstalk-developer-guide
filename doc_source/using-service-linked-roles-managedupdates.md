# The managed\-updates service\-linked role<a name="using-service-linked-roles-managedupdates"></a>

AWS Elastic Beanstalk uses AWS Identity and Access Management \(IAM\)[ service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role)\. A service\-linked role is a unique type of IAM role that is linked directly to Elastic Beanstalk\. Service\-linked roles are predefined by Elastic Beanstalk and include all the permissions that the service requires to call other AWS services on your behalf\. 

A service\-linked role makes setting up Elastic Beanstalk easier because you don’t have to manually add the necessary permissions\. Elastic Beanstalk defines the permissions of its service\-linked roles, and unless defined otherwise, only Elastic Beanstalk can assume its roles\. The defined permissions include the trust policy and the permissions policy, and that permissions policy cannot be attached to any other IAM entity\.

You can delete a service\-linked role only after first deleting their related resources\. This protects your Elastic Beanstalk resources because you can't inadvertently remove permission to access the resources\.

For information about other services that support service\-linked roles, see [AWS Services That Work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) and look for the services that have **Yes **in the **Service\-Linked Role** column\. Choose a **Yes** with a link to view the service\-linked role documentation for that service\.

## Service\-linked role permissions for Elastic Beanstalk<a name="service-linked-role-permissions-managedupdates"></a>

Elastic Beanstalk uses the service\-linked role named **AWSServiceRoleForElasticBeanstalkManagedUpdates** – Allows Elastic Beanstalk to perform scheduled platform updates of your running environments\.

The AWSServiceRoleForElasticBeanstalkManagedUpdates service\-linked role trusts the following services to assume the role:
+ `managedupdates.elasticbeanstalk.amazonaws.com`

The permissions policy of the AWSServiceRoleForElasticBeanstalkManagedUpdates service\-linked role contains all of the permissions that Elastic Beanstalk needs to complete managed update actions on your behalf:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowPassRoleToElasticBeanstalkAndDownstreamServices",
            "Effect": "Allow",
            "Action": "iam:PassRole",
            "Resource": "*",
            "Condition": {
                "StringLikeIfExists": {
                    "iam:PassedToService": [
                        "elasticbeanstalk.amazonaws.com",
                        "ec2.amazonaws.com",
                        "autoscaling.amazonaws.com",
                        "elasticloadbalancing.amazonaws.com",
                        "ecs.amazonaws.com",
                        "cloudformation.amazonaws.com"
                    ]
                }
            }
        },
        {
            "Sid": "SingleInstanceAPIs",
            "Effect": "Allow",
            "Action": [
                "ec2:releaseAddress",
                "ec2:allocateAddress",
                "ec2:DisassociateAddress",
                "ec2:AssociateAddress"
            ],
            "Resource": "*"
        },
        {
            "Sid": "ECS",
            "Effect": "Allow",
            "Action": [
                "ecs:RegisterTaskDefinition",
                "ecs:DeRegisterTaskDefinition",
                "ecs:List*",
                "ecs:Describe*"
            ],
            "Resource": "*"
        },
        {
            "Sid": "ElasticBeanstalkAPIs",
            "Effect": "Allow",
            "Action": [
                "elasticbeanstalk:*"
            ],
            "Resource": "*"
        },
        {
            "Sid": "ReadOnlyAPIs",
            "Effect": "Allow",
            "Action": [
                "cloudformation:Describe*",
                "cloudformation:List*",
                "ec2:Describe*",
                "autoscaling:Describe*",
                "elasticloadbalancing:Describe*"
            ],
            "Resource": "*"
        },
        {
            "Sid": "ASG",
            "Effect": "Allow",
            "Action": [
                "autoscaling:AttachInstances",
                "autoscaling:CreateAutoScalingGroup",
                "autoscaling:CreateLaunchConfiguration",
                "autoscaling:DeleteAutoScalingGroup",
                "autoscaling:DeleteLaunchConfiguration",
                "autoscaling:DeleteScheduledAction",
                "autoscaling:DetachInstances",
                "autoscaling:PutNotificationConfiguration",
                "autoscaling:PutScalingPolicy",
                "autoscaling:PutScheduledUpdateGroupAction",
                "autoscaling:ResumeProcesses",
                "autoscaling:SuspendProcesses",
                "autoscaling:TerminateInstanceInAutoScalingGroup",
                "autoscaling:UpdateAutoScalingGroup"
            ],
            "Resource": [
                "arn:aws:autoscaling:*:*:launchConfiguration:*:launchConfigurationName/awseb-e-*",
                "arn:aws:autoscaling:*:*:autoScalingGroup:*:autoScalingGroupName/awseb-e-*"
            ]
        },
        {
            "Sid": "CFN",
            "Effect": "Allow",
            "Action": [
                "cloudformation:CreateStack",
                "cloudformation:DeleteStack",
                "cloudformation:GetTemplate",
                "cloudformation:UpdateStack"
            ],
            "Resource": "arn:aws:cloudformation:*:*:stack/awseb-e-*"
        },
        {
            "Sid": "EC2",
            "Effect": "Allow",
            "Action": [
                "ec2:TerminateInstances"
            ],
            "Resource": "arn:aws:ec2:*:*:instance/*",
            "Condition": {
                "StringLike": {
                    "ec2:ResourceTag/aws:cloudformation:stack-id": "arn:aws:cloudformation:*:*:stack/awseb-e-*"
                }
            }
        },
        {
            "Sid": "S3Obj",
            "Effect": "Allow",
            "Action": [
                "s3:DeleteObject",
                "s3:GetObject",
                "s3:GetObjectAcl",
                "s3:GetObjectVersion",
                "s3:GetObjectVersionAcl",
                "s3:PutObject",
                "s3:PutObjectAcl",
                "s3:PutObjectVersionAcl"
            ],
            "Resource": "arn:aws:s3:::elasticbeanstalk-*/*"
        },
        {
            "Sid": "S3Bucket",
            "Effect": "Allow",
            "Action": [
                "s3:GetBucketLocation",
                "s3:GetBucketPolicy",
                "s3:ListBucket",
                "s3:PutBucketPolicy"
            ],
            "Resource": "arn:aws:s3:::elasticbeanstalk-*"
        },
        {
            "Sid": "CWL",
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogGroup",
                "logs:DeleteLogGroup",
                "logs:PutRetentionPolicy"
            ],
            "Resource": "arn:aws:logs:*:*:log-group:/aws/elasticbeanstalk/*"
        },
        {
            "Sid": "ELB",
            "Effect": "Allow",
            "Action": [
                "elasticloadbalancing:RegisterTargets",
                "elasticloadbalancing:DeRegisterTargets",
                "elasticloadbalancing:DeregisterInstancesFromLoadBalancer",
                "elasticloadbalancing:RegisterInstancesWithLoadBalancer"
            ],
            "Resource": [
                "arn:aws:elasticloadbalancing:*:*:targetgroup/awseb-*",
                "arn:aws:elasticloadbalancing:*:*:loadbalancer/awseb-e-*"
            ]
        }
    ]
}
```

You must configure permissions to allow an IAM entity \(such as a user, group, or role\) to create, edit, or delete a service\-linked role\. For more information, see [Service\-Linked Role Permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#service-linked-role-permissions) in the *IAM User Guide*\.

Alternatively, you can use an AWS managed policy to [provide full access](AWSHowTo.iam.managed-policies.md) to Elastic Beanstalk\.

## Creating a service\-linked role for Elastic Beanstalk<a name="create-service-linked-role-managedupdates"></a>

You don't need to manually create a service\-linked role\. When you create an Elastic Beanstalk environment using the Elastic Beanstalk API, enable managed updates, and specify `AWSServiceRoleForElasticBeanstalkManagedUpdates` as the value for the `ServiceRoleForManagedUpdates` option of the `aws:elasticbeanstalk:managedactions` namespace, Elastic Beanstalk creates the service\-linked role for you\. 

When Elastic Beanstalk tries to create the AWSServiceRoleForElasticBeanstalkManagedUpdates service\-linked role for your account when you create an environment, you must have the `iam:CreateServiceLinkedRole` permission\. If you don't have this permission, environment creation fails, and you see a message explaining the issue\.

As an alternative, another user with permission to create service\-linked roles can use IAM to pre\-create the service linked\-role in advance\. You can then create your environment even without having the `iam:CreateServiceLinkedRole` permission\.

You \(or another user\) can use the IAM console to create a service\-linked role with the **Elastic Beanstalk Managed Updates** use case\. In the IAM CLI or the IAM API, create a service\-linked role with the `managedupdates.elasticbeanstalk.amazonaws.com` service name\. For more information, see [Creating a Service\-Linked Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#create-service-linked-role) in the *IAM User Guide*\. If you delete this service\-linked role, you can use this same process to create the role again\.

If you delete this service\-linked role, and then need to create it again, you can use the same process to recreate the role in your account\. When you create an Elastic Beanstalk environment using the Elastic Beanstalk API, enable managed updates, and specify `AWSServiceRoleForElasticBeanstalkManagedUpdates` as the value for the `ServiceRoleForManagedUpdates` option of the `aws:elasticbeanstalk:managedactions` namespace, Elastic Beanstalk creates the service\-linked role for you again\. 

## Editing a service\-linked role for Elastic Beanstalk<a name="edit-service-linked-role-managedupdates"></a>

Elastic Beanstalk does not allow you to edit the AWSServiceRoleForElasticBeanstalkManagedUpdates service\-linked role\. After you create a service\-linked role, you cannot change the name of the role because various entities might reference the role\. However, you can edit the description of the role using IAM\. For more information, see [Editing a Service\-Linked Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#edit-service-linked-role) in the *IAM User Guide*\.

## Deleting a service\-linked role for Elastic Beanstalk<a name="delete-service-linked-role-managedupdates"></a>

If you no longer need to use a feature or service that requires a service\-linked role, we recommend that you delete that role\. That way you don’t have an unused entity that is not actively monitored or maintained\. However, you must clean up the resources for your service\-linked role before you can manually delete it\.

### Cleaning up a service\-linked role<a name="service-linked-role-review-before-delete-managedupdates"></a>

Before you can use IAM to delete a service\-linked role, you must first be sure that Elastic Beanstalk environments with managed updates enabled are either using a different service role or are terminated\.

**Note**  
If the Elastic Beanstalk service is using the service\-linked role when you try to terminate the environments, then the termination might fail\. If that happens, wait for a few minutes and try the operation again\.

**To terminate an Elastic Beanstalk environment that uses the AWSServiceRoleForElasticBeanstalkManagedUpdates \(console\)**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. Choose **Actions**, and then choose **Terminate Environment**\.

1. Use the on\-screen dialog box to confirm environment termination\.

See [eb terminate](eb3-terminate.md) for details about terminating an Elastic Beanstalk environment using the EB CLI\.

See [TerminateEnvironment](https://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_TerminateEnvironment.html) for details about terminating an Elastic Beanstalk environment using the API\.

### Manually delete the service\-linked role<a name="slr-manual-delete-managedupdates"></a>

Use the IAM console, the IAM CLI, or the IAM API to delete the AWSServiceRoleForElasticBeanstalkManagedUpdates service\-linked role\. For more information, see [Deleting a Service\-Linked Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#delete-service-linked-role) in the *IAM User Guide*\.

## Supported Regions for Elastic Beanstalk service\-linked roles<a name="slr-regions-managedupdates"></a>

Elastic Beanstalk supports using service\-linked roles in all of the regions where the service is available\. For more information, see [AWS Elastic Beanstalk Endpoints and Quotas](https://docs.aws.amazon.com/general/latest/gr/elasticbeanstalk.html)\.