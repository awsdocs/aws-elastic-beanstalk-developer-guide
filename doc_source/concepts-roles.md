# Service roles, instance profiles, and user policies<a name="concepts-roles"></a>

When you create an environment, AWS Elastic Beanstalk prompts you to provide two AWS Identity and Access Management \(IAM\) roles: a service role and an instance profile\. The [service role](concepts-roles-service.md) is assumed by Elastic Beanstalk to use other AWS services on your behalf\. The [instance profile](concepts-roles-instance.md) is applied to the instances in your environment and allows them to retrieve [application versions](concepts.md#concepts-version) from Amazon Simple Storage Service \(Amazon S3\), upload logs to Amazon S3, and perform other tasks that vary depending on the environment type and platform\.

The best way to get a properly configured service role and instance profile is to [create an environment running a sample application](using-features.environments.md) in the Elastic Beanstalk console or by using the Elastic Beanstalk Command Line Interface \(EB CLI\)\. When you create an environment, the clients create the required roles and assign them [managed policies](AWSHowTo.iam.managed-policies.md) that include all of the necessary permissions\.

In addition to the two roles that you assign to your environment, you can also create [user policies](concepts-roles-user.md) and apply them to IAM users and groups in your account to allow users to create and manage Elastic Beanstalk applications and environments\. Elastic Beanstalk provides managed policies for full access and read\-only access\.

You can create your own instance profiles and user policies for advanced scenarios\. If your instances need to access services that are not included in the default policies, you can add additional policies to the default or create a new one\. You can also create more restrictive user policies if the managed policy is too permissive\. See the [AWS Identity and Access Management User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/IAMGettingStarted.html) for in\-depth coverage of AWS permissions\.

**Topics**
+ [Elastic Beanstalk service role](concepts-roles-service.md)
+ [Elastic Beanstalk instance profile](concepts-roles-instance.md)
+ [Elastic Beanstalk user policy](concepts-roles-user.md)