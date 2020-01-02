# Using the EB CLI with Git<a name="eb3-cli-git"></a>

The EB CLI provides integration with Git\. This section provides an overview of how to use Git with the EB CLI\.

**To install Git and initialize your Git repository**

1. Download the most recent version of Git by visiting [http://git\-scm\.com](http://git-scm.com)\.

1. Initialize your Git repository by typing the following:

   ```
   ~/eb$ git init
   ```

   EB CLI will now recognize that your application is set up with Git\.

1. If you haven't already run eb init, do that now: 

   ```
   ~/eb$ eb init
   ```

## Associating Elastic Beanstalk environments with Git branches<a name="eb3-cli-git.branches"></a>

You can associate a different environment with each branch of your code\. When you checkout a branch, changes are deployed to the associated environment\. For example, you can type the following to associate your production environment with your master branch, and a separate development environment with your development branch:

```
~/eb$ git checkout master
~/eb$ eb use prod
~/eb$ git checkout develop
~/eb$ eb use dev
```

## Deploying changes<a name="eb3-cli-git.deploy"></a>

By default, the EB CLI deploys the latest commit in the current branch, using the commit ID and message as the application version label and description, respectively\. If you want to deploy to your environment without committing, you can use the `--staged` option to deploy changes that have been added to the staging area\.

**To deploy changes without committing**

1. Add new and changed files to the staging area:

   ```
   ~/eb$ git add .
   ```

1. Deploy the staged changes with eb deploy:

   ```
   ~/eb$ eb deploy --staged
   ```

If you have configured the EB CLI to [deploy an artifact](eb-cli3-configuration.md#eb-cli3-artifact), and you don't commit the artifact to your git repository, use the `--staged` option to deploy the latest build\.

## Using Git submodules<a name="eb3-cli-git.submodules"></a>

Some code projects benefit from having Git submodules — repositories within the top\-level repository\. When you deploy your code using eb create or eb deploy, the EB CLI can include submodules in the application version zip file and upload them with the rest of the code\.

You can control the inclusion of submodules by using the `include_git_submodules` option in the `global` section of the EB CLI configuration file, `.elasticbeanstalk/config.yml` in your project folder\.

To include submodules, set this option to `true`:

```
global:
  include_git_submodules: true
```

When the `include_git_submodules` option is missing or set to `false`, EB CLI does not include submodules in the uploaded zip file\.

See [Git Tools \- Submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules) for more details about Git submodules\.

**Default behavior**  
When you run eb init to configure your project, the EB CLI adds the `include_git_submodules` option and sets it to `true`\. This ensures that any submodules you have in your project are included in your deployments\.  
The EB CLI did not always support including submodules\. To avoid an accidental and undesirable change to projects that had existed before we added submodule support, the EB CLI does not include submodules when the `include_git_submodules` option is missing\. If you have one of these existing projects and you want to include submodules in your deployments, add the option and set it to `true` as explained in this section\.

**CodeCommit behavior**  
Elastic Beanstalk's integration with [CodeCommit](eb-cli-codecommit.md) doesn't support submodules at this time\. If you enabled your environment to integrate with CodeCommit, submodules are not included in your deployments\.

## Assigning Git tags to your application version<a name="eb3-cli-git.tags"></a>

You can use a Git tag as your version label to identify what application version is running in your environment\. For example, type the following:

```
~/eb$ git tag -a v1.0 -m "My version 1.0"
```