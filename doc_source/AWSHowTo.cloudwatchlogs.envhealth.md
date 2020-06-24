# Streaming Elastic Beanstalk environment health information to Amazon CloudWatch Logs<a name="AWSHowTo.cloudwatchlogs.envhealth"></a>

If you enable [enhanced health](health-enhanced.md) reporting for your environment, you can configure the environment to stream health information to CloudWatch Logs\. This streaming is independent from Amazon EC2 instance log streaming\. This topic describes environment health information streaming\. For information about instance log streaming, see [Using Elastic Beanstalk with Amazon CloudWatch Logs](AWSHowTo.cloudwatchlogs.md)\.

When you configure environment health streaming, Elastic Beanstalk creates a CloudWatch Logs log group for environment health\. The log group's name is `/aws/elasticbeanstalk/environment-name/environment-health.log`\. Within this log group, Elastic Beanstalk creates log streams named `YYYY-MM-DD#<hash-suffix>` \(there might be more than one log stream per date\)\.

When the environment's health status changes, Elastic Beanstalk adds a record to the health log stream\. The record represents the health status transition—the new status and a description of the cause of change\. For example, an environment's status might change to Severe because the load balancer is failing\. For a description of enhanced health statuses, see [Health colors and statuses](health-enhanced-status.md)\.

## Prerequisites to environment health streaming to CloudWatch Logs<a name="AWSHowTo.cloudwatchlogs.envhealth.prereqs"></a>

To enable environment health streaming to CloudWatch Logs, you must meet the following conditions:
+ *Platform* – You must be using a platform version that supports enhanced health reporting\.
+ *Permissions* – You must grant certain logging\-related permissions to Elastic Beanstalk so that it can act on your behalf to stream health information for your environment\. If your environment isn't using a service role that Elastic Beanstalk created for it, `aws-elasticbeanstalk-service-role`, or your account's service\-linked role, `AWSServiceRoleForElasticBeanstalk`, be sure to add the following permissions to your custom service role\.

  ```
  {
        "Effect": "Allow",
        "Action": [
          "logs:DescribeLogStreams",
          "logs:CreateLogStream",
          "logs:PutLogEvents"
        ],
        "Resource": "arn:aws:logs:*:*:log-group:/aws/elasticbeanstalk/*:log-stream:*"
  }
  ```

## Streaming environment health logs to CloudWatch Logs<a name="AWSHowTo.cloudwatchlogs.envhealth.streaming"></a>

You can enable environment health streaming to CloudWatch Logs using the Elastic Beanstalk console, the EB CLI, or configuration options\.

### Environment health log streaming using the Elastic Beanstalk console<a name="AWSHowTo.cloudwatchlogs.envhealth.streaming.console"></a>

**To stream environment health logs to CloudWatch Logs**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Configuration**\.

1. In the **Monitoring** configuration category, choose **Edit**\.

1. Under **Health reporting**, make sure that the reporting **System** is set to **Enhanced**\.

1. Under **Health event streaming to CloudWatch Logs**
   + Enable **Log streaming**\.
   + Set **Retention** to the number of days to save the logs\.
   + Select the **Lifecycle** setting that determines whether the logs are saved after the environment is terminated\.

1. Choose **Apply**\.

After you enable log streaming, you can return to the **Monitoring** configuration category or page and find the **Log Group** link\. Click this link to see your environment health logs in the CloudWatch console\.

### Environment health log streaming using the EB CLI<a name="AWSHowTo.cloudwatchlogs.envhealth.streaming.ebcli"></a>

To enable environment health log streaming to CloudWatch Logs using the EB CLI, use the [eb logs](eb3-logs.md) command\.

```
$ eb logs --cloudwatch-logs enable --cloudwatch-log-source environment-health
```

You can also use eb logs to retrieve logs from CloudWatch Logs\. For example, the following command retrieves all the health logs for your environment, and saves them to a directory under `.elasticbeanstalk/logs`\.

```
$ eb logs --all --cloudwatch-log-source environment-health
```

### Environment health log streaming using configuration files<a name="AWSHowTo.cloudwatchlogs.envhealth.files"></a>

When you create or update an environment, you can use a configuration file to set up and configure environment health log streaming to CloudWatch Logs\. To use the example below, copy the text into a file with the `.config` extension in the `.ebextensions` directory at the top level of your application source bundle\. The example configures Elastic Beanstalk to enable environment health log streaming, keep the logs after terminating the environment, and save them for 30 days\.

**Example [Health streaming configuration file](samples/aws_eb_cloudwatchlogs-envhealth.zip)**  

```
############################################################################
##  Sets up Elastic Beanstalk to stream environment health information
##  to Amazon CloudWatch Logs.
##  Works only for environments that have enhanced health reporting enabled.
############################################################################

option_settings:
  aws:elasticbeanstalk:cloudwatch:logs:health:
    HealthStreamingEnabled: true
    ### Settings below this line are optional.
    # DeleteOnTerminate: Delete the log group when the environment is
    # terminated. Default is false. If false, the health data is kept
    # RetentionInDays days.
    DeleteOnTerminate: false
    # RetentionInDays: The number of days to keep the archived health data
    # before it expires, if DeleteOnTerminate isn't set. Default is 7 days.
    RetentionInDays: 30
```

For option defaults and valid values, see [`aws:elasticbeanstalk:cloudwatch:logs:health`](command-options-general.md#command-options-general-cloudwatchlogs-health)\.