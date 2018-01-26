# Install Python, pip, and the EB CLI on Linux<a name="eb-cli3-install-linux"></a>

The EB CLI requires Python 2\.7 or 3\.4\+\. If your distribution didn't come with Python, or came with an older version, install Python before installing `pip` and the EB CLI\. 

**To install Python 3\.4 on Linux**

1. Check to see if Python is already installed\.

   ```
   $ python --version
   ```
**Note**  
If your Linux distribution came with Python, you might need to install the Python developer package to get the headers and libraries required to compile extensions and install the EB CLI\. Install the developer package \(typically named python\-dev or python\-devel\) using your package manager\.

1. If Python 2\.7 or later is not installed, install Python 3\.4 with your distribution's package manager\. The command and package name varies:

   + On Debian derivatives such as Ubuntu, use `APT`\.

     ```
     $ sudo apt-get install python3.4
     ```

   + On Red Hat and derivatives, use `yum`\.

     ```
     $ sudo yum install python34
     ```

   + On SUSE and derivatives, use `zypper`\.

     ```
     $ sudo zypper install python3-3.4.1
     ```

1. Open a command prompt or shell and run the following command to verify that Python installed correctly\.

   ```
   $ python3 --version
   Python 3.4.3
   ```

Install `pip` by using the script provided by the Python Packaging Authority, and then install the EB CLI\.

**To install pip and the EB CLI**

1. Download the installation script from [pypa\.io](https://www.pypa.io/):

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

   Invoking Python version 3 directly by using the `python3` command instead of `python` ensures that `pip` is installed in the proper location, even if an older system version of Python is present on your system\.

1. Add the executable path, `~/.local/bin`, to your PATH variable\.

   To modify your PATH variable \(Linux, macOS, or Unix\):

   1. Find your shell's profile script in your user folder\. If you are not sure which shell you have, run `echo $SHELL`\.

      ```
      $ ls -a ~
      .  ..  .bash_logout  .bash_profile  .bashrc  Desktop  Documents  Downloads
      ```

      + **Bash** – `.bash_profile`, `.profile`, or `.bash_login`\.

      + **Zsh** – `.zshrc`

      + **Tcsh** – `.tcshrc`, `.cshrc` or `.login`\.

   1. Add an export command to your profile script\. The following example adds the path represented by *LOCAL\_PATH* to the current PATH variable\.

      ```
      export PATH=LOCAL_PATH:$PATH
      ```

   1. Load the profile script described in the first step into your current session\. The following example loads the profile script represented by *PROFILE\_SCRIPT* into your current session\.

      ```
      $ source ~/PROFILE_SCRIPT
      ```

1. Verify that `pip` is installed correctly\.

   ```
   $ pip --version
   pip 8.1.2 from ~/.local/lib/python3.4/site-packages (python 3.4)
   ```

1. Finally, use `pip` to install the EB CLI\.

   ```
   $ pip install awsebcli --upgrade --user
   Collecting awsebcli
     Downloading awsebcli-3.7.8.tar.gz (176kB)
   Collecting pyyaml>=3.11 (from awsebcli)
     Downloading PyYAML-3.12.tar.gz (253kB)
   Collecting botocore>=1.0.1 (from awsebcli)
     Downloading botocore-1.4.53-py2.py3-none-any.whl (2.6MB)s
   Collecting cement==2.8.2 (from awsebcli)
     Downloading cement-2.8.2.tar.gz (165kB)
   Collecting colorama==0.3.7 (from awsebcli)
     Downloading colorama-0.3.7-py2.py3-none-any.whl
   Collecting pathspec==0.3.4 (from awsebcli)
     Downloading pathspec-0.3.4.tar.gz
   Requirement already satisfied (use --upgrade to upgrade): setuptools>=20.0 in ./.local/lib/python3.4/site-packages (from awsebcli)
   Collecting docopt<0.7,>=0.6.1 (from awsebcli)
     Downloading docopt-0.6.2.tar.gz
   Collecting requests<=2.9.1,>=2.6.1 (from awsebcli)
     Downloading requests-2.9.1-py2.py3-none-any.whl (501kB)
   Collecting texttable<0.9,>=0.8.1 (from awsebcli)
     Downloading texttable-0.8.4.tar.gz
   Collecting websocket-client<1.0,>=0.11.0 (from awsebcli)
     Downloading websocket_client-0.37.0.tar.gz (194kB)
   Collecting docker-py<=1.7.2,>=1.1.0 (from awsebcli)
     Downloading docker-py-1.7.2.tar.gz (68kB)
   Collecting dockerpty<=0.4.1,>=0.3.2 (from awsebcli)
     Downloading dockerpty-0.4.1.tar.gz
   Collecting semantic_version==2.5.0 (from awsebcli)
     Downloading semantic_version-2.5.0-py3-none-any.whl
   Collecting blessed==1.9.5 (from awsebcli)
     Downloading blessed-1.9.5-py2.py3-none-any.whl (77kB)
   Collecting docutils>=0.10 (from botocore>=1.0.1->awsebcli)
     Downloading docutils-0.12-py3-none-any.whl (508kB)
   Collecting python-dateutil<3.0.0,>=2.1 (from botocore>=1.0.1->awsebcli)
     Downloading python_dateutil-2.5.3-py2.py3-none-any.whl (201kB)
   Collecting jmespath<1.0.0,>=0.7.1 (from botocore>=1.0.1->awsebcli)
     Downloading jmespath-0.9.0-py2.py3-none-any.whl
   Collecting six (from websocket-client<1.0,>=0.11.0->awsebcli)
     Downloading six-1.10.0-py2.py3-none-any.whl
   Collecting wcwidth>=0.1.0 (from blessed==1.9.5->awsebcli)
     Downloading wcwidth-0.1.7-py2.py3-none-any.whl
   Building wheels for collected packages: awsebcli, pyyaml, cement, pathspec, docopt, texttable, websocket-client, docker-py, dockerpty
     Running setup.py bdist_wheel for awsebcli ... done
     Running setup.py bdist_wheel for pyyaml ... done
     Running setup.py bdist_wheel for cement ... done
     Running setup.py bdist_wheel for pathspec ... done
     Running setup.py bdist_wheel for docopt ... done
     Running setup.py bdist_wheel for texttable ... done
     Running setup.py bdist_wheel for websocket-client ... done
     Running setup.py bdist_wheel for docker-py ... done
     Running setup.py bdist_wheel for dockerpty ... done
   Successfully built awsebcli pyyaml cement pathspec docopt texttable websocket-client docker-py dockerpty
   Installing collected packages: pyyaml, docutils, six, python-dateutil, jmespath, botocore, cement, colorama, pathspec, docopt, requests, texttable, websocket-client, docker-py, dockerpty, semantic-version, wcwidth, blessed, awsebcli
   Successfully installed awsebcli-3.7.8 blessed-1.9.5 botocore-1.4.53 cement-2.8.2 colorama-0.3.7 docker-py-1.7.2 dockerpty-0.4.1 docopt-0.6.2 docutils-0.12 jmespath-0.9.0 pathspec-0.3.4 python-dateutil-2.5.3 pyyaml-3.12 requests-2.9.1 semantic-version-2.5.0 six-1.10.0 texttable-0.8.4 wcwidth-0.1.7 websocket-client-0.37.0
   ```

1. Verify that the EB CLI installed correctly\.

   ```
   $ eb --version
   EB CLI 3.7.8 (Python 3.4.3)
   ```

To upgrade to the latest version, run the installation command again\.

```
$ pip install awsebcli --upgrade --user
```