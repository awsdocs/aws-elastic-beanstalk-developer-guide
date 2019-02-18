# eb use<a name="eb3-use"></a>

## Description<a name="eb3-usedescription"></a>

Sets the specified environment as the default environment\.

When using Git, eb use sets the default environment for the current branch\. Run this command once in each branch that you want to deploy to Elastic Beanstalk\.

## Syntax<a name="eb3-usesyntax"></a>

 eb use *environment\-name* 

## Options<a name="eb3-useoptions"></a>


****  

|  Name  |  Description  | 
| --- | --- | 
|  `--source codecommit/repository-name/branch-name`  |  CodeCommit repository and branch\. See [Using the EB CLI with AWS CodeCommit](eb-cli-codecommit.md)\.  | 
|  `-r region` `--region region`  |  Change the region in which you create environments\.  | 
|  [Common options](eb3-cmd-options.md)  |  | 