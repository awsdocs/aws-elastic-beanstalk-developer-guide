# Install the EB CLI in a virtual environment<a name="eb-cli3-install-virtualenv"></a>

You can avoid version requirement conflicts with other `pip` packages by installing the EB CLI in a virtual environment\.

**To install the EB CLI in a virtual environment**

1. Install `virtualenv` with `pip`\.

   ```
   $ pip install --user virtualenv
   ```

1. Create a virtual environment\.

   ```
   $ virtualenv ~/eb-ve
   ```

   To use a Python executable other than the default, use the `-p` option\. 

   ```
   $ virtualenv -p /usr/bin/python3.7 ~/eb-ve
   ```

1. Activate the virtual environment\.

   **Linux, Unix, or macOS**

   ```
   $ source ~/eb-ve/bin/activate
   ```

   **Windows**

   ```
   $ %USERPROFILE%\eb-ve\Scripts\activate
   ```

1. Install the EB CLI\.

   ```
   (eb-ve)~$ pip install awsebcli --upgrade
   ```

1. Verify that the EB CLI is installed correctly\.

   ```
   $ eb --version
   EB CLI 3.14.8 (Python 3.7)
   ```

You can use the `deactivate` command to exit the virtual environment\. Whenever you start a new session, run the activation command again\.

To upgrade to the latest version, run the installation command again\.

```
(eb-ve)~$ pip install awsebcli --upgrade
```