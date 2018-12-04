# eb scale<a name="eb3-scale"></a>

## Description<a name="eb3-scaledescription"></a>

Scales the environment to always run on a specified number of instances, setting both the minimum and maximum number of instances to the specified number\.

## Syntax<a name="eb3-scalesyntax"></a>

 eb scale *number\-of\-instances* 

 eb scale *number\-of\-instances* *environment\-name* 

## Options<a name="eb3-scaleoptions"></a>


****  

|  Name  |  Description  | 
| --- | --- | 
|  \-\-timeout  |  The number of minutes before the command times out\.  | 
|  [Common options](eb3-cmd-options.md)  |  | 

## Output<a name="eb3-scaleoutput"></a>

If successful, the command updates the number of minimum and maximum instances to run to the specified number\.

## Example<a name="eb3-scaleexample"></a>

The following example sets the number of instances to 2\.

```
$ eb scale 2
2018-07-11 21:05:22    INFO: Environment update is starting.
2018-07-11 21:05:27    INFO: Updating environment tmp-dev's configuration settings.
2018-07-11 21:08:53    INFO: Added EC2 instance 'i-5fce3d53' to Auto Scaling Group 'awseb-e-2cpfjbra9a-stack-AWSEBAutoScalingGroup-7AXY7U13ZQ6E'.
2018-07-11 21:08:58    INFO: Successfully deployed new configuration to environment.
2018-07-11 21:08:59    INFO: Environment update completed successfully.
```