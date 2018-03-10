# stop<a name="stop"></a>

**Note**  
 This version of the EB CLI and its documentation have been replaced with version 3 \(in this section, EB CLI 3 represents version 3 and later of the EB CLI\)\. For information on the new version, see [The Elastic Beanstalk Command Line Interface \(EB CLI\)](eb-cli3.md)\. 

## Description<a name="stopdescription"></a>

Terminates the environment\. For a tutorial that includes a description of how to terminate an environment using `eb stop`, see [Getting Started with Eb](command-reference-get-started.md)\.

**Note**  
The `stop` operation applies to environments, not applications\. To delete an application along with its environments, use `eb delete`\.

## Syntax<a name="stopsyntax"></a>

 **`eb stop`** 

## Options<a name="stopoptions"></a>


****  

|  Name  |  Description  |  Required  | 
| --- | --- | --- | 
|  `-e` or `--environment-name` *`ENVIRONMENT_NAME`*   |  The environment you want to terminate\. The environment must contain the current application; you cannot specify an application other than the one in the repository you're currently working in\. Type: String Default: Current setting  |  No  | 
|  Common options  |  For more information, see [Eb Common Options](eb-cmd-options.md)\.  |  No  | 

## Output<a name="stopoutput"></a>

If successful, the command returns the status of the `stop` operation\.

## Example1<a name="stopexample1"></a>

The following example request terminates the environment\.

```
PROMPT> eb stop

If you terminate your environment, your RDS DB Instance will be deleted and you will lose your data.
Terminate environment? [y/n]: y
Stopping environment "MyApp-env". This may take a few minutes.
2014-05-13 07:18:10 INFO terminateEnvironment is starting.
2014-05-13 07:18:17 INFO Waiting for EC2 instances to terminate. This may take
a few minutes.
2014-05-13 07:19:43 INFO Deleted EIP: xxx.xxx.xxx.xx
2014-05-13 07:19:43 INFO Deleted security group named: awseb-e-zEXAMPLEng-stack-
AWSEBSecurityGroup-MEEXAMPLENHQ
2014-05-13 07:19:51 INFO Deleting SNS topic for environment MyApp-env.
2014-05-13 07:19:52 INFO terminateEnvironment completed successfully.
Stop of environment "MyApp-env" has completed.
```

## Example 2<a name="stopexample2"></a>

The following example request terminates the environment named *MyApp\-test\-env*\.

```
PROMPT> eb stop -e MyApp-test-env

If you terminate your environment, your RDS DB Instance will be deleted and you will lose your data.
Terminate environment? [y/n]: y
Stopping environment "MyApp-test-env". This may take a few minutes.
2014-05-15 17:27:09 INFO terminateEnvironment is starting.
2014-05-15 17:27:16 INFO Waiting for EC2 instances to terminate. This
may take a few minutes.
2014-05-15 17:27:42 INFO Deleted EIP: xxx.xxx.xxx.xx
2014-05-15 17:27:42 INFO Deleted security group named: awseb-e-zEXAMPLEngstack-
AWSEBSecurityGroup-MEEXAMPLENHQ
2014-05-15 17:34:50 INFO Deleting SNS topic for environment MyApp-testenv.
2014-05-15 17:34:51 INFO terminateEnvironment completed successfully.
2013-05-15 17:29:55     INFO    Deleted Auto Scaling group named: awseb-e-mqmp6mmcpk-stack-AWSEBAutoScalingGroup-QALO012HZJVJ
2013-05-15 17:29:56     INFO    Deleted Auto Scaling launch configuration named: awseb-e-mqmp6mmcpk-stack-AWSEBAutoScalingLaunchConfiguration-1DBGPQ99YFX08
2013-05-15 17:34:11     INFO    Deleted RDS database named: aauel5gap2gqb4
2013-05-15 17:34:16     INFO    Deleted security group named: awseb-e-mqmp6mmcpk-stack-AWSEBSecurityGroup-1LDYFT0256P0B
2013-05-15 17:34:17     INFO    Deleted load balancer named: awseb-e-m-AWSEBLoa-CT74SPXN541T
2013-05-15 17:34:29     INFO    Deleting SNS topic for environment MyOtherApp-env.
2013-05-15 17:34:30     INFO    terminateEnvironment completed successfully.
Stop of environment "MyApp-test-env" has completed.
```