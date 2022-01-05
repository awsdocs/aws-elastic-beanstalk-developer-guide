# eb config<a name="eb3-config"></a>

## Description<a name="eb3-configdescription"></a>

Manages the active [configuration](concepts.md#concepts-environmentconfig) settings and [saved configurations](concepts.md#concepts-configuration) of your environment\. You can use this command to upload, download, or list the saved configurations of your environment\. You can also use it to download, display, or update its active configuration settings\. 



If the root directory contains a `platform.yaml` file specifying a custom platform, this command also changes the builder configuration settings\. This is done based on the values that are set in `platform.yaml`\.

**Note**  
eb config doesn't show environment properties\. To set environment properties that you can read from within your application, use [eb setenv](environment-configuration-methods-after.md#configuration-options-after-ebcli-ebsetenv) instead\.

## Syntax<a name="eb3-configsyntax"></a>

The following are parts of the syntax that's used for the eb config command to work with the active [configuration settings](concepts.md#concepts-environmentconfig) of your environment\. For specific examples, see the [Examples](#eb3-configexample) section later in this topic\.
+  eb config – Displays the active configuration settings of your environment in a text editor that you configured as the EDITOR environment variable\. When you save changes to the file and close the editor, the environment is updated with the option settings that you saved in the file\.
**Note**  
If you didn't configure an EDITOR environment variable, EB CLI displays your option settings in your default editor for YAML files\.
+  eb config *environment\-name* – Displays and updates the configuration for the named environment\. The configuration is either displayed in a text editor that you configured or your default editor YAML files\.
+ eb config save – Saves the active configuration settings for the current environment to `.elasticbeanstalk/saved_configs/` with the filename `[configuration-name].cfg.yml`\. By default, the EB CLI saves the configuration settings with a *configuration\-name* based on the environment name\. You can specify a different configuration name by including the `--cfg` option with your desired configuration name when you run the command\.

  You can tag your saved configuration using the `--tags` option\.
+ eb config `--display` – Writes an environment's active configuration settings to *stdout *instead of a file\. By default this displays the configuration settings to the terminal\.
+ eb config `--update configuration_string | file_path` – Updates the active configuration settings for the current environment with the information that's specified in *configuration\_string* or inside the file identified by *file\_path*\.

**Note**  
The `--display` and `--update` options provide flexibility for reading and revising an environment's configuration settings programmatically\.

The following describes the syntax for using the eb config command to work with [saved configurations](concepts.md#concepts-configuration)\. For examples, see the [Examples](#eb3-configexample) section later in this topic\.
+ eb config get *config\-name* – Downloads the named saved configuration from Amazon S3\.

  
+ eb config delete *config\-name*  – Deletes the named saved configuration from Amazon S3\. Also deletes it locally, if you already downloaded it\.
+ eb config list – Lists the saved configurations that you have in Amazon S3\.
+ eb config put *filename* – Uploads the named saved configuration to an Amazon S3 bucket\. The *filename* must have the file extension `.cfg.yml`\. To specify the file name without a path, you can save the file to the `.elasticbeanstalk` folder or to the `.elasticbeanstalk/saved_configs/` folder before you run the command\. Alternatively, you can specify the *filename* by providing the full path\.

## Options<a name="eb3-configoptions"></a>


****  

|  Name  |  Description  | 
| --- | --- | 
|  `--cfg config-name`  |  The name to use for a saved configuration\. This option works with eb config save only\.  | 
|  `-d` or `--display`  |  Displays the configuration settings for the current environment \(writes to *stdout*\)\. Use with the `--format` option to specify the output to be in JSON or YAML\. If you don't specify, the output is in YAML format\. This option only works if you use the eb config command without any of the other subcommands\.  | 
|  `-f format_type` or `--format format_type`  |  Specifies display format\. Valid values are JSON or YAML\.  Defaults to YAML\. This option works with the `--display` option only\.  | 
|  `-﻿-﻿tags key1=value1[,key2=value2 ...]`  |  Tags to add to your saved configuration\. When specifying tags in the list, specify them as key=value pairs and separate each one with a comma\. For more information, see [Tagging saved configurations](environment-configuration-savedconfig-tagging.md)\. This option works with eb config save only\.  | 
|  `--timeout timeout`  |  The number of minutes before the command times out\.  | 
|  `-u configuration_string \| file_path` or `--update configuration_string \| file_path`  |  Updates the active configuration settings for the current environment\. This option only works if you use the eb config command without any of the other subcommands\. The `configuration_string \| file_path` parameter is of the type string\. The string provides the list of namespaces and corresponding options to add to, update, or remove from the configuration settings for your environment\. Alternatively, the input string can represent a file that contains the same information\. To specify a file name, the input string must follow the format `"file://<path><filename>"`\. To specify the file name without a `path`, save the file to the folder where you run the command\. Alternatively, specify the filename by providing the full path\. The configuration information must meet the following conditions\. At least one of the sections, OptionSettings or OptionsToRemove, is required\. Use OptionSettings to add or change options\. Use OptionsToRemove to remove options from a namespace\. For specific examples, see the [Examples](#eb3-configexample) section later in this topic\. 

**Example**  
*YAML Format*  

```
OptionSettings:
  namespace1:
    option-name-1: option-value-1
    option-name-2: option-value-2
    ...
OptionsToRemove:
  namespace1:
    option-name-1
    option-name-2
    ...
``` 

**Example**  
*JSON Format*  

```
{
   "OptionSettings": {
      "namespace1": {
         "option-name-1": "option-value-1",
         "option-name-2": "option-value-2",
         ...
      }
   },
   "OptionsToRemove": {
      "namespace1": {
         "option-name-1",
         "option-name-2",
         ...
      }
   }
}
```  | 
|  [Common options](eb3-cmd-options.md)  |  | 

## Output<a name="eb3-configoutput"></a>

If the eb config or eb config *environment\-name* command is run successfully with no subcommands or options added, the command displays your current option settings in the text editor that you configured as the EDITOR environment variable\. If you didn't configure an EDITOR environment variable, EB CLI displays your option settings in your default editor for YAML files\.

When you save changes to the file and close the editor, the environment is updated with the option settings that you saved in the file\. The following output is displayed to confirm the configuration update\.

```
$ eb config myApp-dev
    Printing Status:
    2021-05-19 18:09:45    INFO    Environment update is starting.
    2021-05-19 18:09:55    INFO    Updating environment myApp-dev's configuration settings.
    2021-05-19 18:11:20    INFO    Successfully deployed new configuration to environment.
```

If the command runs successfully with the `--display` option, it displays the configuration settings for the current environment \(writes to *stdout*\)\.

If the command runs successfully with the `get` parameter, the command displays the location of the local copy that you downloaded\.

If the command runs successfully with the `save` parameter, the command displays the location of the saved file\.

## Examples<a name="eb3-configexample"></a>

This section describes how to change the text editor that you use to view and edit your option settings file\.

For Linux and UNIX, the following example changes the editor to vim:

```
$ export EDITOR=vim
```

For Linux and UNIX, the following example changes the editor to whatever is installed at `/usr/bin/kate`\.

```
$ export EDITOR=/usr/bin/kate
```

For Windows, the following example changes the editor to Notepad\+\+\.

```
> set EDITOR="C:\Program Files\Notepad++\Notepad++.exe
```

This section provides examples for the eb config command when it's run with subcommands\.

The following example deletes the saved configuration named `app-tmp`\.

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

The following example saves configuration settings from the current running environment\. If you don't provide a name to use for the saved configuration, then Elastic Beanstalk names the configuration file according to the environment name\. For example, an environment named *tmp\-dev* would be called `tmp-dev.cfg.yml`\. Elastic Beanstalk saves the file to the `/.elasticbeanstalk/saved_configs/` folder\.

```
$ eb config save
```

In the following example, the `--cfg` option is used to save the configuration settings from the environment tmp\-dev to a file called `v1-app-tmp.cfg.yml`\. Elastic Beanstalk saves the file to the folder `/.elasticbeanstalk/saved_configs/`\. If you don't specify an environment name, Elastic Beanstalk saves configuration settings from the current running environment\.

```
$ eb config save tmp-dev --cfg v1-app-tmp
```

This section provides examples for the eb config command when it's run without subcommands\.

The following command displays the option settings of your current environment in a text editor\.

```
$ eb config
```

The following command displays the option settings for the *my\-env* environment in a text editor\.

```
$ eb config my-env
```

The following example displays the options settings for your current environment\. It outputs in the YAML format because no specific format was specified with the `--format` option\.

```
$ eb config --display
```

The following example updates the options settings for your current environment with the specifications in the file named `example.txt`\. The file is in either the YAML or JSON format\. The EB CLI automatically detects the file format\.
+  The Minsize option is set to 1 for the namespace `aws:autoscaling:asg`\. 
+  The batch size for the namespace `aws:elasticbeanstalk:command` is set to 30%\. 
+  It removes the option setting of *IdleTimeout: None* from the namespace `AWSEBV2LoadBalancer.aws:elbv2:loadbalancer`\. 

```
$ eb config --update "file://example.txt"
```

**Example \- filename: `example.txt` \- YAML format**  

```
OptionSettings:
  'aws:elasticbeanstalk:command':
    BatchSize: '30'
    BatchSizeType: Percentage
  'aws:autoscaling:asg':
    MinSize: '1'
OptionsToRemove:
  'AWSEBV2LoadBalancer.aws:elbv2:loadbalancer':
    IdleTimeout
```

**Example \- filename: `example.txt` \- JSON format**  

```
{
    "OptionSettings": {
        "aws:elasticbeanstalk:command": {
            "BatchSize": "30",
            "BatchSizeType": "Percentage"
        },
        "aws:autoscaling:asg": {
            "MinSize": "1"
        }
    },
    "OptionsToRemove": {
        "AWSEBV2LoadBalancer.aws:elbv2:loadbalancer": {
            "IdleTimeout"
        }
    }
}
```

The following examples update the options settings for your current environment\. The command sets the Minsize option to 1 for the`aws:autoscaling:asg` namespace\.

**Note**  
These examples are specific to Windows PowerShell\. They escape literal occurrences of the double\-quote \(`"`\) character by preceding it with a slash \(`\`\) character\. Different operating systems and command\-line environments might have different escape sequences\. For this reason, we recommend using the file option that's shown in the previous examples\. Specifying the configuration options in a file doesn't require escaping characters and is consistent across different operating systems\.

The following example is in JSON format\. The EB CLI detects if the format is in JSON or YAML\.

```
PS C:\Users\myUser\EB_apps\myApp-env>eb config --update '{\"OptionSettings\":{\"aws:autoscaling:asg\":{\"MaxSize\":\"1\"}}}'
```

The following example is in YAML format\. To enter the YAML string in the correct format, the command includes spacing and end\-of\-line returns that are required in a YAML file\.
+ End each line with the "enter" or "return" key\.
+ Start the second line with two spaces, and start the third line with four spaces\.

```
PS C:\Users\myUser\EB_apps\myApp-env>eb config --update 'OptionSettings:
>>  aws:autoscaling:asg:
>>    MinSize: \"1\"'
```