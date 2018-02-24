# Operations<a name="OperationList-cmd"></a>

**Note**  
 This tool, the Elastic Beanstalk API CLI, and its documentation have been replaced with the AWS CLI\. See the [AWS CLI User Guide ](http://docs.aws.amazon.com/cli/latest/userguide/) to get started with the AWS CLI\. Also try the [EB CLI](eb-cli3.md) for a simplified, higher\-level command line experience\. 


+ [elastic\-beanstalk\-check\-dns\-availability](#CLIReference-cmd-CheckDNSAvailability)
+ [elastic\-beanstalk\-create\-application](#CLIReference-cmd-CreateApplication)
+ [elastic\-beanstalk\-create\-application\-version](#CLIReference-cmd-CreateApplicationVersion)
+ [elastic\-beanstalk\-create\-configuration\-template](#CLIReference-cmd-CreateConfigurationTemplate)
+ [elastic\-beanstalk\-create\-environment](#CLIReference-cmd-CreateEnvironment)
+ [elastic\-beanstalk\-create\-storage\-location](#CLIReference-cmd-CreateStorageLocation)
+ [elastic\-beanstalk\-delete\-application](#CLIReference-cmd-DeleteApplication)
+ [elastic\-beanstalk\-delete\-application\-version](#CLIReference-cmd-DeleteApplicationVersion)
+ [elastic\-beanstalk\-delete\-configuration\-template](#CLIReference-cmd-DeleteConfigurationTemplate)
+ [elastic\-beanstalk\-delete\-environment\-configuration](#CLIReference-cmd-DeleteEnvironmentConfiguration)
+ [elastic\-beanstalk\-describe\-application\-versions](#CLIReference-cmd-DescribeApplicationVersions)
+ [elastic\-beanstalk\-describe\-applications](#CLIReference-cmd-DescribeApplications)
+ [elastic\-beanstalk\-describe\-configuration\-options](#CLIReference-cmd-DescribeConfigurationOptions)
+ [elastic\-beanstalk\-describe\-configuration\-settings](#CLIReference-cmd-DescribeConfigurationSettings)
+ [elastic\-beanstalk\-describe\-environment\-resources](#CLIReference-cmd-DescribeEnvironmentResources)
+ [elastic\-beanstalk\-describe\-environments](#CLIReference-cmd-DescribeEnvironments)
+ [elastic\-beanstalk\-describe\-events](#CLIReference-cmd-DescribeEvents)
+ [elastic\-beanstalk\-list\-available\-solution\-stacks](#CLIReference-cmd-ListAvailableSolutionStacks)
+ [elastic\-beanstalk\-rebuild\-environment](#CLIReference-cmd-RebuildEnvironment)
+ [elastic\-beanstalk\-request\-environment\-info](#CLIReference-cmd-RequestEnvironmentInfo)
+ [elastic\-beanstalk\-restart\-app\-server](#CLIReference-cmd-RestartAppServer)
+ [elastic\-beanstalk\-retrieve\-environment\-info](#CLIReference-cmd-RetrieveEnvironmentInfo)
+ [elastic\-beanstalk\-swap\-environment\-cnames](#CLIReference-cmd-SwapEnvironmentCNAMEs)
+ [elastic\-beanstalk\-terminate\-environment](#CLIReference-cmd-TerminateEnvironment)
+ [elastic\-beanstalk\-update\-application](#CLIReference-cmd-UpdateApplication)
+ [elastic\-beanstalk\-update\-application\-version](#CLIReference-cmd-UpdateApplicationVersion)
+ [elastic\-beanstalk\-update\-configuration\-template](#CLIReference-cmd-UpdateConfigurationTemplate)
+ [elastic\-beanstalk\-update\-environment](#CLIReference-cmd-UpdateEnvironment)
+ [elastic\-beanstalk\-validate\-configuration\-settings](#CLIReference-cmd-ValidateConfigurationSettings)

## elastic\-beanstalk\-check\-dns\-availability<a name="CLIReference-cmd-CheckDNSAvailability"></a>

### Description<a name="CLIReference-cmd-CheckDNSAvailability-Description"></a>

Checks if the specified CNAME is available\.

### Syntax<a name="CLIReference-cmd-CheckDNSAvailability-Syntax"></a>

 ** elastic\-beanstalk\-check\-dns\-availability \-c \[*CNAMEPrefix*\] ** 

### Options<a name="CLIReference-cmd-CheckDNSAvailability-Request"></a>


****  

| Name | Description | Required | 
| --- | --- | --- | 
|   `-c`   `--cname-prefix` *CNAMEPrefix*   |  The name of the CNAME to check\. Type: String Default: None  |  Yes  | 

### Output<a name="CLIReference-cmd-CheckDNSAvailability-Response"></a>

The command returns a table with the following information:

+ **Available—**Shows `true` if the CNAME is available; otherwise, shows `false`\.

+ **FullyQualifiedCNAME—**Shows the fully qualified CNAME if it is available; otherwise shows N/A\.

### Examples<a name="CLIReference-cmd-CheckDNSAvailability-Examples"></a>

#### Checking to Availability of a CNAME<a name="CLIReference-cmd-CheckDNSAvailability-Example-Request-1"></a>

This example shows how to check to see if the CNAME prefix "myapp23" is available\.

```
1. PROMPT> elastic-beanstalk-check-dns-availability -c myapp23
```

## elastic\-beanstalk\-create\-application<a name="CLIReference-cmd-CreateApplication"></a>

### Description<a name="CLIReference-cmd-CreateApplication-Description"></a>

Creates an application that has one configuration template named `default` and no application versions\.

**Note**  
The `default` configuration template is for a 32\-bit version of the Amazon Linux operating system running the Tomcat 6 application container\.

### Syntax<a name="CLIReference-cmd-CreateApplication-Syntax"></a>

 ** elastic\-beanstalk\-create\-application \-a \[*name*\] \-d \[*desc*\]** 

### Options<a name="CLIReference-cmd-CreateApplication-Request"></a>


****  

| Name | Description | Required | 
| --- | --- | --- | 
|   `-a`   `--application-name` *name*   |  The name of the application\. Constraint: This name must be unique within your account\. If the specified name already exists, the action returns an `InvalidParameterValue` error\. Type: String Default: None  |  Yes  | 
|   `-d`   `--description` *desc*   |  The description of the application\. Type: String Default: None  |  No  | 

### Output<a name="CLIReference-cmd-CreateApplication-Response"></a>

The command returns a table with the following information:

+ **ApplicationName—**The name of the application\. If no application is found with this name, and `AutoCreateApplication` is `false`, Elastic Beanstalk returns an `InvalidParameterValue` error\.

+ **ConfigurationTemplates—**A list of the configuration templates used to create the application\.

+ **DateCreated—**The date the application was created\.

+ **DateUpdated—**The date the application was last updated\.

+ **Description—**The description of the application\.

+ **Versions—**The versions of the application\.

### Examples<a name="CLIReference-cmd-CreateApplication-Examples"></a>

#### Creating an Application<a name="CLIReference-cmd-CreateApplication-Example-Request-1"></a>

This example shows how to create an application\.

```
1. PROMPT> elastic-beanstalk-create-application -a MySampleApp -d "My description"
```

## elastic\-beanstalk\-create\-application\-version<a name="CLIReference-cmd-CreateApplicationVersion"></a>

### Description<a name="CLIReference-cmd-CreateApplicationVersion-Description"></a>

Creates an application version for the specified application\.

**Note**  
 Once you create an application version with a specified Amazon S3 bucket and key location, you cannot change that Amazon S3 location\. If you change the Amazon S3 location, you receive an exception when you attempt to launch an environment from the application version\. 

### Syntax<a name="CLIReference-cmd-CreateApplicationVersion-Syntax"></a>

 ** elastic\-beanstalk\-create\-application\-version \-a \[*name*\] \-l \[*label*\] \-c \-d \[*desc*\] \-s \[*location*\] ** 

### Options<a name="CLIReference-cmd-CreateApplicationVersion-Request"></a>


****  

| Name | Description | Required | 
| --- | --- | --- | 
|   `-a`   `--application-name` *name*   |  The name of the application\. If no application is found with this name, and `AutoCreateApplication` is `false`, Elastic Beanstalk returns an `InvalidParameterValue` error\. Type: String Default: None Length Constraints: Minimum value of 1\. Maximum value of 100\.  |  Yes  | 
|   `-c`   `--auto-create`   |  Determines how the system behaves if the specified application for this version does not already exist:  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/OperationList-cmd.html) Type: Boolean Valid Values: `true` | `false` Default: `false`  |  No  | 
|   `-d`   `--description` *desc*   |  The description of the version\. Type: String Default: None Length Constraints: Minimum value of 0\. Maximum value of 200\.  |  No  | 
|   `-l`   `--version-label` *label*   |  A label identifying this version\. Type: String Default: None Constraint: Must be unique per application\. If an application version already exists with this label for the specified application, Elastic Beanstalk returns an `InvalidParameterValue` error\.  Length Constraints: Minimum value of 1\. Maximum value of 100\.  |  Yes  | 
|   `-s`   `--source-location` *location*   |  The name of the Amazon S3 bucket and key that identify the location of the source bundle for this version, in the format `bucketname/key`\.  If data found at the Amazon S3 location exceeds the maximum allowed source bundle size, Elastic Beanstalk returns an `InvalidParameterCombination` error\. Type: String Default: If not specified, Elastic Beanstalk uses a sample application\. If only partially specified \(for example, a bucket is provided but not the key\) or if no data is found at the Amazon S3 location, Elastic Beanstalk returns an `InvalidParameterCombination` error\.   |  No  | 

### Output<a name="CLIReference-cmd-CreateApplicationVersion-Response"></a>

The command returns a table with the following information:

+ **ApplicationName—**The name of the application\.

+ **DateCreated—**The date the application was created\.

+ **DateUpdated—**The date the application was last updated\.

+ **Description—**The description of the application\.

+ **SourceBundle—**The location where the source bundle is located for this version\. 

+ **VersionLabel—**A label uniquely identifying the version for the associated application\. 

### Examples<a name="CLIReference-cmd-CreateApplicationVersion-Examples"></a>

#### Creating a Version from a Source Location<a name="CLIReference-cmd-CreateApplicationVersion-Example-Request-1"></a>

This example shows create a version from a source location\.

```
1. PROMPT> elastic-beanstalk-create-application-version -a MySampleApp -d "My version" -l "TestVersion 1" -s amazonaws.com/sample.war
```

## elastic\-beanstalk\-create\-configuration\-template<a name="CLIReference-cmd-CreateConfigurationTemplate"></a>

### Description<a name="CLIReference-cmd-CreateConfigurationTemplate-Description"></a>

Creates a configuration template\. Templates are associated with a specific application and are used to deploy different versions of the application with the same configuration settings\.

### Syntax<a name="CLIReference-cmd-CreateConfigurationTemplate-Syntax"></a>

 ** elastic\-beanstalk\-create\-configuration\-template \-a \[*name*\] \-t \[*name*\] \-E \[*id*\] \-d \[*desc*\] \-s \[*stack*\] \-f \[*filename*\] \-A \[*name*\] \-T \[*name*\] ** 

### Options<a name="CLIReference-cmd-CreateConfigurationTemplate-Request"></a>


****  

| Name | Description | Required | 
| --- | --- | --- | 
|   `-a`   `--application-name` *name*   |  The name of the application to associate with this configuration template\. If no application is found with this name, Elastic Beanstalk returns an `InvalidParameterValue` error\.  Type: String Default: None  |  Yes  | 
|   `-t`   `--template-name` *name*   |  The name of the configuration template\. If a configuration template already exists with this name, Elastic Beanstalk returns an `InvalidParameterValue` error\.  Type: String Default: None Constraint: Must be unique for this application\. Length Constraints: Minimum value of 1\. Maximum value of 100\.  |  Yes  | 
|   `-E`   `--environment-id` *id*   |  The environment ID of the configuration template\. Type: String Default: None  |  No  | 
|   `-d`   `--description` *desc*   |  The description of the configuration\. Type: String Default: None  |  No  | 
|   `-s`   `--solution-stack` *stack*   |  The name of the solution stack used by this configuration\. The solution stack specifies the operating system, architecture, and application server for a configuration template\. It determines the set of configuration options as well as the possible and default values\. Use `elastic-beanstalk-list-available-solution-stacks` to obtain a list of available solution stacks\. A solution stack name or a source configuration parameter must be specified; otherwise, Elastic Beanstalk returns an `InvalidParameterValue` error\.  If a solution stack name is not specified and the source configuration parameter is specified, Elastic Beanstalk uses the same solution stack as the source configuration template\. Type: String Length Constraints: Minimum value of 0\. Maximum value of 100\.  |  No  | 
|   `-f`   `--options-file` *filename*   |  The name of a JSON file that contains a set of key\-value pairs defining configuration options for the configuration template\. The new values override the values obtained from the solution stack or the source configuration template\.  Type: String  |  No  | 
|   `-A`   `--source-application-name` *name*   |  The name of the application to use as the source for this configuration template\. Type: String Default: None  |  No  | 
|   `-T`   `--source-template-name` *name*   |  The name of the template to use as the source for this configuration template\. Type: String Default: None  |  No  | 

### Output<a name="CLIReference-cmd-CreateConfigurationTemplate-Response"></a>

The command returns a table with the following information:

+ **ApplicationName—**The name of the application associated with the configuration set\.

+ **DateCreated—**The date \(in UTC time\) when this configuration set was created\.

+ **DateUpdated—**The date \(in UTC time\) when this configuration set was last modified\.

+ **DeploymentStatus—**If this configuration set is associated with an environment, the deployment status parameter indicates the deployment status of this configuration set:

  + `null`: This configuration is not associated with a running environment\.

  + `pending`: This is a draft configuration that is not deployed to the associated environment but is in the process of deploying\.

  + `deployed`: This is the configuration that is currently deployed to the associated running environment\.

  + `failed`: This is a draft configuration that failed to successfully deploy\. 

+ **Description—**The description of the configuration set\.

+ **EnvironmentName—**If not `null`, the name of the environment for this configuration set\. 

+ **OptionSettings—**A list of configuration options and their values in this configuration set\. 

+ **SolutionStackName—**The name of the solution stack this configuration set uses\. 

+ **TemplateNamel—**If not `null`, the name of the configuration template for this configuration set\. 

### Examples<a name="CLIReference-cmd-CreateConfigurationTemplate-Examples"></a>

#### Creating a Basic Configuration Template<a name="CLIReference-cmd-CreateConfigurationTemplate-Example-Request-1"></a>

This example shows how to create a basic configuration template\. For a list of configuration settings, see [Configuration Options](command-options.md)\.

```
1. PROMPT> elastic-beanstalk-create-configuration-template -a MySampleApp -t myconfigtemplate -E e-eup272zdrw
```

### Related Operations<a name="CLIReference-cmd-CreateConfigurationTemplate-Related"></a>

+  [elastic\-beanstalk\-describe\-configuration\-options](#CLIReference-cmd-DescribeConfigurationOptions) 

+  [elastic\-beanstalk\-describe\-configuration\-settings](#CLIReference-cmd-DescribeConfigurationSettings) 

+  [elastic\-beanstalk\-list\-available\-solution\-stacks](#CLIReference-cmd-ListAvailableSolutionStacks) 

## elastic\-beanstalk\-create\-environment<a name="CLIReference-cmd-CreateEnvironment"></a>

### Description<a name="CLIReference-cmd-CreateEnvironment-Description"></a>

Launches an environment for the specified application using the specified configuration\. 

### Syntax<a name="CLIReference-cmd-CreateEnvironment-Syntax"></a>

 ** elastic\-beanstalk\-create\-environment \-a \[*name*\] \-l \[*label*\] \-e \[*name*\] \[\-t \[*name*\] | \-s \[*stack*\]\] \-c \[*prefix*\] \-d \[*desc*\] \-f\[*filename*\] \-F \[*filename*\] ** 

### Options<a name="CLIReference-cmd-CreateEnvironment-Request"></a>


****  

| Name | Description | Required | 
| --- | --- | --- | 
|   `-a`   `--application-name` *name*   |  The name of the application that contains the version to be deployed\. If no application is found with this name, Elastic Beanstalk returns an `InvalidParameterValue` error\.  Type: String Default: None Length Constraints: Minimum value of 1\. Maximum value of 100\.  |  Yes  | 
|   `-l`   `--version-label` *>label*   |  The name of the application version to deploy\. If the specified application has no associated application versions, Elastic Beanstalk `UpdateEnvironment` returns an `InvalidParameterValue` error\. Default: If not specified, Elastic Beanstalk attempts to launch the sample application in the container\. Type: String Default: None Length Constraints: Minimum value of 1\. Maximum value of 100\.  |  No  | 
|   `-e`   `--environment-name` *name*   |  A unique name for the deployment environment\. Used in the application URL\. Constraint: Must be from 4 to 23 characters in length\. The name can contain only letters, numbers, and hyphens\. It cannot start or end with a hyphen\. This name must be unique in your account\. If the specified name already exists, Elastic Beanstalk returns an `InvalidParameterValue`\. Type: String Default: If the CNAME parameter is not specified, the environment name becomes part of the CNAME, and therefore part of the visible URL for your application\.  |  Yes  | 
|   `-t`   `--template-name` *name*   |  The name of the configuration template to use in the deployment\. If no configuration template is found with this name, Elastic Beanstalk returns an `InvalidParameterValue` error\.  Conditional: You must specify either this parameter or a solution stack name, but not both\. If you specify both, Elastic Beanstalk returns an `InvalidParameterValue` error\. If you do not specify either, Elastic Beanstalk returns a `MissingRequiredParameter`\. Type: String Default: None Constraint: Must be unique for this application\.  |  Conditional  | 
|   `-s`   `--solution-stack` *stack*   |  This is the alternative to specifying a configuration name\. If specified, Elastic Beanstalk sets the configuration values to the default values associated with the specified solution stack\.  Condition: You must specify either this or a `TemplateName`, but not both\. If you specify both, Elastic Beanstalk returns an `InvalidParameterCombination` error\. If you do not specify either, Elastic Beanstalk returns `MissingRequiredParameter` error\. Type: String Default: None  |  Conditional  | 
|   `-c`   `--cname-prefix` *prefix*   |  If specified, the environment attempts to use this value as the prefix for the CNAME\. If not specified, the environment uses the environment name\. Type: String Default: None Length Constraints: Minimum value of 4\. Maximum value of 23\.  |  No  | 
|   `-d`   `--description` *desc*   |  The description of the environment\. Type: String Default: None  |  No  | 
|   `-f`   `--options-file` *filename*   |  The name of a JSON file that contains a set of key\-value pairs defining configuration options for this new environment\. These override the values obtained from the solution stack or the configuration template\. Type: String  |  No  | 
|   `-F`   `--options-to-remove-file` *value*   |  The name of a JSON file that contains configuration options to remove from the configuration set for this new environment\. Type: String Default: None  |  No  | 

### Output<a name="CLIReference-cmd-CreateEnvironment-Response"></a>

The command returns a table with the following information:

+ **ApplicationName—**The name of the application associated with this environment\.

+ **CNAME—**The URL to the CNAME for this environment\.

+ **DateCreated—**The date the environment was created\.

+ **DateUpdated—**The date the environment was last updated\.

+ **Description—**The description of the environment\.

+ **EndpointURL—**The URL to the load balancer for this environment\. 

+ **EnvironmentID—**The ID of this environment\. 

+ **EnvironmentName—**The name of this environment\. 

+ **Health—**Describes the health status of the environment\. Elastic Beanstalk indicates the failure levels for a running environment:

  + `Red`: Indicates the environment is not responsive\. Occurs when three or more consecutive failures occur for an environment\.

  + `Yellow`: Indicates that something is wrong\. Occurs when two consecutive failures occur for an environment\.

  + `Green`: Indicates the environment is healthy and fully functional\.

  + `Gray`: Default health for a new environment\. The environment is not fully launched and health checks have not started or health checks are suspended during an `UpdateEnvironment` or `RestartEnvironment`request\.

+ **Resources—**A list of AWS resources used in this environment\. 

+ **SolutionStackName—**The name of the solution stack deployed with this environment\.

+ **Status—**The current operational status of the environment:

  + `Launching`: Environment is in the process of initial deployment\.

  + `Updating`: Environment is in the process of updating its configuration settings or application version\.

  + `Ready`: Environment is available to have an action performed on it, such as update or terminate\.

  + `Terminating`: Environment is in the shut\-down process\.

  + `Terminated`: Environment is not running\.

+ **TemplateName—**The name of the configuration template used to originally launch this environment\.

+ **VersionLabel—**The application version deployed in this environment\.

### Examples<a name="CLIReference-cmd-CreateEnvironment-Examples"></a>

#### Creating an Environment Using a Basic Configuration Template<a name="CLIReference-cmd-CreateEnvironment-Example-Request-1"></a>

This example shows how to create an environment using a basic configuration template as well as pass in a file to edit configuration settings and a file to remove configuration settings\. For a list of configuration settings, see [Configuration Options](command-options.md)\.

```
$ elastic-beanstalk-create-environment -a MySampleApp -t myconfigtemplate -e MySampleAppEnv -f options.txt -F options_remove.txt
```

**options\.txt**

```
[
    {
        "Namespace": "aws:autoscaling:asg",
        "OptionName": "MinSize",
        "Value": "2"
    },
    {
        "Namespace": "aws:autoscaling:asg",
        "OptionName": "MaxSize",
        "Value": "3"
    }
]
```

**options\_remove\.txt**

```
[
    {
        "Namespace": "aws:elasticbeanstalk:sns:topics",
        "OptionName": "PARAM4"
    }
]
```

## elastic\-beanstalk\-create\-storage\-location<a name="CLIReference-cmd-CreateStorageLocation"></a>

### Description<a name="CLIReference-cmd-CreateStorageLocation-Description"></a>

Creates theAmazon S3 storage location for the account\. This location is used to store user log files and is used by the AWS Management Console to upload application versions\. You do not need to create this bucket in order to work with Elastic Beanstalk\. 

### Syntax<a name="CLIReference-cmd-CreateStorageLocation-Syntax"></a>

 ** elastic\-beanstalk\-create\-storage\-location ** 

### Examples<a name="CLIReference-cmd-CreateStorageLocation-Examples"></a>

#### Creating the Storage Location<a name="CLIReference-cmd-CreateStorageLocation-Example-Request-1"></a>

This example shows how to create a storage location\.

```
1. PROMPT> elastic-beanstalk-create-storage-location
```

This command will output the name of the Amazon S3 bucket created\.

## elastic\-beanstalk\-delete\-application<a name="CLIReference-cmd-DeleteApplication"></a>

### Description<a name="CLIReference-cmd-DeleteApplication-Description"></a>

Deletes the specified application along with all associated versions and configurations\. 

**Note**  
 You cannot delete an application that has a running environment\. 

### Syntax<a name="CLIReference-cmd-DeleteApplication-Syntax"></a>

 ** elastic\-beanstalk\-delete\-application \-a \[*name*\] \-f** 

### Options<a name="CLIReference-cmd-DeleteApplication-Request"></a>


****  

| Name | Description | Required | 
| --- | --- | --- | 
|   `-a`   `--application-name` *name*   |  The name of the application to delete\. Type: String Default: None Length Constraints: Minimum value of 1\. Maximum value of 100\.  |  Yes  | 
|   `-f`   `--force-terminate-env`   |  Determines if all running environments should be deleted before deleting the application\.  Type: Boolean Valid Values: `true | false` Default: `false`  |  No  | 

### Output<a name="CLIReference-cmd-DeleteApplication-Response"></a>

The command returns the string `Application deleted.`

### Examples<a name="CLIReference-cmd-DeleteApplication-Examples"></a>

#### Deleting an Application<a name="CLIReference-cmd-DeleteApplication-Example-Request-1"></a>

This example shows how to delete an application\.

```
1. PROMPT> elastic-beanstalk-delete-application -a MySampleApp
```

## elastic\-beanstalk\-delete\-application\-version<a name="CLIReference-cmd-DeleteApplicationVersion"></a>

### Description<a name="CLIReference-cmd-DeleteApplicationVersion-Description"></a>

Deletes the specified version from the specified application\.

**Note**  
 You cannot delete an application version that is associated with a running environment\. 

### Syntax<a name="CLIReference-cmd-DeleteApplicationVersion-Syntax"></a>

 ** elastic\-beanstalk\-delete\-application\-version \-a \[*name*\] \-l \[*label*\] \-d ** 

### Options<a name="CLIReference-cmd-DeleteApplicationVersion-Request"></a>


****  

| Name | Description | Required | 
| --- | --- | --- | 
|   `-a`   `--application-name` *name*   |  The name of the application to delete releases from\. Type: String Default: None  |  Yes  | 
|   `-l`   `--version-label`   |  The label of the version to delete\. Type: String Default: None Length Constraints: Minimum value of 1\. Maximum value of 100\.  |  Yes  | 
|   `-d`   `--delete-source-bundle`   |  Indicates whether to delete the associated source bundle from Amazon S3\. `true`: An attempt is made to delete the associated Amazon S3 source bundle specified at time of creation\. `false`: No action is taken on the Amazon S3 source bundle specified at time of creation\. Type: Boolean Valid Values: `true` | `false` Default: `false`  |  No  | 

### Output<a name="CLIReference-cmd-DeleteApplicationVersion-Response"></a>

The command returns the string `Application version deleted.`

### Examples<a name="CLIReference-cmd-DeleteApplicationVersion-Examples"></a>

#### Deleting an Application Version<a name="CLIReference-cmd-DeleteApplicationVersion-Example-Request-1"></a>

This example shows how to delete an application version\.

```
1. PROMPT> elastic-beanstalk-delete-application-version -a MySampleApp -l MyAppVersion
```

#### Deleting an Application Version and Amazon S3 Source Bundle<a name="CLIReference-cmd-DeleteApplicationVersion-Example-Request-2"></a>

This example shows how to delete an application version\.

```
1. PROMPT> elastic-beanstalk-delete-application-version -a MySampleApp -l MyAppVersion -d
```

## elastic\-beanstalk\-delete\-configuration\-template<a name="CLIReference-cmd-DeleteConfigurationTemplate"></a>

### Description<a name="CLIReference-cmd-DeleteConfigurationTemplate-Description"></a>

Deletes the specified configuration template\.

**Note**  
When you launch an environment using a configuration template, the environment gets a copy of the template\. You can delete or modify the environment's copy of the template without affecting the running environment\. 

### Syntax<a name="CLIReference-cmd-DeleteConfigurationTemplate-Syntax"></a>

 ** elastic\-beanstalk\-delete\-configuration\-template \-a \[*name*\] \-t \[*name*\]** 

### Options<a name="CLIReference-cmd-DeleteConfigurationTemplate-Request"></a>


****  

| Name | Description | Required | 
| --- | --- | --- | 
|   `-a`   `--application-name` *name*   |  The name of the application to delete the configuration template from\.  Type: String Default: None Length Constraints: Minimum value of 1\. Maximum value of 100\.  |  Yes  | 
|   `-t`   `--template-name`   |  The name of the configuration template to delete\. Type: String Default: None Length Constraints: Minimum value of 1\. Maximum value of 100\.  |  Yes  | 

### Output<a name="CLIReference-cmd-DeleteConfigurationTemplate-Response"></a>

The command returns the string `Configuration template deleted.`

### Examples<a name="CLIReference-cmd-DeleteConfigurationTemplate-Examples"></a>

#### Deleting a Configuration Template<a name="CLIReference-cmd-DeleteConfigurationTemplate-Example-Request-1"></a>

This example shows how to delete a configuration template\.

```
1. PROMPT> elastic-beanstalk-delete-configuration-template -a MySampleApp -t MyConfigTemplate
```

## elastic\-beanstalk\-delete\-environment\-configuration<a name="CLIReference-cmd-DeleteEnvironmentConfiguration"></a>

### Description<a name="CLIReference-cmd-DeleteEnvironmentConfiguration-Description"></a>

Deletes the draft configuration associated with the running environment\. 

**Note**  
Updating a running environment with any configuration changes creates a draft configuration set\. You can get the draft configuration using `elastic-beanstalk-describe-configuration-settings` while the update is in progress or if the update fails\. The deployment status for the draft configuration indicates whether the deployment is in process or has failed\. The draft configuration remains in existence until it is deleted with this action\. 

### Syntax<a name="CLIReference-cmd-DeleteEnvironmentConfiguration-Syntax"></a>

 ** elastic\-beanstalk\-delete\-environment\-configuration \-a \[*name*\] \-e \[*name*\]** 

### Options<a name="CLIReference-cmd-DeleteEnvironmentConfiguration-Request"></a>


****  

| Name | Description | Required | 
| --- | --- | --- | 
|   `-a`   `--application-name` *name*   |  The name of the application the environment is associated with\.  Type: String Default: None Length Constraints: Minimum value of 1\. Maximum value of 100\.  |  Yes  | 
|   `-e`   `--environment-name` *name*   |  The name of the environment to delete the draft configuration from\.  Type: String Default: None Length Constraints: Minimum value of 4\. Maximum value of 23\.  |  Yes  | 

### Output<a name="CLIReference-cmd-DeleteEnvironmentConfiguration-Response"></a>

The command returns the string `Environment configuration deleted.`

### Examples<a name="CLIReference-cmd-DeleteEnvironmentConfiguration-Examples"></a>

#### Deleting a Configuration Template<a name="CLIReference-cmd-DeleteEnvironmentConfiguration-Example-Request-1"></a>

This example shows how to delete a configuration template\.

```
1. PROMPT> elastic-beanstalk-delete-environment-configuration -a MySampleApp -e MyEnvConfig
```

## elastic\-beanstalk\-describe\-application\-versions<a name="CLIReference-cmd-DescribeApplicationVersions"></a>

### Description<a name="CLIReference-cmd-DescribeApplicationVersions-Description"></a>

Returns information about existing application versions\.

### Syntax<a name="CLIReference-cmd-DescribeApplicationVersions-Syntax"></a>

 ** elastic\-beanstalk\-describe\-application\-versions \-a \[*name*\] \-l \[*labels \[,label\.\.\]*\] ** 

### Options<a name="CLIReference-cmd-DescribeApplicationVersions-Request"></a>


****  

| Name | Description | Required | 
| --- | --- | --- | 
|   `-a`   `--application-name` *value*   |  The name of the application\. If specified, Elastic Beanstalk restricts the returned descriptions to only include ones that are associated with the specified application\.  Type: String Default: None Length Constraints: Minimum value of 1\. Maximum value of 100\.  |  No  | 
|   `-l`   `--version-label` *labels*   |  Comma\-delimited list of version labels\. If specified, restricts the returned descriptions to only include ones that have the specified version labels\.  Type: String\[\] Default: None  |  No  | 

### Output<a name="CLIReference-cmd-DescribeApplicationVersions-Response"></a>

The command returns a table with the following information:

+ **ApplicationName—**The name of the application associated with this release\.

+ **DateCreated—**The date the application was created\.

+ **DateUpdated—**The date the application version was last updated\.

+ **Description—**The description of the application version\.

+ **SourceBundle—**The location where the source bundle is located for this version\. 

+ **VersionLabel—**A label uniquely identifying the version for the associated application\. 

### Examples<a name="CLIReference-cmd-DescribeApplicationVersions-Examples"></a>

#### Describing Application Versions<a name="CLIReference-cmd-DescribeApplicationVersions-Example-Request-1"></a>

This example shows how to describe all application versions for this account\.

```
1. PROMPT> elastic-beanstalk-describe-application-versions
```

#### Describing Application Versions for a Specified Application<a name="CLIReference-cmd-DescribeApplicationVersions-Example-Request-2"></a>

This example shows how to describe application versions for a specific application\.

```
1. PROMPT> elastic-beanstalk-describe-application-versions -a MyApplication
```

#### Describing Multiple Application Versions<a name="CLIReference-cmd-DescribeApplicationVersions-Example-Request-3"></a>

This example shows how to describe multiple specified application versions\.

```
1. PROMPT> elastic-beanstalk-describe-application-versions -l MyAppVersion1, MyAppVersion2
```

## elastic\-beanstalk\-describe\-applications<a name="CLIReference-cmd-DescribeApplications"></a>

### Description<a name="CLIReference-cmd-DescribeApplications-Description"></a>

Returns descriptions about existing applications\.

### Syntax<a name="CLIReference-cmd-DescribeApplications-Syntax"></a>

 ** elastic\-beanstalk\-describe\-applications \-a \[*names \[,name\.\.\]*\] ** 

### Options<a name="CLIReference-cmd-DescribeApplications-Request"></a>


****  

| Name | Description | Required | 
| --- | --- | --- | 
|   `-a`   `--application-names` *name*   |  The name of one or more applications, separated by commas\. If specified, Elastic Beanstalk restricts the returned descriptions to only include those with the specified names\.  Type: String\[\] Default: None  |  No  | 

### Output<a name="CLIReference-cmd-DescribeApplications-Response"></a>

The command returns a table with the following information:

+ **ApplicationName—**The name of the application\.

+ **ConfigurationTemplates—**A list of the configuration templates used to create the application\.

+ **DateCreated—**The date the application was created\.

+ **DateUpdated—**The date the application was last updated\.

+ **Description—**The description of the application\.

+ **Versions—**The names of the versions for this application\.

### Examples<a name="CLIReference-cmd-DescribeApplications-Examples"></a>

#### Describing the Applications<a name="CLIReference-cmd-DescribeApplications-Example-Request-1"></a>

This example shows how to describe all applications for this account\.

```
1. PROMPT> elastic-beanstalk-describe-applications
```

#### Describing a Specific Application<a name="CLIReference-cmd-DescribeApplications-Example-Request-2"></a>

This example shows how to describe a specific application\.

```
1. PROMPT> elastic-beanstalk-describe-applications -a MyApplication
```

## elastic\-beanstalk\-describe\-configuration\-options<a name="CLIReference-cmd-DescribeConfigurationOptions"></a>

### Description<a name="CLIReference-cmd-DescribeConfigurationOptions-Description"></a>

Describes the configuration options that are used in a particular configuration template or environment, or that a specified solution stack defines\. The description includes the values, the options, their default values, and an indication of the required action on a running environment if an option value is changed\. 

### Syntax<a name="CLIReference-cmd-DescribeConfigurationOptions-Syntax"></a>

 ** elastic\-beanstalk\-describe\-configuration\-options \-a \[*name*\] \-t \[*name*\] \-e \[*name*\] \-s \[*stack*\] \-f \[*filename*\] ** 

### Options<a name="CLIReference-cmd-DescribeConfigurationOptions-Request"></a>


****  

| Name | Description | Required | 
| --- | --- | --- | 
|   `-a`   `--application-name` *name*   |  The name of the application associated with the configuration template or environment\. Only needed if you want to describe the configuration options associated with either the configuration template or environment\.  Type: String Default: None Length Constraints: Minimum value of 1\. Maximum value of 100\.  |  No  | 
|   `-t`   `--template-name` *name*   |  The name of the configuration template whose configuration options you want to describe\. Type: String Default: None Length Constraints: Minimum value of 1\. Maximum value of 100\.  |  No  | 
|   `-e`   `--environment-name` *name*   |  The name of the environment whose configuration options you want to describe\. Type: String Length Constraints: Minimum value of 4\. Maximum value of 23\.  |  No  | 
|   `-s`   `--solution-stack` *stack*   |  The name of the solution stack whose configuration options you want to describe\. Type: String Default: None Length Constraints: Minimum value of 0\. Maximum value of 100\.  |  No  | 
|   `-f`   `--options-file` *filename*   |  The name of a JSON file that contains the options you want described\.  Type: String  |  No  | 

### Output<a name="CLIReference-cmd-DescribeConfigurationOptions-Response"></a>

The command returns a table with the following information:

+ **Options—**A list of the configuration options\.

+ **SolutionStackName—**The name of the `SolutionStack` these configuration options belong to\.

### Examples<a name="CLIReference-cmd-DescribeConfigurationOptions-Examples"></a>

#### Describing Configuration Options for an Environment<a name="CLIReference-cmd-DescribeConfigurationOptions-Example-Request-1"></a>

This example shows how to describe configuration options for an environment\.

```
1. PROMPT> elastic-beanstalk-describe-configuration-options -a MySampleApp -t myconfigtemplate -e MySampleAppEnv
```

## elastic\-beanstalk\-describe\-configuration\-settings<a name="CLIReference-cmd-DescribeConfigurationSettings"></a>

### Description<a name="CLIReference-cmd-DescribeConfigurationSettings-Description"></a>

 Returns a description of the settings for the specified configuration set, that is, either a configuration template or the configuration set associated with a running environment\.

When describing the settings for the configuration set associated with a running environment, it is possible to receive two sets of setting descriptions\. One is the deployed configuration set, and the other is a draft configuration of an environment that is either in the process of deployment or that failed to deploy\. 

### Syntax<a name="CLIReference-cmd-DescribeConfigurationSettings-Syntax"></a>

 ** elastic\-beanstalk\-describe\-configuration\-settings \-a \[*name*\] \[\-t \[*name*\] | \-e \[*name*\]\] ** 

### Options<a name="CLIReference-cmd-DescribeConfigurationSettings-Request"></a>


****  

| Name | Description | Required | 
| --- | --- | --- | 
|   `-a`   `--application-name` *name*   |  The application name for the environment or configuration template\.  Type: String Default: None Length Constraints: Minimum value of 1\. Maximum value of 100\.  |  Yes  | 
|   `-t`   `--template-name` *name*   |  The name of the configuration template to describe\. If no configuration template is found with this name, Elastic Beanstalk returns an `InvalidParameterValue` error\.  Conditional: You must specify either this parameter or an environment name, but not both\. If you specify both, Elastic Beanstalk returns an `InvalidParameterValue` error\. If you do not specify either, Elastic Beanstalk returns a `MissingRequiredParameter` error\. Type: String Default: None Length Constraints: Minimum value of 1\. Maximum value of 100\.  |  Conditional  | 
|   `-e`   `--environment-name` *name*   |  The name of the environment to describe\. Type: String Default: None Length Constraints: Minimum value of 4\. Maximum value of 23\.  |  Conditional  | 

### Output<a name="CLIReference-cmd-DescribeConfigurationSettings-Response"></a>

The command returns a table with the following information:

+ **ConfigurationSettings—**A list of the configuration settings\.

### Examples<a name="CLIReference-cmd-DescribeConfigurationSettings-Examples"></a>

#### Describing Configuration Settings for an Environment<a name="CLIReference-cmd-DescribeConfigurationSettings-Example-Request-1"></a>

This example shows how to describe the configuration options for an environment\.

```
1. PROMPT> elastic-beanstalk-describe-configuration-settings -a MySampleApp -e MySampleAppEnv
```

### Related Operations<a name="CLIReference-cmd-DescribeConfigurationSettings-Related"></a>

+  [elastic\-beanstalk\-delete\-environment\-configuration](#CLIReference-cmd-DeleteEnvironmentConfiguration) 

## elastic\-beanstalk\-describe\-environment\-resources<a name="CLIReference-cmd-DescribeEnvironmentResources"></a>

### Description<a name="CLIReference-cmd-DescribeEnvironmentResources-Description"></a>

 Returns AWS resources for this environment\.

### Syntax<a name="CLIReference-cmd-DescribeEnvironmentResources-Syntax"></a>

 ** elastic\-beanstalk\-describe\-environment\-resources \[\-e \[*name*\] | \-E \[*id*\]\] ** 

### Options<a name="CLIReference-cmd-DescribeEnvironmentResources-Request"></a>


****  

| Name | Description | Required | 
| --- | --- | --- | 
|   `-e`   `--environment-name` *name*   |  The name of the environment to retrieve AWS resource usage data\. Type: String Default: None Length Constraints: Minimum value of 4\. Maximum value of 23\.  |  Conditional  | 
|   `-E`   `--environment-id` *id*   |  The ID of the environment to retrieve AWS resource usage data\. Type: String Default: None  |  Conditional  | 

### Output<a name="CLIReference-cmd-DescribeEnvironmentResources-Response"></a>

The command returns a table with the following information:

+ **AutoScalingGroups—**A list of AutoScaling groups used by this environment\.

+ **EnvironmentName—**The name of the environment\.

+ **Instances—**The Amazon EC2 instances used by this environment\.

+ **LaunchConfigurations—**The Auto Scaling launch configurations in use by this environment\.

+ **LoadBalancers—**The load balancers in use by this environment\.

+ **Triggers—**The Auto Scaling triggers in use by this environment\.

### Examples<a name="CLIReference-cmd-DescribeEnvironmentResources-Examples"></a>

#### Describing Environment Resources for an Environment<a name="CLIReference-cmd-DescribeEnvironmentResources-Example-Request-1"></a>

This example shows how to describe environment resources for an environment\.

```
1. PROMPT> elastic-beanstalk-describe-environment-resources -e MySampleAppEnv
```

## elastic\-beanstalk\-describe\-environments<a name="CLIReference-cmd-DescribeEnvironments"></a>

### Description<a name="CLIReference-cmd-DescribeEnvironments-Description"></a>

 Returns descriptions for existing environments\.

### Syntax<a name="CLIReference-cmd-DescribeEnvironments-Syntax"></a>

 ** elastic\-beanstalk\-describe\-environments \-e \[*names \[,name\.\.\.\]*\] \-E \[*ids \[,id\.\.\.\]*\] \-a \[*name*\] \-l \[*label*\] \-d \-D \[*timestamp*\] ** 

### Options<a name="CLIReference-cmd-DescribeEnvironments-Request"></a>


****  

| Name | Description | Required | 
| --- | --- | --- | 
|   `-e`   `--environment-names` *names*   |  A list of environment names\. Type: String\[\] Default: None  |  No  | 
|   `-E`   `--environment-ids` *ids*   |  A list of environment IDs\. Type: String\[\] Default: None  |  No  | 
|   `-a`   `--application-name` *name*   |  A list of descriptions associated with the application\. Type: String Default: None Length Constraints: Minimum value of 1\. Maximum value of 100\.  |  No  | 
|   `-l`   `--version-label` *>label*   |  A list of descriptions associated with the application version\. Type: String Default: None Length Constraints: Minimum value of 1\. Maximum value of 100\.  |  No  | 
|   `-d`   `--include-deleted`   |  Indicates whether to include deleted environments\. `true`: Environments that have been deleted after `--include-deleted-back-to` are displayed\. `false`: Do not include deleted environments\. Type: Boolean Default: `true`  |  No  | 
|   `-D`   `--include-deleted-back-to` *timestamp*   |  If \-\-include\-deleted is set to `true`, then a list of environments that were deleted after this date are displayed\. Type: Date Time Default: None  |  No  | 

### Output<a name="CLIReference-cmd-DescribeEnvironments-Response"></a>

The command returns a table with the following information:

+ **ApplicationName—**The name of the application associated with this environment\.

+ **CNAME—**The URL to the CNAME for this environment\.

+ **DateCreated—**The date the environment was created\.

+ **DateUpdated—**The date the environment was last updated\.

+ **Description—**The description of the environment\.

+ **EndpointURL—**The URL to the load balancer for this environment\. 

+ **EnvironmentID—**The ID of this environment\. 

+ **EnvironmentName—**The name of this environment\. 

+ **Health—**Describes the health status of the environment\. Elastic Beanstalk indicates the failure levels for a running environment:

  + `Red`: Indicates the environment is not responsive\. Occurs when three or more consecutive failures occur for an environment\.

  + `Yellow`: Indicates that something is wrong\. Occurs when two consecutive failures occur for an environment\.

  + `Green`: Indicates the environment is healthy and fully functional\.

  + `Gray`: Default health for a new environment\. The environment is not fully launched and health checks have not started or health checks are suspended during an `UpdateEnvironment` or `RestartEnvironment`request\.

+ **Resources—**A list of AWS resources used in this environment\. 

+ **SolutionStackName—**The name of the `SolutionStack` deployed with this environment\.

+ **Status—**The current operational status of the environment:

  + `Launching`: Environment is in the process of initial deployment\.

  + `Updating`: Environment is in the process of updating its configuration settings or application version\.

  + `Ready`: Environment is available to have an action performed on it, such as update or terminate\.

  + `Terminating`: Environment is in the shut\-down process\.

  + `Terminated`: Environment is not running\.

+ **TemplateName—**The name of the configuration template used to originally launch this environment\.

+ **VersionLabel—**The application version deployed in this environment\.

### Examples<a name="CLIReference-cmd-DescribeEnvironments-Examples"></a>

#### Describing Environments<a name="CLIReference-cmd-DescribeEnvironments-Example-Request-1"></a>

This example shows how to describe existing environments\.

```
1. PROMPT> elastic-beanstalk-describe-environments
```

## elastic\-beanstalk\-describe\-events<a name="CLIReference-cmd-DescribeEvents"></a>

### Description<a name="CLIReference-cmd-DescribeEvents-Description"></a>

 Returns a list of event descriptions matching criteria up to the last 6 weeks\.

**Note**  
This action returns the most recent 1,000 events from the specified `NextToken`\.

### Syntax<a name="CLIReference-cmd-DescribeEvents-Syntax"></a>

 ** elastic\-beanstalk\-describe\-events \-a \[*name*\] \-e \[*name*\] \-E \[*id*\] \-l \[*label*\] \-L \[*timestamp*\] \-m \[*count*\] \-n \[*token*\] \-r \[*id*\] \-s \[*level*\] \-S \[*timestamp*\] \-t \[*name*\] ** 

### Options<a name="CLIReference-cmd-DescribeEvents-Request"></a>


****  

| Name | Description | Required | 
| --- | --- | --- | 
|   `-a`   `--application-name` *name*   |  The name of the application\. Type: String Default: None Length Constraints: Minimum value of 1\. Maximum value of 100\.  |  No  | 
|   `-e`   `--environment-name` *name*   |  The name of the environment\. Type: String Default: None Length Constraints: Minimum value of 4\. Maximum value of 23\.  |  No  | 
|   `-E`   `--environment-id` *id*   |  The ID of the environment\. Type: String Default: None  |  No  | 
|   `-l`   `--version-label` *>label*   |  The application version\. Type: String Default: None Length Constraints: Minimum value of 1\. Maximum value of 100\.  |  No  | 
|   `-L`   `--end-time` *timestamp*   |  If specified, a list of events that occurred up to but not including the specified time is returned\. Type: Date Time Default: None  |  No  | 
|   `-m`   `--max-records` *count*   |  Specifies the maximum number of events that can be returned, beginning with the most recent event\.  Type: Integer Default: None  |  No  | 
|   `-n`   `--next-token` *token*   |  Pagination token\. Used to return the next batch of results\. Type: String Default: None  |  No  | 
|   `-r`   `--request-id` *id*   |  The request ID\. Type: String Default: None  |  No  | 
|   `-s`   `--severity ` *level*   |  If specified, a list of events with the specified severity level or higher is returned\. Type: String Valid Values: `TRACE` | `DEBUG` | `INFO` | `WARN` | `ERROR` | `FATAL` Default: None  |  No  | 
|   `-S`   `--start-time` *timestamp*   |  If specified, a list of events that occurred after the specified time is returned\. Type: Date Time Default: None  |  No  | 
|   `-t`   `--template-name` *name*   |  The name of the configuration template\. Type: String Default: None Length Constraints: Minimum value of 1\. Maximum value of 100\.  |  No  | 

### Output<a name="CLIReference-cmd-DescribeEvents-Response"></a>

The command returns a table with the following information:

+ **ApplicationName—**The name of the application associated with the event\.

+ **EnvironmentName—**The name of the environment associated with the event\. 

+ **EventDate—**The date of the event\.

+ **Message—**The event's message\. 

+ **RequestID—**The web service request ID for the activity of this event\.

+ **Severity—**The severity level of the event\. 

+ **TemplateName—**The name of the configuration associated with this event\.

+ **VersionLabel—**The release label for the application version associated with this event\.

### Examples<a name="CLIReference-cmd-DescribeEvents-Examples"></a>

#### Describing Events for an Environment with a Security Level<a name="CLIReference-cmd-DescribeEvents-Example-Request-1"></a>

This example shows how to describe events that have a severity level of WARN or higher for an environment\.

```
1. PROMPT> elastic-beanstalk-describe-events -e MySampleAppEnv -s WARN
```

## elastic\-beanstalk\-list\-available\-solution\-stacks<a name="CLIReference-cmd-ListAvailableSolutionStacks"></a>

### Description<a name="CLIReference-cmd-ListAvailableSolutionStacks-Description"></a>

 Returns a list of available solution stack names\.

### Syntax<a name="CLIReference-cmd-ListAvailableSolutionStacks-Syntax"></a>

 ** elastic\-beanstalk\-list\-available\-solution\-stacks ** 

### Output<a name="CLIReference-cmd-ListAvailableSolutionStacks-Response"></a>

The command returns of available solution stack names\.

### Examples<a name="CLIReference-cmd-ListAvailableSolutionStacks-Examples"></a>

#### Listing the Available Solution Stacks<a name="CLIReference-cmd-ListAvailableSolutionStacks-Example-Request-1"></a>

This example shows how to get the list of available solution stacks\.

```
1. PROMPT> elastic-beanstalk-list-available-solution-stacks
```

## elastic\-beanstalk\-rebuild\-environment<a name="CLIReference-cmd-RebuildEnvironment"></a>

### Description<a name="CLIReference-cmd-RebuildEnvironment-Description"></a>

Deletes and recreates all of the AWS resources \(for example: the Auto Scaling group, Elastic Load Balancing, etc\.\) for a specified environment and forces a restart\.

### Syntax<a name="CLIReference-cmd-RebuildEnvironment-Syntax"></a>

 ** elastic\-beanstalk\-rebuild\-environment \[\-e \[*name*\] | \-E \[*id*\]\]** 

### Options<a name="CLIReference-cmd-RebuildEnvironment-Request"></a>


****  

| Name | Description | Required | 
| --- | --- | --- | 
|   `-e`   `--environment-name` *name*   |  A name of the environment to rebuild\. Type: String Default: None Length Constraints: Minimum value of 4\. Maximum value of 23\.  |  Conditional  | 
|   `-E`   `--environment-id` *id*   |  The ID of the environment to rebuild\. Type: String Default: None  |  Conditional  | 

### Output<a name="CLIReference-cmd-RebuildEnvironment-Response"></a>

The command outputs `Rebuilding environment`\.

### Examples<a name="CLIReference-cmd-RebuildEnvironment-Examples"></a>

#### Rebuilding an Environment<a name="CLIReference-cmd-RebuildEnvironment-Example-Request-1"></a>

This example shows how to rebuild an environment\.

```
1. PROMPT> elastic-beanstalk-rebuild-environment -e MySampleAppEnv
```

## elastic\-beanstalk\-request\-environment\-info<a name="CLIReference-cmd-RequestEnvironmentInfo"></a>

### Description<a name="CLIReference-cmd-RequestEnvironmentInfo-Description"></a>

Initiates a request to compile the specified type of information of the deployed environment\.

Setting the InfoType to `tail` compiles the last lines from the application server log files of every Amazon EC2 instance in your environment\. Use `RetrieveEnvironmentInfo` to access the compiled information\.

### Syntax<a name="CLIReference-cmd-RequestEnvironmentInfo-Syntax"></a>

 ** elastic\-beanstalk\-request\-environment\-info \[\-e \[*name*\] | \-E \[*id*\]\] \-i \[*type*\] ** 

### Options<a name="CLIReference-cmd-RequestEnvironmentInfo-Request"></a>


****  

| Name | Description | Required | 
| --- | --- | --- | 
|   `-e`   `--environment-name` *name*   |  The name of the environment of the requested data\. Type: String Default: None Length Constraints: Minimum value of 4\. Maximum value of 23\.  |  Conditional  | 
|   `-E`   `--environment-id` *id*   |  The ID of the environment of the requested data\. Type: String Default: None  |  Conditional  | 
|   `-i`   `--info-type` *type*   |  The type of information to request\. Type: String Valid Values: `tail` Default: None  |  Yes  | 

### Examples<a name="CLIReference-cmd-RequestEnvironmentInfo-Examples"></a>

#### Requesting Environment Information<a name="CLIReference-cmd-RequestEnvironmentInfo-Example-Request-1"></a>

This example shows how to request environment information\.

```
1. PROMPT> elastic-beanstalk-request-environment-info -e MySampleAppEnv -i tail
```

### Related Operations<a name="CLIReference-cmd-RequestEnvironmentInfo-Related"></a>

+  [elastic\-beanstalk\-retrieve\-environment\-info](#CLIReference-cmd-RetrieveEnvironmentInfo) 

## elastic\-beanstalk\-restart\-app\-server<a name="CLIReference-cmd-RestartAppServer"></a>

### Description<a name="CLIReference-cmd-RestartAppServer-Description"></a>

Causes the environment to restart the application container server running on each Amazon EC2 instance\.

### Syntax<a name="CLIReference-cmd-RestartAppServer-Syntax"></a>

 ** elastic\-beanstalk\-restart\-app\-server \[\-e \[*name*\] | \-E \[*id*\]\] ** 

### Options<a name="CLIReference-cmd-RestartAppServer-Request"></a>


****  

| Name | Description | Required | 
| --- | --- | --- | 
|   `-e`   `--environment-name` *name*   |  The name of the environment to restart the server for\. Type: String Default: None Length Constraints: Minimum value of 4\. Maximum value of 23\.  |  Conditional  | 
|   `-E`   `--environment-id` *id*   |  The ID of the environment to restart the server for\. Type: String Default: None  |  Conditional  | 

### Examples<a name="CLIReference-cmd-RestartAppServer-Examples"></a>

#### Restarting the Application Server<a name="CLIReference-cmd-RestartAppServer-Example-Request-1"></a>

This example shows how to restart the application server\.

```
1. PROMPT> elastic-beanstalk-restart-app-server -e MySampleAppEnv
```

## elastic\-beanstalk\-retrieve\-environment\-info<a name="CLIReference-cmd-RetrieveEnvironmentInfo"></a>

### Description<a name="CLIReference-cmd-RetrieveEnvironmentInfo-Description"></a>

Retrieves the compiled information from a `RequestEnvironmentInfo` request\.

### Syntax<a name="CLIReference-cmd-RetrieveEnvironmentInfo-Syntax"></a>

 ** elastic\-beanstalk\-retrieve\-environment\-info \[\-e \[*name*\] | \-E \[*id*\]\] \-i \[*type*\] ** 

### Options<a name="CLIReference-cmd-RetrieveEnvironmentInfo-Request"></a>


****  

| Name | Description | Required | 
| --- | --- | --- | 
|   `-e`   `--environment-name` *name*   |  The name of the data's environment\. If no environments are found, Elastic Beanstalk returns an `InvalidParameterValue` error\. Type: String Default: None Length Constraints: Minimum value of 4\. Maximum value of 23\.  |  Conditional  | 
|   `-E`   `--environment-id` *id*   |  The ID of the data's environment\. The name of the data's environment\. If no environments are found, Elastic Beanstalk returns an `InvalidParameterValue` error\. Type: String Default: None  |  Conditional  | 
|   `-i`   `--info-type` *type*   |  The type of information to retrieve\. Type: String Valid Values: tail Default: None  |  Yes  | 

### Output<a name="CLIReference-cmd-RetrieveEnvironmentInfo-Response"></a>

The command returns a table with the following information:

+ **EC2InstanceId—**The Amazon EC2 instance ID for this information\.

+ **InfoType—**The type of information retrieved\. 

+ **Message—**The retrieved information\.

+ **SampleTimestamp—**The time stamp when this information was retrieved\. 

### Examples<a name="CLIReference-cmd-RetrieveEnvironmentInfo-Examples"></a>

#### Retrieving Environment Information<a name="CLIReference-cmd-RetrieveEnvironmentInfo-Example-Request-1"></a>

This example shows how to retrieve environment information\.

```
1. PROMPT> elastic-beanstalk-retrieve-environment-info -e MySampleAppEnv -i tail
```

### Related Operations<a name="CLIReference-cmd-RetrieveEnvironmentInfo-Related"></a>

+  [elastic\-beanstalk\-request\-environment\-info](#CLIReference-cmd-RequestEnvironmentInfo) 

## elastic\-beanstalk\-swap\-environment\-cnames<a name="CLIReference-cmd-SwapEnvironmentCNAMEs"></a>

### Description<a name="CLIReference-cmd-SwapEnvironmentCNAMEs-Description"></a>

 Swaps the CNAMEs of two environments\.

### Syntax<a name="CLIReference-cmd-SwapEnvironmentCNAMEs-Syntax"></a>

 ** elastic\-beanstalk\-swap\-environment\-cnames \[\-s \[*name*\] | \-S \[*desc*\]\] \[\-d \[*desc*\] | \-D \[*desc*\]\] ** 

### Options<a name="CLIReference-cmd-SwapEnvironmentCNAMEs-Request"></a>


****  

| Name | Description | Required | 
| --- | --- | --- | 
|   `-s`   `--source-environment-name` *name*   |  The name of the source environment\. Type: String Default: None  |  Conditional  | 
|   `-S`   `--source-environment-id` *id*   |  The ID of the source environment\. Type: String Default: None  |  Conditional  | 
|   `-d`   `--destination-environment-name` *name*   |  The name of the destination environment\. Type: String Default: None  |  Conditional  | 
|   `-D`   `--destination-environment-id` *id*   |  The ID of the destination environment\. Type: String Default: None  |  Conditional  | 

### Examples<a name="CLIReference-cmd-SwapEnvironmentCNAMEs-Examples"></a>

#### Swapping Environment CNAMEs<a name="CLIReference-cmd-SwapEnvironmentCNAMEs-Example-Request-1"></a>

This example shows how to swap the CNAME for two environments\.

```
1. PROMPT> elastic-beanstalk-swap-environment-cnames -s MySampleAppEnv -d MySampleAppEnv2
```

## elastic\-beanstalk\-terminate\-environment<a name="CLIReference-cmd-TerminateEnvironment"></a>

### Description<a name="CLIReference-cmd-TerminateEnvironment-Description"></a>

Terminates the specified environment\.

### Syntax<a name="CLIReference-cmd-TerminateEnvironment-Syntax"></a>

 ** elastic\-beanstalk\-terminate\-environment \[\-e \[*name*\] | \-E \[*id*\]\] \-t ** 

### Options<a name="CLIReference-cmd-TerminateEnvironment-Request"></a>


****  

| Name | Description | Required | 
| --- | --- | --- | 
|   `-e`   `--environment-name` *name*   |  The name of the environment to terminate\. Type: String Default: None Length Constraints: Minimum value of 4\. Maximum value of 23\.  |  Conditional  | 
|   `-E`   `--environment-id` *id*   |  The ID of the environment to terminate\. Type: String Default: None  |  Conditional  | 
|   `-t`   `--terminate-resources`   |  Indicates whether the associated AWS resources should shut down when the environment is terminated: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/OperationList-cmd.html) [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/OperationList-cmd.html) Type: Boolean Valid Values: `true` | `false` Default: true  You can specify this parameter \(`-t`\) only for legacy environments because only legacy environments can have resources running when you terminate the environment\.   |  No  | 

### Output<a name="CLIReference-cmd-TerminateEnvironment-Response"></a>

The command returns a table with the following information:

+ **ApplicationName—**The name of the application associated with this environment\.

+ **CNAME—**The URL to the CNAME for this environment\.

+ **DateCreated—**The date the environment was created\.

+ **DateUpdated—**The date the environment was last updated\.

+ **Description—**The description of the environment\.

+ **EndpointURL—**The URL to the load balancer for this environment\. 

+ **EnvironmentID—**The ID of this environment\. 

+ **EnvironmentName—**The name of this environment\. 

+ **Health—**Describes the health status of the environment\. Elastic Beanstalk indicates the failure levels for a running environment:

  + `Red`: Indicates the environment is not responsive\. Occurs when three or more consecutive failures occur for an environment\.

  + `Yellow`: Indicates that something is wrong\. Occurs when two consecutive failures occur for an environment\.

  + `Green`: Indicates the environment is healthy and fully functional\.

  + `Gray`: Default health for a new environment\. The environment is not fully launched, and health checks have not started or health checks are suspended during an `UpdateEnvironment` or `RestartEnvironment` request\.

+ **Resources—**A list of AWS resources used in this environment\. 

+ **SolutionStackName—**The name of the `SolutionStack` deployed with this environment\.

+ **Status—**The current operational status of the environment:

  + `Launching`: Environment is in the process of initial deployment\.

  + `Updating`: Environment is in the process of updating its configuration settings or application version\.

  + `Ready`: Environment is available to have an action performed on it, such as update or terminate\.

  + `Terminating`: Environment is in the shutdown process\.

  + `Terminated`: Environment is not running\.

+ **TemplateName—**The name of the configuration template used to originally launch this environment\.

+ **VersionLabel—**The application version deployed in this environment\.

### Examples<a name="CLIReference-cmd-TerminateEnvironment-Examples"></a>

#### Terminating an Environment<a name="CLIReference-cmd-TerminateEnvironment-Example-Request-1"></a>

This example shows how to terminate an environment\.

```
1. PROMPT> elastic-beanstalk-terminate-environment -e MySampleAppEnv
```

## elastic\-beanstalk\-update\-application<a name="CLIReference-cmd-UpdateApplication"></a>

### Description<a name="CLIReference-cmd-UpdateApplication-Description"></a>

Updates the specified application to have the specified properties\.

**Note**  
If a property \(for example, `description`\) is not provided, the value remains unchanged\. To clear these properties, specify an empty string\.

### Syntax<a name="CLIReference-cmd-UpdateApplication-Syntax"></a>

 ** elastic\-beanstalk\-update\-application \-a \[*name*\] \-d \[*desc*\] ** 

### Options<a name="CLIReference-cmd-UpdateApplication-Request"></a>


****  

| Name | Description | Required | 
| --- | --- | --- | 
|   `-a`   `--application-name` *name*   |  The name of the application to update\. If no such application is found, Elastic Beanstalk returns an `InvalidParameterValue` error\. Type: String Default: None Length Constraints: Minimum value of 1\. Maximum value of 100\.  |  Yes  | 
|   `-d`   `--description` *desc*   |  A new description for the application\. Type: String Default: If not specified, Elastic Beanstalk does not update the description\. Length Constraints: Minimum value of 0\. Maximum value of 200\.  |  No  | 

### Output<a name="CLIReference-cmd-UpdateApplication-Response"></a>

The command returns a table with the following information:

+ **ApplicationName—**The name of the application\.

+ **ConfigurationTemplate—**The names of the configuration templates associated with this application\.

+ **DateCreated—**The date the environment was created\.

+ **DateUpdated—**The date the environment was last updated\.

+ **Description—**The description of the environment\.

+ **Versions—**The names of the versions for this application\.

### Examples<a name="CLIReference-cmd-UpdateApplication-Examples"></a>

#### Updating an Application<a name="CLIReference-cmd-UpdateApplication-Example-Request-1"></a>

This example shows how to update an application\.

```
1. PROMPT> elastic-beanstalk-update-application -a MySampleApp -d "My new description"
```

## elastic\-beanstalk\-update\-application\-version<a name="CLIReference-cmd-UpdateApplicationVersion"></a>

### Description<a name="CLIReference-cmd-UpdateApplicationVersion-Description"></a>

Updates the specified application version to have the specified properties\.

**Note**  
If a property \(for example, `description`\) is not provided, the value remains unchanged\. To clear these properties, specify an empty string\.

### Syntax<a name="CLIReference-cmd-UpdateApplicationVersion-Syntax"></a>

 ** elastic\-beanstalk\-update\-application\-version \-a \[*name*\] \-l \[*label*\] \-d \[*desc*\] ** 

### Options<a name="CLIReference-cmd-UpdateApplicationVersion-Request"></a>


****  

| Name | Description | Required | 
| --- | --- | --- | 
|   `-a`   `--application-name` *name*   |  The name of the application associated with this version\. If no such application is found, Elastic Beanstalk returns an `InvalidParameterValue` error\. Type: String Default: None Length Constraints: Minimum value of 1\. Maximum value of 100\.  |  Yes  | 
|   `-l`   `--version-label`   |  The name of the version to update\. If no application version is found with this label, Elastic Beanstalk returns an `InvalidParaemterValue` error\. Type: String Length Constraints: Minimum value of 1\. Maximum value of 100\.  |  Yes  | 
|   `-d`   `--description`   |  A new description for the release\. Type: String Default: If not specified, Elastic Beanstalk does not update the description\. Length Constraints: Minimum value of 0\. Maximum value of 200\.  |  No  | 

### Output<a name="CLIReference-cmd-UpdateApplicationVersion-Response"></a>

The command returns a table with the following information:

+ **ApplicationName—**The name of the application associated with this release\.

+ **DateCreated—**The creation date of the application version\.

+ **DateUpdated—**The last modified date of the application version\.

+ **Description—**The description of this application version\.

+ **SourceBundle—**The location where the source bundle is located for this version\.

+ **VersionLabel—**A label identifying the version for the associated application\.

### Examples<a name="CLIReference-cmd-UpdateApplicationVersion-Examples"></a>

#### Updating an Application Version<a name="CLIReference-cmd-UpdateApplicationVersion-Example-Request-1"></a>

This example shows how to update an application version\.

```
1. PROMPT> elastic-beanstalk-update-application-version -a MySampleApp -d "My new version" -l "TestVersion 1"
```

## elastic\-beanstalk\-update\-configuration\-template<a name="CLIReference-cmd-UpdateConfigurationTemplate"></a>

### Description<a name="CLIReference-cmd-UpdateConfigurationTemplate-Description"></a>

Updates the specified configuration template to have the specified properties or configuration option values\.

**Note**  
If a property \(for example, `ApplicationName`\) is not provided, its value remains unchanged\. To clear such properties, specify an empty string\.

### Syntax<a name="CLIReference-cmd-UpdateConfigurationTemplate-Syntax"></a>

 ** elastic\-beanstalk\-update\-configuration\-template \-a \[*name*\] \-t \[*name*\] \-d \[*desc*\] \-f \[*filename*\] \-F \[*filename*\]** 

### Options<a name="CLIReference-cmd-UpdateConfigurationTemplate-Request"></a>


****  

| Name | Description | Required | 
| --- | --- | --- | 
|   `-a`   `--application-name` *name*   |  The name of the application associated with the configuration template to update\. If no application is found with this name, Elastic Beanstalk returns an `InvalidParameterValue` error\.  Type: String Default: None Length Constraints: Minimum value of 1\. Maximum value of 100\.  |  Yes  | 
|   `-t`   `--template-name` *name*   |  The name of the configuration template to update\. If no configuration template is found with this name, UpdateConfigurationTemplate returns an `InvalidParameterValue` error\.  Type: String Default: None Length Constraints: Minimum value of 1\. Maximum value of 100\.  |  Yes  | 
|   `-d`   `--description` *desc*   |  A new description for the configuration\. Type: String Default: None Length Constraints: Minimum value of 0\. Maximum value of 200\.  |  No  | 
|   `-f`   `--options-file` *filename*   |  The name of a JSON file that contains option settings to update with the new specified option value\.  Type: String  |  No  | 
|   `-F`   `--options-to-remove-file` *value*   |  The name of a JSON file that contains configuration options to remove\. Type: String Default: None  |  No  | 

### Output<a name="CLIReference-cmd-UpdateConfigurationTemplate-Response"></a>

The command returns a table with the following information:

+ **ApplicationName—**The name of the application associated with this configuration set\.

+ **DateCreated—**The date \(in UTC time\) when this configuration set was created\.

+ **DateUpdated—**The date \(in UTC time\) when this configuration set was last modified\.

+ **DeploymentStatus—**If this configuration set is associated with an environment, the *DeploymentStatus* parameter indicates the deployment status of this configuration set:

  + `null`: This configuration is not associated with a running environment\.

  + `pending`: This is a draft configuration that is not deployed to the associated environment but is in the process of deploying\.

  + `deployed`: This is the configuration that is currently deployed to the associated running environment\.

  + `failed`: This is a draft configuration that failed to successfully deploy\.

+ **Description—**The description of the configuration set\.

+ **EnvironmentName—**If not null, the name of the environment for this configuration set\.

+ **OptionSettings—**A list of configuration options and their values in this configuration set\.

+ **SolutionStackName—**The name of the solution stack this configuration set uses\.

+ **TemplateName—**If not null, the name of the configuration template for this configuration set\.

### Examples<a name="CLIReference-cmd-UpdateConfigurationTemplate-Examples"></a>

#### Updating a Configuration Template<a name="CLIReference-cmd-UpdateConfigurationTemplate-Example-Request-1"></a>

This example shows how to update a configuration template\. For a list of configuration settings, see [Configuration Options](command-options.md)\.

```
1. PROMPT> elastic-beanstalk-update-configuration-template -a MySampleApp -t myconfigtemplate -d "My updated configuration template" -f "options.txt"
```

**options\.txt**

```
[
    {
        "Namespace": "aws:elasticbeanstalk:application:environment",
        "OptionName": "my_custom_param_1",
        "Value": "firstvalue"
    },
    {
        "Namespace": "aws:elasticbeanstalk:application:environment",
        "OptionName": "my_custom_param_2",
        "Value": "secondvalue"
    }
]
```

### Related Operations<a name="CLIReference-cmd-UpdateConfigurationTemplate-Related"></a>

+  [elastic\-beanstalk\-describe\-configuration\-options](#CLIReference-cmd-DescribeConfigurationOptions) 

## elastic\-beanstalk\-update\-environment<a name="CLIReference-cmd-UpdateEnvironment"></a>

### Description<a name="CLIReference-cmd-UpdateEnvironment-Description"></a>

Updates the environment description, deploys a new application version, updates the configuration settings to an entirely new configuration template, or updates select configuration option values in the running environment\.

 Attempting to update both the release and configuration is not allowed and Elastic Beanstalk returns an `InvalidParameterCombination` error\.

When updating the configuration settings to a new template or individual settings, a draft configuration is created and `DescribeConfigurationSettings` for this environment returns two setting descriptions with different `DeploymentStatus` values\.

### Syntax<a name="CLIReference-cmd-UpdateEnvironment-Syntax"></a>

 ** elastic\-beanstalk\-update\-environment \[\-e \[*name*\] | \-E \[*id*\]\] \-l \[*label*\] \-t \[*name*\] \-d \[*desc*\] \-f \[*filename*\] \-F \[*filename*\] ** 

### Options<a name="CLIReference-cmd-UpdateEnvironment-Request"></a>


****  

| Name | Description | Required | 
| --- | --- | --- | 
|   `-e`   `--environment-name` *name*   |  The name of the environment to update\. If no environment with this name exists, Elastic Beanstalk returns an `InvalidParameterValue` error\. Type: String Default: None Length Constraints: Minimum value of 4\. Maximum value of 23\.  |  Conditional  | 
|   `-E`   `--environment-id` *id*   |  The ID of the environment to update\. If no environment with this ID exists, Elastic Beanstalk returns an `InvalidParameterValue` error\. Type: String Default: None  |  Conditional  | 
|   `-l`   `--version-label` *>label*   |  If this parameter is specified, Elastic Beanstalk deploys the named application version to the environment\. If no such application version is found, Elastic Beanstalk returns an `InvalidParameterValue` error\. Type: String Default: None Length Constraints: Minimum value of 1\. Maximum value of 100\.  |  No  | 
|   `-t`   `--template-name` *name*   |  If this parameter is specified, Elastic Beanstalk deploys this configuration template to the environment\. If no such configuration template is found, Elastic Beanstalk returns an `InvalidParameterValue` error\. Type: String Default: None Length Constraints: Minimum value of 1\. Maximum value of 100\.  |  No  | 
|   `-d`   `--description` *desc*   |  If this parameter is specified, Elastic Beanstalk updates the description of this environment\. Type: String Default: None Length Constraints: Minimum value of 0\. Maximum value of 200\.  |  No  | 
|   `-f`   `--options-file` *filename*   |  A file containing option settings to update\. If specified, Elastic Beanstalk updates the configuration set associated with the running environment and sets the specified configuration options to the requested values\. Type: String Default: None  |  No  | 
|   `-F`   `--options-to-remove-file` *filename*   |  A file containing option settings to remove\. If specified, Elastic Beanstalk removes the option settings from the configuration set associated with the running environment\. Type: String Default: None  |  No  | 

### Output<a name="CLIReference-cmd-UpdateEnvironment-Response"></a>

The command returns a table with the following information:

+ **ApplicationName—**The name of the application associated with this environment\.

+ **CNAME—**The URL to the CNAME for this environment\.

+ **DateCreated—**The date the environment was created\.

+ **DateUpdated—**The date the environment was last updated\.

+ **Description—**The description of the environment\.

+ **EndpointURL—**The URL to the load balancer for this environment\. 

+ **EnvironmentID—**The ID of this environment\. 

+ **EnvironmentName—**The name of this environment\. 

+ **Health—**Describes the health status of the environment\. Elastic Beanstalk indicates the failure levels for a running environment:

  + `Red`: Indicates the environment is not responsive\. Occurs when three or more consecutive failures occur for an environment\.

  + `Yellow`: Indicates that something is wrong\. Occurs when two consecutive failures occur for an environment\.

  + `Green`: Indicates the environment is healthy and fully functional\.

  + `Gray`: Default health for a new environment\. The environment is not fully launched, and health checks have not started or health checks are suspended during an `UpdateEnvironment` or `RestartEnvironment`request\.

+ **Resources—**A list of AWS resources used in this environment\. 

+ **SolutionStackName—**The name of the `SolutionStack` deployed with this environment\.

+ **Status—**The current operational status of the environment:

  + `Launching`: Environment is in the process of initial deployment\.

  + `Updating`: Environment is in the process of updating its configuration settings or application version\.

  + `Ready`: Environment is available to have an action performed on it, such as update or terminate\.

  + `Terminating`: Environment is in the shutdown process\.

  + `Terminated`: Environment is not running\.

+ **TemplateName—**The name of the configuration template used to originally launch this environment\.

+ **VersionLabel—**The application version deployed in this environment\.

### Examples<a name="CLIReference-cmd-UpdateEnvironment-Examples"></a>

#### Updating an Existing Environment<a name="CLIReference-cmd-UpdateEnvironment-Example-Request-1"></a>

This example shows how to update an existing environment\. It passes in a file named options\.txt that updates the size of the instance to a t1\.micro and sets two environment variables\. For a list of possible configuration settings, see [Configuration Options](command-options.md)\.

```
1. PROMPT> elastic-beanstalk-update-environment -e MySampleAppEnv -f "options.txt"
```

**options\.txt**

```
[
    {
        "Namespace": "aws:autoscaling:launchconfiguration",
        "OptionName": "InstanceType",
        "Value": "t1.micro"
    },
    {
        "Namespace": "aws:elasticbeanstalk:application:environment",
        "OptionName": "my_custom_param_1",
        "Value": "firstvalue"
    },
    {
        "Namespace": "aws:elasticbeanstalk:application:environment",
        "OptionName": "my_custom_param_2",
        "Value": "secondvalue"
    }
]
```

## elastic\-beanstalk\-validate\-configuration\-settings<a name="CLIReference-cmd-ValidateConfigurationSettings"></a>

### Description<a name="CLIReference-cmd-ValidateConfigurationSettings-Description"></a>

Takes a set of configuration settings and either a configuration template or environment, and determines whether those values are valid\.

This action returns a list of messages indicating any errors or warnings associated with the selection of option values\.

### Syntax<a name="CLIReference-cmd-ValidateConfigurationSettings-Syntax"></a>

 ** elastic\-beanstalk\-validate\-configuration\-settings \-a \[*name*\] \-t \[*name*\] \-e \[*name*\] \-f \[*filename*\] ** 

### Options<a name="CLIReference-cmd-ValidateConfigurationSettings-Request"></a>


****  

| Name | Description | Required | 
| --- | --- | --- | 
|   `-a`   `--application-name` *name*   |  The name of the application that the configuration template or environment belongs to\.  Type: String Default: None Length Constraints: Minimum value of 1\. Maximum value of 100\.  |  Yes  | 
|   `-t`   `--template-name` *name*   |  The name of the configuration template to validate the settings against\.  Condition: You cannot specify both the configuration template name and the environment name\. Type: String Default: None Length Constraints: Minimum value of 1\. Maximum value of 100\.  |  No  | 
|   `-e`   `--environment-name` *name*   |  The name of the environment to validate the settings against\. Type: String Default: None Length Constraints: Minimum value of 4\. Maximum value of 23\.  |  No  | 
|   `-f`   `--options-file` *>filename*   |  The name of a JSON file that contains a list of options and specified values to evaluate\. Type: String  |  Yes  | 

### Output<a name="CLIReference-cmd-ValidateConfigurationSettings-Response"></a>

The command returns a table with the following information:

+ **Message—**A message describing the error or warning\.

+ **Namespace**

+ **OptionName**

+ **Severity—**An indication of the severity of this message:

  + `error`: This message indicates that this is not a valid setting for an option\.

  + `warning`: This message provides information you should consider\.

### Examples<a name="CLIReference-cmd-ValidateConfigurationSettings-Examples"></a>

#### Validating Configuration Settings for an Environment<a name="CLIReference-cmd-ValidateConfigurationSettings-Example-Request-1"></a>

This example shows how to validate the configuration settings for an environment\.

```
1. PROMPT> elastic-beanstalk-validate-configuration-settings -a MySampleApp -e MySampleAppEnv -f MyOptionSettingsFile.json
```