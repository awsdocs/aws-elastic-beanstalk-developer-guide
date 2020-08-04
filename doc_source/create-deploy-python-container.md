# Using the Elastic Beanstalk Python platform<a name="create-deploy-python-container"></a>

**Important**  
Amazon Linux 2 platform versions are fundamentally different than Amazon Linux AMI platform versions \(preceding Amazon Linux 2\)\. These different platform generations are incompatible in several ways\. If you are migrating to an Amazon Linux 2 platform version, be sure to read the information in [Migrating your Elastic Beanstalk Linux application to Amazon Linux 2](using-features.migration-al.md)\.

The AWS Elastic Beanstalk Python platform is a set of [platform versions](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.python) for Python web applications that can run behind a proxy server with WSGI\. Each platform branch corresponds to a version of Python, such as Python 3\.4\.

Starting with Amazon Linux 2 platform branches, Elastic Beanstalk provides [Gunicorn](https://gunicorn.org/) as the default WSGI server\.

You can add a `Procfile` to your source bundle to specify and configure the WSGI server for your application\. For details, see [Configuring the WSGI server with a Procfile](python-configuration-procfile.md)\.

You can use the `Pipfile` and `Pipfile.lock` files created by Pipenv to specify Python package dependencies and other requirements\. For details about specifying dependencies, see [Specifying dependencies using a requirements file](python-configuration-requirements.md)\.

Elastic Beanstalk provides [configuration options](command-options.md) that you can use to customize the software that runs on the EC2 instances in your Elastic Beanstalk environment\. You can configure environment variables needed by your application, enable log rotation to Amazon S3, and map folders in your application source that contain static files to paths served by the proxy server\.

Configuration options are available in the Elastic Beanstalk console for [modifying the configuration of a running environment](environment-configuration-methods-after.md)\. To avoid losing your environment's configuration when you terminate it, you can use [saved configurations](environment-configuration-savedconfig.md) to save your settings and later apply them to another environment\.

To save settings in your source code, you can include [configuration files](ebextensions.md)\. Settings in configuration files are applied every time you create an environment or deploy your application\. You can also use configuration files to install packages, run scripts, and perform other instance customization operations during deployments\.

Settings applied in the Elastic Beanstalk console override the same settings in configuration files, if they exist\. This lets you have default settings in configuration files, and override them with environment\-specific settings in the console\. For more information about precedence, and other methods of changing settings, see [Configuration options](command-options.md)\.

For Python packages available from `pip`, you can include a requirements file in the root of your application source code\. Elastic Beanstalk installs any dependency packages specified in a requirements file during deployment\. For details, see [Specifying dependencies using a requirements file](python-configuration-requirements.md)\.

For details about the various ways you can extend an Elastic Beanstalk Linux\-based platform, see [Extending Elastic Beanstalk Linux platforms](platforms-linux-extend.md)\.

## Configuring your Python environment<a name="create-deploy-python-container-console"></a>

The Python platform settings let you fine\-tune the behavior of your Amazon EC2 instances\. You can edit the Elastic Beanstalk environment's Amazon EC2 instance configuration using the Elastic Beanstalk console\.

Use the Elastic Beanstalk console to configure Python process settings, enable AWS X\-Ray, enable log rotation to Amazon S3, and configure variables that your application can read from the environment\.

**To configure your Python environment in the Elastic Beanstalk console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Configuration**\.

1. In the **Software** configuration category, choose **Edit**\.

### Python settings<a name="python-console-settings"></a>
+ **Proxy server** – The proxy server to use on your environment instances\. By default, nginx is used\.
+ **WSGI Path** – The name of or path to your main application file\. For example, `application.py`, or `django/wsgi.py`\.
+ **NumProcesses** – The number of processes to run on each application instance\.
+ **NumThreads** – The number of threads to run in each process\.

### AWS X\-Ray settings<a name="python-console-xray"></a>
+ **X\-Ray daemon** – Run the AWS X\-Ray daemon to process trace data from the [AWS X\-Ray SDK for Python](https://docs.aws.amazon.com/xray/latest/devguide/xray-sdk-python.html)\.

### Log options<a name="create-deploy-python-container.console.logoptions"></a>

The Log Options section has two settings:
+ **Instance profile**– Specifies the instance profile that has permission to access the Amazon S3 bucket associated with your application\.
+ **Enable log file rotation to Amazon S3** – Specifies whether log files for your application's Amazon EC2 instances should be copied to the Amazon S3 bucket associated with your application\.

### Static files<a name="python-platform-staticfiles"></a>

To improve performance, the **Static files** section lets you configure the proxy server to serve static files \(for example, HTML or images\) from a set of directories inside your web application\. For each directory, you set the virtual path to directory mapping\. When the proxy server receives a request for a file under the specified path, it serves the file directly instead of routing the request to your application\.

For details about configuring static files using the Elastic Beanstalk console, see [Serving static files](environment-cfg-staticfiles.md)\.

By default, the proxy server in a Python environment serves any files in a folder named `static` at the `/static` path\. For example, if your application source contains a file named `logo.png` in a folder named `static`, the proxy server serves it to users at `subdomain.elasticbeanstalk.com/static/logo.png`\. You can configure additional mappings as explained in this section\.

### Environment properties<a name="create-deploy-python-custom-container-envprop"></a>

You can use environment properties to provide information to your application and configure environment variables\. For example, you can create an environment property named `CONNECTION_STRING` that specifies a connection string that your application can use to connect to a database\.

Inside the Python environment running in Elastic Beanstalk, these values are accessible using Python's `os.environ` dictionary\. For more information, go to [http://docs\.python\.org/library/os\.html](http://docs.python.org/library/os.html)\.

You can use code that looks similar to the following to access the keys and parameters:

```
import os
endpoint = os.environ['API_ENDPOINT']
```

Environment properties can also provide information to a framework\. For example, you can create a property named `DJANGO_SETTINGS_MODULE` to configure Django to use a specific settings module\. Depending on the environment, the value could be `development.settings`, `production.settings`, etc\.

See [Environment properties and other software settings](environments-cfg-softwaresettings.md) for more information\.

## Python configuration namespaces<a name="python-namespaces"></a>

You can use a [configuration file](ebextensions.md) to set configuration options and perform other instance configuration tasks during deployments\. Configuration options can be defined by the Elastic Beanstalk service or the platform that you use and are organized into *namespaces*\.

The Python platform defines options in the `aws:elasticbeanstalk:environment:proxy`, `aws:elasticbeanstalk:environment:proxy:staticfiles`, and `aws:elasticbeanstalk:container:python` namespaces\.

The following example configuration file specifies configuration option settings to create an environment property named `DJANGO_SETTINGS_MODULE`, choose the Apache proxy server, specify two static files options that map a directory named `statichtml` to the path `/html` and a directory named `staticimages` to the path `/images`, and specify additional settings in the `[aws:elasticbeanstalk:container:python](command-options-specific.md#command-options-python)` namespace\. This namespace contains options that let you specify the location of the WSGI script in your source code, and the number of threads and processes to run in WSGI\.

```
option_settings:
  aws:elasticbeanstalk:application:environment:
    DJANGO_SETTINGS_MODULE: production.settings
  aws:elasticbeanstalk:environment:proxy:
    ProxyServer: apache
  aws:elasticbeanstalk:environment:proxy:staticfiles:
    /html: statichtml
    /images: staticimages
  aws:elasticbeanstalk:container:python:
    WSGIPath: ebdjango.wsgi:application
    NumProcesses: 3
    NumThreads: 20
```

**Notes**  
If you're using an Amazon Linux AMI Python platform version \(preceding Amazon Linux 2\), replace the value for `WSGIPath` with `ebdjango/wsgi.py`\. The value in the example works with the Gunicorn WSGI server, which isn't supported on Amazon Linux AMI platform versions\.
In addition, these older platform versions use a different namespace for configuring static files—`aws:elasticbeanstalk:container:python:staticfiles`\. It has the same option names and semantics as the standard static file namespace\.

Configuration files also support several keys to further [modify the software on your environment's instances](customize-containers-ec2.md)\. This example uses the [packages](customize-containers-ec2.md#linux-packages) key to install Memcached with `yum` and [container commands](customize-containers-ec2.md#linux-container-commands) to run commands that configure the server during deployment:

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

Elastic Beanstalk provides many configuration options for customizing your environment\. In addition to configuration files, you can also set configuration options using the console, saved configurations, the EB CLI, or the AWS CLI\. See [Configuration options](command-options.md) for more information\.