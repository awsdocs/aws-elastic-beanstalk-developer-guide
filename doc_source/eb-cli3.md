# The Elastic Beanstalk Command Line Interface \(EB CLI\)<a name="eb-cli3"></a>

The EB CLI is a command line interface for Elastic Beanstalk that provides interactive commands that simplify creating, updating and monitoring environments from a local repository\. Use the EB CLI as part of your everyday development and testing cycle as an alternative to the AWS Management Console\.

**Note**  
The current version of the EB CLI has a different base set of commands than versions prior to version 3\.0\. If you are using an older version, see [Migrating to EB CLI 3 and CodeCommit](eb-cli.md#eb-cli2-migrating) for migration information\.

Once you've installed the EB CLI and configured a repository, you can create environments with a single command:

```
~/my-app$ eb create my-env
```

Previously, Elastic Beanstalk supported a separate CLI that provided direct access to API operations called the [Elastic Beanstalk API CLI](using-api-cli.md)\. This has been replaced with the [AWS CLI](chapter-devenv.md#devenv-awscli), which provides the same functionality but for all AWS services' APIs\.

With the AWS CLI you have direct access to the Elastic Beanstalk API\. The AWS CLI is great for scripting, but is not as easy to use from the command line because of the number of commands that you need to run and the number of parameters on each command\. For example, creating an environment requires a series of commands:

```
~$ aws elasticbeanstalk check-dns-availability --cname-prefix my-cname
~$ aws elasticbeanstalk create-application-version --application-name my-application --version-label v1 --source-bundle S3Bucket=my-bucket,S3Key=php-proxy-sample.zip
~$ aws elasticbeanstalk create-environment --cname-prefix my-cname --application-name my-app --version-label v1 --environment-name my-env --solution-stack-name "64bit Amazon Linux 2015.03 v2.0.0 running Ruby 2.2 (Passenger Standalone)"
```

For information about installing the EB CLI, configuring a repository, and working with environments, see the following topics:

**Topics**
+ [Install the EB CLI Using Setup Scripts](eb-cli3-install.md)
+ [Configure the EB CLI](eb-cli3-configuration.md)
+ [Managing Elastic Beanstalk Environments with the EB CLI](eb-cli3-getting-started.md)
+ [Using the EB CLI with AWS CodeBuild](eb-cli-codebuild.md)
+ [Using the EB CLI with Git](eb3-cli-git.md)
+ [Using the EB CLI with AWS CodeCommit](eb-cli-codecommit.md)
+ [Using the EB CLI to Monitor Environment Health](health-enhanced-ebcli.md)
+ [Managing Multiple AWS Elastic Beanstalk Environments as a Group with the EB CLI](ebcli-compose.md)
+ [Troubleshooting issues with the EB CLI](eb-cli-troubleshooting.md)
+ [EB CLI Command Reference](eb3-cmd-commands.md)
+ [EB CLI 2\.6 \(Retired\)](eb-cli.md)
+ [Elastic Beanstalk API Command Line Interface \(Retired\)](using-api-cli.md)