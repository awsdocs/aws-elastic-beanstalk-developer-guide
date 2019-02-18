# Controlling Access to Elastic Beanstalk Resources Using Tags<a name="AWSHowTo.iam.policies.access-tags"></a>

Conditions in AWS Identity and Access Management \(IAM\) user policy statements are part of the syntax that you use to specify permissions to resources that Elastic Beanstalk actions need to complete\. For details about specifying policy statement conditions, see [Resources and Conditions for Elastic Beanstalk Actions](AWSHowTo.iam.policies.actions.md)\.

Using tags in conditions is one way to control access to resources and requests\. For information about tagging Elastic Beanstalk resources, see [Tagging Resources in Your Elastic Beanstalk Environment](using-features.tagging.md)\. This topic discusses tag\-based access control\.

Tags can be attached to the resource or passed in the request to services that support tagging\. In Elastic Beanstalk, environments can have tags, and some actions can include tags\. When you create an IAM policy, you can use tag condition keys to control:
+ Which users can perform actions on an environment, based on tags that it already has\.
+ What tags can be passed in an action's request\.
+ Whether specific tag keys can be used in a request\.

For the complete syntax and semantics of tag condition keys, see [Controlling Access Using Tags](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_tags.html) in the *IAM User Guide*\.

The following examples demonstrate how to specify tag conditions in policies for Elastic Beanstalk users\.

**Example 1: Limit actions based on resource tags**  <a name="admins_policy"></a>
The following policy denies unauthorized users permission to perform actions on Elastic Beanstalk production environments\. To do that, it denies specific actions if the environment has a tag named `stage` with one of the values `gamma` or `prod`\. A customer's administrator can attach this IAM policy to unauthorized IAM users, in addition to the managed user policy\.  

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": [
        "elasticbeanstalk:AddTags",
        "elasticbeanstalk:RemoveTags",
        "elasticbeanstalk:DescribeEnvironments",
        "elasticbeanstalk:TerminateEnvironment",
        "elasticbeanstalk:UpdateEnvironment",
        "elasticbeanstalk:ListTagsForResource"
      ],
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "aws:ResourceTag/stage": ["gamma", "prod"]
        }
      }
    }
  ]
}
```

**Example 2: Limit actions based on tags in the request**  <a name="admins_policy"></a>
The following policy denies unauthorized users permission to create Elastic Beanstalk production environments\. To do that, it denies the `CreateEnvironment` action if the request specifies a tag named `stage` with one of the values `gamma` or `prod`\. In addition, the policy prevents these unauthorized users from tampering with the stage of production environments by not allowing tag modification actions to include these same tag values or to remove the `stage` tag altogether\. A customer's administrator can attach this IAM policy to unauthorized IAM users, in addition to the managed user policy\.  

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": [
        "elasticbeanstalk:CreateEnvironment",
        "elasticbeanstalk:AddTags"
      ],
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "aws:RequestTag/stage": ["gamma", "prod"]
        },
        "ForAllValues:StringEquals": {
          "aws:TagKeys": ["stage"]
        }
      }
    },
    {
      "Effect": "Deny",
      "Action": [
        "elasticbeanstalk:RemoveTags"
      ],
      "Resource": "*",
      "Condition": {
        "ForAllValues:StringEquals": {
          "aws:TagKeys": ["stage"]
        }
      }
    }
  ]
}
```