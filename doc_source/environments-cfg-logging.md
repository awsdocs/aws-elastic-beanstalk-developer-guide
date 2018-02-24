# Viewing Logs from Your Elastic Beanstalk Environment's Amazon EC2 Instances<a name="environments-cfg-logging"></a>

AWS Elastic Beanstalk provides two ways for you to regularly view logs from the Amazon EC2 instances that run your application:

+ You can configure your Elastic Beanstalk environment to upload rotated instance logs to the environment's Amazon S3 bucket\.

+ You can configure the environment to stream instance logs to Amazon CloudWatch Logs\.

When you configure CloudWatch Logs, Elastic Beanstalk creates log groups for proxy and deployment logs on the Amazon EC2 instances and transfers these log files to CloudWatch Logs in real time\.

For more information about log viewing, see [Viewing Logs from Your Elastic Beanstalk Environment's Amazon EC2 Instances](using-features.logging.md)\.

## Configuring Log Viewing<a name="environments-cfg-logging-console"></a>

You can enable log rotation and log streaming in the Elastic Beanstalk console\.

**To configure log rotation and log streaming in the Elastic Beanstalk console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. Choose **Configuration**\.

1. On the **Software** configuration card, choose **Modify**\.

1. Under **S3 log storage**, choose **Rotate logs** to enable uploading rotated logs to Amazon S3\.

1. Under **CloudWatch logs**, configure the following settings\.

   + **Log streaming** – Choose to enable log streaming\.

   + **Retention** – Specify the number of days to retain logs in CloudWatch Logs\.

   + **Lifecycle** – Set to **Delete logs upon termination** to delete logs from CloudWatch Logs immediately if the environment is terminated, instead of waiting for them to expire\.

1. Choose **Apply**\.

After you enable log streaming, you can return to the configuration page to find a link to the log group in the CloudWatch console\.

![\[CloudWatch logs settings\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/log-streaming-screen.png)

## Log Viewing Namespaces<a name="environments-cfg-logging-namespaces"></a>

Settings related to log viewing are in the following namespaces:

+ [`aws:elasticbeanstalk:hostmanager`](command-options-general.md#command-options-general-elasticbeanstalkhostmanager) – Configure uploading rotated logs to Amazon S3\.

+ [`aws:elasticbeanstalk:cloudwatch:logs`](command-options-general.md#command-options-general-cloudwatchlogs) – Configure log streaming to CloudWatch\.