# The monitoring service\-linked role<a name="using-service-linked-roles-monitoring"></a>

AWS Elastic Beanstalk uses AWS Identity and Access Management \(IAM\)[ service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role)\. A service\-linked role is a unique type of IAM role that is linked directly to Elastic Beanstalk\. Service\-linked roles are predefined by Elastic Beanstalk and include all the permissions that the service requires to call other AWS services on your behalf\. 

A service\-linked role makes setting up Elastic Beanstalk easier because you don’t have to manually add the necessary permissions\. Elastic Beanstalk defines the permissions of its service\-linked roles, and unless defined otherwise, only Elastic Beanstalk can assume its roles\. The defined permissions include the trust policy and the permissions policy, and that permissions policy cannot be attached to any other IAM entity\.

You can delete a service\-linked role only after first deleting their related resources\. This protects your Elastic Beanstalk resources because you can't inadvertently remove permission to access the resources\.

For information about other services that support service\-linked roles, see [AWS Services That Work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) and look for the services that have **Yes **in the **Service\-Linked Role** column\. Choose a **Yes** with a link to view the service\-linked role documentation for that service\.

## Service\-linked role permissions for Elastic Beanstalk<a name="service-linked-role-permissions-monitoring"></a>

Elastic Beanstalk uses the service\-linked role named **AWSServiceRoleForElasticBeanstalk** – Allows Elastic Beanstalk to monitor the health of running environments and publish health event notifications\.

The AWSServiceRoleForElasticBeanstalk service\-linked role trusts the following services to assume the role:
+ `elasticbeanstalk.amazonaws.com`

The permissions policy of the AWSServiceRoleForElasticBeanstalk service\-linked role contains all of the permissions that Elastic Beanstalk needs to complete actions on your behalf:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowCloudformationReadOperationsOnElasticBeanstalkStacks",
            "Effect": "Allow",
            "Action": [
                "cloudformation:DescribeStackResource",
                "cloudformation:DescribeStackResources",
                "cloudformation:DescribeStacks"
            ],
            "Resource": [
                "arn:aws:cloudformation:*:*:stack/awseb-*",
                "arn:aws:cloudformation:*:*:stack/eb-*"
            ]
        },
        {
            "Sid": "AllowOperations",
            "Effect": "Allow",
            "Action": [
                "autoscaling:DescribeAutoScalingGroups",
                "autoscaling:DescribeAutoScalingInstances",
                "autoscaling:DescribeNotificationConfigurations",
                "autoscaling:DescribeScalingActivities",
                "autoscaling:PutNotificationConfiguration",
                "ec2:DescribeInstanceStatus",
                "ec2:AssociateAddress",
                "ec2:DescribeAddresses",
                "ec2:DescribeInstances",
                "ec2:DescribeSecurityGroups",
                "elasticloadbalancing:DescribeInstanceHealth",
                "elasticloadbalancing:DescribeLoadBalancers",
                "elasticloadbalancing:DescribeTargetHealth",
                "elasticloadbalancing:DescribeTargetGroups",
                "sqs:GetQueueAttributes",
                "sqs:GetQueueUrl",
                "sns:Publish"
            ],
            "Resource": [
                "*"
            ]
        }
    ]
}
```

You must configure permissions to allow an IAM entity \(such as a user, group, or role\) to create, edit, or delete a service\-linked role\. For more information, see [Service\-Linked Role Permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#service-linked-role-permissions) in the *IAM User Guide*\.

Alternatively, you can use an AWS managed policy to [provide full access](AWSHowTo.iam.managed-policies.md) to Elastic Beanstalk\.

## Creating a service\-linked role for Elastic Beanstalk<a name="create-service-linked-role-monitoring"></a>

You don't need to manually create a service\-linked role\. When you create an Elastic Beanstalk environment using the Elastic Beanstalk API and don't specify a service role, Elastic Beanstalk creates the service\-linked role for you\. 

**Important**  
   If you were using the Elastic Beanstalk service before September 27, 2017, when it began supporting the AWSServiceRoleForElasticBeanstalk service\-linked role, and your account needed it, then Elastic Beanstalk created the AWSServiceRoleForElasticBeanstalk role in your account\.  To learn more, see [A New Role Appeared in My IAM Account](https://docs.aws.amazon.com/IAM/latest/UserGuide/troubleshoot_roles.html#troubleshoot_roles_new-role-appeared)\.

When Elastic Beanstalk tries to create the AWSServiceRoleForElasticBeanstalk service\-linked role for your account when you create an environment, you must have the `iam:CreateServiceLinkedRole` permission\. If you don't have this permission, environment creation fails, and you see a message explaining the issue\.

As an alternative, another user with permission to create service\-linked roles can use IAM to pre\-create the service linked\-role in advance\. You can then create your environment even without having the `iam:CreateServiceLinkedRole` permission\.

You \(or another user\) can use the IAM console to create a service\-linked role with the **Elastic Beanstalk** use case\. In the IAM CLI or the IAM API, create a service\-linked role with the `elasticbeanstalk.amazonaws.com` service name\. For more information, see [Creating a Service\-Linked Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#create-service-linked-role) in the *IAM User Guide*\. If you delete this service\-linked role, you can use this same process to create the role again\.

If you delete this service\-linked role, and then need to create it again, you can use the same process to recreate the role in your account\. When you create an Elastic Beanstalk environment using the Elastic Beanstalk API and don't specify a service role, Elastic Beanstalk creates the service\-linked role for you again\. 

## Editing a service\-linked role for Elastic Beanstalk<a name="edit-service-linked-role-monitoring"></a>

Elastic Beanstalk does not allow you to edit the AWSServiceRoleForElasticBeanstalk service\-linked role\. After you create a service\-linked role, you cannot change the name of the role because various entities might reference the role\. However, you can edit the description of the role using IAM\. For more information, see [Editing a Service\-Linked Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#edit-service-linked-role) in the *IAM User Guide*\.

## Deleting a service\-linked role for Elastic Beanstalk<a name="delete-service-linked-role-monitoring"></a>

If you no longer need to use a feature or service that requires a service\-linked role, we recommend that you delete that role\. That way you don’t have an unused entity that is not actively monitored or maintained\. However, you must clean up the resources for your service\-linked role before you can manually delete it\.

### Cleaning up a service\-linked role<a name="service-linked-role-review-before-delete-monitoring"></a>

Before you can use IAM to delete a service\-linked role, you must first be sure that all Elastic Beanstalk environments are either using a different service role or are terminated\.

**Note**  
If the Elastic Beanstalk service is using the service\-linked role when you try to terminate the environments, then the termination might fail\. If that happens, wait for a few minutes and try the operation again\.

**To terminate an Elastic Beanstalk environment that uses the AWSServiceRoleForElasticBeanstalk \(console\)**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. Choose **Environment actions**, and then choose **Terminate environment**\.

1. Use the on\-screen dialog box to confirm environment termination\.

See [eb terminate](eb3-terminate.md) for details about terminating an Elastic Beanstalk environment using the EB CLI\.

See [TerminateEnvironment](https://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_TerminateEnvironment.html) for details about terminating an Elastic Beanstalk environment using the API\.

### Manually delete the service\-linked role<a name="slr-manual-delete-monitoring"></a>

Use the IAM console, the IAM CLI, or the IAM API to delete the AWSServiceRoleForElasticBeanstalk service\-linked role\. For more information, see [Deleting a Service\-Linked Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#delete-service-linked-role) in the *IAM User Guide*\.

## Supported regions for Elastic Beanstalk service\-linked roles<a name="slr-regions-monitoring"></a>

Elastic Beanstalk supports using service\-linked roles in all of the regions where the service is available\. For more information, see [AWS Elastic Beanstalk Endpoints and Quotas](https://docs.aws.amazon.com/general/latest/gr/elasticbeanstalk.html)\.