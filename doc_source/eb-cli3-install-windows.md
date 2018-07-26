# Install Python, pip, and the EB CLI on Windows<a name="eb-cli3-install-windows"></a>

The Python Software Foundation provides installers for Windows that include `pip`\.

**To install Python 3\.6 and pip \(Windows\)**

1. Download the Python 3\.6 Windows x86\-64 executable installer from the [downloads page](https://www.python.org/downloads/release/python-362/) of [Python\.org](https://www.python.org)\.

1. Run the installer\.

1. Choose **Add Python 3\.6 to PATH**\.

1. Choose **Install Now**\.

The installer installs Python in your user folder and adds its executable directories to your user path\.

**To install the AWS CLI with pip \(Windows\)**

1. Open the Windows Command Processor from the Start menu\.

1. Verify that Python and `pip` are both installed correctly by using the following commands\.

   ```
   C:\Windows\System32> python --version
   Python 3.6.2
   C:\Windows\System32> pip --version
   pip 9.0.1 from c:\users\myname\appdata\local\programs\python\python36\lib\site-packages (python 3.6)
   ```

1. Install the EB CLI using `pip`\.

   ```
   C:\Windows\System32> pip install awsebcli --upgrade --user
   Collecting awsebcli
     Downloading awsebcli-3.2.2.tar.gz (828kB)
   Collecting pyyaml>=3.11 (from awsebcli)
     Downloading PyYAML-3.11.tar.gz (248kB)
   Collecting cement==2.4 (from awsebcli)
     Downloading cement-2.4.0.tar.gz (129kB)
   Collecting python-dateutil<3.0.0,>=2.1 (from awsebcli)
     Downloading python_dateutil-2.4.2-py2.py3-none-any.whl (188kB)
   Collecting jmespath>=0.6.1 (from awsebcli)
     Downloading jmespath-0.6.2.tar.gz
   Collecting six>=1.5 (from python-dateutil<3.0.0,>=2.1->awsebcli)
     Downloading six-1.9.0-py2.py3-none-any.whl
   Installing collected packages: six, jmespath, python-dateutil, cement, pyyaml, awsebcli
     Running setup.py install for jmespath
     Running setup.py install for cement
     Running setup.py install for pyyaml
       checking if libyaml is compilable
       Microsoft Visual C++ 10.0 is required (Unable to find vcvarsall.bat).
       skipping build_ext
     Running setup.py install for awsebcli
       Installing eb-script.py script to C:\Python34\Scripts
       Installing eb.exe script to C:\Python34\Scripts
       Installing eb.exe.manifest script to C:\Python34\Scripts
   Successfully installed awsebcli-3.2.2 cement-2.4.0 jmespath-0.6.2 python-dateutil-2.4.2 pyyaml-3.11 six-1.9.0
   ```

1. Add the executable path, `%USERPROFILE%\AppData\roaming\Python\Python36\scripts`, to your PATH environment variable\. The location may differ depending on whether you install Python for one user or all users\.

   To modify your PATH variable \(Windows\):

   1. Press the Windows key, and then type **environment variables**\.

   1. Choose **Edit environment variables for your account**\.

   1. Choose **PATH**, and then choose **Edit**\.

   1. Add paths to the **Variable value** field, separated by semicolons\. For example: `C:\item1\path;C:\item2\path`

   1. Choose **OK** twice to apply the new settings\.

   1. Close any running command prompts and reopen command prompt\.

1.  Restart a new command shell for the new PATH variable to take effect\. 

1. Verify that the EB CLI is installed correctly\.

   ```
   C:\Windows\System32> eb --version
   EB CLI 3.2.2 (Python 3.4.3)
   ```

To upgrade to the latest version, run the installation command again\.

```
C:\Windows\System32> pip install awsebcli --upgrade --user
```