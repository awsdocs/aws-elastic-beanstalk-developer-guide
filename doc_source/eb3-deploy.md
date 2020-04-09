# eb deploy<a name="eb3-deploy"></a>

## Description<a name="eb3-deploydescription"></a>

Deploys the application source bundle from the initialized project directory to the running application\.

If git is installed, EB CLI uses the `git archive` command to create a `.zip` file from the contents of the most recent `git commit` command\.

However, when `.ebignore` is present in your project directory, the EB CLI doesn't use git commands and semantics to create your source bundle\. This means that EB CLI ignores files specified in `.ebignore`, and includes all other files\. In particular, it includes uncommitted source files\.

**Note**  
You can configure the EB CLI to deploy an artifact from your build process instead of creating a ZIP file of your project folder\. See [Deploying an artifact instead of the project folder](eb-cli3-configuration.md#eb-cli3-artifact) for details\.

## Syntax<a name="eb3-deploysyntax"></a>

 eb deploy 

 eb deploy *environment\-name* 

## Options<a name="eb3-deployoptions"></a>


****  

|  Name  |  Description  | 
| --- | --- | 
|  `-l` *version\_label* or `--label` *version\_label*  |  Specify a label to use for the version that the EB CLI creates\. If the label has already been used, the EB CLI redeploys the previous version with that label\. Type: String  | 
| \-\-env\-group\-suffix groupname | Group name to append to the environment name\. Only for use with [Compose Environments](ebcli-compose.md)\. | 
|  `-m` "*version\_description*" or `--message` "*version\_description*"  |  The description for the application version, enclosed in double quotation marks\. Type: String  | 
|  `--modules` *component\-a component\-b*  | List of components to update\. Only for use with [Compose Environments](ebcli-compose.md)\. | 
|  `-p` or `--process`  |  Preprocess and validate the environment manifest and configuration files in the source bundle\. Validating configuration files can identify issues prior to deploying the application version to an environment\.  | 
|  `--source codecommit/repository-name/branch-name`  |  CodeCommit repository and branch\. See [Using the EB CLI with AWS CodeCommit](eb-cli-codecommit.md)\.  | 
|  `--staged`  |  Deploy files staged in the git index instead of the HEAD commit\.  | 
|  `--timeout` *minutes*  |  The number of minutes before the command times out\.  | 
|  `--version` *version\_label*  |  An existing application version to deploy\. Type: String  | 
|  [Common options](eb3-cmd-options.md)  |  | 

## Output<a name="eb3-deployoutput"></a>

If successful, the command returns the status of the `deploy` operation\.

If you enabled CodeBuild support in your application, eb deploy displays information from CodeBuild as your code is built\. For information about CodeBuild support in Elastic Beanstalk, see [Using the EB CLI with AWS CodeBuild](eb-cli-codebuild.md)\.

## Example<a name="eb3-deployexample"></a>

The following example deploys the current application\.

```
$ eb deploy
2018-07-11 21:05:22    INFO: Environment update is starting.
2018-07-11 21:05:27    INFO: Deploying new version to instance(s).
2018-07-11 21:05:53    INFO: New application version was deployed to running EC2 instances.
2018-07-11 21:05:53    INFO: Environment update completed successfully.
```