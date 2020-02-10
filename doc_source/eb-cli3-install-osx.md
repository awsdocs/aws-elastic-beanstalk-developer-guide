# Install the EB CLI on macOS<a name="eb-cli3-install-osx"></a>

If you use the `Homebrew` package manager, you can install the EB CLI by using the `brew` command\. You can also install Python and `pip`, and then use `pip` to install the EB CLI\.

## Install the EB CLI with homebrew<a name="eb-cli3-install-osx-homebrew"></a>

The latest version of the EB CLI is typically available from `Homebrew` a couple of days after it appears in `pip`\.

**To install the EB CLI with `Homebrew`**

1. Ensure you have the latest version of `Homebrew`\.

   ```
   $ brew update
   ```

1. Run `brew install awsebcli`\.

   ```
   $ brew install awsebcli
   ```

1. Verify that the EB CLI is installed correctly\.

   ```
   $ eb --version
   EB CLI 3.14.8 (Python 3.7)
   ```

## Install Python, pip, and the EB CLI on macOS<a name="eb-cli3-install-osx-pip"></a>

You can install the latest version of Python and `pip` and then use them to install the EB CLI\.

**To install the EB CLI on macOS**

1. Download and install Python from the [downloads page](https://www.python.org/downloads/release/python) of [Python\.org](https://www.python.org)\. We use version 3\.7 to demonstrate\.
**Note**  
The EB CLI requires Python 2 version 2\.7, or Python 3 version in the range of 3\.4 to 3\.7\.

1. Install `pip` with the script that the Python Packaging Authority provides\.

   ```
   $ curl -O https://bootstrap.pypa.io/get-pip.py
   $ python3 get-pip.py --user
   ```

1. Use `pip` to install the EB CLI\.

   ```
   $ pip3 install awsebcli --upgrade --user
   ```

1. Add the executable path, `~/Library/Python/3.7/bin`, to your `PATH` variable\.

   To modify your `PATH` variable \(Linux, Unix, or macOS\):

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

1. Verify that the EB CLI is installed correctly\.

   ```
   $ eb --version
   EB CLI 3.14.8 (Python 3.7)
   ```

To upgrade to the latest version, run the installation command again\.

```
$ pip3 install awsebcli --upgrade --user
```