# eb clone<a name="eb3-clone"></a>

## Description<a name="eb3-clonedescription"></a>

Clones an environment to a new environment so that both have identical environment settings\.

**Note**  
By default, regardless of the solution stack version of the environment from which you create the clone, the eb clone command creates the clone environment with the most recent solution stack\. You can suppress this by including the `--exact` option when you run the command\.

## Syntax<a name="eb3-clonesyntax"></a>

 eb clone 

 eb clone *environment\-name* 

## Options<a name="eb3-cloneoptions"></a>


****  

|  Name  |  Description  | 
| --- | --- | 
|  `-n` *string* or `--clone_name` *string*  |  Desired name for the cloned environment\.  | 
|  `-c` *string* or `--cname` *string*  |  Desired CNAME prefix for the cloned environment\.  | 
|  `--envvars`  |  Environment properties in a comma\-separated list with the format *name*=*value*\. Type: String Constraints: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb3-clone.html)  | 
|  `--exact`  |  Prevents Elastic Beanstalk from updating the solution stack version for the new clone environment to the most recent version available \(for the original environment's platform\)\.  | 
|  `--scale` *number*  |  The number of instances to run in the clone environment when it is launched\.  | 
|  `--tags` *name*=*value*  |  [Tags](using-features.tagging.md) for the resources in your environment in a comma\-separated list with the format *name*=*value*\.  | 
|  `--timeout`  |  The number of minutes before the command times out\.  | 
|  [Common options](eb3-cmd-options.md)  |  | 

## Output<a name="eb3-cloneoutput"></a>

If successful, the command creates an environment that has the same settings as the original environment or with modifications to the environment as specified by any eb clone options\.

## Example<a name="eb3-cloneexample"></a>

The following example clones the specified environment\.

```
$ eb clone
Enter name for Environment Clone
(default is tmp-dev-clone):
Enter DNS CNAME prefix
(default is tmp-dev-clone):
Environment details for: tmp-dev-clone
  Application name: tmp
  Region: us-west-2
  Deployed Version: app-141029_144740
  Environment ID: e-vjvrqnn5pv
  Platform: 64bit Amazon Linux 2014.09 v1.0.9 running PHP 5.5
  Tier: WebServer-Standard-1.0
  CNAME: tmp-dev-clone.elasticbeanstalk.com
  Updated: 2014-10-29 22:00:23.008000+00:00
Printing Status:
2018-07-11 21:04:20    INFO: createEnvironment is starting.
2018-07-11 21:04:21    INFO: Using elasticbeanstalk-us-west-2-888888888888 as Amazon S3 storage bucket for environment data.
...
2018-07-11 21:07:10    INFO: Successfully launched environment: tmp-dev-clone
```