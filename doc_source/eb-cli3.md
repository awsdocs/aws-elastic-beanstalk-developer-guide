# Using the Elastic Beanstalk command line interface \(EB CLI\)<a name="eb-cli3"></a>

The EB CLI is a command line interface for AWS Elastic Beanstalk that provides interactive commands that simplify creating, updating and monitoring environments from a local repository\. Use the EB CLI as part of your everyday development and testing cycle as an alternative to the Elastic Beanstalk console\.

**Note**  
The current version of the EB CLI has a different base set of commands than versions prior to version 3\.0\. If you are using an older version, see [Migrating to EB CLI 3 and CodeCommit](eb-cli.md#eb-cli2-migrating) for migration information\.

After you [install the EB CLI](eb-cli3-install.md) and configure a project directory, you can create environments with a single command:

```
~/my-app$ eb create my-env
```

The source code for the EB CLI is an open\-source project\. It resides in the `[aws/aws\-elastic\-beanstalk\-cli](https://github.com/aws/aws-elastic-beanstalk-cli)` GitHub repository\. You can participate by reporting issues, making suggestions, and submitting pull requests\. We value your contributions\! For an environment where you only intend to use the EB CLI as is, we recommend that you install it using one of the EB CLI setup scripts, as detailed in [Install the EB CLI using setup scripts](eb-cli3-install.md#eb-cli3-install.scripts)\.

Previously, Elastic Beanstalk supported a separate CLI that provided direct access to API operations called the [Elastic Beanstalk API CLI](using-api-cli.md)\. This has been replaced with the [AWS CLI](chapter-devenv.md#devenv-awscli), which provides the same functionality but for all AWS services' APIs\.

With the AWS CLI you have direct access to the Elastic Beanstalk API\. The AWS CLI is great for scripting, but is not as easy to use from the command line because of the number of commands that you need to run and the number of parameters on each command\. For example, creating an environment requires a series of commands:

```
~$ aws elasticbeanstalk check-dns-availability --cname-prefix my-cname
~$ aws elasticbeanstalk create-application-version --application-name my-application --version-label v1 --source-bundle S3Bucket=my-bucket,S3Key=php-proxy-sample.zip
~$ aws elasticbeanstalk create-environment --cname-prefix my-cname --application-name my-app --version-label v1 --environment-name my-env --solution-stack-name "64bit Amazon Linux 2015.03 v2.0.0 running Ruby 2.2 (Passenger Standalone)"
```

For information about installing the EB CLI, configuring a repository, and working with environments, see the following topics\.

**Topics**
+ [Install the EB CLI](eb-cli3-install.md)
+ [Configure the EB CLI](eb-cli3-configuration.md)
+ [Managing Elastic Beanstalk environments with the EB CLI](eb-cli3-getting-started.md)
+ [Using the EB CLI with AWS CodeBuild](eb-cli-codebuild.md)
+ [Using the EB CLI with Git](eb3-cli-git.md)
+ [Using the EB CLI with AWS CodeCommit](eb-cli-codecommit.md)
+ [Using the EB CLI to monitor environment health](health-enhanced-ebcli.md)
+ [Managing multiple Elastic Beanstalk environments as a group with the EB CLI](ebcli-compose.md)
+ [Troubleshooting issues with the EB CLI](eb-cli-troubleshooting.md)
+ [EB CLI command reference](eb3-cmd-commands.md)
+ [EB CLI 2\.6 \(retired\)](eb-cli.md)
+ [Elastic Beanstalk API command line interface \(retired\)](using-api-cli.md)