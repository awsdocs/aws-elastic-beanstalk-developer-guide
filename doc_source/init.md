# init<a name="init"></a>

**Note**  
 This version of EB CLI and its documentation have been replaced with version 3 \(in this section, EB CLI 3 represents version 3 and later of EB CLI\)\. For information on the new version, see \. 

## Description<a name="initdescription"></a>

Sets various default values for AWS Elastic Beanstalk environments created with eb, including your AWS credentials and region\. The values you set with `init` apply only to the current directory and repository\. You can override some defaults with operation options \(for example, use `-e` or `--environment-name` to target `` to a specific environment\)\.

**Note**  
Until you run the `init` command, the current running environment is unchanged\. Each time you run the `init` command, new settings get appended to the `config` file\.

For a tutorial that shows you how to use `eb init` to deploy a sample application, see [Getting Started with Eb](command-reference-get-started.md)\.

## Syntax<a name="initsyntax"></a>

 **`eb init` ** 

## Options<a name="initoptions"></a>

None of these options are required\. If you run `eb init` without any options, you will be prompted to enter or select a value for each setting\.


****  

|  Name  |  Description  |  Required  | 
| --- | --- | --- | 
|  `-a` `or` `--application-name` *`APPLICATION_NAME`*   |  The application managed by the current repository\. Type: String Default: None  |  No  | 
|   `--aws-credential-file` * `FILE_PATH_NAME` *   |  The file location where your AWS credentials are saved\. \(You can use the environment variable `AWS_CREDENTIAL_FILE` to set the file location\.\) Type: String Default: None  |  No  | 
|   `-e`  or  `--environment-name` * `ENVIRONMENT_NAME` *   |  The environment on which you want to perform operations\. Type: String Default: `<application-name>-env`  |  No  | 
|   `-I`  or  `--access-key-id` * `ACCESS_KEY_ID` *   |  Your AWS access key ID\. Type: String Default: None  |  No  | 
|   `-S`  or  `--secret-key` * `SECRET_ACCESS_KEY` *   |  Your AWS secret access key\. Type: String Default: None  |  No  | 
|   `-s`  or  `--solution-stack` * `SOLUTION_STACK_LABEL` *   |  The solution stack used as the application container type\. If you run `eb init` without this option, you will be prompted to choose from a list of supported solution stacks\. Solution stack names change when AMIs are updated\. Type: String Default: None  |  No  | 
|  Common options  |  For more information, see [Eb Common Options](eb-cmd-options.md)\.  |  No  | 

## Output<a name="initoutput"></a>

If successful, the command guides you through setting up a new AWS Elastic Beanstalk application through a series of prompts\.

## Example<a name="initexample"></a>

The following example request initializes eb and prompts you to enter information about your application\. Replace the red placeholder text with your own values\.

```
PROMPT> eb init

C:\>eb init
Enter your AWS Access Key ID (current value is "AKIAI*****5ZB7Q"):
Enter your AWS Secret Access Key (current value is "DHSAi*****xKPo6"):
Select an AWS Elastic Beanstalk service region (current value is "US East (Virginia)".
Available service regions are:
1) US East (Virginia)
2) US West (Oregon)
3) US West (North California)
4) EU West (Ireland)
5) Asia Pacific (Singapore)
6) Asia Pacific (Tokyo)
7) Asia Pacific (Sydney)
8) South America (Sao Paulo)
Select (1 to 8): 2
Enter an AWS Elastic Beanstalk application name (current value is "MyApp"): MyApp
Enter an AWS Elastic Beanstalk environment name (current value is "MyApp-env"): MyApp-env
Select a solution stack (current value is "64bit Amazon Linux running Python").
Available solution stacks are:
1) 32bit Amazon Linux running PHP 5.4
2) 64bit Amazon Linux running PHP 5.4
3) 32bit Amazon Linux running PHP 5.3
4) 64bit Amazon Linux running PHP 5.3
5) 32bit Amazon Linux running Node.js
6) 64bit Amazon Linux running Node.js
7) 64bit Windows Server 2008 R2 running IIS 7.5
8) 64bit Windows Server 2012 running IIS 8
9) 32bit Amazon Linux running Tomcat 7
10) 64bit Amazon Linux running Tomcat 7
11) 32bit Amazon Linux running Tomcat 6
12) 64bit Amazon Linux running Tomcat 6
13) 32bit Amazon Linux running Python
14) 64bit Amazon Linux running Python
15) 32bit Amazon Linux running Ruby 1.8.7
16) 64bit Amazon Linux running Ruby 1.8.7
17) 32bit Amazon Linux running Ruby 1.9.3
18) 64bit Amazon Linux running Ruby 1.9.3
[...]
Select (1 to 70): 60
Select an environment type (current value is "LoadBalanced").
Available environment types are:
1) LoadBalanced
2) SingleInstance
Select (1 to 2): 1
Create an RDS DB Instance? [y/n] (current value is "Yes"): y
Create an RDS BD Instance from (current value is "[No snapshot]"):
1) [No snapshot]
2) [Other snapshot]
Select (1 to 2): 1
Enter an RDS DB master password (current value is "******"):
Retype password to confirm:
If you terminate your environment, your RDS DB Instance will be deleted and you will lose your data.
Create snapshot? [y/n] (current value is "Yes"): y
Attach an instance profile (current value is "aws-elasticbeanstalk-ec2-role"):
1) [Create a default instance profile]
2) AppServer-AppServerInstanceProfile-TK2exampleHP
3) AppServer-AppServerInstanceProfile-1G2exampleK8
4) aws-opsworks-ec2-role
5) aws-elasticbeanstalk-ec2-role
6) [Other instance profile]
Select (1 to 6): 5
Updated AWS Credential file at "C:\Users\YourName\.elasticbeanstalk\aws_credential_file".
```