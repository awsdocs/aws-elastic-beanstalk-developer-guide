# update<a name="update"></a>

**Note**  
 This version of EB CLI and its documentation have been replaced with version 3 \(in this section, EB CLI 3 represents version 3 and later of EB CLI\)\. For information on the new version, see \. 

## Description<a name="updatedescription"></a>

Updates the specified environment by reading the `.elasticbeanstalk/optionsettings`\. \(Setting values in `.elasticbeanstalk/optionsettings` take precedence over the values specified for the same settings specified in `.ebextensions/*.conf` if the settings are configured in both places\.\) Use this operation after making changes to your settings \(for example, via `init` or `branch`\)\.

## Syntax<a name="updatesyntax"></a>

 **`eb update`** 

## Options<a name="updateoptions"></a>


****  

|  Name  |  Description  |  Required  | 
| --- | --- | --- | 
|  `-e` or `--environment-name` *`ENVIRONMENT_NAME`*   |  The environment you want to update\. Type: String Default: Current setting  |  No  | 
|  Common options  |  For more information, see [Eb Common Options](eb-cmd-options.md)\.  |  No  | 

## Output<a name="updateoutput"></a>

If successful, the command returns the status of the update operation\.

## Example<a name="updateexample"></a>

The following example request updates the environment\.

```
PROMPT> eb update 

Update environment? [y/n]: y
Updating environment "MyApp-env". This may take a few minutes.
2014-05-15 17:10:34 INFO Updating environment MyApp-env's configuration
settings.
2014-05-15 17:11:12 INFO Successfully deployed new configuration to en
vironment.
2014-05-15 17:11:12 INFO Environment update completed successfully.
Update of environment "MyApp-env" has completed.
```