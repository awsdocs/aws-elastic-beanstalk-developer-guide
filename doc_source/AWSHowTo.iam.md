# Using Elastic Beanstalk with AWS Identity and Access Management<a name="AWSHowTo.iam"></a>

AWS Identity and Access Management \(IAM\) helps you securely control access to your AWS resources\. This section includes reference materials for working with IAM policies, instance profiles, and service roles\.

For an overview of permissions, see [Service roles, instance profiles, and user policies](concepts-roles.md)\. For most environments, the service role and instance profile that the Elastic Beanstalk console prompts you to create when you launch your first environment have all of the permissions that you need\. Likewise, the [managed policies](AWSHowTo.iam.managed-policies.md) provided by Elastic Beanstalk for full access and read\-only access contain all of the user permissions required for daily use\.

The [IAM User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/IAMGettingStarted.html) provides in\-depth coverage of AWS permissions\.

**Topics**
+ [Managing Elastic Beanstalk instance profiles](iam-instanceprofile.md)
+ [Managing Elastic Beanstalk service roles](iam-servicerole.md)
+ [Using service\-linked roles for Elastic Beanstalk](using-service-linked-roles.md)
+ [Managing Elastic Beanstalk user policies](AWSHowTo.iam.managed-policies.md)
+ [Amazon resource name format for Elastic Beanstalk](AWSHowTo.iam.policies.arn.md)
+ [Resources and conditions for Elastic Beanstalk actions](AWSHowTo.iam.policies.actions.md)
+ [Using tags to control access to Elastic Beanstalk resources](AWSHowTo.iam.policies.access-tags.md)
+ [Example policies based on managed policies](ExamplePolicies_AEB.md)
+ [Example policies based on resource permissions](AWSHowTo.iam.example.resource.md)