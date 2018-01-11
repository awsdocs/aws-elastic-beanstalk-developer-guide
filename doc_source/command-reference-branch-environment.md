# Deploying a Git Branch to a Specific Environment<a name="command-reference-branch-environment"></a>

**Note**  
 This version of EB CLI and its documentation have been replaced with version 3 \(in this section, EB CLI 3 represents version 3 and later of EB CLI\)\. For information on the new version, see \. 

Developers often use branching in a project to manage code intended for different target environments\. For example, you might have a test branch where you perform component or integration testing and a prod branch where you manage the code for your live or production code\. With version 2\.3 and later of the eb command line interface and AWS DevTools, you can use the `eb init` command to configure the `eb push` command to push your current git branch to a specific Elastic Beanstalk environment\.

**To set up a Git branch to deploy to a specific environment**

1. Make sure you have version 2\.3 of the Elastic Beanstalk command line tools installed\.

   To check what version you have installed, use the following command:

   ```
   eb --version
   ```

   To download the command line tools, go to [Elastic Beanstalk Command Line Tool](http://aws.amazon.com/code/6752709412171743) page and follow the instructions in the README\.txt file in the `.zip` file\.

1. From a command prompt, change directories to the location of the local repository containing the code you want to deploy\.

   If you have not set up a Git repository, you need to create one to continue\. For information about how to use Git, see the [Git documentation](http://git-scm.com/doc)\.

1. Make sure that the current branch for your local repository is the one you want to map to an Elastic Beanstalk environment\.

   To switch to a branch, you use the `git checkout` command\. For example, you would use the following command to switch to the prod branch\.

   ```
   git checkout prod
   ```

   For more information about creating and managing branches in Git, see the [Git documentation](http://git-scm.com/book/en/Git-Branching)\.

1. If you have not done so already, use the `eb init` command to configure eb to use Elastic Beanstalk with a specific settings for credentials, application, region, environment, and solution stack\. The values set with `eb init` will be used as defaults for the environments that you create for your branches\. For detailed instructions, see [Step 2: Configure Elastic Beanstalk](command-reference-get-started.md#command-reference-get-started-init)\.

1. Use the `eb branch` command to map the current branch to a specific environment\.

   1. Type the following command\.

      ```
      eb branch
      ```

   1. When prompted for an environment name, enter the name of the environment that you want to map to the current branch\.

      The eb command will suggest a name in parentheses and you can accept that name by pressing the **Enter** key or type the name that you want\.

      ```
      The current branch is "myotherbranch".
          Enter an Elastic Beanstalk environment name (auto-generated value is "test-myotherbranch-en"):
      ```

      You'll notice that eb displays the current branch in your Git repository so you know which branch you're working with\. You can specify an existing environment or a new one\. If you specify a new one, you'll need to create it with the `eb start` command\.

   1. When prompted about using the settings from the default environment, type **y** unless you explicitly don't want to use the `optionsettings` file from the default environment for the environment for this branch\.

      ```
      Do you want to copy the settings from the default environment "main-env" for the new branch? [y/n]: y
      ```

1. If you specified a new environment for your branch, use the `eb start` command to create and start the environment\.

   When this command is successful, you're ready for the next step\.

1. Use the `eb push` command to deploy the changes in the current branch to the environment that you mapped to the branch\.