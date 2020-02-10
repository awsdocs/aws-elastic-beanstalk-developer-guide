# Elastic Beanstalk API command line interface \(retired\)<a name="using-api-cli"></a>

This tool, the Elastic Beanstalk API command line interface \(API CLI\), has been replaced by the AWS CLI, which provides API equivalent commands for all AWS services\. See the AWS Command Line Interface User Guide to get started with the AWS CLI\. Also try the [EB CLI](eb-cli3.md) for a simplified, higher\-level command line experience\.

## Converting Elastic Beanstalk API CLI scripts<a name="apicli-vs-awscli"></a>

Convert your old EB API CLI scripts to use the AWS CLI or Tools for Windows PowerShell to get access to the latest Elastic Beanstalk APIs\. The following table lists the Elastic Beanstalk API\-based CLI commands and their equivalent commands in the AWS CLI and Tools for Windows PowerShell\.


****  

| Elastic Beanstalk API CLI | AWS CLI | AWS Tools for Windows PowerShell | 
| --- | --- | --- | 
|  `elastic-beanstalk-check-dns-availability`  |  `[check\-dns\-availability](https://docs.aws.amazon.com/cli/latest/reference/check-dns-availability.html)`  |  `Get-EBDNSAvailability`  | 
|  `elastic-beanstalk-create-application`  |  `[create\-application](https://docs.aws.amazon.com/cli/latest/reference/create-application.html)`  |  `New-EBApplication`  | 
|  `elastic-beanstalk-create-application-version`  |  `[create\-application\-version](https://docs.aws.amazon.com/cli/latest/reference/create-application-version.html)`  |  `New-EBApplicationVersion`  | 
|  `elastic-beanstalk-create-configuration-template`  |  `[create\-configuration\-template](https://docs.aws.amazon.com/cli/latest/reference/create-configuration-template.html)`  |  `New-EBConfigurationTemplate`  | 
|  `elastic-beanstalk-create-environment`  |  `[create\-environment](https://docs.aws.amazon.com/cli/latest/reference/create-environment.html)`  |  `New-EBEnvironment`  | 
|  `elastic-beanstalk-create-storage-location`  |  `[create\-storage\-location](https://docs.aws.amazon.com/cli/latest/reference/create-storage-location.html)`  |  `New-EBStorageLocation`  | 
|  `elastic-beanstalk-delete-application`  |  `[delete\-application](https://docs.aws.amazon.com/cli/latest/reference/delete-application.html)`  |  `Remove-EBApplication`  | 
|  `elastic-beanstalk-delete-application-version`  |  `[delete\-application\-version](https://docs.aws.amazon.com/cli/latest/reference/delete-application-version.html)`  |  `Remove-EBApplicationVersion`  | 
|  `elastic-beanstalk-delete-configuration-template`  |  `[delete\-configuration\-template](https://docs.aws.amazon.com/cli/latest/reference/delete-configuration-template.html)`  |  `Remove-EBConfigurationTemplate`  | 
|  `elastic-beanstalk-delete-environment-configuration`  |  `[delete\-environment\-configuration](https://docs.aws.amazon.com/cli/latest/reference/delete-environment-configuration.html)`  |  `Remove-EBEnvironmentConfiguration`  | 
|  `elastic-beanstalk-describe-application-versions`  |  `[describe\-application\-versions](https://docs.aws.amazon.com/cli/latest/reference/describe-application-versions.html)`  |  `Get-EBApplicationVersion`  | 
|  `elastic-beanstalk-describe-applications`  |  `[describe\-applications](https://docs.aws.amazon.com/cli/latest/reference/describe-applications.html)`  |  `Get-EBApplication`  | 
|  `elastic-beanstalk-describe-configuration-options`  |  `[describe\-configuration\-options](https://docs.aws.amazon.com/cli/latest/reference/describe-configuration-options.html)`  |  `Get-EBConfigurationOption`  | 
|  `elastic-beanstalk-describe-configuration-settings`  |  `[describe\-configuration\-settings](https://docs.aws.amazon.com/cli/latest/reference/describe-configuration-settings.html)`  |  `Get-EBConfigurationSetting`  | 
|  `elastic-beanstalk-describe-environment-resources`  |  `[describe\-environment\-resources](https://docs.aws.amazon.com/cli/latest/reference/describe-environment-resources.html)`  |  `Get-EBEnvironmentResource`  | 
|  `elastic-beanstalk-describe-environments`  |  `[describe\-environments](https://docs.aws.amazon.com/cli/latest/reference/describe-environments.html)`  |  `Get-EBEnvironment`  | 
|  `elastic-beanstalk-describe-events`  |  `[describe\-events](https://docs.aws.amazon.com/cli/latest/reference/describe-events.html)`  |  `Get-EBEvent`  | 
|  `elastic-beanstalk-list-available-solution-stacks`  |  `[list\-available\-solution\-stacks](https://docs.aws.amazon.com/cli/latest/reference/list-available-solution-stacks.html)`  |  `Get-EBAvailableSolutionStack`  | 
|  `elastic-beanstalk-rebuild-environment`  |  `[rebuild\-environment](https://docs.aws.amazon.com/cli/latest/reference/rebuild-environment.html)`  |  `Start-EBEnvironmentRebuild`  | 
|  `elastic-beanstalk-request-environment-info`  |  `[request\-environment\-info](https://docs.aws.amazon.com/cli/latest/reference/request-environment-info.html)`  |  `Request-EBEnvironmentInfo`  | 
|  `elastic-beanstalk-restart-app-server`  |  `[restart\-app\-server](https://docs.aws.amazon.com/cli/latest/reference/restart-app-server.html)`  |  `Restart-EBAppServer`  | 
|  `elastic-beanstalk-retrieve-environment-info`  |  `[retrieve\-environment\-info](https://docs.aws.amazon.com/cli/latest/reference/retrieve-environment-info.html)`  |  `Get-EBEnvironmentInfo`  | 
|  `elastic-beanstalk-swap-environment-cnames`  |  `[swap\-environment\-cnames](https://docs.aws.amazon.com/cli/latest/reference/swap-environment-cnames.html)`  |  `Set-EBEnvironmentCNAME`  | 
|  `elastic-beanstalk-terminate-environment`  |  `[terminate\-environment](https://docs.aws.amazon.com/cli/latest/reference/terminate-environment.html)`  |  `Stop-EBEnvironment`  | 
|  `elastic-beanstalk-update-application`  |  `[update\-application](https://docs.aws.amazon.com/cli/latest/reference/update-application.html)`  |  `Update-EBApplication`  | 
|  `elastic-beanstalk-update-application-version`  |  `[update\-application\-version](https://docs.aws.amazon.com/cli/latest/reference/update-application-version.html)`  |  `Update-EBApplicationVersion`  | 
|  `elastic-beanstalk-update-configuration-template`  |  `[update\-configuration\-template](https://docs.aws.amazon.com/cli/latest/reference/update-configuration-template.html)`  |  `Update-EBConfigurationTemplate`  | 
|  `elastic-beanstalk-update-environment`  |  `[update\-environment](https://docs.aws.amazon.com/cli/latest/reference/update-environment.html)`  |  `Update-EBEnvironment`  | 
|  `elastic-beanstalk-validate-configuration-settings`  |  `[validate\-configuration\-settings](https://docs.aws.amazon.com/cli/latest/reference/validate-configuration-settings.html)`  |  `Test-EBConfigurationSetting`  | 