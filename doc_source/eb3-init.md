# `eb init`<a name="eb3-init"></a>

## Description<a name="eb3-initdescription"></a>

Sets default values for Elastic Beanstalk applications created with EB CLI by prompting you with a series of questions\.

**Note**  
The values you set with `init` apply only to the current directory and repository\.

## Syntax<a name="eb3-initsyntax"></a>

 `eb init` 

 `eb init` *application\-name* 

## Options<a name="eb3-initoptions"></a>

If you run `eb init` without specifying any options, the EB CLI prompts you to enter a value for each setting\.

**Note**  
To use `eb init` to create a new key pair, you must have `ssh-keygen` installed on your local machine and available from the command line\.


****  

|  Name  |  Description  | 
| --- | --- | 
|  `-i` `--interactive`  |  Forces EB CLI to prompt you to provide a value for every `eb init` command option\.  The `init` command prompts you to provide values for `eb init` command options that do not have a \(default\) value\. After the first time you run the `eb init` command in a directory, EB CLI might not prompt you about any command options\. Therefore, use the `--interactive` option when you want to change a setting that you previously set\.   | 
|  `-k` *keyname* `--keyname` *keyname*  |  The name of the Amazon EC2 key pair to use with the Secure Shell \(SSH\) client to securely log in to the Amazon EC2 instances running your Elastic Beanstalk application\.  | 
|  `--modules folder-1 folder-2`  |  List of child directories to initialize\. Only for use with Compose Environments\.  | 
|  `-p` *platform\-name*  `--platform` *platform\-name*  |  Specify the platform to skip interactive configuration\. The platform name can include a version if the platform supports multiple configurations\. If you do not specify a configuration version, EB CLI uses the most recent configuration\. Use `eb platform list` to see a list of available platforms\. For example: `php`, `PHP`, `php5.5`, `"PHP 5.5"`, `node.js`, `"64bit Amazon Linux 2014.03 v1.0.7 running PHP 5.5"`  When you specify this option, then EB CLI does not prompt you for values for any other options\. Instead, it assumes default values for each option\. You can specify options for anything for which you do not want to use default values\.   | 
|  `--source codecommit/repository-name/branch-name`  |  AWS CodeCommit repository and branch\. See [Using the EB CLI with AWS CodeCommit](eb-cli-codecommit.md)\.  | 
|  Common options  |  | 

## AWS CodeBuild Support<a name="eb3-init-codebuild"></a>

If you run `eb init` in a folder that contains a [buildspec\.yml](http://docs.aws.amazon.com/codebuild/latest/userguide/build-spec-ref.html) file, Elastic Beanstalk parses the file for an `eb_codebuild_settings` entry with the following format:

```
eb_codebuild_settings:
  CodeBuildServiceRole: role-name
  ComputeType: size
  Image: image
  Timeout: minutes
```

CodeBuildServiceRole  
The name \(not ARN\) of the IAM role for AWS CodeBuild\. This value is required and if omitted any subsequent `eb create` or `eb deploy` command fails\.

ComputeType  
The amount of resources for the Docker container\. Valid values are BUILD\_GENERAL1\_SMALL, BUILD\_GENERAL1\_MEDIUM, and BUILD\_GENERAL1\_LARGE\.

Image  
The name of the Docker Hub or Amazon ECR image that AWS CodeBuild creates for Elastic Beanstalk\. This value is optional and if omitted the `eb init` command prompts you for a platform and other options\. see [Build Environment Reference for AWS CodeBuild](http://docs.aws.amazon.com/codebuild/latest/userguide/build-env-ref.html) for a list of images\. 

Timeout  
The duration, in minutes, that the AWS CodeBuild build runs before timing out\. This value is optional\. See [Create a Build Project in AWS CodeBuild](http://docs.aws.amazon.com/codebuild/latest/userguide/create-project.html) for the default value and range of values\.

**Note**  
Some regions don't offer AWS CodeBuild\. The integration between Elastic Beanstalk and AWS CodeBuild doesn't work in these regions\.  
For information about the AWS services offered in each region, see [Region Table](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/)\.

Learn more about AWS CodeBuild support in Elastic Beanstalk in the [Using the EB CLI with AWS CodeBuild](eb-cli-codebuild.md) topic\.

## Output<a name="eb3-initoutput"></a>

If successful, the command guides you through setting up a new Elastic Beanstalk application through a series of prompts\.

## Example<a name="eb3-initexample"></a>

The following example request initializes EB CLI and prompts you to enter information about your application\. Replace the red placeholder text with your own values\.

```
$ eb init -i
Select a default region
1) us-east-1 : US East (N. Virginia)
2) us-west-1 : US West (N. California)
3) us-west-2 : US West (Oregon)
4) eu-west-1 : EU (Ireland)
5) eu-central-1 : EU (Frankfurt)
6) ap-south-1 : Asia Pacific (Mumbai)
7) ap-southeast-1 : Asia Pacific (Singapore)
8) ap-southeast-2 : Asia Pacific (Sydney)
9) ap-northeast-1 : Asia Pacific (Tokyo)
10) ap-northeast-2 : Asia Pacific (Seoul)
11) sa-east-1 : South America (Sao Paulo)
12) cn-north-1 : China (Beijing)
13) us-east-2 : US East (Columbus)
14) ca-central-1 : Canada (Central)
15) eu-west-2 : EU (London)
(default is 3): 3

Select an application to use
1) HelloWorldApp
2) NewApp
3) [ Create new Application ]
(default is 3): 3

Enter Application Name
(default is "tmp"):
Application tmp has been created.

It appears you are using PHP. Is this correct?
(y/n): y

Select a platform version.
1) PHP 5.5
2) PHP 5.4
3) PHP 5.3
(default is 1): 1
Do you want to set up SSH for your instances?
(y/n): y

Select a keypair.
1) aws-eb
2) [ Create new KeyPair ]
(default is 2): 1
```