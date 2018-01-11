# EB CLI 2\.6 \(Deprecated\)<a name="eb-cli"></a>

**Note**  
 This version of EB CLI and its documentation have been replaced with version 3 \(in this section, EB CLI 3 represents version 3 and later of EB CLI\)\. For information on the new version, see \. 

This section describes how to set up EB CLI 2\.6 and how to create a sample application using EB CLI 2\.6\. This section also includes a command reference for Eb CLI 2\.6\.


+ [Differences from Version 3 of EB CLI](#eb-cli2-differences)
+ [Migrating to EB CLI 3 and AWS CodeCommit](#eb-cli2-migrating)
+ [Getting Started with Eb](command-reference-get-started.md)
+ [Deploying a Git Branch to a Specific Environment](command-reference-branch-environment.md)
+ [Eb Common Options](eb-cmd-options.md)
+ [EB CLI 2 Commands](eb-cmd-commands.md)

## Differences from Version 3 of EB CLI<a name="eb-cli2-differences"></a>

EB is a command line interface \(CLI\) tool for Elastic Beanstalk that you can use to deploy applications quickly and more easily\. The latest version of EB was introduced by Elastic Beanstalk in EB CLI 3\. Although Elastic Beanstalk still supports EB 2\.6 for customers who previously installed and continue to use it, you should migrate to the latest version of EB CLI 3, as it can manage environments that you launched using EB CLI 2\.6 or earlier versions of EB CLI\. EB CLI automatically retrieves settings from an environment created using EB if the environment is running\. Note that EB CLI 3 does not store option settings locally, as in earlier versions\.

EB CLI introduces the commands `eb create`, `eb deploy`, `eb open`, `eb console`, `eb scale`, `eb setenv`, `eb config`, `eb terminate`, `eb clone`, `eb list`, `eb use`, `eb printenv`, and `eb ssh`\. In EB CLI 3\.1 or later, you can also use the `eb swap` command\. In EB CLI 3\.2 only, you can use the `eb abort`, `eb platform`, and `eb upgrade` commands\. In addition to these new commands, EB CLI 3 commands differ from EB CLI 2\.6 commands in several cases:

+ **`eb init`** – Use `eb init` to create an `.elasticbeanstalk` directory in an existing project directory and create a new Elastic Beanstalk application for the project\. Unlike with previous versions, EB CLI 3 and later versions do not prompt you to create an environment\.

+ **`eb start`** – EB CLI 3 does not include the command `eb start`\. Use `eb create` to create an environment\.

+ **`eb stop`** – EB CLI 3 does not include the command `eb stop`\. Use `eb terminate` to completely terminate an environment and clean up\.

+ **`eb push`** and **`git aws.push`** – EB CLI 3 does not include the commands `eb push` or `git aws.push`\. Use `eb deploy` to update your application code\.

+ **`eb update`** – EB CLI 3 does not include the command `eb update`\. Use `eb config` to update an environment\.

+ **`eb branch`** – EB CLI 3 does not include the command `eb branch`\.

For more information about using EB CLI 3 commands to create and manage an application, see [EB CLI Command Reference](eb3-cmd-commands.md)\. For a command reference for EB 2\.6, see [EB CLI 2 Commands](eb-cmd-commands.md)\. For a walkthrough of how to deploy a sample application using EB CLI 3, see [Managing Elastic Beanstalk Environments with the EB CLI](eb-cli3-getting-started.md)\. For a walkthrough of how to deploy a sample application using eb 2\.6, see [Getting Started with Eb](command-reference-get-started.md)\. For a walkthrough of how to use EB 2\.6 to map a Git branch to a specific environment, see [Deploying a Git Branch to a Specific Environment](command-reference-branch-environment.md)\. 

## Migrating to EB CLI 3 and AWS CodeCommit<a name="eb-cli2-migrating"></a>

Elastic Beanstalk has not only deprecated EB CLI 2\.6, but is also removing some 2\.6 functionality\. The most significant change from 2\.6 is that EB CLI no longer natively supports incremental code updates \(`eb push`, `git aws.push`\) or branching \(`eb branch`\)\. This section describes how to migrate from EB CLI 2\.6 to the latest version of EB CLI and use AWS CodeCommit as your code repository\.

If you have not done so already, create a code repository in AWS CodeCommit, as described in [Migrate to AWS CodeCommit](http://docs.aws.amazon.com/codecommit/latest/userguide/how-to-migrate-repository.html)\.

Once you have installed and configured EB CLI, you have two opportunities to associate your application with your AWS CodeCommit repository, including a specific branch\. 

+ When executing `eb init`, such in the following example where *myRepo* is the name of your AWS CodeCommit repository and *myBranch* is the branch in AWS CodeCommit\.

  ```
  eb init --source codecommit/myRepo/myBranch
  ```

+ When executing `eb deploy`, such in the following example where *myRepo* is the name of your AWS CodeCommit repository and *myBranch* is the branch in AWS CodeCommit\.

  ```
  eb deploy --source codecommit/myRepo/myBranch
  ```

For further information, including how to deploy incremental code updates to an Elastic Beanstalk environment without having to re\-upload your entire project, see [Using the EB CLI with AWS CodeCommit](eb-cli-codecommit.md)\.