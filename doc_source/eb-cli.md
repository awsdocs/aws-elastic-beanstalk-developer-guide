# EB CLI 2\.6 \(retired\)<a name="eb-cli"></a>

 This version of the EB CLI and its documentation have been replaced with version 3 \(in this section, EB CLI 3 represents version 3 and later of the EB CLI\)\. For information on the new version, see [Using the Elastic Beanstalk command line interface \(EB CLI\)](eb-cli3.md)\. 

You should migrate to the latest version of EB CLI 3\. It can manage environments that you launched using EB CLI 2\.6 or earlier versions of EB CLI\.

## Differences from version 3 of EB CLI<a name="eb-cli2-differences"></a>

EB is a command line interface \(CLI\) tool for Elastic Beanstalk that you can use to deploy applications quickly and more easily\. The latest version of EB was introduced by Elastic Beanstalk in EB CLI 3\. EB CLI automatically retrieves settings from an environment created using EB if the environment is running\. Note that EB CLI 3 does not store option settings locally, as in earlier versions\.

EB CLI introduces the commands eb create, eb deploy, eb open, eb console, eb scale, eb setenv, eb config, eb terminate, eb clone, eb list, eb use, eb printenv, and eb ssh\. In EB CLI 3\.1 or later, you can also use the eb swap command\. In EB CLI 3\.2 only, you can use the eb abort, eb platform, and eb upgrade commands\. In addition to these new commands, EB CLI 3 commands differ from EB CLI 2\.6 commands in several cases:
+ **eb init** – Use eb init to create an `.elasticbeanstalk` directory in an existing project directory and create a new Elastic Beanstalk application for the project\. Unlike with previous versions, EB CLI 3 and later versions do not prompt you to create an environment\.
+ **eb start** – EB CLI 3 does not include the command eb start\. Use eb create to create an environment\.
+ **eb stop** – EB CLI 3 does not include the command eb stop\. Use eb terminate to completely terminate an environment and clean up\.
+ **eb push** and **`git aws.push`** – EB CLI 3 does not include the commands eb push or `git aws.push`\. Use eb deploy to update your application code\.
+ **eb update** – EB CLI 3 does not include the command eb update\. Use eb config to update an environment\.
+ **eb branch** – EB CLI 3 does not include the command eb branch\.

For more information about using EB CLI 3 commands to create and manage an application, see [EB CLI command reference](eb3-cmd-commands.md)\. For a walkthrough of how to deploy a sample application using EB CLI 3, see [Managing Elastic Beanstalk environments with the EB CLI](eb-cli3-getting-started.md)\.

## Migrating to EB CLI 3 and CodeCommit<a name="eb-cli2-migrating"></a>

Elastic Beanstalk has not only retired EB CLI 2\.6, but has also removed some 2\.6 functionality\. The most significant change from 2\.6 is that EB CLI no longer natively supports incremental code updates \(eb push, `git aws.push`\) or branching \(eb branch\)\. This section describes how to migrate from EB CLI 2\.6 to the latest version of EB CLI and use CodeCommit as your code repository\.

If you have not done so already, create a code repository in CodeCommit, as described in [Migrate to CodeCommit](https://docs.aws.amazon.com/codecommit/latest/userguide/how-to-migrate-repository.html)\.

Once you have [installed](eb-cli3-install.md) and [configured](eb-cli3-configuration.md) EB CLI, you have two opportunities to associate your application with your CodeCommit repository, including a specific branch\. 
+ When executing eb init, such in the following example where *myRepo* is the name of your CodeCommit repository and *myBranch* is the branch in CodeCommit\.

  ```
  eb init --source codecommit/myRepo/myBranch
  ```
+ When executing eb deploy, such in the following example where *myRepo* is the name of your CodeCommit repository and *myBranch* is the branch in CodeCommit\.

  ```
  eb deploy --source codecommit/myRepo/myBranch
  ```

For further information, including how to deploy incremental code updates to an Elastic Beanstalk environment without having to re\-upload your entire project, see [Using the EB CLI with AWS CodeCommit](eb-cli-codecommit.md)\.