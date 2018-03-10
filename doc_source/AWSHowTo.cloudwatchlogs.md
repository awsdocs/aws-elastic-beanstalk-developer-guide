# Using Elastic Beanstalk with Amazon CloudWatch Logs<a name="AWSHowTo.cloudwatchlogs"></a>

With CloudWatch Logs, you can monitor and archive your Elastic Beanstalk application, system, and custom log files\. Furthermore, you can configure alarms that make it easier for you to take actions in response to specific log stream events that your metric filters extract\. The CloudWatch Logs agent installed on each Amazon EC2 in your environment publishes metric data points to the CloudWatch service for each log group you configure\. Each log group applies its own filter patterns to determine what log stream events to send to CloudWatch as data points\. Log streams that belong to the same log group share the same retention, monitoring, and access control settings\. You can configure Elastic Beanstalk to automatically stream logs to the CloudWatch service, as described in [Streaming CloudWatch Logs](#AWSHowTo.cloudwatchlogs.streaming)\. For more information about CloudWatch Logs, including terminology and concepts, go to [Monitoring System, Application, and Custom Log Files](http://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/WhatIsCloudWatchLogs.html)\.

The following figure displays graphs on the **Monitoring** page for an environment that is configured with CloudWatch Logs integration\. The example metrics in this environment are named **CWLHttp4xx** and **CWLHttp5xx**\. In the image, the **CWLHttp4xx** metric has triggered an alarm according to conditions specified in the configuration files\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-monitor-metrics-cwl.png)

The following figure displays graphs on the **Alarms** page for the example alarms named **AWSEBCWLHttp4xxPercentAlarm** and **AWSEBCWLHttp5xxCountAlarm** that correspond to the **CWLHttp4xx** and **CWLHttp5xx** metrics, respectively\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-alarms-cwl.png)

## Streaming CloudWatch Logs<a name="AWSHowTo.cloudwatchlogs.streaming"></a>

Since Elastic Beanstalk can spin up Amazon EC2 instances on demand \(provided you have enabled that feature\), Elastic Beanstalk provides another option so that you can stream log entries from those Amazon EC2 instances to CloudWatch\. To enable this feature, select **Enabled** for **Log streaming**, set **Retention** to the number of days to save the logs, and select the **Lifecycle** setting for whether the logs are saved after the instance is terminated, as shown in the following figure, which saves the logs for 7 days and keeps the logs after terminating the instance\. You can also enable these settings using the [`eb logs`](eb3-logs.md) command\. Note that this feature is only available in platform configurations since [this release](https://aws.amazon.com/releasenotes/6677534638371416)\.

Note also that once you enable CloudWatch logs, you'll see the **View in CloudWatch Console** link\. Click that link to see your CloudWatch logs in the CloudWatch console\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/log-streaming-screen.png)

**Note**  
If you do not have the *AWSElasticBeanstalkWebTier* or *AWSElasticBeanstalkWorkerTier* Elastic Beanstalk managed policy in your [Elastic Beanstalk instance profile](concepts-roles-instance.md), you must add the following to your profile to enable this feature\.  

```
{
  "Version": "2012-10-17",
  "Statement": [
  {
    "Effect": "Allow",
    "Action": [
      "logs:PutLogEvents",
      "logs:CreateLogStream"
    ],
    "Resource": [
    "arn:aws:logs:*:*:log-group:/aws/elasticbeanstalk*"
    ]
  }
  ]
}
```

Elastic Beanstalk installs a CloudWatch log agent with the default configuration settings on each instance it creates\. Learn more at [CloudWatch Logs Agent Reference](http://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AgentReference.html)\.

Different platforms stream different logs\. The following table lists the logs, by platform\.


****  

|  Platform  |  Logs  | 
| --- | --- | 
|   `Docker`   |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/AWSHowTo.cloudwatchlogs.html)  | 
|   `Multi-Docker(generic)`   |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/AWSHowTo.cloudwatchlogs.html)  | 
|   `Glass fish (Preconfigured docker)`   |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/AWSHowTo.cloudwatchlogs.html)  | 
|   `Go (Preconfigured docker)`   |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/AWSHowTo.cloudwatchlogs.html)  | 
|   `Python (Preconfigured docker)`   |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/AWSHowTo.cloudwatchlogs.html)  | 
|   `Go`   |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/AWSHowTo.cloudwatchlogs.html)  | 
|   `Java`   |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/AWSHowTo.cloudwatchlogs.html)  | 
|   `Tomcat`   |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/AWSHowTo.cloudwatchlogs.html)  | 
|   `.NET on Windows Server`   |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/AWSHowTo.cloudwatchlogs.html)  | 
|   `Node.js`   |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/AWSHowTo.cloudwatchlogs.html)  | 
|   `Php`   |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/AWSHowTo.cloudwatchlogs.html)  | 
|   `Python`   |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/AWSHowTo.cloudwatchlogs.html)  | 
|   `Ruby (Puma)`   |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/AWSHowTo.cloudwatchlogs.html)  | 
|   `Ruby (passenger)`   |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/AWSHowTo.cloudwatchlogs.html)  | 

You can also enable CloudWatch logs using the [ eb logs \-\-cloudwatch enable](eb3-logs.md) command\.

## Setting Up CloudWatch Logs Integration with Configuration Files<a name="AWSHowTo.cloudwatchlogs.files"></a>

When you create or update an environment, you can use the sample configuration files in the following list to set up and configure integration with CloudWatch Logs\. You can include the `.zip` file that contains following configuration files or the extracted configuration files in the `.ebextensions` directory at the top level of your application source bundle\. Use the appropriate files for the web server for your platform\. For more information about the web server used by each platform, see [Elastic Beanstalk Supported Platforms](concepts.platforms.md)\.

Before you can configure integration with CloudWatch Logs using configuration files, you must set up IAM permissions to use with the CloudWatch Logs agent\. You can attach the following custom policy to the [instance profile](concepts-roles-instance.md) that you assign to your environment:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:GetLogEvents",
        "logs:PutLogEvents",
        "logs:DescribeLogGroups",
        "logs:DescribeLogStreams",
        "logs:PutRetentionPolicy"
      ],
      "Resource": [
        "arn:aws:logs:us-west-2:*:*"
      ]
    }
  ]
}
```

Replace the region in the above policy with the region in which you launch your environment\.

You can download the configuration files at the following locations:

+ [Tomcat \(Java\) configuration files](samples/aws_eb_cloudwatchlogs-apache-tomcat.zip)

+ [Apache \(PHP and Python\) configuration files](samples/aws_eb_cloudwatchlogs-apache.zip)

+ [nginx \(Go, Ruby, Node\.js, and Docker\) configuration files](samples/aws_eb_cloudwatchlogs-nginx.zip)

Each `.zip` file contains the following configuration files:

+ `cwl-setup.config` – This file installs the CloudWatch Logs agent on each Amazon EC2 instance in your environment and then configures the agent\. This file also creates the `general.conf` file when Elastic Beanstalk launches the instance\. You can use the `cwl-setup.config` file without any modifications\. 

  If you prefer, you can manually set up the CloudWatch Logs agent on a new instance as explained in either [Quick Start: Install and Configure the CloudWatch Logs Agent on a New EC2 Instance](http://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/EC2NewInstanceCWL.html) \(for new instances\) or [Quick Start: Install and Configure the CloudWatch Logs Agent on an Existing EC2 Instance](http://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/QuickStartEC2Instance.html) \(for existing instances\) in the *Amazon CloudWatch Developer Guide*\.

+ `cwl-webrequest-metrics.config` – This file specifies which logs the CloudWatch Logs agent monitors\. The file also specifies the metric filters the CloudWatch Logs agent applies to each log that it monitors\. Metric filters include filter patterns that map to the space\-delimited entries in your log files\. \(If you have custom logs, update or replace the example filter patterns in this example configuration file as needed\.\)

  Metric filters also include metric transformations that specify what metric name and value to use when the CloudWatch Logs agent sends metric data points to the CloudWatch service\. The CloudWatch Logs agent sends metric data points based on whether any entries in the web server access log file match the filter patterns\.

  Finally, the configuration file also includes an alarm action to send a message to an Amazon Simple Notification Service topic, if you created one for your environment, when the alarm conditions specified in the `cwl-setup.config` file are met\. For more information about filter patterns, see [Filter and Pattern Syntax](http://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/FilterAndPatternSyntax.html) in the *Amazon CloudWatch Developer Guide*\. For more information about Amazon SNS, go to the [Amazon Simple Notification Service Developer Guide](http://docs.aws.amazon.com/sns/latest/dg/welcome.html)\. For more information about managing alarms from the Elastic Beanstalk management console, see [Manage Alarms](using-features.alarms.md)\.
**Note**  
CloudWatch costs are applied to your AWS account for any alarms that you use\.

+ `eb-logs.config` – This file sets up the CloudWatch Logs log files for the CloudWatch Logs agent\. This configuration file also ensures that log files are copied to Amazon S3 as part of log rotation\. You can use this file without any modifications\.

## Troubleshooting CloudWatch Logs Integration<a name="AWSHowTo.cloudwatchlogs.troubleshoot"></a>

If Elastic Beanstalk cannot launch your environment when you try to integrate Elastic Beanstalk with CloudWatch Logs, you can investigate the following common issues:

+ Your IAM role lacks the required IAM permissions\.

+ You attempted to launch an environment in a region where CloudWatch Logs is not supported\.

+ Access logs do not exist at the path specified in the `cwl-webrequest-metrics.config` file \(/var/log/httpd/elasticbeanstalk\-access\_log\)\.