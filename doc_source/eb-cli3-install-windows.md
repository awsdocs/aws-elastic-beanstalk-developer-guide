# Install Python, pip, and the EB CLI on Windows<a name="eb-cli3-install-windows"></a>

The Python Software Foundation provides installers for Windows that include `pip`\.

**To install Python 3\.7 and `pip` \(Windows\)**

1. Download the Python 3\.7 Windows x86\-64 executable installer from the [downloads page](https://www.python.org/downloads/) of [Python\.org](https://www.python.org)\.

1. Run the installer\.

1. Choose **Add Python 3\.7 to PATH**\.

1. Choose **Install Now**\.

The installer installs Python in your user folder and adds its executable directories to your user path\.

**To install the AWS CLI with `pip` \(Windows\)**

1. From the Start menu, open a Command Prompt window\.

1. Verify that Python and `pip` are both installed correctly by using the following commands\.

   ```
   C:\Windows\System32> python --version
   Python 3.7.3
   C:\Windows\System32> pip --version
   pip 9.0.1 from c:\users\myname\appdata\local\programs\python\python37\lib\site-packages (python 3.7)
   ```

1. Install the EB CLI using `pip`\.

   ```
   C:\Windows\System32> pip install awsebcli --upgrade --user
   ```

1. Add the executable path, `%USERPROFILE%\AppData\roaming\Python\Python37\scripts`, to your `PATH` environment variable\. The location might be different, depending on whether you install Python for one user or all users\.

   To modify your `PATH` variable \(Windows\):

   1. Press the Windows key, and then enter **environment variables**\.

   1. Choose **Edit environment variables for your account**\.

   1. Choose **PATH**, and then choose **Edit**\.

   1. Add paths to the **Variable value** field, separated by semicolons\. For example: `C:\item1\path;C:\item2\path`

   1. Choose **OK** twice to apply the new settings\.

   1. Close any running Command Prompt windows, and then reopen a Command Prompt window\.

1.  Restart a new command shell for the new `PATH` variable to take effect\. 

1. Verify that the EB CLI is installed correctly\.

   ```
   C:\Windows\System32> eb --version
   EB CLI 3.14.8 (Python 3.7)
   ```

To upgrade to the latest version, run the installation command again\.

```
C:\Windows\System32> pip install awsebcli --upgrade --user
```