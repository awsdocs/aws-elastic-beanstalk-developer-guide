# eb restore<a name="eb3-restore"></a>

## Description<a name="eb3-restoredescription"></a>

Rebuilds a terminated environment, creating a new environment with the same name, ID, and configuration\. The environment name, domain name, and application version must be available for use in order for the rebuild to succeed\.

## Syntax<a name="eb3-restoresyntax"></a>

 eb restore 

 eb restore *environment\_id* 

## Options<a name="eb3-restoreoptions"></a>


****  

|  Name  |  Description  | 
| --- | --- | 
|  [Common options](eb3-cmd-options.md)  |  | 

## Output<a name="eb3-restoreoutput"></a>

The EB CLI displays a list of terminated environments that are available to restore\.

## Example<a name="eb3-restoreexample"></a>

```
$ eb restore
Select a terminated environment to restore

  #   Name          ID             Application Version      Date Terminated        Ago

  3   gamma         e-s7mimej8e9   app-77e3-161213_211138   2016/12/14 20:32 PST   13 mins
  2   beta          e-sj28uu2wia   app-77e3-161213_211125   2016/12/14 20:32 PST   13 mins
  1   alpha         e-gia8mphu6q   app-77e3-161213_211109   2016/12/14 16:21 PST   4 hours

 (Commands: Quit, Restore, ▼ ▲)

Selected environment alpha
Application:    scorekeep
Description:    Environment created from the EB CLI using "eb create"
CNAME:          alpha.h23tbtbm92.us-east-2.elasticbeanstalk.com
Version:        app-77e3-161213_211109
Platform:       64bit Amazon Linux 2016.03 v2.1.6 running Java 8
Terminated:     2016/12/14 16:21 PST
Restore this environment? [y/n]: y

2018-07-11 21:04:20    INFO: restoreEnvironment is starting.
2018-07-11 21:04:39    INFO: Created security group named: sg-e2443f72
...
```