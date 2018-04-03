# Using Service\-Linked Roles for Elastic Beanstalk<a name="using-service-linked-roles"></a>

AWS Elastic Beanstalk can use AWS Identity and Access Management \(IAM\)[ service\-linked roles](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role)\. A service\-linked role is a unique type of IAM role that is linked directly to Elastic Beanstalk\. Service\-linked roles are predefined by Elastic Beanstalk and include all the permissions that the service requires to call other AWS services on your behalf\. Elastic Beanstalk uses a service\-linked role when you create an environment and don't explicitly specify a service role for it\.

A service\-linked role makes setting up Elastic Beanstalk easier because you don’t have to manually add the necessary permissions\. Elastic Beanstalk defines the permissions of its service\-linked roles, and unless defined otherwise, only Elastic Beanstalk can assume its roles\. The defined permissions include the trust policy and the permissions policy, and that permissions policy cannot be attached to any other IAM entity\.

You can delete the roles only after first deleting their related resources\. This protects your Elastic Beanstalk resources because you can't inadvertently remove permission to access the resources\.

For information about other services that support service\-linked roles, see [AWS Services That Work with IAM](http://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) and look for the services that have **Yes **in the **Service\-Linked Role** column\. Choose a **Yes** with a link to view the service\-linked role documentation for that service\.

## Service\-Linked Role Permissions for Elastic Beanstalk<a name="service-linked-role-permissions"></a>

Elastic Beanstalk uses the service\-linked role named **AWSServiceRoleForElasticBeanstalk**\. Elastic Beanstalk uses this service\-linked role to call other AWS services on your behalf\.

The AWSServiceRoleForElasticBeanstalk service\-linked role trusts the `elasticbeanstalk.amazonaws.com` service to assume the role\.

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

You must configure permissions to allow an IAM entity \(such as a user, group, or role\) to create, edit, or delete a service\-linked role\.

**To allow an IAM entity to create the AWSServiceRoleForElasticBeanstalk service\-linked role**

Add the following statement to the permissions policy for the IAM entity that needs to create the service\-linked role:

```
{
    "Effect": "Allow",
    "Action": [
        "iam:CreateServiceLinkedRole",
        "iam:PutRolePolicy"
    ],
    "Resource": "arn:aws:iam::*:role/aws-service-role/elasticbeanstalk.amazonaws.com/AWSServiceRoleForElasticBeanstalk*",
    "Condition": {"StringLike": {"iam:AWSServiceName": "elasticbeanstalk.amazonaws.com"}}
}
```

**To allow an IAM entity to edit the description of the AWSServiceRoleForElasticBeanstalk service\-linked role**

Add the following statement to the permissions policy for the IAM entity that needs to edit the description of a service\-linked role:

```
{
    "Effect": "Allow",
    "Action": [
        "iam:UpdateRoleDescription"
    ],
    "Resource": "arn:aws:iam::*:role/aws-service-role/elasticbeanstalk.amazonaws.com/AWSServiceRoleForElasticBeanstalk*",
    "Condition": {"StringLike": {"iam:AWSServiceName": "elasticbeanstalk.amazonaws.com"}}
}
```

**To allow an IAM entity to delete the AWSServiceRoleForElasticBeanstalk service\-linked role**

Add the following statement to the permissions policy for the IAM entity that needs to delete a service\-linked role:

```
{
    "Effect": "Allow",
    "Action": [
        "iam:DeleteServiceLinkedRole",
        "iam:GetServiceLinkedRoleDeletionStatus"
    ],
    "Resource": "arn:aws:iam::*:role/aws-service-role/elasticbeanstalk.amazonaws.com/AWSServiceRoleForElasticBeanstalk*",
    "Condition": {"StringLike": {"iam:AWSServiceName": "elasticbeanstalk.amazonaws.com"}}
}
```

Alternatively, you can use an AWS managed policy to [provide full access](AWSHowTo.iam.managed-policies.md) to Elastic Beanstalk\.

## Creating a Service\-Linked Role for Elastic Beanstalk<a name="create-service-linked-role"></a>

You don't need to manually create the AWSServiceRoleForElasticBeanstalk role\. When you create an Elastic Beanstalk environment using the Elastic Beanstalk API and don't specify a service role, Elastic Beanstalk creates the service\-linked role for you\. 

You can also use the IAM console, the AWS CLI, or the IAM API to create a service\-linked role using the **Elastic Beanstalk** use case\. For more information, see [Creating a Service\-Linked Role](http://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#create-service-linked-role) in the *IAM User Guide*\.

**Important**  
If you were using the Elastic Beanstalk service before September 27, 2017, when it began supporting service\-linked roles, Elastic Beanstalk created the AWSServiceRoleForElasticBeanstalk role in your account\. To learn more, see [A New Role Appeared in My IAM Account](http://docs.aws.amazon.com/IAM/latest/UserGuide/troubleshoot_roles.html#troubleshoot_roles_new-role-appeared)\.

## Editing a Service\-Linked Role for Elastic Beanstalk<a name="edit-service-linked-role"></a>

Elastic Beanstalk does not allow you to edit the AWSServiceRoleForElasticBeanstalk service\-linked role\. After you create a service\-linked role, you cannot change the name of the role because various entities might reference the role\. However, you can edit the description of the role using IAM\.

### Editing a Service\-Linked Role Description \(IAM Console\)<a name="edit-service-linked-role-iam-console"></a>

You can use the IAM console to edit the description of a service\-linked role\.

**To edit the description of a service\-linked role \(console\)**

1. In the navigation pane of the IAM console, choose **Roles**\.

1. Choose the name of the role to modify\.

1. To the far right of **Role description**, choose **Edit**\. 

1. Type a new description in the box, and then choose **Save**\.

### Editing a Service\-Linked Role Description \(IAM CLI\)<a name="edit-service-linked-role-iam-cli"></a>

You can use IAM commands from the AWS Command Line Interface to edit the description of a service\-linked role\.

**To change the description of a service\-linked role \(CLI\)**

1. \(Optional\) To view the current description for a role, use the following commands:

   ```
   $ aws iam [get\-role](http://docs.aws.amazon.com/cli/latest/reference/iam/get-role.html) --role-name role-name
   ```

   Use the role name, not the ARN, to refer to roles with the CLI commands\. For example, if a role has the following ARN: `arn:aws:iam::123456789012:role/myrole`, you refer to the role as **myrole**\.

1. To update a service\-linked role's description, use one of the following commands:

   ```
   $ aws iam [update\-role\-description](http://docs.aws.amazon.com/cli/latest/reference/iam/update-role-description.html) --role-name role-name --description description
   ```

### Editing a Service\-Linked Role Description \(IAM API\)<a name="edit-service-linked-role-iam-api"></a>

You can use the IAM API to edit the description of a service\-linked role\.

**To change the description of a service\-linked role \(API\)**

1. \(Optional\) To view the current description for a role, use the following command:

   IAM API: [GetRole](http://docs.aws.amazon.com/IAM/latest/APIReference/API_GetRole.html) 

1. To update a role's description, use the following command: 

   IAM API: [UpdateRoleDescription](http://docs.aws.amazon.com/IAM/latest/APIReference/API_UpdateRoleDescription.html)

## Deleting a Service\-Linked Role for Elastic Beanstalk<a name="delete-service-linked-role"></a>

If you no longer need to use a feature or service that requires a service\-linked role, we recommend that you delete that role\. That way you don’t have an unused entity that is not actively monitored or maintained\. However, you must clean up your service\-linked role before you can delete it\.

### Cleaning up a Service\-Linked Role<a name="service-linked-role-review-before-delete"></a>

Before you can use IAM to delete a service\-linked role, you must first confirm that the role has no active sessions and remove any resources used by the role\.

**To check whether the service\-linked role has an active session in the IAM console**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane of the IAM console, choose **Roles**\. Then choose the name \(not the check box\) of the AWSServiceRoleForElasticBeanstalk role\.

1. On the **Summary** page for the selected role, choose the **Access Advisor** tab\.

1. On the **Access Advisor** tab, review recent activity for the service\-linked role\.
**Note**  
If you are unsure whether Elastic Beanstalk is using the AWSServiceRoleForElasticBeanstalk role, you can try to delete the role\. If the service is using the role, the deletion fails and you can view the regions where the role is being used\. If the role is being used, you must wait for the session to end before you can delete the role\. You cannot revoke the session for a service\-linked role\. 

When you find out which Elastic Beanstalk environments are using the AWSServiceRoleForElasticBeanstalk role, you can terminate them, and then delete the role\.

**To terminate an Elastic Beanstalk environment \(console\)**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. Choose **Actions**, and then choose **Terminate Environment**\.

1. In the **Confirm Termination** dialog box, type the environment name, and then choose **Terminate**\.

See [`eb terminate`](eb3-terminate.md) for details about terminating an Elastic Beanstalk environment using the EB CLI\.

See [TerminateEnvironment](http://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_TerminateEnvironment.html) for details about terminating an Elastic Beanstalk environment using the API\.

### Deleting a Service\-Linked Role<a name="delete-service-linked-role-iam"></a>

You can use the IAM console, the AWS CLI, or the IAM API to delete a service\-linked role\. For more information, see [Deleting a Service\-Linked Role](http://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#delete-service-linked-role) in the *IAM User Guide*\.