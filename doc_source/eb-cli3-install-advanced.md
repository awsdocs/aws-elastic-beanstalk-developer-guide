# Manually install the EB CLI<a name="eb-cli3-install-advanced"></a>

## <a name="eb-cli3-install-advanced.intro"></a>

To install the EB CLI, we recommend using the [EB CLI setup scripts](https://github.com/aws/aws-elastic-beanstalk-cli-setup)\. If the setup scripts aren't compatible with your development environment, manually install the EB CLI\.

The primary distribution method for the EB CLI on Linux, macOS, and Windows is `pip`\. This is a package manager for Python that provides an easy way to install, upgrade, and remove Python packages and their dependencies\. For macOS, you can also get the latest version of the EB CLI with `Homebrew`\.

## Compatibility notes<a name="eb-cli3-install-advanced.compat"></a>

The EB CLI is developed in Python and requires Python version 2\.7, 3\.4, or later\. 

**Note**  
Amazon Linux, starting with version 2015\.03, comes with Python 2\.7 and `pip`\.

We recommend using the [EB CLI setup scripts](https://github.com/aws/aws-elastic-beanstalk-cli-setup) to install the EB CLI and its dependencies\. If you manually install the EB CLI, it can be difficult to manage dependency conflicts in your development environment\.

The EB CLI and the [AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/) \(AWS CLI\) share a dependency on the [botocore](https://botocore.amazonaws.com/v1/documentation/api/latest/index.html) Python package\. Due to a breaking change in `botocore`, different versions of these two CLI tools depend on different versions of `botocore`\.

The latest versions of the two CLIs are compatible\. If you need to use an earlier version, see the following table for a compatible version to use\.


|  **EB CLI version**  |  **Compatible AWS CLI version**  | 
| --- | --- | 
|  3\.14\.5 or earlier  |  1\.16\.9 or earlier  | 
|  3\.14\.6 or later  |  1\.16\.11 or later  | 

## Install the EB CLI<a name="eb-cli3-install-advanced.install"></a>

If you already have `pip` and a supported version of Python, use the following procedure to install the EB CLI\.

If you don't have Python and `pip`, use the procedure for the operating system you're using\. 
+ [Install Python, pip, and the EB CLI on Linux ](eb-cli3-install-linux.md)
+ [Install the EB CLI on macOS](eb-cli3-install-osx.md)
+ [Install Python, pip, and the EB CLI on Windows](eb-cli3-install-windows.md)

**To install the EB CLI**

1. Run the following command\.

   ```
   $ pip install awsebcli --upgrade --user
   ```

   The `--upgrade` option tells `pip` to upgrade any requirements that are already installed\. The `--user` option tells `pip` to install the program to a subdirectory of your user directory to avoid modifying libraries that your operating system uses\.
**Note**  
If you encounter issues when you try to install the EB CLI with `pip`, you can [install the EB CLI in a virtual environment](eb-cli3-install-virtualenv.md) to isolate the tool and its dependencies, or use a different version of Python than you normally do\.

1. Add the path to the executable file to your `PATH` variable:
   + On Linux and macOS:

     **Linux** – `~/.local/bin`

     **macOS** – `~/Library/Python/3.7/bin`

     To modify your `PATH` variable \(Linux, Unix, or macOS \):

     1. Find your shell's profile script in your user folder\. If you are not sure which shell you have, run `echo $SHELL`\.

        ```
        $ ls -a ~
        .  ..  .bash_logout  .bash_profile  .bashrc  Desktop  Documents  Downloads
        ```
        + **Bash** – `.bash_profile`, `.profile`, or `.bash_login`\.
        + **Zsh** – `.zshrc`
        + **Tcsh** – `.tcshrc`, `.cshrc` or `.login`\.

     1. Add an export command to your profile script\. The following example adds the path represented by *LOCAL\_PATH* to the current `PATH` variable\.

        ```
        export PATH=LOCAL_PATH:$PATH
        ```

     1. Load the profile script described in the first step into your current session\. The following example loads the profile script represented by *PROFILE\_SCRIPT*\. 

        ```
        $ source ~/PROFILE_SCRIPT
        ```
   + On Windows:

     **Python 3\.7** – `%USERPROFILE%\AppData\Roaming\Python\Python37\Scripts`

     **Python earlier versions** – `%USERPROFILE%\AppData\Roaming\Python\Scripts`

     To modify your `PATH` variable \(Windows\):

     1. Press the Windows key, and then enter **environment variables**\.

     1. Choose **Edit environment variables for your account**\.

     1. Choose **PATH**, and then choose **Edit**\.

     1. Add paths to the **Variable value** field, separated by semicolons\. For example: `C:\item1\path;C:\item2\path`

     1. Choose **OK** twice to apply the new settings\.

     1. Close any running Command Prompt windows, and then reopen a Command Prompt window\.

1. Verify that the EB CLI installed correctly by running eb \-\-version\.

   ```
   $ eb --version
   EB CLI 3.14.8 (Python 3.7)
   ```

The EB CLI is updated regularly to add functionality that supports [the latest Elastic Beanstalk features](https://docs.aws.amazon.com/elasticbeanstalk/latest/relnotes/)\. To update to the latest version of the EB CLI, run the installation command again\.

```
$ pip install awsebcli --upgrade --user
```

If you need to uninstall the EB CLI, use `pip uninstall`\.

```
$ pip uninstall awsebcli
```