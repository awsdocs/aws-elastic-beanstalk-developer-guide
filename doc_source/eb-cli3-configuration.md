# Configure the EB CLI<a name="eb-cli3-configuration"></a>

After [installing the EB CLI](eb-cli3-install.md), you are ready to configure your project directory and the EB CLI by running eb init\.

The following example shows the configuration steps when running eb init for the first time in a project folder named `eb`\.

**To initialize an EB CLI project**

1. First, the EB CLI prompts you to select a region\. Type the number that corresponds to the region that you want to use, and then press **Enter**\.

   ```
   ~/eb $ eb init
   Select a default region
   1) us-east-1 : US East (N. Virginia)
   2) us-west-1 : US West (N. California)
   3) us-west-2 : US West (Oregon)
   4) eu-west-1 : Europe (Ireland)
   5) eu-central-1 : Europe (Frankfurt)
   6) ap-south-1 : Asia Pacific (Mumbai)
   7) ap-southeast-1 : Asia Pacific (Singapore)
   ...
   (default is 3): 3
   ```

1. Next, provide your access key and secret key so that the EB CLI can manage resources for you\. Access keys are created in the AWS Identity and Access Management console\. If you don't have keys, see [How Do I Get Security Credentials?](https://docs.aws.amazon.com/general/latest/gr/getting-aws-sec-creds.html) in the *Amazon Web Services General Reference*\.

   ```
   You have not yet set up your credentials or your credentials are incorrect.
   You must provide your credentials.
   (aws-access-id): AKIAJOUAASEXAMPLE
   (aws-secret-key): 5ZRIrtTM4ciIAvd4EXAMPLEDtm+PiPSzpoK
   ```

1. An application in Elastic Beanstalk is a resource that contains a set of application versions \(source\), environments, and saved configurations that are associated with a single web application\. Each time you deploy your source code to Elastic Beanstalk using the EB CLI, a new application version is created and added to the list\.

   ```
   Select an application to use
   1) [ Create new Application ]
   (default is 1): 1
   ```

1. The default application name is the name of the folder in which you run eb init\. Enter any name that describes your project\.

   ```
   Enter Application Name
   (default is "eb"): eb
   Application eb has been created.
   ```

1. Select a platform that matches the language or framework that your web application is developed in\. If you haven't started developing an application yet, choose a platform that you're interested in\. You will see how to launch a sample application shortly, and you can always change this setting later\.

   ```
   Select a platform.
   1) Node.js
   2) PHP
   3) Python
   4) Ruby
   5) Tomcat
   6) IIS
   7) Docker
   8) Multi-container Docker
   9) GlassFish
   10) Go
   11) Java
   (default is 1): 1
   ```

1. Choose **yes** to assign an SSH key pair to the instances in your Elastic Beanstalk environment\. This allows you to connect directly to them for troubleshooting\.

   ```
   Do you want to set up SSH for your instances?
   (y/n): y
   ```

1. Choose an existing key pair or create a new one\. To use eb init to create a new key pair, you must have ssh\-keygen installed on your local machine and available from the command line\. The EB CLI registers the new key pair with Amazon EC2 for you and stores the private key locally in a folder named `.ssh` in your user directory\.

   ```
   Select a keypair.
   1) [ Create new KeyPair ]
   (default is 1): 1
   ```

Your EB CLI installation is now configured and ready to use\. See [Managing Elastic Beanstalk environments with the EB CLI](eb-cli3-getting-started.md) for instructions on creating and working with an Elastic Beanstalk environment\.

**Topics**
+ [Ignoring files using \.ebignore](#eb-cli3-ebignore)
+ [Using named profiles](#eb-cli3-profile)
+ [Deploying an artifact instead of the project folder](#eb-cli3-artifact)
+ [Configuration settings and precedence](#eb-cli3-credentials)
+ [Instance metadata](#eb-cli3-metadata)

## Ignoring files using \.ebignore<a name="eb-cli3-ebignore"></a>

You can tell the EB CLI to ignore certain files in your project directory by adding the file `.ebignore` to the directory\. This file works like a `.gitignore` file\. When you deploy your project directory to Elastic Beanstalk and create a new application version, the EB CLI doesn't include files specified by `.ebignore` in the source bundle that it creates\.

If `.ebignore` isn't present, but `.gitignore` is, the EB CLI ignores files specified in `.gitignore`\. If `.ebignore` is present, the EB CLI doesn't read `.gitignore`\.

When `.ebignore` is present, the EB CLI doesn't use git commands to create your source bundle\. This means that EB CLI ignores files specified in `.ebignore`, and includes all other files\. In particular, it includes uncommitted source files\.

**Note**  
In Windows, adding `.ebignore` causes the EB CLI to follow symbolic links and include the linked file when creating a source bundle\. This is a known issue and will be fixed in a future update\.

## Using named profiles<a name="eb-cli3-profile"></a>

If you store your credentials as a named profile in a `credentials` or `config` file, you can use the [`--profile`](eb3-cmd-options.md) option to explicitly specify a profile\. For example, the following command creates a new application using the `user2` profile\.

```
$ eb init --profile user2
```

You can also change the default profile by setting the `AWS_EB_PROFILE` environment variable\. When this variable is set, the EB CLI reads credentials from the specified profile instead of `default` or eb\-cli\.

**Linux, macOS, or Unix**

```
$ export AWS_EB_PROFILE=user2
```

**Windows**

```
> set AWS_EB_PROFILE=user2
```

## Deploying an artifact instead of the project folder<a name="eb-cli3-artifact"></a>

You can tell the EB CLI to deploy a ZIP file or WAR file that you generate as part of a separate build process by adding the following lines to `.elasticbeanstalk/config.yml` in your project folder\.

```
deploy:
  artifact: path/to/buildartifact.zip
```

If you configure the EB CLI in your [Git repository](eb3-cli-git.md), and you don't commit the artifact to source, use the `--staged` option to deploy the latest build\.

```
~/eb$ eb deploy --staged
```

## Configuration settings and precedence<a name="eb-cli3-credentials"></a>

The EB CLI uses a *provider chain* to look for AWS credentials in a number of different places, including system or user environment variables and local AWS configuration files\.

The EB CLI looks for credentials and configuration settings in the following order:

1. **Command line options** – Specify a named profile by using `--profile` to override default settings\.

1. **Environment variables** – `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`\.

1. **The AWS credentials file** – Located at `~/.aws/credentials` on Linux and OS X systems, or at `C:\Users\USERNAME\.aws\credentials` on Windows systems\. This file can contain multiple named profiles in addition to a default profile\.

1. **The [AWS CLI configuration file](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html#cli-config-files)** – Located at `~/.aws/config` on Linux and OS X systems or `C:\Users\USERNAME\.aws\config` on Windows systems\. This file can contain a default profile, [named profiles](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html#cli-multiple-profiles), and AWS CLI–specific configuration parameters for each\.

1. **Legacy EB CLI configuration file** – Located at `~/.elasticbeanstalk/config` on Linux and OS X systems or `C:\Users\USERNAME\.elasticbeanstalk\config` on Windows systems\.

1. **Instance profile credentials** – These credentials can be used on Amazon EC2 instances with an assigned instance role, and are delivered through the Amazon EC2 metadata service\. The [instance profile](concepts-roles-instance.md) must have permission to use Elastic Beanstalk\.

If the credentials file contains a named profile with the name "eb\-cli", the EB CLI will prefer that profile over the default profile\. If no profiles are found, or a profile is found but does not have permission to use Elastic Beanstalk, the EB CLI prompts you to enter keys\.

## Instance metadata<a name="eb-cli3-metadata"></a>

To use the EB CLI from an Amazon EC2 instance, create a role that has access to the resources needed and assign that role to the instance when it is launched\. Launch the instance and install the EB CLI by using `pip`\.

```
~$ sudo pip install awsebcli
```

`pip` comes preinstalled on Amazon Linux\.

The EB CLI reads credentials from the instance metadata\. For more information, see [ Granting Applications that Run on Amazon EC2 Instances Access to AWS Resources](https://docs.aws.amazon.com/IAM/latest/UserGuide/role-usecase-ec2app.html) in *IAM User Guide*\.