# Setting up your Python development environment<a name="python-development-environment"></a>

Set up a Python development environment to test your application locally prior to deploying it to AWS Elastic Beanstalk\. This topic outlines development environment setup steps and links to installation pages for useful tools\.

To follow the procedures in this guide, you will need a command line terminal or shell to run commands\. Commands are shown in listings preceded by a prompt symbol \($\) and the name of the current directory, when appropriate\.

```
~/eb-project$ this is a command
this is output
```

On Linux and macOS, use your preferred shell and package manager\. On Windows 10, you can [install the Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/install-win10) to get a Windows\-integrated version of Ubuntu and Bash\.

For common setup steps and tools that apply to all languages, see [Configuring your development machine for use with Elastic Beanstalk](chapter-devenv.md)\.

**Topics**
+ [Installing Python and pip](#python-common-prereq)
+ [Using a virtual environment](#python-common-setup-venv)
+ [Configuring a Python project for Elastic Beanstalk](#python-common-configuring)

## Installing Python and pip<a name="python-common-prereq"></a>

For all Python applications that you'll deploy with Elastic Beanstalk, these prerequisites are common:

1. A Python version matching the Elastic Beanstalk Python platform version your application will use\.

1. The `pip` utility, matching your Python version\. This is used to install and list dependencies for your project, so that Elastic Beanstalk knows how to set up your application's environment\.

1. The `virtualenv` package\. This is used to create an environment used to develop and test your application, so that the environment can be replicated by Elastic Beanstalk without installing extra packages that aren't needed by your application\.

1. The `awsebcli` package\. This is used to initialize your application with the files necessary for deploying with Elastic Beanstalk\.

1. A working `ssh` installation\. This is used to connect with your running instances when you need to examine or debug a deployment\.

For instructions on installing Python, pip, and the EB CLI, see [Install the EB CLI](eb-cli3-install.md)\.

## Using a virtual environment<a name="python-common-setup-venv"></a>

Once you have the prerequisites installed, set up a virtual environment with `virtualenv` to install your application's dependencies\. By using a virtual environment, you can discern exactly which packages are needed by your application so that the required packages are installed on the EC2 instances that are running your application\.

**To set up a virtual environment**

1. Open a command\-line window and type:

   ```
   $ virtualenv /tmp/eb_python_app
   ```

   Replace *eb\_python\_app* with a name that makes sense for your application \(using your application's name is a good idea\)\. The `virtualenv` command creates a virtual environment for you in the specified directory and prints the results of its actions:

   ```
   Running virtualenv with interpreter /usr/bin/python
   New python executable in /tmp/eb_python_app/bin/python3.7
   Also creating executable in /tmp/eb_python_app/bin/python
   Installing setuptools, pip...done.
   ```

1. Once your virtual environment is ready, start it by running the `activate` script located in the environment's `bin` directory\. For example, to start the eb\_python\_app environment created in the previous step, you would type:

   ```
   $ source /tmp/eb_python_app/bin/activate
   ```

   The virtual environment prints its name \(for example: `(eb_python_app)`\) at the beginning of each command prompt, reminding you that you're in a virtual Python environment\.

1. To stop using your virtual environment and go back to the system’s default Python interpreter with all its installed libraries, run the `deactivate` command\.

   ```
   (eb_python_app) $ deactivate
   ```

**Note**  
Once created, you can restart the virtual environment at any time by running its `activate` script again\.

## Configuring a Python project for Elastic Beanstalk<a name="python-common-configuring"></a>

You can use the Elastic Beanstalk CLI to prepare your Python applications for deployment with Elastic Beanstalk\.

**To configure a Python application for deployment with Elastic Beanstalk**

1. From within your [virtual environment](#python-common-setup-venv), return to the top of your project's directory tree \(`python_eb_app`\), and type:

   ```
   pip freeze >requirements.txt
   ```

   This command copies the names and versions of the packages that are installed in your virtual environment to requirements\.txt, For example, if the *PyYAML* package, version *3\.11* is installed in your virtual environment, the file will contain the line:

   ```
   PyYAML==3.11
   ```

   This allows Elastic Beanstalk to replicate your application's Python environment using the same packages and same versions that you used to develop and test your application\.

1. Configure the EB CLI repository with the eb init command\. Follow the prompts to choose a region, platform and other options\. For detailed instructions, see [Managing Elastic Beanstalk environments with the EB CLI](eb-cli3-getting-started.md)\.

By default, Elastic Beanstalk looks for a file called `application.py` to start your application\. If this doesn't exist in the Python project that you've created, some adjustment of your application's environment is necessary\. You will also need to set environment variables so that your application's modules can be loaded\. See [Using the Elastic Beanstalk Python platform](create-deploy-python-container.md) for more information\.