# Streaming Logs to Amazon CloudWatch<a name="using-features.managing.cw"></a>

You can configure your AWS Elastic Beanstalk environment to stream Amazon CloudWatch logs from the Amazon EC2 instances that run your application\.

When you configure CloudWatch logs, Elastic Beanstalk creates CloudWatch log groups for proxy and deployment logs on the Amazon EC2 instances and transfers these log files to CloudWatch logs in real time\.

## Configuring Log Streaming<a name="configuring-cw-logs-console"></a>

You can enable log streaming in the Elastic Beanstalk console\.

**To configure log streaming in the Elastic Beanstalk console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the management page for your environment\.

1. Choose **Configuration**\.

1. In the **Software Configuration** section, choose the settings icon \( ![\[Edit\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/cog.png)\)\.

1. Under **CloudWatch logs**, configure the following settings\.

   + **Log streaming** – Check to enable log streaming\.

   + **Retention** – The number of days to retain logs in CloudWatch

   + **Lifecycle** – Set to **Delete logs upon termination** to delete logs from CloudWatch immediately if the environment is terminated, instead of waiting for them to expire\.

1. Choose **Apply**\.

After you enable log streaming, you can return to the configuration page to find a link to log group in the CloudWatch console\.

![\[CloudWatch logs settings\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/log-streaming-screen.png)

## The aws:elasticbeanstalk:cloudwatch:logs Namespace<a name="aws-elasticbeanstalk-cw-logs"></a>

Use the options in the `aws:elasticbeanstalk:cloudwatch:logs` namespace to configure CloudWatch logs\. These options can be set by using configuration files, a CLI or an SDK\.

+ `StreamLogs` – Set this option to `true` to create groups in CloudWatch logs for proxy and deployment logs, and stream logs from each instance in your environment\.

+ `DeleteOnTerminate` – Set this option to `true` to delete the log groups when the environment is terminated\. If `false`, the logs are kept `RetentionInDays` days\.

+ `RetentionInDays` – Set this option to the number of days to keep log events before they expire\.