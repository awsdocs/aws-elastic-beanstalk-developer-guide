# Install Python, pip, and the EB CLI on Linux<a name="eb-cli3-install-linux"></a>

The EB CLI requires Python 2\.7, 3\.4, or later\. If your distribution didn't come with Python, or came with an earlier version, install Python before installing `pip` and the EB CLI\. 

**To install Python 3\.7 on Linux**

1. Determine whether Python is already installed\.

   ```
   $ python --version
   ```
**Note**  
If your Linux distribution came with Python, you might need to install the Python developer package to get the headers and libraries required to compile extensions and install the EB CLI\. Use your package manager to install the developer package \(typically named `python-dev` or `python-devel`\)\.

1. If Python 2\.7 or later isn't installed, install Python 3\.7 using your distribution's package manager\. The command and package name vary:
   + On Debian derivatives, such as Ubuntu, use `APT`\.

     ```
     $ sudo apt-get install python3.7
     ```
   + On Red Hat and derivatives, use `yum`\.

     ```
     $ sudo yum install python37
     ```
   + On SUSE and derivatives, use `zypper`\.

     ```
     $ sudo zypper install python3-3.7
     ```

1. To verify that Python installed correctly, open a terminal or shell and run the following command\.

   ```
   $ python3 --version
   Python 3.7.3
   ```

Install `pip` by using the script provided by the Python Packaging Authority, and then install the EB CLI\.

**To install `pip` and the EB CLI**

1. Download the installation script from [pypa\.io](https://www.pypa.io/)\.

   ```
   $ curl -O https://bootstrap.pypa.io/get-pip.py
   ```

   The script downloads and installs the latest version of `pip` and another required package named `setuptools`\. 

1. Run the script with Python\.

   ```
   $ python3 get-pip.py --user
   Collecting pip
     Downloading pip-8.1.2-py2.py3-none-any.whl (1.2MB)
   Collecting setuptools
     Downloading setuptools-26.1.1-py2.py3-none-any.whl (464kB)
   Collecting wheel
     Downloading wheel-0.29.0-py2.py3-none-any.whl (66kB)
   Installing collected packages: pip, setuptools, wheel
   Successfully installed pip setuptools wheel
   ```

   Invoking Python version 3 directly by using the `python3` command instead of `python` ensures that `pip` is installed in the proper location, even if an earlier version of Python is present on your system\.

1. Add the executable path, `~/.local/bin`, to your `PATH` variable\.

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

1. Verify that `pip` is installed correctly\.

   ```
   $ pip --version
   pip 8.1.2 from ~/.local/lib/python3.7/site-packages (python 3.7)
   ```

1. Use `pip` to install the EB CLI\.

   ```
   $ pip install awsebcli --upgrade --user
   ```

1. Verify that the EB CLI installed correctly\.

   ```
   $ eb --version
   EB CLI 3.14.8 (Python 3.7)
   ```

To upgrade to the latest version, run the installation command again\.

```
$ pip install awsebcli --upgrade --user
```