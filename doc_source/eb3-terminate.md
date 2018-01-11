# `eb terminate`<a name="eb3-terminate"></a>

## Description<a name="eb3-terminatedescription"></a>

Terminates the running environment so that you do not incur charges for unused AWS resources\.

If the root directory contains a `platform.yaml` file specifying a custom platform, this command terminates the running custom environment\.

**Note**  
You can always launch a new environment using the same version later\. If you have data from an environment that you would like to preserve, create a snapshot of your current database instance before you terminate the environment\. You can later use it as the basis for new DB instance when you create a new environment\. For more information, see [Creating a DB Snapshot](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_CreateSnapshot.html) in the *Amazon Relational Database Service User Guide*\.

## Syntax<a name="eb3-terminatesyntax"></a>

 `eb terminate` 

 `eb terminate environment_name` 

## Options<a name="eb3-terminateoptions"></a>


****  

|  Name  |  Description  | 
| --- | --- | 
|  `--all`  |  Terminates the environment, application, and all resources\.  | 
|  `--force`  |  Terminate the environment without prompting for confirmation\.  | 
|  `--ignore-links`  |  Terminate the environment even if there are dependent environments with links to it\. See Compose Environments\.  | 
|  `--timeout`  |  The number of minutes before the command times out\.  | 

## Output<a name="eb3-terminateoutput"></a>

If successful, the command returns the status of the `terminate` operation\.

## Example<a name="eb3-terminateexample"></a>

The following example request terminates the environment tmp\-dev\.

```
$ eb terminate
The environment "tmp-dev" and all associated instances will be terminated.
To confirm, type the environment name: tmp-dev
INFO: terminateEnvironment is starting.
INFO: Deleted CloudWatch alarm named: awseb-e-2cpfjbra9a-stack-AWSEBCloudwatchAlarmHigh-16V08YOF2KQ7U
INFO: Deleted CloudWatch alarm named: awseb-e-2cpfjbra9a-stack-AWSEBCloudwatchAlarmLow-6ZAWH9F20P7C
INFO: Deleted Auto Scaling group policy named: arn:aws:autoscaling:us-west-2:11122223333:scalingPolicy:5d7d3e6b-d59b-47c5-b102-3e11fe3047be:autoScalingGroupName/awseb-e-2cpfjbra9a-stack-AWSEBAutoScalingGroup-7AXY7U13ZQ6E:policyName/awseb-e-2cpfjbra9a-stack-AWSEBAutoSca
lingScaleUpPolicy-1876U27JEC34J
INFO: Deleted Auto Scaling group policy named: arn:aws:autoscaling:us-west-2:11122223333:scalingPolicy:29c6e7c7-7ac8-46fc-91f5-cfabb65b985b:autoScalingGroupName/awseb-e-2cpfjbra9a-stack-AWSEBAutoScalingGroup-7AXY7U13ZQ6E:policyName/awseb-e-2cpfjbra9a-stack-AWSEBAutoSca
lingScaleDownPolicy-SL4LHODMOMU
INFO: Waiting for EC2 instances to terminate. This may take a few minutes.
INFO: Deleted Auto Scaling group named: awseb-e-2cpfjbra9a-stack-AWSEBAutoScalingGroup-7AXY7U13ZQ6E
INFO: Deleted Auto Scaling launch configuration named: awseb-e-2cpfjbra9a-stack-AWSEBAutoScalingLaunchConfiguration-19UFHYGYWORZ
INFO: Deleted security group named: awseb-e-2cpfjbra9a-stack-AWSEBSecurityGroup-XT4YYGFL7I99
INFO: Deleted load balancer named: awseb-e-2-AWSEBLoa-AK6RRYFQVV3S
INFO: Deleting SNS topic for environment tmp-dev.
INFO: terminateEnvironment completed successfully.
```