# Using Elastic Beanstalk with Amazon CloudWatch Logs<a name="AWSHowTo.cloudwatchlogs"></a>

With CloudWatch Logs, you can monitor and archive your Elastic Beanstalk application, system, and custom log files from Amazon EC2 instances of your environments\. You can also configure alarms that make it easier for you to react to specific log stream events that your metric filters extract\. The CloudWatch Logs agent installed on each Amazon EC2 instance in your environment publishes metric data points to the CloudWatch service for each log group you configure\. Each log group applies its own filter patterns to determine what log stream events to send to CloudWatch as data points\. Log streams that belong to the same log group share the same retention, monitoring, and access control settings\. You can configure Elastic Beanstalk to automatically stream logs to the CloudWatch service, as described in [Streaming instance logs to CloudWatch Logs](#AWSHowTo.cloudwatchlogs.streaming)\. For more information about CloudWatch Logs, including terminology and concepts, see the [Amazon CloudWatch Logs User Guide](http://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/WhatIsCloudWatchLogs.html)\.

In addition to instance logs, if you enable [enhanced health](health-enhanced.md) for your environment, you can configure the environment to stream health information to CloudWatch Logs\. See [Streaming Elastic Beanstalk environment health information to Amazon CloudWatch Logs](AWSHowTo.cloudwatchlogs.envhealth.md)\.

The following figure shows the **Monitoring** page and graphs for an environment that is configured with CloudWatch Logs integration\. The example metrics in this environment are named **CWLHttp4xx** and **CWLHttp5xx**\. One of the graphs shows that the **CWLHttp4xx** metric has triggered an alarm based on conditions specified in the configuration files\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-monitor-metrics-cwl.png)

The following figure shows the **Alarms** page and graphs for the example alarms named **AWSEBCWLHttp4xxPercentAlarm** and **AWSEBCWLHttp5xxCountAlarm** that correspond to the **CWLHttp4xx** and **CWLHttp5xx** metrics, respectively\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-alarms-cwl.png)

**Topics**
+ [Prerequisites to instance log streaming to CloudWatch Logs](#AWSHowTo.cloudwatchlogs.prereqs)
+ [How Elastic Beanstalk sets up CloudWatch Logs](#AWSHowTo.cloudwatchlogs.loggroups)
+ [Streaming instance logs to CloudWatch Logs](#AWSHowTo.cloudwatchlogs.streaming)
+ [Troubleshooting CloudWatch Logs integration](#AWSHowTo.cloudwatchlogs.troubleshoot)
+ [Streaming Elastic Beanstalk environment health information to Amazon CloudWatch Logs](AWSHowTo.cloudwatchlogs.envhealth.md)

## Prerequisites to instance log streaming to CloudWatch Logs<a name="AWSHowTo.cloudwatchlogs.prereqs"></a>

To enable streaming of logs from your environment's Amazon EC2 instances to CloudWatch Logs, you must meet the following conditions\.
+ *Platform* – Because this feature is only available in platform versions released on or after [this release](https://aws.amazon.com/releasenotes/6677534638371416), if you are using an earlier platform version, update your environment to a current one\.
+ If you don't have the *AWSElasticBeanstalkWebTier* or *AWSElasticBeanstalkWorkerTier* Elastic Beanstalk managed policy in your [Elastic Beanstalk instance profile](concepts-roles-instance.md), you must add the following to your profile to enable this feature\.

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
      "*"
      ]
    }
    ]
  }
  ```

## How Elastic Beanstalk sets up CloudWatch Logs<a name="AWSHowTo.cloudwatchlogs.loggroups"></a>

Elastic Beanstalk installs a CloudWatch log agent with the default configuration settings on each instance it creates\. Learn more in the [CloudWatch Logs Agent Reference](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AgentReference.html)\.

When you enable instance log streaming to CloudWatch Logs, Elastic Beanstalk sends log files from your environment's instances to CloudWatch Logs\. Different platforms stream different logs\. The following table lists the logs, by platform\.


****  

|  Platform  |  Logs  | 
| --- | --- | 
|  Docker  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/AWSHowTo.cloudwatchlogs.html)  | 
|  Go Corretto \.NET Core on Linux  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/AWSHowTo.cloudwatchlogs.html)  | 
|  Node\.js Python  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/AWSHowTo.cloudwatchlogs.html)  | 
|  Tomcat PHP  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/AWSHowTo.cloudwatchlogs.html)  | 
|  \.NET on Windows Server  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/AWSHowTo.cloudwatchlogs.html)  | 
|  Ruby  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/AWSHowTo.cloudwatchlogs.html)  | 

### Log files on Amazon Linux AMI platforms<a name="AWSHowTo.cloudwatchlogs.loggroups.alami"></a>

The following table lists the log files streamed from instances on platform branches based on Amazon Linux AMI \(preceding Amazon Linux 2\), by platform\.


****  

|  Platform  |  Logs  | 
| --- | --- | 
|  Docker  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/AWSHowTo.cloudwatchlogs.html)  | 
|  Multicontainer Docker\(generic\)  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/AWSHowTo.cloudwatchlogs.html)  | 
|  Glassfish \(Preconfigured Docker\)  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/AWSHowTo.cloudwatchlogs.html)  | 
|  Go \(Preconfigured Docker\)  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/AWSHowTo.cloudwatchlogs.html)  | 
|  Python \(Preconfigured Docker\)  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/AWSHowTo.cloudwatchlogs.html)  | 
|  Go  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/AWSHowTo.cloudwatchlogs.html)  | 
|  Java SE  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/AWSHowTo.cloudwatchlogs.html)  | 
|  Tomcat  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/AWSHowTo.cloudwatchlogs.html)  | 
|  Node\.js  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/AWSHowTo.cloudwatchlogs.html)  | 
|  PHP  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/AWSHowTo.cloudwatchlogs.html)  | 
|  Python  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/AWSHowTo.cloudwatchlogs.html)  | 
|  Ruby \(Puma\)  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/AWSHowTo.cloudwatchlogs.html)  | 
|  Ruby \(Passenger\)  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/AWSHowTo.cloudwatchlogs.html)  | 

Elastic Beanstalk configures log groups in CloudWatch Logs for the various log files that it streams\. To retrieve specific log files from CloudWatch Logs, you have to know the name of the corresponding log group\. The log group naming scheme depends on the platform's operating system\.

For Linux platforms, prefix the on\-instance log file location with `/aws/elasticbeanstalk/environment_name` to get the log group name\. For example, to retrieve the file `/var/log/nginx/error.log`, specify the log group `/aws/elasticbeanstalk/environment_name/var/log/nginx/error.log`\.

For Windows platforms, see the following table for the log group corresponding to each log file\.


|  On\-instance log file  |  Log group  | 
| --- | --- | 
|  `C:\Program Files\Amazon\ElasticBeanstalk\logs\AWSDeployment.log`  |  `/aws/elasticbeanstalk/<environment-name>/EBDeploy-Log`  | 
|  `C:\Program Files\Amazon\ElasticBeanstalk\logs\Hooks.log`  |  `/aws/elasticbeanstalk/<environment-name>/EBHooks-Log`  | 
|  `C:\inetpub\logs\LogFiles` \(the entire directory\)  |  `/aws/elasticbeanstalk/<environment-name>/IIS-Log`  | 

## Streaming instance logs to CloudWatch Logs<a name="AWSHowTo.cloudwatchlogs.streaming"></a>

You can enable instance log streaming to CloudWatch Logs using the Elastic Beanstalk console, the EB CLI, or configuration options\.

Before you enable it, set up IAM permissions to use with the CloudWatch Logs agent\. You can attach the following custom policy to the [instance profile](concepts-roles-instance.md) that you assign to your environment\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "logs:CreateLogStream",
        "logs:PutLogEvents",
        "logs:DescribeLogGroups",
        "logs:DescribeLogStreams"
      ],
      "Resource": [
        "*"
      ]
    }
  ]
}
```

### Instance log streaming using the Elastic Beanstalk console<a name="AWSHowTo.cloudwatchlogs.streaming.console"></a>

**To stream instance logs to CloudWatch Logs**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Configuration**\.

1. In the **Software** configuration category, choose **Edit**\.

1. Under **Instance log streaming to CloudWatch Logs**:
   + Enable **Log streaming**\.
   + Set **Retention** to the number of days to save the logs\.
   + Select the **Lifecycle** setting that determines whether the logs are saved after the environment is terminated\.

1. Choose **Apply**\.

After you enable log streaming, you can return to the **Software** configuration category or page and find the **Log Groups** link\. Click this link to see your logs in the CloudWatch console\.

### Instance log streaming using the EB CLI<a name="AWSHowTo.cloudwatchlogs.streaming.ebcli"></a>

To enable instance log streaming to CloudWatch Logs using the EB CLI, use the [eb logs](eb3-logs.md) command\.

```
$ eb logs --cloudwatch-logs enable
```

You can also use eb logs to retrieve logs from CloudWatch Logs\. You can retrieve all the environment's instance logs, or use the command's many options to specify subsets of logs to retrieve\. For example, the following command retrieves the complete set of instance logs for your environment, and saves them to a directory under `.elasticbeanstalk/logs`\.

```
$ eb logs --all
```

In particular, the `--log-group` option enables you to retrieve instance logs of a specific log group, corresponding to a specific on\-instance log file\. To do that, you need to know the name of the log group that corresponds to the log file you want to retrieve\. You can find this information in [How Elastic Beanstalk sets up CloudWatch Logs](#AWSHowTo.cloudwatchlogs.loggroups)\.

### Instance log streaming using configuration files<a name="AWSHowTo.cloudwatchlogs.files"></a>

When you create or update an environment, you can use a configuration file to set up and configure instance log streaming to CloudWatch Logs\. The following example configuration file enables default instance log streaming\. Elastic Beanstalk streams the default set of log files for your environment's platform\. To use the example, copy the text into a file with the `.config` extension in the `.ebextensions` directory at the top level of your application source bundle\.

```
option_settings:
  - namespace: aws:elasticbeanstalk:cloudwatch:logs
    option_name: StreamLogs
    value: true
```

### Custom log file streaming<a name="AWSHowTo.cloudwatchlogs.streaming.custom"></a>

The Elastic Beanstalk integration with CloudWatch Logs doesn't directly support the streaming of custom log files that your application generates\. To stream custom logs, use a configuration file to directly install the CloudWatch Logs agent and to configure the files to be pushed\. For an example configuration file, see [https://github.com/awsdocs/elastic-beanstalk-samples/tree/master/configuration-files/aws-provided/instance-configuration/logs-streamtocloudwatch-linux.config](https://github.com/awsdocs/elastic-beanstalk-samples/tree/master/configuration-files/aws-provided/instance-configuration/logs-streamtocloudwatch-linux.config)\.

**Note**  
The example doesn't work on the Windows platform\.

For more information about configuring CloudWatch Logs, see the [CloudWatch Logs Agent Reference](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AgentReference.html) in the *Amazon CloudWatch Logs User Guide*\.

## Troubleshooting CloudWatch Logs integration<a name="AWSHowTo.cloudwatchlogs.troubleshoot"></a>

If you can't find some of the environment's instance logs you expect in CloudWatch Logs, you can investigate the following common issues:
+ Your IAM role lacks the required IAM permissions\.
+ You launched your environment in an AWS Region that doesn't support CloudWatch Logs\.
+ One of your custom log files doesn't exist in the path you specified\.