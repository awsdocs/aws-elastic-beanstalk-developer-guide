# eb upgrade<a name="eb3-upgrade"></a>

## Description<a name="eb3-upgradedescription"></a>

Upgrades the platform of your environment to the most recent version of the platform on which it is currently running\.

If the root directory contains a `platform.yaml` file specifying a custom platform, this command upgrades the environment to the most recent version of the custom platform on which it is currently running\.

## Syntax<a name="eb3-upgradesyntax"></a>

 eb upgrade 

 eb upgrade *environment\-name* 

## Options<a name="eb3-upgradeoptions"></a>


****  

|  Name  |  Description  | 
| --- | --- | 
|  `--force`  |  Upgrades without requiring you to confirm the environment name before starting the upgrade process\.  | 
|  `--noroll`  |  Updates all instances without using rolling updates to keep some instances in service during the upgrade\.  | 
|  [Common options](eb3-cmd-options.md)  |  | 

## Output<a name="eb3-upgradeoutput"></a>

The command shows an overview of the change and prompts you to confirm the upgrade by typing the environment name\. If successful, your environment is updated and then launched with the most recent version of the platform\.

## Example<a name="eb3-upgradeexample"></a>

The following example upgrades the current platform version of the specified environment to the most recently available platform version\.

```
$ eb upgrade
Current platform: 64bit Amazon Linux 2014.09 v1.0.9 running Python 2.7
Latest platform:  64bit Amazon Linux 2014.09 v1.2.0 running Python 2.7

WARNING: This operation replaces your instances with minimal or zero downtime. You may cancel the upgrade after it has started by typing "eb abort".
You can also change your platform version by typing "eb clone" and then "eb swap".

To continue, type the environment name:
```