# eb config<a name="eb3-config"></a>

## Description<a name="eb3-configdescription"></a>

Changes the environment configuration settings\. This command saves the environment configuration settings as well as uploads, downloads, or lists saved configurations\.

If the root directory contains a `platform.yaml` file specifying a custom platform, this command also changes the builder configuration settings, based on the values set in `platform.yaml`\.

**Note**  
eb config does not show environment properties\. To set environment properties that you can read from within your application, use [eb setenv](environment-configuration-methods-after.md#configuration-options-after-ebcli-ebsetenv)\.

## Syntax<a name="eb3-configsyntax"></a>

 eb config 

 eb config *environment\-name* 

The following describes the syntax for using the eb config command to work with saved configurations\. For examples, see the see the [Examples](#eb3-configexample) section later in this topic\.
+ eb config delete *filename*  – Deletes the named saved configuration\.
+ eb config get *filename* – Downloads the named saved configuration\.
+ eb config list – Lists the saved configurations that you have in Amazon S3\.
+ eb config put *filename* – Uploads the named saved configuration to an Amazon S3 bucket\. The *filename* must have the file extension `.cfg.yml`\. To specify the file name without a path, you can save the file to the `.elasticbeanstalk` folder or to the `.elasticbeanstalk/saved_configs/` folder before you run the command\. Alternatively, you can specify the *filename* by providing the full path\.
+ eb config save – Saves the environment configuration settings for the current running environment to `.elasticbeanstalk/saved_configs/` with the filename `[configuration-name].cfg.yml`\. By default, the EB CLI saves the configuration settings with a *configuration\-name* based on the environment name\. You can specify a different configuration name by including the `--cfg` option with your desired configuration name when you run the command\.

  You can tag your saved configuration using the `--tags` option\.

## Options<a name="eb3-configoptions"></a>


****  

|  Name  |  Description  | 
| --- | --- | 
|  `--cfg config-name`  |  The name to use for a saved configuration \(which you can later specify to create or update an environment from a saved configuration\)\. This option works with eb config save only\.  | 
|  `-﻿-﻿tags key1=value1[,key2=value2 ...]`  |  Tags to add to your saved configuration\. Tags are specified as a comma\-separated list of `key=value` pairs\. For more details, see [Tagging saved configurations](environment-configuration-savedconfig-tagging.md)\. This option works with eb config save only\.  | 
|  `--timeout timeout`  |  The number of minutes before the command times out\.  | 
|  [Common options](eb3-cmd-options.md)  |  | 

## Output<a name="eb3-configoutput"></a>

If the command runs successfully with no parameters, the command displays your current option settings in the text editor that you configured as the EDITOR environment variable\. \(If you have not configured an EDITOR environment variable, then EB CLI displays your option settings in your computer's default editor for YAML files\.\) When you save changes to the file and close the editor, the environment is updated with the option settings in the file\.

If the command runs successfully with the `get` parameter, the command displays the location of the local copy that you downloaded\.

If the command runs successfully with the `save` parameter, the command displays the location of the saved file\.

## Examples<a name="eb3-configexample"></a>

This section describes how to change the text editor that you use to view and edit your option settings file\.

For Linux/UNIX, the following example changes the editor to vim:

```
$ export EDITOR=vim
```

For Linux/UNIX,the following example changes the editor to what is installed at `/usr/bin/kate`\.

```
$ export EDITOR=/usr/bin/kate
```

For Windows, the following example changes the editor to Notepad\+\+\.

```
> set EDITOR="C:\Program Files\Notepad++\Notepad++.exe
```

This section provides examples for the eb config command when it is run with parameters\.

The following example deletes the saved configuration named app\-tmp\.

```
$ eb config delete app-tmp
```

The following example downloads the saved configuration with the name app\-tmp from your Amazon S3 bucket\.

```
$ eb config get app-tmp
```

The following example lists the names of saved configurations that are stored in your Amazon S3 bucket\.

```
$ eb config list
```

The following example uploads the local copy of the saved configuration named app\-tmp to your Amazon S3 bucket\.

```
$ eb config put app-tmp
```

The following example saves configuration settings from the current running environment\. If you do not provide a name to use for the saved configuration, then Elastic Beanstalk names the configuration file according to the environment name\. For example, an environment named tmp\-dev would be called `tmp-dev.cfg.yml`\. Elastic Beanstalk saves the file to the folder `/.elasticbeanstalk/saved_configs/`\.

```
$ eb config save
```

The following example shows how to use the `--cfg` option to save the configuration settings from the environment tmp\-dev to a file called `v1-app-tmp.cfg.yml`\. Elastic Beanstalk saves the file to the folder `/.elasticbeanstalk/saved_configs/`\. If you do not specify an environment name, Elastic Beanstalk saves configuration settings from the current running environment\.

```
$ eb config save tmp-dev --cfg v1-app-tmp
```