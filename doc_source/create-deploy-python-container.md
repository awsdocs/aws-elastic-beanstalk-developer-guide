# Using the AWS Elastic Beanstalk Python Platform<a name="create-deploy-python-container"></a>

The AWS Elastic Beanstalk Python platform is a set of environment configurations for Python web applications that can run behind an Apache proxy server with WSGI\. Each configuration corresponds to a version of Python, including Python 2\.6, Python 2\.7, and Python 3\.4\.

Elastic Beanstalk provides configuration options that you can use to customize the software that runs on the EC2 instances in your Elastic Beanstalk environment\. You can configure environment variables needed by your application, enable log rotation to Amazon S3, and map folders in your application source that contain static files to paths served by the proxy server\.

Platform\-specific configuration options are available in the AWS Management Console for modifying the configuration of a running environment\. To avoid losing your environment's configuration when you terminate it, you can use saved configurations to save your settings and later apply them to another environment\.

To save settings in your source code, you can include configuration files\. Settings in configuration files are applied every time you create an environment or deploy your application\. You can also use configuration files to install packages, run scripts, and perform other instance customization operations during deployments\.

For Python packages available from pip, you can also include a requirements file named `requirements.txt` in the root of your application source code\. Elastic Beanstalk installs any packages specified in a requirements file during deployment\.

Settings applied in the AWS Management Console override the same settings in configuration files, if they exist\. This lets you have default settings in configuration files, and override them with environment specific settings in the console\. For more information about precedence, and other methods of changing settings, see \.

## Configuring Your Python Environment<a name="create-deploy-python-container-console"></a>

You can use the AWS Management Console to enable log rotation to Amazon S3, configure variables that your application can read from the environment, and map folders in your application source that contain static files to paths served by the proxy server\. 

**To configure your Python environment in the Elastic Beanstalk console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the management page for your environment\.

1. Choose **Configuration**\.

1. In the **Software Configuration** section, choose the settings icon \( ![\[Edit\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/cog.png)\)\.

### Log Options<a name="create-deploy-python-container.console.logoptions"></a>

The Log Options section has two settings:

+ **Instance profile**– Specifies the instance profile that has permission to access the Amazon S3 bucket associated with your application\.

+ **Enable log file rotation to Amazon S3** – Specifies whether log files for your application's Amazon EC2 instances should be copied to your Amazon S3 bucket associated with your application\.

### Static Files<a name="python-platform-staticfiles"></a>

The **Static Files** section lets you configure the proxy server to serve static assets directly to the user without hitting your Python application\.

By default, the proxy server serves any files in a folder named `static` at the `/static` path\. For example, if your application source contains a file named `logo.png` in a folder named `static`, the proxy server will serve it to users at `subdomain.elasticbeanstalk.com/static/logo.png`\.

You can configure additional mappings by adding entries and choosing **Apply**\. Each entry takes a key and value that map a path in your application to a directory in your source code\.

### Environment Properties<a name="create-deploy-python-custom-container-envprop"></a>

You can use environment properties to provide information to your application and configure environment variables\. For example, you can create an environment property named `CONNECTION_STRING` that specifies a connection string that your application can use to connect to a database\.

Inside the Python environment running in Elastic Beanstalk, these values are accessible using Python's `os.environ` dictionary\. For more information, go to [http://docs\.python\.org/library/os\.html](http://docs.python.org/library/os.html)\.

You can use code that looks similar to the following to access the keys and parameters:

```
import os
endpoint = os.environ['API_ENDPOINT']
```

Environment properties can also provide information to a framework\. For example, you can create a property named `DJANGO_SETTINGS_MODULE` to configure Django to use a specific settings module\. Depending on the environment, the value could be `development.settings`, `production.settings`, etc\.

See  for more information\.

## Python Configuration Namespaces<a name="python-namespaces"></a>

You can use a configuration file to set configuration options and perform other instance configuration tasks during deployments\. Configuration options can be defined by the Elastic Beanstalk service or the platform that you use and are organized into *namespaces*\.

The Python platform defines options in the `aws:elasticbeanstalk:container:python:staticfiles` and `aws:elasticbeanstalk:container:python` namespaces\. The following example configuration file specifies configuration option settings to create an environment property named `DJANGO_SETTINGS_MODULE`, a static files option that maps a directory named `staticimages` to the path `/images`, and additional settings in the `aws:elasticbeanstalk:container:python` namespace\. This namespace contains options that let you specify the location of the WSGI script in your source code, and the number of threads and processes to run in WSGI\.

```
option_settings:
  aws:elasticbeanstalk:application:environment:
    DJANGO_SETTINGS_MODULE: production.settings
  aws:elasticbeanstalk:container:python:staticfiles:
    "/images/": "staticimages/"
  aws:elasticbeanstalk:container:python:
    WSGIPath: ebdjango/wsgi.py
    NumProcesses: 3
    NumThreads: 20
```

Configuration files also support several keys to further modify the software on your environment's instances\. This example uses the packages key to install Memcached with `yum` and container commands to run commands that configure the server during deployment:

```
packages:
  yum:
    libmemcached-devel: '0.31'

container_commands:
  collectstatic:
    command: "django-admin.py collectstatic --noinput"
  01syncdb:
    command: "django-admin.py syncdb --noinput"
    leader_only: true
  02migrate:
    command: "django-admin.py migrate"
    leader_only: true
  03wsgipass:
    command: 'echo "WSGIPassAuthorization On" >> ../wsgi.conf'
  99customize:
    command: "scripts/customize.sh"
```

Elastic Beanstalk provides many configuration options for customizing your environment\. In addition to configuration files, you can also set configuration options using the console, saved configurations, the EB CLI, or the AWS CLI\. See  for more information\.