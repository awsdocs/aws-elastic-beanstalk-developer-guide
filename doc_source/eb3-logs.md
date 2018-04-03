# `eb logs`<a name="eb3-logs"></a>

## Description<a name="eb3-logsdescription"></a>

Returns logs for the specified or default environment\. Relevant logs vary by container type\.

If the root directory contains a `platform.yaml` file specifying a custom platform, this command also returns logs for the builder environment\.

## Syntax<a name="eb3-logssyntax"></a>

 `eb logs` 

 `eb logs environment_name` 

## Options<a name="eb3-logsoptions"></a>


****  

|  Name  |  Description  | 
| --- | --- | 
|  `-a` or `--all`  |  Retrieves all logs and saves them to the `.elasticbeanstalk/logs` directory\.  | 
|  `-cw [enable \| disable]` or `--cloudwatch [enable \| disable]`  |  Enables or disables CloudWatch Logs\. If no argument is supplied, the logs are enabled\.  | 
|  `-g log-group` or `--log-group log-group`  |  Specifies the location where Elastic Beanstalk stores CloudWatch Logs\. CloudWatch Logs must be enabled for this option to take effect\. If you enable CloudWatch Logs, but do not specify a location, the default location is `/aws/elasticbeanstalk/env-name/var/log/eb-activity.log`\. Elastic Beanstalk emits an error if the location does not exist\.  | 
|  `--instance instance-id`  |  Retrieve logs for the specified instance only\.  | 
|  `--stream`  |  Stream deployment logs that were set up with CloudWatch\.  | 
|  `--zip`  |  Retrieves all logs, compresses them into a `.zip` file, and then saves the file to the `.elasticbeanstalk/logs` directory\.  | 
|  [Common options](eb3-cmd-options.md)  |  | 

## Output<a name="eb3-logsoutput"></a>

Shows the logs directly in the terminal by default \(press q to close\)\. `--all` and `--zip` options save the logs locally and output the location of the file\(s\)\.

## Example<a name="logsexample"></a>

```
$ eb logs --zip
Retrieving logs...
Logs were saved to /home/workspace/environment/.elasticbeanstalk/logs/150622_173444.zip
```