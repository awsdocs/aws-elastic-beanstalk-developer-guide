# status<a name="status"></a>

**Note**  
 This version of the EB CLI and its documentation have been replaced with version 3 \(in this section, EB CLI 3 represents version 3 and later of the EB CLI\)\. For information on the new version, see [The Elastic Beanstalk Command Line Interface \(EB CLI\)](eb-cli3.md)\. 

## Description<a name="statusdescription"></a>

Describes the status of the specified environment\. For a tutorial that includes a description of how to view an environment's status using `eb status`, see [Getting Started with Eb](command-reference-get-started.md)\.

## Syntax<a name="statussyntax"></a>

 **`eb status`** 

## Options<a name="statusoptions"></a>

You might want to use the `--verbose` option with `status`\.


****  

|  Name  |  Description  |  Required  | 
| --- | --- | --- | 
|  `-e` or `--environment-name` *`ENVIRONMENT_NAME`*   |  The environment for which you want to display status\. Type: String Default: Current setting  |  No  | 
|  Common options  |  For more information, see [Eb Common Options](eb-cmd-options.md)\.  |  No  | 

## Output<a name="statusoutput"></a>

If successful, the command returns the status of the environment\.

## Example 1<a name="statusexample1"></a>

The following example request returns the status of the environment\.

```
PROMPT> eb status --verbose
Retrieving status of environment "MyNodeApp-env".
URL     : MyNodeApp-env-tnEXAMPLEcf.elasticbeanstalk.com
Status  : Ready
Health  : Green
Environment Name: MyNodeApp-env
Environment ID : e-vmEXAMPLEp
Environment Tier: WebServer::Standard::1.0
Solution Stack : 64bit Amazon Linux 2014.02 running Node.js
Version Label : Sample Application
Date Created : 2014-05-14 07:25:35
Date Updated : 2014-05-14 07:36:24
Description :

RDS Database: AWSEBRDSDatabase | aavcEXAMPLEd5y.clak1.us-west-2.rds.amazon
aws.com:3306
Database Engine: mysql 5.6.37
Allocated Storage: 5
Instance Class: db.t2.micro
Multi AZ: False
Master Username: ebroot
Creation Time: 2014-05-15 07:29:39
DB Instance Status: available
```

## Example 2<a name="statusexample2"></a>

The following example request returns the status of an application named *MyNodeApp* in an environment called *MyNodeApp\-test\-env*\.

```
PROMPT> eb status -e MyNodeApp-test-env -a MyNodeApp
Retrieving status of environment "MyNodeApp-test-env".
URL : MyNodeApp-test-env-tnEXAMPLEcf.elasticbeanstalk.com
Status  : Ready
Health  : Green

RDS Database: AWSEBRDSDatabase | aavcEXAMPLEd5y.clak1.us-west-2.rds.amazon
aws.com:3306
```