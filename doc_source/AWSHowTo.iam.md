# Using Elastic Beanstalk with AWS Identity and Access Management<a name="AWSHowTo.iam"></a>

AWS Identity and Access Management \(IAM\) helps you securely control access to your AWS resources\. This section includes reference materials for working with IAM policies, instance profiles, and service roles\.

For an overview of permissions, see [Service Roles, Instance Profiles, and User Policies](concepts-roles.md)\. For most environments, the service role and instance profile that the AWS Management Console prompts you to create when you launch your first environment have all of the permissions that you need\. Likewise, the [managed policies](AWSHowTo.iam.managed-policies.md) provided by Elastic Beanstalk for full access and read\-only access contain all of the user permissions required for daily use\.

The [IAM User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/IAMGettingStarted.html) provides in\-depth coverage of AWS permissions\.

**Topics**
+ [Managing Elastic Beanstalk Instance Profiles](iam-instanceprofile.md)
+ [Managing Elastic Beanstalk Service Roles](iam-servicerole.md)
+ [Controlling Access to Elastic Beanstalk](AWSHowTo.iam.managed-policies.md)
+ [Amazon Resource Name Format for Elastic Beanstalk](AWSHowTo.iam.policies.arn.md)
+ [Resources and Conditions for Elastic Beanstalk Actions](AWSHowTo.iam.policies.actions.md)
+ [Using Tags to Control Access to Elastic Beanstalk Resources](AWSHowTo.iam.policies.access-tags.md)
+ [Example Policies Based on Managed Policies](ExamplePolicies_AEB.md)
+ [Example Policies Based on Resource Permissions](AWSHowTo.iam.example.resource.md)