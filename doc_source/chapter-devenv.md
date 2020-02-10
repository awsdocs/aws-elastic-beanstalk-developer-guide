# Configuring your development machine for use with Elastic Beanstalk<a name="chapter-devenv"></a>

This page shows you how to set up your local machine for development of an AWS Elastic Beanstalk application\. It covers folder structure, source control, and CLI tools\.

**Topics**
+ [Creating a project folder](#devenv-project-folder)
+ [Setting up source control](#devenv-git)
+ [Configuring a remote repository](#devenv-git-backup)
+ [Installing the EB CLI](#devenv-ebcli)
+ [Installing the AWS CLI](#devenv-awscli)

## Creating a project folder<a name="devenv-project-folder"></a>

Create a folder for your project\. You can store the folder anywhere on your local disk as long as you have permission to read from and write to it\. Creating a folder in your user folder is acceptable\. If you plan on working on multiple applications, create your project folders inside another folder named something like `workspace` or `projects` to keep everything organized:

```
workspace/
|-- my-first-app
`-- my-second-app
```

The contents of your project folder will vary depending on the web container or framework that your application uses\.

**Note**  
Avoid folders and paths with single\-quote \('\) or double\-quote \("\) characters in the folder name or any path element\. Some Elastic Beanstalk commands fail when run within a folder with either character in the name\.

## Setting up source control<a name="devenv-git"></a>

Set up source control to protect yourself from accidentally deleting files or code in your project folder, and for a way to revert changes that break your project\.

If you don't have a source control system, consider Git, a free and easy\-to\-use option, and it integrates well with the Elastic Beanstalk Command Line Interface \(CLI\)\. Visit the [Git homepage](https://git-scm.com/) to install Git\.

Follow the instructions on the Git website to install and configure Git, and then run `git init` in your project folder to set up a local repository:

```
~/workspace/my-first-app$ git init
Initialized empty Git repository in /home/local/username/workspace/my-first-app/.git/
```

As you add content to your project folder and update content, commit the changes to your Git repository:

```
~/workspace/my-first-app$ git add default.jsp
~/workspace/my-first-app$ git commit -m "add default JSP"
```

Every time you commit, you create a snapshot of your project that you can restore later if anything goes wrong\. For much more information on Git commands and workflows, see the [Git documentation](https://git-scm.com/doc)\.

## Configuring a remote repository<a name="devenv-git-backup"></a>

What if your hard drive crashes, or you want to work on your project on a different computer? To back up your source code online and access it from any computer, configure a remote repository to which you can push your commits\.

AWS CodeCommit lets you create a private repository in the AWS cloud\. CodeCommit is free in the [AWS free tier](https://aws.amazon.com/free/) for up to five AWS Identity and Access Management \(IAM\) users in your account\. For pricing details, see [AWS CodeCommit Pricing](https://aws.amazon.com/codecommit/pricing/)\. 

Visit the [AWS CodeCommit User Guide](https://docs.aws.amazon.com/codecommit/latest/userguide/setting-up.html) for instructions on getting set up\.

GitHub is another popular option for storing your project code online\. It lets you create a public online repository for free and also supports private repositories for a monthly charge\. Sign up for GitHub at [github\.com](https://github.com/)\.

After you've created a remote repository for your project, attach it to your local repository with `git remote add`:

```
~/workspace/my-first-app$ git remote add origin ssh://git-codecommit.us-east-2.amazonaws.com/v1/repos/my-repo
```

## Installing the EB CLI<a name="devenv-ebcli"></a>

Use the [EB CLI](eb-cli3.md) to manage your Elastic Beanstalk environments and monitor health from the command line\. See [Install the EB CLI](eb-cli3-install.md) for installation instructions\.

By default, the EB CLI packages everything in your project folder and uploads it to Elastic Beanstalk as a source bundle\. When you use Git and the EB CLI together, you can prevent built class files from being committed to source with `.gitignore` and prevent source files from being deployed with `.ebignore`\.

You can also [configure the EB CLI to deploy a build artifact](eb-cli3-configuration.md#eb-cli3-artifact) \(a WAR or ZIP file\) instead of the contents of your project folder\.

## Installing the AWS CLI<a name="devenv-awscli"></a>

The AWS Command Line Interface \(AWS CLI\) is a unified client for AWS services that provides commands for all public API operations\. These commands are lower level than those provided by the EB CLI, so it often takes more commands to do an operation with the AWS CLI\. On the other hand, the AWS CLI allows you to work with any application or environment running in your account without setting up a repository on your local machine\. Use the AWS CLI to create scripts that simplify or automate operational tasks\.

For more information about supported services and to download the AWS Command Line Interface, see [AWS Command Line Interface](https://aws.amazon.com/cli/)\.