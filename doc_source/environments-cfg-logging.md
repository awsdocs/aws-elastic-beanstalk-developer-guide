# Viewing Your Elastic Beanstalk Environment Logs<a name="environments-cfg-logging"></a>

AWS Elastic Beanstalk provides two ways to regularly view logs from the Amazon EC2 instances that run your application:
+ Configure your Elastic Beanstalk environment to upload rotated instance logs to the environment's Amazon S3 bucket\.
+ Configure the environment to stream instance logs to Amazon CloudWatch Logs\.

When you configure instance log streaming to CloudWatch Logs, Elastic Beanstalk creates CloudWatch Logs log groups for proxy and deployment logs on the Amazon EC2 instances, and transfers these log files to CloudWatch Logs in real time\. For more information about instance logs, see [Viewing Logs from Amazon EC2 Instances in Your Elastic Beanstalk Environment](using-features.logging.md)\.

In addition to instance logs, if you enable [enhanced health](health-enhanced.md) for your environment, you can configure the environment to stream health information to CloudWatch Logs\. When the environment's health status changes, Elastic Beanstalk adds a record to a health log group, with the new status and a description of the cause of the change\. For information about environment health streaming, see [Streaming Elastic Beanstalk Environment Health Information to Amazon CloudWatch Logs](AWSHowTo.cloudwatchlogs.envhealth.md)\.

## Configuring Instance Log Viewing<a name="environments-cfg-logging-console"></a>

To view instance logs, you can enable instance log rotation and log streaming in the Elastic Beanstalk console\.

**To configure instance log rotation and log streaming in the Elastic Beanstalk console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. Choose **Configuration**\.

1. In the **Software** configuration category, choose **Modify**\.

1. Under **S3 log storage**, choose **Rotate logs** to enable uploading rotated logs to Amazon S3\.

1. Under **Instance log streaming to CloudWatch Logs**, configure the following settings:
   + **Log streaming** – Choose to enable log streaming\.
   + **Retention** – Specify the number of days to retain logs in CloudWatch Logs\.
   + **Lifecycle** – Set to **Delete logs upon termination** to delete logs from CloudWatch Logs immediately if the environment is terminated, instead of waiting for them to expire\.

1. Choose **Apply**\.

After you enable log streaming, you can return to the **Software** configuration category or page and find the **Log Groups** link\. Click this link to see your instance logs in the CloudWatch console\.

![\[CloudWatch logs settings for instance log streaming\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/log-streaming-screen.png)

## Configuring Environment Health Log Viewing<a name="environments-cfg-logging-health-console"></a>

To view environment health logs, you can enable environment health log streaming in the Elastic Beanstalk console\.

**To configure environment health log streaming in the Elastic Beanstalk console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. Choose **Configuration**\.

1. In the **Monitoring** configuration category, choose **Modify**\.

1. Under **Health event streaming to CloudWatch Logs**, configure the following settings:
   + **Log streaming** – Choose to enable log streaming\.
   + **Retention** – Specify the number of days to retain logs in CloudWatch Logs\.
   + **Lifecycle** – Set to **Delete logs upon termination** to delete logs from CloudWatch Logs immediately if the environment is terminated, instead of waiting for them to expire\.

1. Choose **Apply**\.

After you enable log streaming, you can return to the **Monitoring** configuration category or page and find the **Log Group** link\. Click this link to see your environment health logs in the CloudWatch console\.

![\[CloudWatch logs settings for health event streaming\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/log-streaming-health-screen.png)

## Log Viewing Namespaces<a name="environments-cfg-logging-namespaces"></a>

The following namespaces contain settings for log viewing:
+ [`aws:elasticbeanstalk:hostmanager`](command-options-general.md#command-options-general-elasticbeanstalkhostmanager) – Configure uploading rotated logs to Amazon S3\.
+ [`aws:elasticbeanstalk:cloudwatch:logs`](command-options-general.md#command-options-general-cloudwatchlogs) – Configure instance log streaming to CloudWatch\.
+ [`aws:elasticbeanstalk:cloudwatch:logs:health`](command-options-general.md#command-options-general-cloudwatchlogs-health) – Configure environment health streaming to CloudWatch\.