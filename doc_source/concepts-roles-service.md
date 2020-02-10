# Elastic Beanstalk service role<a name="concepts-roles-service"></a>

A service role is the IAM role that Elastic Beanstalk assumes when calling other services on your behalf\. For example, Elastic Beanstalk uses the service role that you specify when creating an Elastic Beanstalk environment when it calls Amazon Elastic Compute Cloud \(Amazon EC2\), Elastic Load Balancing, and Amazon EC2 Auto Scaling APIs to gather information about the health of its AWS resources for [enhanced health monitoring](health-enhanced.md)\.

The `AWSElasticBeanstalkEnhancedHealth` managed policy contains all of the permissions that Elastic Beanstalk needs to monitor environment health:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "elasticloadbalancing:DescribeInstanceHealth",
                "elasticloadbalancing:DescribeLoadBalancers",
                "elasticloadbalancing:DescribeTargetHealth",
                "ec2:DescribeInstances",
                "ec2:DescribeInstanceStatus",
                "ec2:GetConsoleOutput",
                "ec2:AssociateAddress",
                "ec2:DescribeAddresses",
                "ec2:DescribeSecurityGroups",
                "sqs:GetQueueAttributes",
                "sqs:GetQueueUrl",
                "autoscaling:DescribeAutoScalingGroups",
                "autoscaling:DescribeAutoScalingInstances",
                "autoscaling:DescribeScalingActivities",
                "autoscaling:DescribeNotificationConfigurations",
                "sns:Publish"
            ],
            "Resource": [
                "*"
            ]
        }
    ]
}
```

This policy also includes Amazon SQS actions to allow Elastic Beanstalk to monitor queue activity for worker environments\. 

When you create an environment using the Elastic Beanstalk console, Elastic Beanstalk prompts you to create a service role named `aws-elasticbeanstalk-service-role` with the default set of permissions and a trust policy that allows Elastic Beanstalk to assume the service role\. If you enable [managed platform updates](environment-platform-update-managed.md), Elastic Beanstalk attaches another policy with permissions that enable that feature\.

Similarly, when you create an environment using the [eb create](eb3-create.md) command of the Elastic Beanstalk Command Line Interface \(EB CLI\) and don't specify a service role through the `--service-role` option, Elastic Beanstalk creates the default service role `aws-elasticbeanstalk-service-role`\. If the default service role already exists, Elastic Beanstalk uses it for the new environment\.

When you create an environment by using the `CreateEnvironment` action of the Elastic Beanstalk API, and don't specify a service role, Elastic Beanstalk creates a monitoring service\-linked role\. This is a unique type of service role that is predefined by Elastic Beanstalk to include all the permissions that the service requires to call other AWS services on your behalf\. The service\-linked role is associated with your account\. Elastic Beanstalk creates it once, then reuses it when creating additional environments\. You can also use IAM to create your account's monitoring service\-linked role in advance\. When your account has a monitoring service\-linked role, you can use it to create an environment by using the Elastic Beanstalk API, the Elastic Beanstalk console, or the EB CLI\. For details about using service\-linked roles with Elastic Beanstalk environments, see [Using service\-linked roles for Elastic Beanstalk](using-service-linked-roles.md)\.

For more information about service roles, see [Managing Elastic Beanstalk service roles](iam-servicerole.md)\.