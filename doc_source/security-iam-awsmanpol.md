# AWS managed policies for AWS Elastic Beanstalk<a name="security-iam-awsmanpol"></a>







To add permissions to users, groups, and roles, it is easier to use AWS managed policies than to write policies yourself\. It takes time and expertise to [create IAM customer managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create-console.html) that provide your team with only the permissions they need\. To get started quickly, you can use our AWS managed policies\. These policies cover common use cases and are available in your AWS account\. For more information about AWS managed policies, see [AWS managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\.

AWS services maintain and update AWS managed policies\. You can't change the permissions in AWS managed policies\. Services occasionally add additional permissions to an AWS managed policy to support new features\. This type of update affects all identities \(users, groups, and roles\) where the policy is attached\. Services are most likely to update an AWS managed policy when a new feature is launched or when new operations become available\. Services do not remove permissions from an AWS managed policy, so policy updates won't break your existing permissions\.

Additionally, AWS supports managed policies for job functions that span multiple services\. For example, the **ViewOnlyAccess** AWS managed policy provides read\-only access to many AWS services and resources\. When a service launches a new feature, AWS adds read\-only permissions for new operations and resources\. For a list and descriptions of job function policies, see [AWS managed policies for job functions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html) in the *IAM User Guide*\.





## Elastic Beanstalk updates to AWS managed policies<a name="security-iam-awsmanpol-updates"></a>

View details about updates to AWS managed policies for Elastic Beanstalk since March 1, 2021\.




| Change | Description | Date | 
| --- | --- | --- | 
|  **AWSElasticBeanstalkService** – Deprecated  |  This policy has been replaced by `AWSElasticBeanstalkManagedUpdatesCustomerRolePolicy`\. This policy will be phased out on a future date\. As soon as the date is established, it will be published in this table\. When this policy is phased out, it will no longer be available for attachment to new IAM users, groups, or roles\. For more information, see [Managed service role policies](iam-servicerole.md#iam-servicerole-policy)\.  | To Be Determined | 
|  **AWSElasticBeanstalkReadOnlyAccess** – DeprecatedGovCloud \(US\) AWS Region  |  This policy has been replaced by `AWSElasticBeanstalkReadOnly`\. This policy will be phased out in the GovCloud \(US\) AWS Region\. When this policy is phased out, it will no longer be available for attachment to new IAM users, groups, or roles after June 17, 2021\.  For more information, see [User policies](AWSHowTo.iam.managed-policies.md)\.  | June 17, 2021 | 
|  **AWSElasticBeanstalkManagedUpdatesCustomerRolePolicy** –Updated  |  This policy was updated to allow Elastic Beanstalk to read attributes for EC2 Availability Zones\. It enables Elastic Beanstalk to provide more effective validation of your instance type selection across Availability Zones\. For more information, see [Managed service role policies](iam-servicerole.md#iam-servicerole-policy)\.  | June 16, 2021 | 
|  **AWSElasticBeanstalkFullAccess** – DeprecatedGovCloud \(US\) AWS Region  |  This policy has been replaced by `AdministratorAccess-AWSElasticBeanstalk`\. This policy will be phased out in the GovCloud \(US\) AWS Region\. When this policy is phased out, it will no longer be available for attachment to new IAM users, groups, or roles after June 10, 2021\.  For more information, see [User policies](AWSHowTo.iam.managed-policies.md)\.  | June 10, 2021 | 
|  The following managed policies were deprecated in all of the China AWS Regions: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/security-iam-awsmanpol.html)  |  The `AWSElasticBeanstalkFullAccess` policy has been replaced by `AdministratorAccess-AWSElasticBeanstalk`\. The `AWSElasticBeanstalkReadOnlyAccess` policy has been replaced by `AWSElasticBeanstalkReadOnly`\. These policies were phased out in all of the China AWS Regions\. These policies will no longer be available for attachment to new IAM users, groups, or roles after June 3, 2021\. For more information, see [User policies](AWSHowTo.iam.managed-policies.md)\.  | June 3, 2021 | 
|  The following managed policies were deprecated in all AWS Regions, except for China and GovCloud \(US\): [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/security-iam-awsmanpol.html)  |  The `AWSElasticBeanstalkFullAccess` policy has been replaced by `AdministratorAccess-AWSElasticBeanstalk`\. The `AWSElasticBeanstalkReadOnlyAccess` policy has been replaced by `AWSElasticBeanstalkReadOnly`\. These policies were phased out in all the AWS Regions, except for China and GovCloud \(US\)\. These policies will no longer be available for attachment to new IAM users, groups, or roles after April 16, 2021\.  For more information, see [User policies](AWSHowTo.iam.managed-policies.md)\.  | April 16, 2021 | 
|  The following managed policies were updated: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/security-iam-awsmanpol.html)  |  Both of these policies now support PassRole permissions in China AWS Regions\. For more information about `AdministratorAccess-AWSElasticBeanstalk`, see [User policies](AWSHowTo.iam.managed-policies.md)\. For more information about `AWSElasticBeanstalkManagedUpdatesCustomerRolePolicy`, see [Managed service role policies](iam-servicerole.md#iam-servicerole-policy)\.  | March 9, 2021 | 
|  **AWSElasticBeanstalkManagedUpdatesCustomerRolePolicy** – New policy  |  Elastic Beanstalk added a new policy to replace the `AWSElasticBeanstalkService` managed policy\. This new managed policy improves security for your resources by applying a more restrictive set of permissions\. For more information, see [Managed service role policies](iam-servicerole.md#iam-servicerole-policy)\.  | March 3, 2021 | 
|  Elastic Beanstalk started tracking changes  |  Elastic Beanstalk started tracking changes for AWS managed policies\.  | March 1, 2021 | 