# Deploying a Django Application to Elastic Beanstalk<a name="create-deploy-python-django"></a>

This tutorial walks through the deployment of a default, auto\-generated [Django](https://www.djangoproject.com/) website to an Elastic Beanstalk environment running Python 3\.6\. The tutorial uses the EB CLI as a deployment mechanism, but you can also use the AWS Management Console to deploy a ZIP file containing your project's contents\. The EB CLI is an interactive command line interface written in Python that uses the Python SDK for AWS \(boto\)\.

**Note**  
This tutorial uses SQLite, a simple database engine included in Python\. The database is deployed with your project files\. For production environments, we recommend that you use Amazon Relational Database Service \(Amazon RDS\), and that you separate it from your environment\. For details, see [Adding an Amazon RDS DB Instance to Your Python Application Environment](create-deploy-python-rds.md)\.

**Topics**
+ [Prerequisites](#python-django-prereq)
+ [Set Up a Python Virtual Environment with Django](#python-django-setup-venv)
+ [Create a Django Project](#python-django-create-app)
+ [Configure Your Django Application for Elastic Beanstalk](#python-django-configure-for-eb)
+ [Deploy Your Site With the EB CLI](#python-django-deploy)
+ [Updating Your Application](#python-django-update-app)
+ [Clean Up and Next Steps](#python-django-stopping)

## Prerequisites<a name="python-django-prereq"></a>

To use any Amazon Web Service \(AWS\), including Elastic Beanstalk, you need to have an AWS account and credentials\. To learn more and to sign up, visit [https://aws\.amazon\.com/](https://aws.amazon.com/)\.

To follow this tutorial, you should have all of the [Common Prerequisites](python-development-environment.md) for Python installed, including the following packages:
+ Python 3\.6
+ pip
+ virtualenv
+ awsebcli

The [Django](https://www.djangoproject.com/) framework will be installed as part of the tutorial\.

**Note**  
Creating environments with the EB CLI requires a [service role](concepts-roles-service.md)\. You can create a service role by creating an environment in the Elastic Beanstalk console\. If you don't have a service role, the EB CLI attempts to create one when you run `eb create`\.

## Set Up a Python Virtual Environment with Django<a name="python-django-setup-venv"></a>

Create a virtual environment with `virtualenv` and use it to install Django and its dependencies\. By using a virtual environment, you can discern exactly which packages are needed by your application so that the required packages are installed on the EC2 instances that are running your application\.

**To set up your virtual environment**

1. Create a virtual environment named `eb-virt`\.

   On Unix\-based systems, such as Linux or OS X, enter the following command:

   ```
   ~$ virtualenv ~/eb-virt
   ```

   On Windows, enter the following command:

   ```
   C:\> virtualenv %HOMEPATH%\eb-virt
   ```

1. Activate the virtual environment\.

   On Unix\-based systems, enter the following command:

   ```
   ~$ source ~/eb-virt/bin/activate
   (eb-virt) ~$
   ```

   On Windows, enter the following command:

   ```
   C:\>%HOMEPATH%\eb-virt\Scripts\activate
   (eb-virt) C:\>
   ```

   You will see `(eb-virt)` prepended to your command prompt, indicating that you're in a virtual environment\.
**Note**  
The remainder of these instructions show the Linux command prompt in your home directory `~$`\. On Windows this is `C:\Users\USERNAME>`, where *USERNAME* is your Windows login name\.

1. Use *pip* to install Django\.

   ```
   (eb-virt)~$ pip install django==2.1.1
   ```
**Note**  
The Django version you install must be compatible with the Python version on the Elastic Beanstalk Python configuration that you choose for deploying your application\. For deployment details, see [Deploy Your Site With the EB CLI](#python-django-deploy) in this topic\.  
For details on current Python platform versions, see [Python](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.python) in the *AWS Elastic Beanstalk Platforms* document\.  
For Django version compatibility with Python, see [What Python version can I use with Django?](https://docs.djangoproject.com/en/2.1/faq/install/#what-python-version-can-i-use-with-django) However, please note that Django 2\.2 is incompatible with the Elastic Beanstalk Python 3\.6 platform\. The latest compatible version is Django 2\.1\.

1. To verify that Django has been installed, type:

   ```
   (eb-virt)~$ pip freeze
   Django==2.1.1
   ...
   ```

   This command lists all of the packages installed in your virtual environment\. Later you will use the output of this command to configure your project for use with Elastic Beanstalk\.

## Create a Django Project<a name="python-django-create-app"></a>

Now you are ready to create a Django project and run it on your machine, using the virtual environment\.

**To generate a Django application**

1. Activate your virtual environment\.

   On Unix\-based systems, enter the following command:

   ```
   ~$ source ~/eb-virt/bin/activate
   (eb-virt) ~$
   ```

   On Windows, enter the following command:

   ```
   C:\>%HOMEPATH%\eb-virt\Scripts\activate
   (eb-virt) C:\>
   ```

   You will see the `(eb-virt)` prefix to your command prompt, indicating that you're in a virtual environment\.
**Note**  
The remainder of these instructions show the Linux command prompt `~$` in your home directory and the Linux home directory `~/`\. On Windows these are `C:\Users\USERNAME>`, where *USERNAME* is your Windows login name\.

1. Use the `django-admin startproject` command to create a new Django project named `ebdjango`:

   ```
   (eb-virt)~$ django-admin startproject ebdjango
   ```

   This command creates a standard Django site named ebdjango with the following directory structure:

   ```
   ~/ebdjango
     |-- ebdjango
     |   |-- __init__.py
     |   |-- settings.py
     |   |-- urls.py
     |   `-- wsgi.py
     `-- manage.py
   ```

1. Run your Django site locally with `manage.py runserver`:

   ```
   (eb-virt) ~$ cd ebdjango
   ```

   ```
   (eb-virt) ~/ebdjango$ python manage.py runserver
   ```

1. Open `http://127.0.0.1:8000/` in a web browser to view the site:  
![\[The welcome page for your Django app running locally\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/eb_django_test_local.png)

1. Check the server log to see the output from your request\. You can stop the web server and return to your virtual environment by typing **Ctrl\+C**:

   ```
   Django version 2.1.1, using settings 'ebdjango.settings'
   Starting development server at http://127.0.0.1:8000/
   Quit the server with CONTROL-C.
   [07/Sep/2018 20:14:09] "GET / HTTP/1.1" 200 16348
   Ctrl+C
   ```

## Configure Your Django Application for Elastic Beanstalk<a name="python-django-configure-for-eb"></a>

Now that you have a Django\-powered site on your local machine, you can configure it for deployment with Elastic Beanstalk\.

By default, Elastic Beanstalk looks for a file called `application.py` to start your application\. Since this doesn't exist in the Django project that you've created, some adjustment of your application's environment is necessary\. You will also need to set environment variables so that your application's modules can be loaded\.

**To configure your site for Elastic Beanstalk**

1. Activate your virtual environment\.

   On Linux\-based systems, enter the following command:

   ```
   ~/ebdjango$ source ~/eb-virt/bin/activate
   ```

   On Windows, enter the following command:

   ```
   C:\Users\USERNAME\ebdjango>%HOMEPATH%\eb-virt\Scripts\activate
   ```

1. Run `pip freeze` and save the output to a file named `requirements.txt`:

   ```
   (eb-virt) ~/ebdjango$ pip freeze > requirements.txt
   ```

   Elastic Beanstalk uses `requirements.txt` to determine which package to install on the EC2 instances that run your application\.

1. Create a new directory, called `.ebextensions`:

   ```
   (eb-virt) ~/ebdjango$ mkdir .ebextensions
   ```

1. Within the `.ebextensions` directory, add a [configuration file](ebextensions.md) named `django.config` with the following text:  
**Example \~/ebdjango/\.ebextensions/django\.config**  

   ```
   option_settings:
     aws:elasticbeanstalk:container:python:
       WSGIPath: ebdjango/wsgi.py
   ```

   This setting, `WSGIPath`, specifies the location of the WSGI script that Elastic Beanstalk uses to start your application\.

1. Deactivate your virtual environment by with the `deactivate` command:

   ```
   (eb-virt) ~/ebdjango$ deactivate
   ```

   Reactivate your virtual environment whenever you need to add additional packages to your application or run your application locally\.

## Deploy Your Site With the EB CLI<a name="python-django-deploy"></a>

You've added everything you need to deploy your application on Elastic Beanstalk\. Your project directory should now look like this:

```
~/ebdjango/
|-- .ebextensions
|   `-- django.config
|-- ebdjango
|   |-- __init__.py
|   |-- settings.py
|   |-- urls.py
|   `-- wsgi.py
|-- db.sqlite3
|-- manage.py
`-- requirements.txt
```

Next, you'll create your application environment and deploy your configured application with Elastic Beanstalk\.

Immediately after deployment, you'll edit Django's configuration to add the domain name that Elastic Beanstalk assigned to your application to Django's `ALLOWED_HOSTS`, and then you'll redeploy your application\. This is a Django security requirement, designed to prevent HTTP Host header attacks\. For details, see [Host header validation](https://docs.djangoproject.com/en/2.1/topics/security/#host-headers-virtual-hosting)\.

**To create an environment and deploy your Django application**

1. Initialize your EB CLI repository with the eb init command:

   ```
   ~/ebdjango$ eb init -p python-3.6 django-tutorial
   Application django-tutorial has been created.
   ```

   This command creates a new application named `django-tutorial` and configures your local repository to create environments with the latest Python 3\.6 platform version\.

1. \(optional\) Run eb init again to configure a default keypair so that you can connect to the EC2 instance running your application with SSH:

   ```
   ~/ebdjango$ eb init
   Do you want to set up SSH for your instances?
   (y/n): y
   Select a keypair.
   1) my-keypair
   2) [ Create new KeyPair ]
   ```

   Select a key pair if you have one already, or follow the prompts to create a new one\. If you don't see the prompt or need to change your settings later, run eb init \-i\.

1. Create an environment and deploy you application to it with eb create:

   ```
   ~/ebdjango$ eb create django-env
   ```
**Note**  
If you see a "service role required" error message, run `eb create` interactively \(without specifying an environment name\) and the EB CLI creates the role for you\.

   This command creates a load balanced Elastic Beanstalk environment named `django-env`\. Creating an environment takes about 5 minutes\. As Elastic Beanstalk creates the resources necessary to run your application, it outputs informational messages that the EB CLI relays to your terminal\.

1. When the environment creation process completes, find the domain name of your new environment by running eb status:

   ```
   ~/ebdjango$ eb status
   Environment details for: django-env
     Application name: django-tutorial
     ...
     CNAME: eb-django-app-dev.elasticbeanstalk.com
     ...
   ```

   Your environment's domain name is the value of the `CNAME` property\.

1. Edit the `settings.py` file in the `ebdjango` directory, locate the `ALLOWED_HOSTS` setting, and then add your application's domain name that you found in the previous step to the setting's value\. If you can't find this setting in the file, add it to a new line\.

   ```
   ...
   ALLOWED_HOSTS = ['eb-django-app-dev.elasticbeanstalk.com']
   ```

1. Save the file, and then deploy your application by running eb deploy\. When you run eb deploy, the EB CLI bundles up the contents of your project directory and deploys it to your environment\.

   ```
   ~/ebdjango$ eb deploy
   ```
**Note**  
If you are using Git with your project, see [Using the EB CLI with Git](eb3-cli-git.md)\.

1. When the environment update process completes, open your web site with eb open:

   ```
   ~/ebdjango$ eb open
   ```

   This will open a browser window using the domain name created for your application\. You should see the same Django website that you created and tested locally\.  
![\[The welcome page for your Django website deployed with Elastic Beanstalk\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/eb_django_deployed.png)

If you don't see your application running, or get an error message, see [Troubleshooting deployments](troubleshooting-deployments.md) for help with how to determine the cause of the error\.

If you *do* see your application running, then congratulations, you've deployed your first Django application with Elastic Beanstalk\!

## Updating Your Application<a name="python-django-update-app"></a>

Now that you have a running application on Elastic Beanstalk, you can update and redeploy your application or its configuration and Elastic Beanstalk will take care of the work of updating your instances and starting your new application version\.

For this example, we'll enable Django's admin console and configure a few other settings\.

### Modify Your Site Settings<a name="python-django-modify-site"></a>

By default, your Django web site uses the UTC time zone to display time\. You can change this by specifying a time zone in `settings.py`\.

**To change your site's time zone**

1. Modify the `TIME_ZONE` setting in `settings.py`  
**Example \~/ebdjango/ebdjango/settings\.py**  

   ```
   ...
   # Internationalization
   LANGUAGE_CODE = 'en-us'
   TIME_ZONE = 'US/Pacific'
   USE_I18N = True
   USE_L10N = True
   USE_TZ = True
   ```

   For a list of time zones, visit [this page](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)\.

1. Deploy the application to your Elastic Beanstalk environment:

   ```
   ~/ebdjango/$ eb deploy
   ```

### Create a Site Administrator<a name="python-django-create-admin"></a>

You can create a site administrator for your Django application to access the admin console directly from the web site\. Administrator login details are stored securely in the local database image included in the default project that Django generates\.

**To create a site administrator**

1. Initialize your Django application's local database:

   ```
   (eb-virt) ~/ebdjango$ python manage.py migrate
   Operations to perform:
     Apply all migrations: admin, auth, contenttypes, sessions
   Running migrations:
     Applying contenttypes.0001_initial... OK
     Applying auth.0001_initial... OK
     Applying admin.0001_initial... OK
     Applying admin.0002_logentry_remove_auto_add... OK
     Applying admin.0003_logentry_add_action_flag_choices... OK
     Applying contenttypes.0002_remove_content_type_name... OK
     Applying auth.0002_alter_permission_name_max_length... OK
     Applying auth.0003_alter_user_email_max_length... OK
     Applying auth.0004_alter_user_username_opts... OK
     Applying auth.0005_alter_user_last_login_null... OK
     Applying auth.0006_require_contenttypes_0002... OK
     Applying auth.0007_alter_validators_add_error_messages... OK
     Applying auth.0008_alter_user_username_max_length... OK
     Applying auth.0009_alter_user_last_name_max_length... OK
     Applying sessions.0001_initial... OK
   ```

1. Run `manage.py createsuperuser` to create an administrator:

   ```
   (eb-virt) ~/ebdjango$ python manage.py createsuperuser
   Username: admin
   Email address: me@mydomain.com
   Password: ********
   Password (again): ********
   Superuser created successfully.
   ```

1. To tell Django where to store static files, define `STATIC_ROOT` in `settings.py`:  
**Example \~/ebdjango/ebdjango/settings\.py**  

   ```
   # Static files (CSS, JavaScript, Images)
   # https://docs.djangoproject.com/en/2.1/howto/static-files/
   STATIC_URL = '/static/'
   STATIC_ROOT = 'static'
   ```

1. Run `manage.py collectstatic` to populate the `static` directory with static assets \(javascript, CSS and images\) for the admin site:

   ```
   (eb-virt) ~/ebdjango$ python manage.py collectstatic
   119 static files copied to ~/ebdjango/static
   ```

1. Deploy your application:

   ```
   ~/ebdjango$ eb deploy
   ```

1. View the admin console by opening the site in your browser, appending `/admin/` to the site URL, such as:

   ```
   http://djang-env.p33kq46sfh.us-west-2.elasticbeanstalk.com/admin/
   ```  
![\[Enter the username and password you created in step 2 to log in to the admin console.\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/eb_django_admin_login.png)

1. Log in with the username and password that you configured in step 2:  
![\[The Django administration console for your Django website deployed with Elastic Beanstalk\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/eb_django_admin_console.png)

You can use a similar procedure of local updating/testing followed by eb deploy\. Elastic Beanstalk takes care of the work of updating your live servers, so you can focus on application development instead of server administration\!

### Add a Database Migration Configuration File<a name="python-django-migrate-site"></a>

You can add commands to your `.ebextensions` script that will be run when your site is updated\. This allows you to automatically generate database migrations\.

**To add a migrate step when your application is deployed**

1. Create a new [configuration file](ebextensions.md) named `db-migrate.config` with the following content:  
**Example \~/ebdjango/\.ebextensions/db\-migrate\.config**  

   ```
   container_commands:
     01_migrate:
       command: "django-admin.py migrate"
       leader_only: true
   option_settings:
     aws:elasticbeanstalk:application:environment:
       DJANGO_SETTINGS_MODULE: ebdjango.settings
   ```

   This configuration file runs the `django-admin.py migrate` command during the deployment process, prior to starting your application\. Because it runs prior to the application starting, you must also configure the `DJANGO_SETTINGS_MODULE` environment variable explicitly \(usually `wsgi.py` takes care of this for you during startup\)\. Specifying `leader_only: true` in the command ensures that it is run only once when you're deploying to multiple instances\.

1. Deploy your application:

   ```
   ~/ebdjango$ eb deploy
   ```

## Clean Up and Next Steps<a name="python-django-stopping"></a>

To save instance hours and other AWS resources between development sessions, terminate your Elastic Beanstalk environment with eb terminate:

```
~/ebdjango$ eb terminate django-env
```

This command terminates the environment and all of the AWS resources that run within it\. It does not delete the application, however, so you can always create more environments with the same configuration by running eb create again\. For more information on EB CLI commands, see [Managing Elastic Beanstalk Environments with the EB CLI](eb-cli3-getting-started.md)\. 

If you are done with the sample application, you can also remove the project folder and virtual environment:

```
~$ rm -rf ~/eb-virt
~$ rm -rf ~/ebdjango
```

For more information about Django, including an in\-depth tutorial, visit [the official documentation](https://docs.djangoproject.com/en/2.1/)\.

If you'd like to try out another Python web framework, check out [Deploying a Flask Application to AWS Elastic Beanstalk](create-deploy-python-flask.md)\.