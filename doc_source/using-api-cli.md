# Elastic Beanstalk API Command Line Interface \(deprecated\)<a name="using-api-cli"></a>

**Note**  
 This tool, the Elastic Beanstalk API CLI, and its documentation have been replaced with the AWS CLI\. See the [AWS CLI User Guide ](http://docs.aws.amazon.com/cli/latest/userguide/) to get started with the AWS CLI\. Also try the [EB CLI](eb-cli3.md) for a simplified, higher\-level command line experience\. 

This section contains a reference for the old Elastic Beanstalk API command line interface\. This tool's functionality has been replaced by the AWS CLI, which provides API equivalent commands for all AWS services\. See [Installing the AWS Command Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/installing.html) to get started with the AWS CLI\.

**Topics**
+ [Converting Elastic Beanstalk API CLI Scripts](#apicli-vs-awscli)
+ [Getting Set Up](usingCLI.md)
+ [Common Options](CLTRG-common-args-api.md)
+ [Operations](OperationList-cmd.md)

## Converting Elastic Beanstalk API CLI Scripts<a name="apicli-vs-awscli"></a>

Convert your old EB API CLI scripts to use the AWS CLI or Tools for Windows PowerShell to get access to the latest Elastic Beanstalk APIs\. The following table lists the Elastic Beanstalk API\-based CLI commands and their equivalent commands in the AWS CLI and Tools for Windows PowerShell\.


****  

| Elastic Beanstalk API CLI | AWS CLI | AWS Tools for Windows PowerShell | 
| --- | --- | --- | 
|  `[elastic\-beanstalk\-check\-dns\-availability](OperationList-cmd.md#CLIReference-cmd-CheckDNSAvailability)`  |  `[check\-dns\-availability](http://docs.aws.amazon.com/cli/latest/reference/check-dns-availability.html)`  |  `Get-EBDNSAvailability`  | 
|  `[elastic\-beanstalk\-create\-application](OperationList-cmd.md#CLIReference-cmd-CreateApplication)`  |  `[create\-application](http://docs.aws.amazon.com/cli/latest/reference/create-application.html)`  |  `New-EBApplication`  | 
|  `[elastic\-beanstalk\-create\-application\-version](OperationList-cmd.md#CLIReference-cmd-CreateApplicationVersion)`  |  `[create\-application\-version](http://docs.aws.amazon.com/cli/latest/reference/create-application-version.html)`  |  `New-EBApplicationVersion`  | 
|  `[elastic\-beanstalk\-create\-configuration\-template](OperationList-cmd.md#CLIReference-cmd-CreateConfigurationTemplate)`  |  `[create\-configuration\-template](http://docs.aws.amazon.com/cli/latest/reference/create-configuration-template.html)`  |  `New-EBConfigurationTemplate`  | 
|  `[elastic\-beanstalk\-create\-environment](OperationList-cmd.md#CLIReference-cmd-CreateEnvironment)`  |  `[create\-environment](http://docs.aws.amazon.com/cli/latest/reference/create-environment.html)`  |  `New-EBEnvironment`  | 
|  `[elastic\-beanstalk\-create\-storage\-location](OperationList-cmd.md#CLIReference-cmd-CreateStorageLocation)`  |  `[create\-storage\-location](http://docs.aws.amazon.com/cli/latest/reference/create-storage-location.html)`  |  `New-EBStorageLocation`  | 
|  `[elastic\-beanstalk\-delete\-application](OperationList-cmd.md#CLIReference-cmd-DeleteApplication)`  |  `[delete\-application](http://docs.aws.amazon.com/cli/latest/reference/delete-application.html)`  |  `Remove-EBApplication`  | 
|  `[elastic\-beanstalk\-delete\-application\-version](OperationList-cmd.md#CLIReference-cmd-DeleteApplicationVersion)`  |  `[delete\-application\-version](http://docs.aws.amazon.com/cli/latest/reference/delete-application-version.html)`  |  `Remove-EBApplicationVersion`  | 
|  `[elastic\-beanstalk\-delete\-configuration\-template](OperationList-cmd.md#CLIReference-cmd-DeleteConfigurationTemplate)`  |  `[delete\-configuration\-template](http://docs.aws.amazon.com/cli/latest/reference/delete-configuration-template.html)`  |  `Remove-EBConfigurationTemplate`  | 
|  `[elastic\-beanstalk\-delete\-environment\-configuration](OperationList-cmd.md#CLIReference-cmd-DeleteEnvironmentConfiguration)`  |  `[delete\-environment\-configuration](http://docs.aws.amazon.com/cli/latest/reference/delete-environment-configuration.html)`  |  `Remove-EBEnvironmentConfiguration`  | 
|  `[elastic\-beanstalk\-describe\-application\-versions](OperationList-cmd.md#CLIReference-cmd-DescribeApplicationVersions)`  |  `[describe\-application\-versions](http://docs.aws.amazon.com/cli/latest/reference/describe-application-versions.html)`  |  `Get-EBApplicationVersion`  | 
|  `[elastic\-beanstalk\-describe\-applications](OperationList-cmd.md#CLIReference-cmd-DescribeApplications)`  |  `[describe\-applications](http://docs.aws.amazon.com/cli/latest/reference/describe-applications.html)`  |  `Get-EBApplication`  | 
|  `[elastic\-beanstalk\-describe\-configuration\-options](OperationList-cmd.md#CLIReference-cmd-DescribeConfigurationOptions)`  |  `[describe\-configuration\-options](http://docs.aws.amazon.com/cli/latest/reference/describe-configuration-options.html)`  |  `Get-EBConfigurationOption`  | 
|  `[elastic\-beanstalk\-describe\-configuration\-settings](OperationList-cmd.md#CLIReference-cmd-DescribeConfigurationSettings)`  |  `[describe\-configuration\-settings](http://docs.aws.amazon.com/cli/latest/reference/describe-configuration-settings.html)`  |  `Get-EBConfigurationSetting`  | 
|  `[elastic\-beanstalk\-describe\-environment\-resources](OperationList-cmd.md#CLIReference-cmd-DescribeEnvironmentResources)`  |  `[describe\-environment\-resources](http://docs.aws.amazon.com/cli/latest/reference/describe-environment-resources.html)`  |  `Get-EBEnvironmentResource`  | 
|  `[elastic\-beanstalk\-describe\-environments](OperationList-cmd.md#CLIReference-cmd-DescribeEnvironments)`  |  `[describe\-environments](http://docs.aws.amazon.com/cli/latest/reference/describe-environments.html)`  |  `Get-EBEnvironment`  | 
|  `[elastic\-beanstalk\-describe\-events](OperationList-cmd.md#CLIReference-cmd-DescribeEvents)`  |  `[describe\-events](http://docs.aws.amazon.com/cli/latest/reference/describe-events.html)`  |  `Get-EBEvent`  | 
|  `[elastic\-beanstalk\-list\-available\-solution\-stacks](OperationList-cmd.md#CLIReference-cmd-ListAvailableSolutionStacks)`  |  `[list\-available\-solution\-stacks](http://docs.aws.amazon.com/cli/latest/reference/list-available-solution-stacks.html)`  |  `Get-EBAvailableSolutionStack`  | 
|  `[elastic\-beanstalk\-rebuild\-environment](OperationList-cmd.md#CLIReference-cmd-RebuildEnvironment)`  |  `[rebuild\-environment](http://docs.aws.amazon.com/cli/latest/reference/rebuild-environment.html)`  |  `Start-EBEnvironmentRebuild`  | 
|  `[elastic\-beanstalk\-request\-environment\-info](OperationList-cmd.md#CLIReference-cmd-RequestEnvironmentInfo)`  |  `[request\-environment\-info](http://docs.aws.amazon.com/cli/latest/reference/request-environment-info.html)`  |  `Request-EBEnvironmentInfo`  | 
|  `[elastic\-beanstalk\-restart\-app\-server](OperationList-cmd.md#CLIReference-cmd-RestartAppServer)`  |  `[restart\-app\-server](http://docs.aws.amazon.com/cli/latest/reference/restart-app-server.html)`  |  `Restart-EBAppServer`  | 
|  `[elastic\-beanstalk\-retrieve\-environment\-info](OperationList-cmd.md#CLIReference-cmd-RetrieveEnvironmentInfo)`  |  `[retrieve\-environment\-info](http://docs.aws.amazon.com/cli/latest/reference/retrieve-environment-info.html)`  |  `Get-EBEnvironmentInfo`  | 
|  `[elastic\-beanstalk\-swap\-environment\-cnames](OperationList-cmd.md#CLIReference-cmd-SwapEnvironmentCNAMEs)`  |  `[swap\-environment\-cnames](http://docs.aws.amazon.com/cli/latest/reference/swap-environment-cnames.html)`  |  `Set-EBEnvironmentCNAME`  | 
|  `[elastic\-beanstalk\-terminate\-environment](OperationList-cmd.md#CLIReference-cmd-TerminateEnvironment)`  |  `[terminate\-environment](http://docs.aws.amazon.com/cli/latest/reference/terminate-environment.html)`  |  `Stop-EBEnvironment`  | 
|  `[elastic\-beanstalk\-update\-application](OperationList-cmd.md#CLIReference-cmd-UpdateApplication)`  |  `[update\-application](http://docs.aws.amazon.com/cli/latest/reference/update-application.html)`  |  `Update-EBApplication`  | 
|  `[elastic\-beanstalk\-update\-application\-version](OperationList-cmd.md#CLIReference-cmd-UpdateApplicationVersion)`  |  `[update\-application\-version](http://docs.aws.amazon.com/cli/latest/reference/update-application-version.html)`  |  `Update-EBApplicationVersion`  | 
|  `[elastic\-beanstalk\-update\-configuration\-template](OperationList-cmd.md#CLIReference-cmd-UpdateConfigurationTemplate)`  |  `[update\-configuration\-template](http://docs.aws.amazon.com/cli/latest/reference/update-configuration-template.html)`  |  `Update-EBConfigurationTemplate`  | 
|  `[elastic\-beanstalk\-update\-environment](OperationList-cmd.md#CLIReference-cmd-UpdateEnvironment)`  |  `[update\-environment](http://docs.aws.amazon.com/cli/latest/reference/update-environment.html)`  |  `Update-EBEnvironment`  | 
|  `[elastic\-beanstalk\-validate\-configuration\-settings](OperationList-cmd.md#CLIReference-cmd-ValidateConfigurationSettings)`  |  `[validate\-configuration\-settings](http://docs.aws.amazon.com/cli/latest/reference/validate-configuration-settings.html)`  |  `Test-EBConfigurationSetting`  | 