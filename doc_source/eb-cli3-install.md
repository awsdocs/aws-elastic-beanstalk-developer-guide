# Install the EB CLI Using Setup Scripts<a name="eb-cli3-install"></a>

The Elastic Beanstalk Command Line Interface \(EB CLI\) is a command line client that you can use to create, configure, and manage Elastic Beanstalk environments\.

**Topics**
+ [Install the EB CLI](#eb-cli3-install.scripts)
+ [Prerequisites](#eb-cli3-install.scripts.prereqs)
+ [Install the EB CLI on Linux and macOS](#eb-cli3-install.scripts.linux)
+ [Install the EB CLI on Windows](#eb-cli3-install.scripts.windows)
+ [Update the EB CLI](#eb-cli3-install.scripts.update)
+ [Manually Install the EB CLI](eb-cli3-install-advanced.md)

## Install the EB CLI<a name="eb-cli3-install.scripts"></a>

Use the [EB CLI setup scripts](https://github.com/aws/aws-elastic-beanstalk-cli-setup) to install the EB CLI on Linux, macOS, or Windows\. The scripts install the EB CLI and its dependencies, including Python and `pip`\. The scripts also create a virtual environment for the EB CLI\. Use the following procedures to download the scripts and install the EB CLI\.

## Prerequisites<a name="eb-cli3-install.scripts.prereqs"></a>

You must have Git installed\. To see which version of Git you have, use the following command\.

```
$ git --version
git version version-number
```

If you don't have Git, install it\. You can install Git from websites such as [Git Downloads](https://git-scm.com/downloads)\.

## Install the EB CLI on Linux and macOS<a name="eb-cli3-install.scripts.linux"></a>

**For Bash and Zsh**

1. Clone the `aws-elastic-beanstalk-cli-setup` repository from GitHub\.

   ```
   $ git clone https://github.com/aws/aws-elastic-beanstalk-cli-setup.git
   ```

1. Run the bundled installer\.

   ```
   $ ./aws-elastic-beanstalk-cli-setup/scripts/bundled_installer
   ```

1. Follow the instructions that the bundled installer outputs to add Python and the EB CLI to your `PATH` variable\. 

## Install the EB CLI on Windows<a name="eb-cli3-install.scripts.windows"></a>

**In PowerShell or the Command Prompt window**

1. Clone the `aws-elastic-beanstalk-cli-setup` repository from GitHub\.

   ```
   $ git clone https://github.com/aws/aws-elastic-beanstalk-cli-setup.git
   ```

1. Run the bundled installer\.

   ```
   $ .\aws-elastic-beanstalk-cli-setup\scripts\bundled_installer
   ```

## Update the EB CLI<a name="eb-cli3-install.scripts.update"></a>

To upgrade to the latest version of the EB CLI, download the latest version of the [EB CLI setup scripts](https://github.com/aws/aws-elastic-beanstalk-cli-setup) and rerun the bundled installer\. 