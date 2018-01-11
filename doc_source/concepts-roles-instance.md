# Elastic Beanstalk Instance Profile<a name="concepts-roles-instance"></a>

An instance profile is an IAM role that is applied to instances launched in your Elastic Beanstalk environment\. When creating an Elastic Beanstalk environment, you specify the instance profile that is used when your instances:

+ Write logs to Amazon Simple Storage Service

+ In AWS X\-Ray integrated environments, upload debugging data to X\-Ray

+ In multicontainer Docker environments, coordinate container deployments with Amazon Elastic Container Service

+ In worker environments, read from an Amazon Simple Queue Service \(Amazon SQS\) queue

+ In worker environments, perform leader election with Amazon DynamoDB

+ In worker environments, publish instance health metrics to Amazon CloudWatch

The `AWSElasticBeanstalkWebTier` managed policy contains statements that allow instances in your environment to upload logs to Amazon S3 and send debugging information to X\-Ray:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "BucketAccess",
      "Action": [
        "s3:Get*",
        "s3:List*",
        "s3:PutObject"
      ],
      "Effect": "Allow",
      "Resource": [
        "arn:aws:s3:::elasticbeanstalk-*",
        "arn:aws:s3:::elasticbeanstalk-*/*"
      ]
    },
    {
      "Sid": "XRayAccess",
      "Action": [
        "xray:PutTraceSegments",
        "xray:PutTelemetryRecords"
      ],
      "Effect": "Allow",
      "Resource": "*"
    },
    {
      "Sid": "CloudWatchLogsAccess",
      "Action": [
        "logs:PutLogEvents",
        "logs:CreateLogStream",
        "logs:DescribeLogStreams",
        "logs:DescribeLogGroups"
      ],
      "Effect": "Allow",
      "Resource": [
        "arn:aws:logs:*:*:log-group:/aws/elasticbeanstalk*"
      ]
    }
  ]
}
```

Elastic Beanstalk also provides managed policies named `AWSElasticBeanstalkWorkerTier` and `AWSElasticBeanstalkMulticontainerDocker` for the other use cases\. Elastic Beanstalk attaches all of these policies to the default instance profile, `aws-elasticbeanstalk-ec2-role`, when you create an environment with the console or EB CLI\.

If your web application requires access to any other AWS services, add statements or managed policies to the instance profile that allow access to those services\.

For more information about instance profiles, see \.