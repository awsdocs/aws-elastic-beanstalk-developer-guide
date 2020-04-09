# Using the EB CLI with AWS CodeCommit<a name="eb-cli-codecommit"></a>

You can use the EB CLI to deploy your application directly from your AWS CodeCommit repository\. With CodeCommit, you can upload only your changes to the repository when you deploy, instead of uploading your entire project\. This can save you time and bandwidth if you have a large project or limited Internet connectivity\. The EB CLI pushes your local commits and uses them to create application versions when you use eb create or eb deploy\.

To deploy your changes, CodeCommit integration requires you to commit changes first\. However, as you develop or debug, you might not want to push changes that you haven't confirmed are working\. You can avoid committing your changes by staging them and using eb deploy \-\-staged \(which performs a standard deployment\)\. Or commit your changes to a development or testing branch first, and merge to your master branch only when your code is ready\. With eb use, you can configure the EB CLI to deploy to one environment from your development branch, and to a different environment from your master branch\.

**Note**  
Some regions don't offer CodeCommit\. The integration between Elastic Beanstalk and CodeCommit doesn't work in these regions\.  
For information about the AWS services offered in each region, see [Region Table](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/)\.

**Topics**
+ [Prerequisites](#eb-cli-codecommit-prereqs)
+ [Creating a CodeCommit repository with the EB CLI](#eb-cli-codecommit-create)
+ [Deploying from your CodeCommit repository](#eb-cli-codecommit-deploy)
+ [Configuring additional branches and environments](#eb-cli-codecommit-config)
+ [Using an existing CodeCommit repository](#eb-cli-codecommit-existing)

## Prerequisites<a name="eb-cli-codecommit-prereqs"></a>

To use CodeCommit with AWS Elastic Beanstalk, you need a local Git repository \(either one you have already or a new one you create\) with at least one commit, [permission to use CodeCommit](https://docs.aws.amazon.com/codecommit/latest/userguide/access-permissions.html), and an Elastic Beanstalk environment in a region that CodeCommit supports\. Your environment and repository must be in the same region\.

**To initialize a Git repository**

1. Run `git init` in your project folder\.

   ```
   ~/my-app$ git init
   ```

1. Stage your project files with `git add`\.

   ```
   ~/my-app$ git add .
   ```

1. Commit changes with `git commit`\.

   ```
   ~/my-app$ git commit -m "Elastic Beanstalk application"
   ```

## Creating a CodeCommit repository with the EB CLI<a name="eb-cli-codecommit-create"></a>

To get started with CodeCommit, run [eb init](eb3-init.md)\. During repository configuration, the EB CLI prompts you to use CodeCommit to store your code and speed up deployments\. Even if you previously configured your project with eb init, you can run it again to configure CodeCommit\.

**To create a CodeCommit repository with the EB CLI**

1. Run eb init in your project folder\. During configuration, the EB CLI asks if you want to use CodeCommit to store your code and speed up deployments\. If you previously configured your project with eb init, you can still run it again to configure CodeCommit\. Type **y** at the prompt to set up CodeCommit\.

   ```
   ~/my-app$ eb init
   Note: Elastic Beanstalk now supports AWS CodeCommit; a fully-managed source control service. To learn more, see Docs: https://aws.amazon.com/codecommit/
   Do you wish to continue with CodeCommit? (y/n)(default is n): y
   ```

1. Choose **Create new Repository**\.

   ```
   Select a repository
   1) my-repo
   2) [ Create new Repository ]
   (default is 2): 2
   ```

1. Type a repository name or press **Enter** to accept the default name\.

   ```
   Enter Repository Name
   (default is "codecommit-origin"): my-app
   Successfully created repository: my-app
   ```

1. Choose an existing branch for your commits, or use the EB CLI to create a new branch\.

   ```
   Enter Branch Name
   ***** Must have at least one commit to create a new branch with CodeCommit *****
   (default is "master"): ENTER
   Successfully created branch: master
   ```

## Deploying from your CodeCommit repository<a name="eb-cli-codecommit-deploy"></a>

When you configure CodeCommit with your EB CLI repository, the EB CLI uses the contents of the repository to create source bundles\. When you run eb deploy or eb create, the EB CLI pushes new commits and uses the HEAD revision of your branch to create the archive that it deploys to the EC2 instances in your environment\.

**To use CodeCommit integration with the EB CLI**

1. Create a new environment with eb create\.

   ```
   ~/my-app$ eb create my-app-env
   Starting environment deployment via CodeCommit
   --- Waiting for application versions to be pre-processed ---
   Finished processing application version app-ac1ea-161010_201918
   Setting up default branch
   Environment details for: my-app-env
     Application name: my-app
     Region: us-east-2
     Deployed Version: app-ac1ea-161010_201918
     Environment ID: e-pm5mvvkfnd
     Platform: 64bit Amazon Linux 2016.03 v2.1.6 running Java 8
     Tier: WebServer-Standard
     CNAME: UNKNOWN
     Updated: 2016-10-10 20:20:29.725000+00:00
   Printing Status:
   INFO: createEnvironment is starting.
   ...
   ```

   The EB CLI uses the latest commit in the tracked branch to create the application version that is deployed to the environment\.

1. When you have new local commits, use eb deploy to push the commits and deploy to your environment\.

   ```
   ~/my-app$ eb deploy
   Starting environment deployment via CodeCommit
   INFO: Environment update is starting.
   INFO: Deploying new version to instance(s).
   INFO: New application version was deployed to running EC2 instances.
   INFO: Environment update completed successfully.
   ```

1. To test changes before you commit them, use the `--staged` option to deploy changes that you added to the staging area with `git add`\.

   ```
   ~/my-app$ git add new-file
   ~/my-app$ eb deploy --staged
   ```

   Deploying with the `--staged` option performs a standard deployment, bypassing CodeCommit\.

## Configuring additional branches and environments<a name="eb-cli-codecommit-config"></a>

CodeCommit configuration applies to a single branch\. You can use eb use and eb codesource to configure additional branches or modify the current branch's configuration\.

**To configure CodeCommit integration with the EB CLI**

1. To change the remote branch, use the [eb use](eb3-use.md) command's `--source` option\.

   ```
   ~/my-app$ eb use test-env --source my-app/test
   ```

1. To create a new branch and environment, check out a new branch, push it to CodeCommit, create the environment, and then use eb use to connect the local branch, remote branch, and environment\.

   ```
   ~/my-app$ git checkout -b production
   ~/my-app$ git push --set-upstream production
   ~/my-app$ eb create production-env
   ~/my-app$ eb use --source my-app/production production-env
   ```

1. To configure CodeCommit interactively, use [eb codesource codecommit](eb3-codesource.md)\.

   ```
   ~/my-app$ eb codesource codecommit
   Current CodeCommit setup:
     Repository: my-app
     Branch: test
   Do you wish to continue (y/n): y
   
   Select a repository
   1) my-repo
   2) my-app
   3) [ Create new Repository ]
   (default is 2): 2
   
   Select a branch
   1) master
   2) test
   3) [ Create new Branch with local HEAD ]
   (default is 1): 1
   ```

1. To disable CodeCommit integration, use [eb codesource local](eb3-codesource.md)\.

   ```
   ~/my-app$ eb codesource local
   Current CodeCommit setup:
     Repository: my-app
     Branch: master
   Default set to use local sources
   ```

## Using an existing CodeCommit repository<a name="eb-cli-codecommit-existing"></a>

If you already have a CodeCommit repository and want to use it with Elastic Beanstalk, run eb init at the root of your local Git repository\.

**To use an existing CodeCommit repository with the EB CLI**

1. Clone your CodeCommit repository\.

   ```
   ~$ git clone ssh://git-codecommit.us-east-2.amazonaws.com/v1/repos/my-app
   ```

1. Check out and push a branch to use for your Elastic Beanstalk environment\.

   ```
   ~/my-app$ git checkout -b dev-env
   ~/my-app$ git push --set-upstream origin dev-env
   ```

1. Run eb init\. Choose the same region, repository, and branch name that you are currently using\.

   ```
   ~/my-app$ eb init
   Select a default region
   1) us-east-1 : US East (N. Virginia)
   2) us-west-1 : US West (N. California)
   3) us-west-2 : US West (Oregon)
   4) eu-west-1 : Europe (Ireland)
   5) eu-central-1 : Europe (Frankfurt)
   6) ap-south-1 : Asia Pacific (Mumbai)
   7) ap-southeast-1 : Asia Pacific (Singapore)
   ...
   (default is 3): 1
   ...
   Note: Elastic Beanstalk now supports AWS CodeCommit; a fully-managed source control service. To learn more, see Docs: https://aws.amazon.com/codecommit/
   Do you wish to continue with CodeCommit? (y/n)(default is n): y
   
   Select a repository
   1) my-app
   2) [ Create new Repository ]
   (default is 1): 1
   
   Select a branch
   1) master
   2) dev-env
   3) [ Create new Branch with local HEAD ]
   (default is 2): 2
   ```

For more information about using eb init, see [Configure the EB CLI](eb-cli3-configuration.md)\.