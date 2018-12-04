# eb status<a name="eb3-status"></a>

## Description<a name="eb3-status-description"></a>

Provides information about the status of the environment\.

If the root directory contains a `platform.yaml` file specifying a custom platform, this command also provides information about the builder environment\.

## Syntax<a name="eb3-status-syntax"></a>

 eb status 

 eb status *environment\-name* 

## Options<a name="eb3-statusoptions"></a>


****  

|  Name  |  Description  | 
| --- | --- | 
|  `-v` or `--verbose`  |  Provides more information about individual instances, such as their status with the Elastic Load Balancing load balancer\.  | 
|  [Common options](eb3-cmd-options.md)  |  | 

## Output<a name="eb3-statusoutput"></a>

If successful, the command returns the following information about the environment:
+ Environment name
+ Application name
+ Deployed application version
+ Environment ID
+ Platform
+ Environment tier
+ CNAME
+ Time the environment was last updated
+ Status
+ Health

If you use verbose mode, EB CLI also provides you with the number of running Amazon EC2 instances\.

## Example<a name="eb3-statusexample"></a>

The following example shows the status for the environment tmp\-dev\.

```
$ eb status
Environment details for: tmp-dev
  Application name: tmp
  Region: us-west-2
  Deployed Version: None
  Environment ID: e-2cpfjbra9a
  Platform: 64bit Amazon Linux 2014.09 v1.0.9 running PHP 5.5
  Tier: WebServer-Standard-1.0
  CNAME: tmp-dev.elasticbeanstalk.com
  Updated: 2014-10-29 21:37:19.050000+00:00
  Status: Launching
  Health: Grey
```