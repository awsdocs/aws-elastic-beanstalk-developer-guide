# eb codesource<a name="eb3-codesource"></a>

## Description<a name="eb3-codesourcedescription"></a>

Configures the EB CLI to [deploy from a CodeCommit repository](eb-cli-codecommit.md), or disables CodeCommit integration and uploads the source bundle from your local machine\.

**Note**  
Some AWS Regions don't offer CodeCommit\. The integration between Elastic Beanstalk and CodeCommit doesn't work in these Regions\.  
For information about the AWS services offered in each Region, see [Region Table](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/)\.

## Syntax<a name="eb3-codesourcesyntax"></a>

eb codesource 

eb codesource codecommit

eb codesource local

## Options<a name="eb3-codesourceoptions"></a>


****  

|  Name  |  Description  | 
| --- | --- | 
|  [Common options](eb3-cmd-options.md)  |  | 

## Output<a name="eb3-codesourceoutput"></a>

eb codesource prompts you to choose between CodeCommit integration and standard deployments\.

eb codesource codecommit initiates interactive repository configuration for CodeCommit integration\.

eb codesource local shows the original configuration and disables CodeCommit integration\.

## Examples<a name="eb3-codesourceexample"></a>

Use eb codesource codecommit to configure CodeCommit integration for the current branch\.

```
~/my-app$ eb codesource codecommit
Select a repository
1) my-repo
2) my-app
3) [ Create new Repository ]
(default is 1): 1

Select a branch
1) master
2) test
3) [ Create new Branch with local HEAD ]
(default is 1): 1
```

Use eb codesource local to disable CodeCommit integration for the current branch\.

```
~/my-app$ eb codesource local
Current CodeCommit setup:
  Repository: my-app
  Branch: master
Default set to use local sources
```