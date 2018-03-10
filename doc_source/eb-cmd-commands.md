# EB CLI 2 Commands<a name="eb-cmd-commands"></a>

**Note**  
 This version of the EB CLI and its documentation have been replaced with version 3 \(in this section, EB CLI 3 represents version 3 and later of the EB CLI\)\. For information on the new version, see [The Elastic Beanstalk Command Line Interface \(EB CLI\)](eb-cli3.md)\. 

You can use the EB command line interface to perform a wide variety of operations\.


+ [branch](branch.md)
+ [delete](delete.md)
+ [events](eb-events.md)
+ [init](init.md)
+ [logs](logs.md)
+ [push](push.md)
+ [start](start.md)
+ [status](status.md)
+ [stop](stop.md)
+ [update](update.md)

Eb stores environment settings in the `.elasticbeanstalk/optionsettings` file for the repository\. It is designed to read only from local files\. When you run `eb start` or `eb update`, Elastic Beanstalk reads the `.elasticbeanstalk/optionsettings` file and provides its contents as parameters to the `CreateEnvironment` or `UpdateEnvironment` API actions\.

You can use a configuration file in an `.ebextensions/*.conf` directory to configure some of the same settings that are in an `.elasticbeanstalk/optionsettings` file\. However, the values for the settings in `.elasticbeanstalk/optionsettings` will take precedence over anything in `.ebextensions/*.conf` if the settings are configured in both\. Additionally, any option setting that is specified using the API, including through eb, cannot later be changed in an environment using `.ebextensions` configuration files\.

When you run `eb branch`, Elastic Beanstalk will either add a section to the values in `.elasticbeanstalk/optionsettings` or create a new one for a new environment\. The command does not affect any running environments\.

To view your current settings, run `eb status --verbose`\. You might also want to use eb in conjunction with the Elastic Beanstalk console to get a complete picture of your applications and environments\.