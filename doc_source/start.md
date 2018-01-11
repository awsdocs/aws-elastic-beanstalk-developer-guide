# start<a name="start"></a>

**Note**  
 This version of EB CLI and its documentation have been replaced with version 3 \(in this section, EB CLI 3 represents version 3 and later of EB CLI\)\. For information on the new version, see \. 

## Description<a name="startdescription"></a>

Creates and deploys the current application into the specified environment\. For a tutorial that includes a description of how to deploy a sample application using `eb start`, see [Getting Started with Eb](command-reference-get-started.md)\.

## Syntax<a name="startsyntax"></a>

 **`eb start`** 

## Options<a name="startoptions"></a>


****  

|  Name  |  Description  |  Required  | 
| --- | --- | --- | 
|  `-e` or `--environment-name` *`ENVIRONMENT_NAME`*   |  The environment into which you want to create or start the current application\. Type: String Default: Current setting  |  No  | 
|  Common options  |  For more information, see [Eb Common Options](eb-cmd-options.md)\.  |  No  | 

## Output<a name="startoutput"></a>

If successful, the command returns the status of the start operation\. If there were issues during the launch, you can use the [events](eb-events.md) operation to get more details\.

## Example 1<a name="startexample1"></a>

The following example starts the environment\.

```
PROMPT> start

Starting application "MyApp".
Waiting for environment "MyApp-env" to launch.
2014-05-13 07:25:33 INFO createEnvironment is starting.
2014-05-13 07:25:39 INFO Using elasticbeanstalk-us-west-2-8EXAMPLE3 as Amazon
S3 storage bucket for environment data.
2014-05-13 07:26:07 INFO Created EIP: xx.xx.xxx.xx
2014-05-13 07:26:09 INFO Created security group named: awseb-e-vEXAMPLErp-stack-
AWSEBSecurityGroup-1GCEXAMPLEG0
2014-05-13 07:26:17 INFO Creating RDS database named: aavcEXAMPLE5y. This may
take a few minutes.
2014-05-13 07:32:36 INFO Created RDS database named: aavcEXAMPLE5y
2014-05-13 07:34:08 INFO Waiting for EC2 instances to launch. This may take a
few minutes.
2014-05-13 07:36:24 INFO Application available at MyApp-env-z4vsuuxh36.elastic
beanstalk.com.
2014-05-13 07:36:24 INFO Successfully launched environment: MyApp-env
Application is available at "MyApp-env-z4EXAMPLE6.elasticbeanstalk.com"
```

## Example 2<a name="startexample2"></a>

The following example starts the current application into an environment called *MyApp\-test\-env*\.

```
PROMPT> start -e MyApp-test-env

Starting application "MyApp".
Waiting for environment "MyApp-test-env" to launch.
2014-05-13 07:25:33 INFO createEnvironment is starting.
2014-05-13 07:25:39 INFO Using elasticbeanstalk-us-west-2-8EXAMPLE3 as Amazon
S3 storage bucket for environment data.
2014-05-13 07:26:07 INFO Created EIP: xx.xx.xxx.xx
2014-05-13 07:26:09 INFO Created security group named: awseb-e-vEXAMPLErp-stack-
AWSEBSecurityGroup-1GCEXAMPLEG0
2014-05-13 07:26:17 INFO Creating RDS database named: aavcEXAMPLE5y. This may
take a few minutes.
2014-05-13 07:32:36 INFO Created RDS database named: aavcEXAMPLE5y
2014-05-13 07:34:08 INFO Waiting for EC2 instances to launch. This may take a
few minutes.
2014-05-13 07:36:24 INFO Application available at MyApp-test-envz4vsuuxh36.
elasticbeanstalk.com.
2014-05-13 07:36:24 INFO Successfully launched environment: MyApp-test-env
Application is available at "MyApp-test-env-z4EXAMPLE6.elasticbeanstalk.com"
```