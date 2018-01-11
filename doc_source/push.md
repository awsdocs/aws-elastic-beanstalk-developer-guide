# push<a name="push"></a>

**Note**  
 This version of EB CLI and its documentation have been replaced with version 3 \(in this section, EB CLI 3 represents version 3 and later of EB CLI\)\. For information on the new version, see \. 

## Description<a name="pushdescription"></a>

Deploys the current application to the AWS Elastic Beanstalk environment from the Git repository\.

**Note**  
The `eb push` operation does not push to your remote repository, if any\. Use a standard `git push` or similar command to update your remote repository\.
The `-e` or `--environment-name` options are not not valid for `eb push`\. To push to a different environment from the current one \(based on either the `eb init` default settings or the Git branch that is currently checked out\), run `eb branch` before running `eb push`\.

## Syntax<a name="pushsyntax"></a>

 **`eb push` ** 

## Options<a name="pushoptions"></a>


****  

|  Name  |  Description  |  Required  | 
| --- | --- | --- | 
|  Common options  |  For more information, see [Eb Common Options](eb-cmd-options.md)\.  |  No  | 

## Output<a name="pushoutput"></a>

If successful, the command returns the status of the `push` operation\.

## Example<a name="pushexample"></a>

The following example deploys the current application\.

```
PROMPT> eb push
Pushing to environment: MyApp-env
remote:
To https://AKIAXXXXXXXX5ZB7Q:2013092XXXXXXXXXXXXf502a780888b0a49899798aa6cbeaef690c0b
525d0f090c7338cbead589bf14f@git.elasticbeanstalk.us-west-2.amazonaws.com/v1/repos/417
0705XXXXXXXX23632303133/commitid/336264353663396262306463326563663763393EXAMPLExxxxx5
3165643137343939EXAMPLExx036/environment/417070536570743236323031332d6d6173EXAMPLEx65
2013-09-26 17:35:37     INFO    Adding instance 'i-5EXAMPLE' to your environment.
2013-09-26 17:36:12     INFO    Deploying new version to instance(s).
2013-09-26 17:36:20     INFO    New application version was deployed to running EC2 instances.
2013-09-26 17:36:20     INFO    Environment update completed successfully.
Update of environment "MyApp-env" has completed.
```