# Configuring the WSGI server with a Procfile<a name="python-configuration-procfile"></a>

**Important**  
Amazon Linux 2 platform versions are fundamentally different than Amazon Linux AMI platform versions \(preceding Amazon Linux 2\)\. These different platform generations are incompatible in several ways\. If you are migrating to an Amazon Linux 2 platform version, be sure to read the information in [Migrating your Elastic Beanstalk Linux application to Amazon Linux 2](using-features.migration-al.md)\.

You can add a `Procfile` to your source bundle to specify and configure the WSGI server for your application\. The following example uses a `Procfile` to specify uWSGI as the server and configure it\.

**Example Procfile**  

```
web: uwsgi --http :8000 --wsgi-file application.py --master --processes 4 --threads 2
```

The following example uses a `Procfile` to configure Gunicorn, the default WSGI server\.

**Example Procfile**  

```
web: gunicorn --bind :8000 --workers 3 --threads 2 project.wsgi:application
```

**Notes**  
If you configure any WSGI server other than Gunicorn, be sure to also specify it as a dependency of your application, so that it is installed on your environment instances\. For details about dependency specification, see [Specifying dependencies using a requirements file](python-configuration-requirements.md)\.
The default port for the WSGI server is 8000\. If you specify a different port number in your `Procfile` command, set the `PORT` [environment property](environments-cfg-softwaresettings.md) to this port number too\.

When you use a `Procfile`, it overrides `aws:elasticbeanstalk:container:python` namespace options that you set using configuration files\.

For details about `Procfile` usage, expand the *Buildfile and Procfile* section in [Extending Elastic Beanstalk Linux platforms](platforms-linux-extend.md)\.