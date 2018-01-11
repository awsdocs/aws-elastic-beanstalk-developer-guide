# Install the EB CLI in a Virtual Environment<a name="eb-cli3-install-virtualenv"></a>

You can avoid requirement version conflicts with other pip packages by installing the EB CLI in a virtual environment\.

**To install the EB CLI in a virtual environment**

1. Install `virtualenv` with pip\.

   ```
   $ pip install --user virtualenv
   ```

1. Create a virtual environment\.

   ```
   $ virtualenv ~/eb-ve
   ```

   You can use the `-p` option to use a Python executable other than the default\.

   ```
   $ virtualenv -p /usr/bin/python3.4 ~/eb-ve
   ```

1. Activate the virtual environment\.

   **Linux, macOS, or Unix**

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
   EB CLI 3.7.8 (Python 3.4.1)
   ```

You can use the `deactivate` command to exit the virtual environment\. Whenever you start a new session, run the activation command again\.

To upgrade to the latest version, run the installation command again:

```
(eb-ve)~$ pip install awsebcli --upgrade
```