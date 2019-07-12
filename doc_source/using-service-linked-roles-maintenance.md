# The Maintenance Service\-Linked Role<a name="using-service-linked-roles-maintenance"></a>

AWS Elastic Beanstalk uses AWS Identity and Access Management \(IAM\)[ service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role)\. A service\-linked role is a unique type of IAM role that is linked directly to Elastic Beanstalk\. Service\-linked roles are predefined by Elastic Beanstalk and include all the permissions that the service requires to call other AWS services on your behalf\. 

A service\-linked role makes setting up Elastic Beanstalk easier because you don’t have to manually add the necessary permissions\. Elastic Beanstalk defines the permissions of its service\-linked roles, and unless defined otherwise, only Elastic Beanstalk can assume its roles\. The defined permissions include the trust policy and the permissions policy, and that permissions policy cannot be attached to any other IAM entity\.

You can delete a service\-linked role only after first deleting their related resources\. This protects your Elastic Beanstalk resources because you can't inadvertently remove permission to access the resources\.

For information about other services that support service\-linked roles, see [AWS Services That Work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) and look for the services that have **Yes **in the **Service\-Linked Role** column\. Choose a **Yes** with a link to view the service\-linked role documentation for that service\.

## Service\-Linked Role Permissions for Elastic Beanstalk<a name="service-linked-role-permissions-maintenance"></a>

Elastic Beanstalk uses the service\-linked role named **AWSServiceRoleForElasticBeanstalkMaintenance** – Allows Elastic Beanstalk to perform regular maintenance activities for your running environments\.

The AWSServiceRoleForElasticBeanstalkMaintenance service\-linked role trusts the following services to assume the role:
+ `maintenance.elasticbeanstalk.amazonaws.com`

The permissions policy of the AWSServiceRoleForElasticBeanstalkMaintenance service\-linked role contains all of the permissions that Elastic Beanstalk needs to complete actions on your behalf:

```
{
    "Version": "2012-10-17",
    "Statement":
        {
            "Sid": "AllowCloudformationChangeSetOperationsOnElasticBeanstalkStacks",
            "Effect": "Allow",
            "Action": [
                "cloudformation:CreateChangeSet",
                "cloudformation:DescribeChangeSet",
                "cloudformation:ExecuteChangeSet",
                "cloudformation:DeleteChangeSet",
                "cloudformation:ListChangeSets",
                "cloudformation:DescribeStacks"
            ],
            "Resource": [
                "arn:aws:cloudformation:*:*:stack/awseb-*",
                "arn:aws:cloudformation:*:*:stack/eb-*"
            ]
        }
}
```

You must configure permissions to allow an IAM entity \(such as a user, group, or role\) to create, edit, or delete a service\-linked role\. For more information, see [Service\-Linked Role Permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#service-linked-role-permissions) in the *IAM User Guide*\.

## Creating a Service\-Linked Role for Elastic Beanstalk<a name="create-service-linked-role-maintenance"></a>

You don't need to manually create a service\-linked role\. When you create an Elastic Beanstalk environment using the Elastic Beanstalk API and don't specify an instance profile, Elastic Beanstalk creates the service\-linked role for you\. 

**Important**  
  This service\-linked role can appear in your account if you completed an action in another service that uses the features supported by this role\.  If you were using the Elastic Beanstalk service before April 18, 2019, when it began supporting the AWSServiceRoleForElasticBeanstalkMaintenance service\-linked role, and your account needed it, then Elastic Beanstalk created the AWSServiceRoleForElasticBeanstalkMaintenance role in your account\.  To learn more, see [A New Role Appeared in My IAM Account](https://docs.aws.amazon.com/IAM/latest/UserGuide/troubleshoot_roles.html#troubleshoot_roles_new-role-appeared)\.

If you delete this service\-linked role, and then need to create it again, you can use the same process to recreate the role in your account\. When you create an Elastic Beanstalk environment using the Elastic Beanstalk API and don't specify an instance profile, Elastic Beanstalk creates the service\-linked role for you again\. 

## Editing a Service\-Linked Role for Elastic Beanstalk<a name="edit-service-linked-role-maintenance"></a>

Elastic Beanstalk does not allow you to edit the AWSServiceRoleForElasticBeanstalkMaintenance service\-linked role\. After you create a service\-linked role, you cannot change the name of the role because various entities might reference the role\. However, you can edit the description of the role using IAM\. For more information, see [Editing a Service\-Linked Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#edit-service-linked-role) in the *IAM User Guide*\.

## Deleting a Service\-Linked Role for Elastic Beanstalk<a name="delete-service-linked-role-maintenance"></a>

If you no longer need to use a feature or service that requires a service\-linked role, we recommend that you delete that role\. That way you don’t have an unused entity that is not actively monitored or maintained\. However, you must clean up your service\-linked role before you can manually delete it\.

### Cleaning Up a Service\-Linked Role<a name="service-linked-role-review-before-delete-maintenance"></a>

Before you can use IAM to delete a service\-linked role, you must first terminate any Elastic Beanstalk environments that uses the role\.

**Note**  
If the Elastic Beanstalk service is using the role when you try to terminate the environments, then the termination might fail\. If that happens, wait for a few minutes and try the operation again\.

**To terminate an Elastic Beanstalk environment that uses the AWSServiceRoleForElasticBeanstalkMaintenance \(console\)**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. Choose **Actions**, and then choose **Terminate Environment**\.

1. In the **Confirm Termination** dialog box, type the environment name, and then choose **Terminate**\.

See [eb terminate](eb3-terminate.md) for details about terminating an Elastic Beanstalk environment using the EB CLI\.

See [TerminateEnvironment](https://docs.aws.amazon.com/elasticbeanstalk/latest/api/API_TerminateEnvironment.html) for details about terminating an Elastic Beanstalk environment using the API\.

### Manually Delete the Service\-Linked Role<a name="slr-manual-delete-maintenance"></a>

Use the IAM console, the IAM CLI, or the IAM API to delete the AWSServiceRoleForElasticBeanstalkMaintenance service\-linked role\. For more information, see [Deleting a Service\-Linked Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#delete-service-linked-role) in the *IAM User Guide*\.

## Supported Regions for Elastic Beanstalk Service\-Linked Roles<a name="slr-regions-maintenance"></a>

Elastic Beanstalk supports using service\-linked roles in all of the regions where the service is available\. For more information, see [Elastic Beanstalk Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#elasticbeanstalk_region)\.