# eb logs<a name="eb3-logs"></a>

## Description<a name="eb3-logsdescription"></a>

The eb logs command has two distinct purposes: to enable or disable log streaming to CloudWatch Logs, and to retrieve instance logs or CloudWatch Logs logs\. With the `--cloudwatch-logs` \(`-cw`\) option, the command enables or disables log streaming\. Without this option, it retrieves logs\.

When retrieving logs, specify the `--all`, `--zip`, or `--stream` option to retrieve complete logs\. If you don't specify any of these options, Elastic Beanstalk retrieves tail logs\.

The command processes logs for the specified or default environment\. Relevant logs vary by container type\. If the root directory contains a `platform.yaml` file specifying a custom platform, this command also processes logs for the builder environment\.

For more information, see [Using Elastic Beanstalk with Amazon CloudWatch Logs](AWSHowTo.cloudwatchlogs.md)\.

## Syntax<a name="eb3-logssyntax"></a>

 To enable or disable log streaming to CloudWatch Logs: 

```
eb logs --cloudwatch-logs [enable | disable] [--cloudwatch-log-source instance | environment-health | all] [environment-name]
```

 To retrieve instance logs: 

```
eb logs [-all | --zip | --stream] [--cloudwatch-log-source instance] [--instance instance-id] [--log-group log-group] [environment-name]
```

 To retrieve environment health logs: 

```
eb logs [-all | --zip | --stream] --cloudwatch-log-source environment-health [environment-name]
```

## Options<a name="eb3-logsoptions"></a>


****  

|  Name  |  Description  | 
| --- | --- | 
|  `-cw [enable \| disable]` or `--cloudwatch-logs [enable \| disable]`  |  Enables or disables log streaming to CloudWatch Logs\. If no argument is supplied, log streaming is enabled\. If the `--cloudwatch-log-source` \(`-cls`\) option isn't specified in addition, instance log streaming is enabled or disabled\.  | 
|  `-cls instance \| environment-health \| all` or `--cloudwatch-log-source instance \| environment-health \| all`  |  Specifies the source of logs when working with CloudWatch Logs\. With the enable or disable form of the command, these are the logs for which to enable or disable CloudWatch Logs streaming\. With the retrieval form of the command, these are the logs to retrieve from CloudWatch Logs\. Valid values: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb3-logs.html) Value meanings: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb3-logs.html)  | 
|  `-a` or `--all`  |  Retrieves complete logs and saves them to the `.elasticbeanstalk/logs` directory\.  | 
|  `-z` or `--zip`  |  Retrieves complete logs, compresses them into a `.zip` file, and then saves the file to the `.elasticbeanstalk/logs` directory\.  | 
|  `--stream`  |  Streams \(continuously outputs\) complete logs\. With this option, the command keeps running until you interrupt it \(press **Ctrl\+C**\)\.  | 
|  `-i instance-id` or `--instance instance-id`  |  Retrieves logs for the specified instance only\.  | 
|  `-g log-group` or `--log-group log-group`  |  Specifies the CloudWatch Logs log group from which to retrieve logs\. The option is valid only when instance log streaming to CloudWatch Logs is enabled\. If instance log streaming is enabled, and you don't specify the `--log-group` option, the default log group is one of the following: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb3-logs.html) For information about the log group corresponding to each log file, see [How Elastic Beanstalk sets up CloudWatch Logs](AWSHowTo.cloudwatchlogs.md#AWSHowTo.cloudwatchlogs.loggroups)\.  | 
|  [Common options](eb3-cmd-options.md)  |  | 

## Output<a name="eb3-logsoutput"></a>

By default, displays the logs directly in the terminal\. Uses a paging program to display the output\. Press **Q** or **q** to exit\.

With `--stream`, shows existing logs in the terminal and keeps running\. Press **Ctrl\+C** to exit\.

With `--all` and `--zip`, saves the logs to local files and displays the file location\.

## Examples<a name="logsexample"></a>

The following example enables instance log streaming to CloudWatch Logs\.

```
$ eb logs -cw enable
Enabling instance log streaming to CloudWatch for your environment
After the environment is updated you can view your logs by following the link:
https://console.aws.amazon.com/cloudwatch/home?region=us-east-1#logs:prefix=/aws/elasticbeanstalk/environment-name/
Printing Status:
2018-07-11 21:05:20    INFO: Environment update is starting.
2018-07-11 21:05:27    INFO: Updating environment environment-name's configuration settings.
2018-07-11 21:06:45    INFO: Successfully deployed new configuration to environment.
```

The following example retrieves instance logs into a `.zip` file\.

```
$ eb logs --zip
Retrieving logs...
Logs were saved to /home/workspace/environment/.elasticbeanstalk/logs/150622_173444.zip
```