# Elastic Beanstalk user policy<a name="concepts-roles-user"></a>

Create IAM users for each person who uses Elastic Beanstalk to avoid using your root account or sharing credentials\. For increased security, only grant these users permission to access services and features that they need\.

Elastic Beanstalk requires permissions not only for its own API actions, but for several other AWS services as well\. Elastic Beanstalk uses user permissions to launch all of the resources in an environment, including EC2 instances, an Elastic Load Balancing load balancer, and an Auto Scaling group\. Elastic Beanstalk also uses user permissions to save logs and templates to Amazon Simple Storage Service \(Amazon S3\), send notifications to Amazon SNS, assign instance profiles, and publish metrics to CloudWatch\. Elastic Beanstalk requires AWS CloudFormation permissions to orchestrate resource deployments and updates\. It also requires Amazon RDS permissions to create databases when needed, and Amazon SQS permissions to create queues for worker environments\.

The following policy allows access to the actions used to create and manage Elastic Beanstalk environments\. This policy is available in the IAM console as a managed policy named `AWSElasticBeanstalkFullAccess`\. You can apply the managed policy to an IAM user or group to grant permission to use Elastic Beanstalk, or create your own policy that excludes permissions that are not needed by your users\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "elasticbeanstalk:*",
        "ec2:*",
        "ecs:*",
        "ecr:*",
        "elasticloadbalancing:*",
        "autoscaling:*",
        "cloudwatch:*",
        "s3:*",
        "sns:*",
        "cloudformation:*",
        "dynamodb:*",
        "rds:*",
        "sqs:*",
        "logs:*",
        "iam:GetPolicyVersion",
        "iam:GetRole",
        "iam:PassRole",
        "iam:ListRolePolicies",
        "iam:ListAttachedRolePolicies",
        "iam:ListInstanceProfiles",
        "iam:ListRoles",
        "iam:ListServerCertificates",
        "acm:DescribeCertificate",
        "acm:ListCertificates",
        "codebuild:CreateProject",
        "codebuild:DeleteProject",
        "codebuild:BatchGetBuilds",
        "codebuild:StartBuild"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "iam:AddRoleToInstanceProfile",
        "iam:CreateInstanceProfile",
        "iam:CreateRole"
      ],
      "Resource": [
        "arn:aws:iam::*:role/aws-elasticbeanstalk*",
        "arn:aws:iam::*:instance-profile/aws-elasticbeanstalk*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "iam:CreateServiceLinkedRole"
      ],
      "Resource": [
        "arn:aws:iam::*:role/aws-service-role/autoscaling.amazonaws.com/AWSServiceRoleForAutoScaling*"
      ],
      "Condition": {
        "StringLike": {
          "iam:AWSServiceName": "autoscaling.amazonaws.com"
        }
      }
    },
    {
      "Effect": "Allow",
      "Action": [
        "iam:CreateServiceLinkedRole"
      ],
      "Resource": [
        "arn:aws:iam::*:role/aws-service-role/elasticbeanstalk.amazonaws.com/AWSServiceRoleForElasticBeanstalk*"
      ],
      "Condition": {
        "StringLike": {
          "iam:AWSServiceName": "elasticbeanstalk.amazonaws.com"
        }
      }
    },
    {
      "Effect": "Allow",
      "Action": [
        "iam:AttachRolePolicy"
      ],
      "Resource": "*",
      "Condition": {
        "StringLike": {
          "iam:PolicyArn": [
            "arn:aws:iam::aws:policy/AWSElasticBeanstalk*",
            "arn:aws:iam::aws:policy/service-role/AWSElasticBeanstalk*"
          ]
        }
      }
    }
  ]
}
```

Elastic Beanstalk also provides a read\-only managed policy named `AWSElasticBeanstalkReadOnlyAccess`\. This policy allows a user to view, but not modify or create, Elastic Beanstalk environments\.

For more information about user policies, see [Managing Elastic Beanstalk user policies](AWSHowTo.iam.managed-policies.md)\.