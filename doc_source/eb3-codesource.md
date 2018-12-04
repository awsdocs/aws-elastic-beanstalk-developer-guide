# eb codesource<a name="eb3-codesource"></a>

## Description<a name="eb3-codesourcedescription"></a>

Configures the EB CLI to [deploy from an AWS CodeCommit repository](eb-cli-codecommit.md), or disables AWS CodeCommit integration and uploads the source bundle from your local machine\.

**Note**  
Some regions don't offer AWS CodeCommit\. The integration between Elastic Beanstalk and AWS CodeCommit doesn't work in these regions\.  
For information about the AWS services offered in each region, see [Region Table](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/)\.

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

eb codesource prompts you to choose between AWS CodeCommit integration and standard deployments\.

eb codesource codecommit initiates interactive repository configuration for AWS CodeCommit integration\.

eb codesource local shows the original configuration and disables AWS CodeCommit integration\.

## Examples<a name="eb3-codesourceexample"></a>

Use eb codesource codecommit to configure AWS CodeCommit integration for the current branch\.

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

Use eb codesource local to disable AWS CodeCommit integration for the current branch\.

```
~/my-app$ eb codesource local
Current CodeCommit setup:
  Repository: my-app
  Branch: master
Default set to use local sources
```