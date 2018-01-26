# events<a name="eb-events"></a>

**Note**  
 This version of the EB CLI and its documentation have been replaced with version 3 \(in this section, EB CLI 3 represents version 3 and later of the EB CLI\)\. For information on the new version, see \. 

## Description<a name="eb-eventsdescription"></a>

Returns the most recent events for the environment\.

## Syntax<a name="eb-eventssyntax"></a>

 **`eb events`** 

## Options<a name="eb-eventsoptions"></a>


****  

|  Name  |  Description  |  Required  | 
| --- | --- | --- | 
|   `[number]` * `NUMBER` *   |  The number of events to return\. Valid values range from 1 to 1000\. Type: Integer Default: 10  |  Yes  | 

## Output<a name="eb-eventsoutput"></a>

If successful, the command returns the specified number of recent events\.

## Example<a name="eb-eventsexample"></a>

The following example returns the 15 most recent events\.

```
PROMPT> eb events 15

2014-05-19 08:44:51     INFO     terminateEnvironment completed successfully.
2014-05-19 08:44:50     INFO     Deleting SNS topic for environment MyApp-test-env.
2014-05-19 08:44:38     INFO     Deleted security group named: awseb-e-fEXAMPLEre-stack-AWSEBSecurityGroup-1DEXAMPLEKI
2014-05-19 08:44:32     INFO     Deleted RDS database named: aa1k8EXAMPLEdxl
2014-05-19 08:38:33     INFO     Deleted EIP: xx.xx.xxx.xx
2014-05-19 08:37:04     INFO     Waiting for EC2 instances to terminate. This may take a few minutes.
2014-05-19 08:36:45     INFO     terminateEnvironment is starting.
2014-05-19 08:27:54     INFO     Adding instance 'i-fEXAMPLE7' to your environment.
2014-05-19 08:27:50     INFO     Successfully launched environment: MyApp-test-env
2014-05-19 08:27:50     INFO     Application available at MyApp-test-envmEXAMPLEst.elasticbeanstalk.com.
2014-05-19 08:24:04     INFO     Waiting for EC2 instances to launch. This may take a few minutes.
2014-05-19 08:23:21     INFO     Created RDS database named: aa1k8EXAMPLEdxl
2014-05-19 08:17:07     INFO     Creating RDS database named: aa1k8EXAMPLEdxl. This may take a few minutes.
2014-05-19 08:16:58     INFO     Created security group named: awseb-e-fEXAMPLEre-stack-AWSEBSecurityGroup-1D6HEXAMPLEKI
2014-05-19 08:16:54     INFO     Created EIP: 50.18.181.66
```