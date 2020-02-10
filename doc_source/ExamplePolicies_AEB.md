# Example policies based on managed policies<a name="ExamplePolicies_AEB"></a>

This section demonstrates how to control user access to AWS Elastic Beanstalk and includes example policies that provide the required access for common scenarios\. These policies are derived from the Elastic Beanstalk managed policies\. For information about attaching managed policies to users and groups, see [Managing Elastic Beanstalk user policies](AWSHowTo.iam.managed-policies.md)\. 

In this scenario, Example Corp\. is a software company with three teams responsible for the company website: administrators who manage the infrastructure, developers who build the software for the website, and a QA team that tests the website\. To help manage permissions to their Elastic Beanstalk resources, Example Corp\. creates three groups to which members of each respective team belong: Admins, Developers, and Testers\. Example Corp\. wants the Admins group to have full access to all applications, environments, and their underlying resources so that they can create, troubleshoot, and delete all Elastic Beanstalk assets\. Developers require permissions to view all Elastic Beanstalk assets and to create and deploy application versions\. Developers should not be able to create new applications or environments or terminate running environments\. Testers need to view all Elastic Beanstalk resources to monitor and test applications\. The Testers should not be able to make changes to any Elastic Beanstalk resources\.

The following example policies provide the required permissions for each group\.

## Example 1: Admins group – All Elastic Beanstalk and related service APIs<a name="ExamplePolicies_AEB.admin"></a>

The following policy gives users permissions for all actions required to use Elastic Beanstalk\. This policy also allows Elastic Beanstalk to provision and manage resources on your behalf in the following services\. Elastic Beanstalk relies on these additional services to provision underlying resources when creating an environment\. 
+ Amazon Elastic Compute Cloud
+ Elastic Load Balancing
+ Auto Scaling
+ Amazon CloudWatch
+ Amazon Simple Storage Service
+ Amazon Simple Notification Service
+ Amazon Relational Database Service
+ AWS CloudFormation

Note that this policy is an example\. It gives a broad set of permissions to the AWS services that Elastic Beanstalk uses to manage applications and environments\. For example, `ec2:*` allows an AWS Identity and Access Management \(IAM\) user to perform any action on any Amazon EC2 resource in the AWS account\. These permissions are not limited to the resources that you use with Elastic Beanstalk\. As a best practice, you should grant individuals only the permissions they need to perform their duties\.

```
{
  "Version" : "2012-10-17",
  "Statement" : [
    {
      "Effect" : "Allow",
      "Action" : [
        "elasticbeanstalk:*",
        "ec2:*",
        "elasticloadbalancing:*",
        "autoscaling:*",
        "cloudwatch:*",
        "s3:*",
        "sns:*",
        "rds:*",
        "cloudformation:*"
      ],
      "Resource" : "*"
    }
  ]
}
```

## Example 2: Developers group – All but highly privileged operations<a name="ExamplePolicies_AEB.dev"></a>

The following policy denies permission to create applications and environments, and allows all other Elastic Beanstalk actions\.

Note that this policy is an example\. It gives a broad set of permissions to the AWS products that Elastic Beanstalk uses to manage applications and environments\. For example, `ec2:*` allows an IAM user to perform any action on any Amazon EC2 resource in the AWS account\. These permissions are not limited to the resources that you use with Elastic Beanstalk\. As a best practice, you should grant individuals only the permissions they need to perform their duties\.

```
{
  "Version" : "2012-10-17",
  "Statement" : [
    {
      "Action" : [
        "elasticbeanstalk:CreateApplication",
        "elasticbeanstalk:CreateEnvironment",
        "elasticbeanstalk:DeleteApplication",
        "elasticbeanstalk:RebuildEnvironment",
        "elasticbeanstalk:SwapEnvironmentCNAMEs",
        "elasticbeanstalk:TerminateEnvironment"],
      "Effect" : "Deny",
      "Resource" : "*"
    },
    {
      "Action" : [
        "elasticbeanstalk:*",
        "ec2:*",
        "elasticloadbalancing:*",
        "autoscaling:*",
        "cloudwatch:*",
        "s3:*",
        "sns:*",
        "rds:*",
        "cloudformation:*"],
      "Effect" : "Allow",
      "Resource" : "*"
    }
  ]
}
```

## Example 3: Testers – View only<a name="ExamplePolicies_AEB.tester"></a>

The following policy allows read\-only access to all applications, application versions, events, and environments\. It doesn't allow performing any actions\.

```
{
  "Version" : "2012-10-17",
  "Statement" : [
    {
      "Effect" : "Allow",
      "Action" : [
        "elasticbeanstalk:Check*",
        "elasticbeanstalk:Describe*",
        "elasticbeanstalk:List*",
        "elasticbeanstalk:RequestEnvironmentInfo",
        "elasticbeanstalk:RetrieveEnvironmentInfo",
        "ec2:Describe*",
        "elasticloadbalancing:Describe*",
        "autoscaling:Describe*",
        "cloudwatch:Describe*",
        "cloudwatch:List*",
        "cloudwatch:Get*",
        "s3:Get*",
        "s3:List*",
        "sns:Get*",
        "sns:List*",
        "rds:Describe*",
        "cloudformation:Describe*",
        "cloudformation:Get*",
        "cloudformation:List*",
        "cloudformation:Validate*",
        "cloudformation:Estimate*"
      ],
      "Resource" : "*"
    }
  ]
}
```