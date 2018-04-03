# Deploying a Flask Application to AWS Elastic Beanstalk<a name="create-deploy-python-flask"></a>

This tutorial walks through the deployment of a simple [Flask](http://flask.pocoo.org/) website to an Elastic Beanstalk environment running Python 2\.7 or newer\. The tutorial uses the EB CLI as a deployment mechanism, but you can also use the AWS Management Console to deploy a ZIP file containing your project's contents\. The EB CLI is an interactive command line interface written in Python that uses the Python SDK for AWS \(boto\)\.

**Topics**
+ [Prerequisites](#python-flask-prereq)
+ [Set Up a Python Virtual Environment with Flask](#python-flask-setup-venv)
+ [Create a Flask Application](#python-flask-create-app)
+ [Configure Your Flask Application for Elastic Beanstalk](#configure-your-flask-application-for-eb)
+ [Deploy Your Site With the EB CLI](#python-flask-deploy)
+ [Clean Up and Next Steps](#python-flask-more-info)

## Prerequisites<a name="python-flask-prereq"></a>

To use any Amazon Web Service \(AWS\), including Elastic Beanstalk, you need to have an AWS account and credentials\. To learn more and to sign up, visit [https://aws\.amazon\.com/](https://aws.amazon.com/)\.

To follow this tutorial, you should have all of the [Common Prerequisites](create-deploy-python-common-steps.md) for Python installed, including the following packages:
+ Python 2\.7 or newer
+ pip
+ virtualenv
+ awsebcli

The [Flask](http://flask.pocoo.org/) framework will be installed as part of the tutorial\.

**Note**  
Creating environments with the EB CLI requires a [service role](concepts-roles-service.md)\. You can create a service role by creating an environment in the Elastic Beanstalk console\. If you don't have a service role, the EB CLI attempts to create one when you run `eb create`\.

## Set Up a Python Virtual Environment with Flask<a name="python-flask-setup-venv"></a>

Create a virtual environment with `virtualenv` and use it to install Flask and its dependencies\. By using a virtual environment, you can discern exactly which packages are needed by your application so that the required packages are installed on the EC2 instances that are running your application\.

**To set up your virtual environment**

1. Create a virtual environment named `eb-virt`:

   ```
   ~$ virtualenv ~/eb-virt
   ```

1. Activate the virtual environment:

   ```
   ~$ source ~/eb-virt/bin/activate
   (eb-virt) ~$
   ```

   You will see `(eb-virt)` prepended to your command prompt, indicating that you're in a virtual environment\.

1. Use *pip* to install Flask by typing:

   ```
   (eb-virt)~$ pip install flask==0.10.1
   ```

1. To verify that Flask has been installed, type:

   ```
   (eb-virt)~$ pip freeze
   Flask==0.10.1
   itsdangerous==0.24
   Jinja2==2.7.3
   MarkupSafe==0.23
   Werkzeug==0.10.1
   ```

   This command lists all of the packages installed in your virtual environment\. Later you will use the output of this command to configure your project for use with Elastic Beanstalk\.

## Create a Flask Application<a name="python-flask-create-app"></a>

Next, create an application that you'll deploy using Elastic Beanstalk\. We'll create a "Hello World" RESTful web service\.

**To create the Hello World Flask application**

1. Activate your virtual environment:

   ```
   ~$ source ~/eb-virt/bin/activate
   ```

1. Create a directory for your project named `eb-flask`:

   ```
   (eb-virt) ~$ mkdir eb-flask
   ```

   ```
   (eb-virt) ~$ cd eb-flask
   ```

1. Create a new text file in this directory named `application.py` with the following contents:  
**Example `~/eb-flask/application.py`**  

   ```
   from flask import Flask
   
   # print a nice greeting.
   def say_hello(username = "World"):
       return '<p>Hello %s!</p>\n' % username
   
   # some bits of text for the page.
   header_text = '''
       <html>\n<head> <title>EB Flask Test</title> </head>\n<body>'''
   instructions = '''
       <p><em>Hint</em>: This is a RESTful web service! Append a username
       to the URL (for example: <code>/Thelonious</code>) to say hello to
       someone specific.</p>\n'''
   home_link = '<p><a href="/">Back</a></p>\n'
   footer_text = '</body>\n</html>'
   
   # EB looks for an 'application' callable by default.
   application = Flask(__name__)
   
   # add a rule for the index page.
   application.add_url_rule('/', 'index', (lambda: header_text +
       say_hello() + instructions + footer_text))
   
   # add a rule when the page is accessed with a name appended to the site
   # URL.
   application.add_url_rule('/<username>', 'hello', (lambda username:
       header_text + say_hello(username) + home_link + footer_text))
   
   # run the app.
   if __name__ == "__main__":
       # Setting debug to True enables debug output. This line should be
       # removed before deploying a production app.
       application.debug = True
       application.run()
   ```

   This example prints a customized greeting that varies based on the path used to access the service\.
**Note**  
By adding `application.debug = True` before running the application, debug output is enabled in case something goes wrong\. It's a good practice for development, but you should remove debug statements in production code, since debug output can reveal internal aspects of your application\.

   Using `application.py` as the filename and providing a callable `application` object \(the Flask object, in this case\) allows Elastic Beanstalk to easily find your application's code\.

1. Run `application.py` with Python:

   ```
   (eb-virt) ~/eb-flask$ python application.py
   ```

   Flask will start a web server and display the URL to access your application with\. For example:

   ```
   * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
   * Restarting with stat
   ```

1. Open the URL in your web browser\. You should see the application running, showing the index page:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/eb_flask_test_local.png)

1. Check the server log to see the output from your request\. You can stop the web server and return to your virtual environment by typing **Ctrl\-C**\.

If you got debug output instead, fix the errors and make sure the application is running locally before configuring it for Elastic Beanstalk\.

## Configure Your Flask Application for Elastic Beanstalk<a name="configure-your-flask-application-for-eb"></a>

With your application running locally, you're now ready to configure it to deploy with Elastic Beanstalk\.

**To configure your site for Elastic Beanstalk**

1. Activate your virtual environment:

   ```
   ~$ source ~/eb-virt/bin/activate
   ```

1. Run `pip freeze` and save the output to a file named `requirements.txt`:

   ```
   (eb-virt) ~/eb-flask$ pip freeze > requirements.txt
   ```

   Elastic Beanstalk uses to `requirements.txt` to determine which package to install on the EC2 instances that run your application\.

1. Deactivate your virtual environment by with the `deactivate` command:

   ```
   (eb-virt) ~/eb-flask$ deactivate
   ```

   Reactivate your virtual environment whenever you need to add additional packages to your application or run your application locally\.

## Deploy Your Site With the EB CLI<a name="python-flask-deploy"></a>

You've added everything you need to deploy your application on Elastic Beanstalk\. Your project directory should now look like this:

```
~/eb-flask/
|-- application.py
`-- requirements.txt
```

Next, you'll create your application environment and deploy your configured application with Elastic Beanstalk\.

**To create an environment and deploy your Flask application**

1. Initialize your EB CLI repository with the `eb init` command:

   ```
   ~/eb-flask$ eb init -p python2.7 flask-tutorial
   Application flask-tutorial has been created.
   ```

   This command creates a new application named `flask-tutorial` and configures your local repository to create environments with the latest Python 2\.7 platform configuration\.

1. \(optional\) Run `eb init` again to configure a default keypair so that you can connect to the EC2 instance running your application with SSH:

   ```
   ~/eb-flask$ eb init
   Do you want to set up SSH for your instances?
   (y/n): y
   Select a keypair.
   1) my-keypair
   2) [ Create new KeyPair ]
   ```

   Select a key pair if you have one already, or follow the prompts to create a new one\. If you don't see the prompt or need to change your settings later, run `eb init -i`\.

1. Create an environment and deploy your application to it with `eb create`:

   ```
   ~/eb-flask$ eb create flask-env
   ```
**Note**  
If you see a "service role required" error message, run `eb create` interactively \(without specifying an environment name\) and the EB CLI creates the role for you\.

   This command creates a load balanced Elastic Beanstalk environment named `flask-env`\. Creating an environment takes about 5 minutes\. As Elastic Beanstalk creates the resources necessary to run your application, it outputs informational messages that the EB CLI relays to your terminal\.

1. When the environment creation process completes, open your web site with `eb open`:

   ```
   ~/eb-flask$ eb open
   ```

   This will open a browser window using the domain name created for your application\. You should see the same Flask website that you created and tested locally\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/eb_flask_deployed.png)

If you don't see your application running, or get an error message, see [Troubleshooting Deployments](troubleshooting-deployments.md) for help with how to determine the cause of the error\.

If you *do* see your application running, then congratulations, you've deployed your first Flask application with Elastic Beanstalk\!

## Clean Up and Next Steps<a name="python-flask-more-info"></a>

To save instance hours and other AWS resources between development sessions, terminate your Elastic Beanstalk environment with `eb terminate`:

```
~/eb-flask$ eb terminate flask-env
```

This command terminates the environment and all of the AWS resources that run within it\. It does not delete the application, however, so you can always create more environments with the same configuration by running `eb create` again\. For more information on EB CLI commands, see [Managing Elastic Beanstalk Environments with the EB CLI](eb-cli3-getting-started.md)\. 

If you are done with the sample application, you can also remove the project folder and virtual environment:

```
~$ rm -rf ~/eb-virt
~$ rm -rf ~/eb-flask
```

For more information about Flask, including an in\-depth tutorial, visit [the official documentation](http://flask.pocoo.org/docs/0.10/)\.

If you'd like to try out another Python web framework, check out [Deploying a Django Application to Elastic Beanstalk](create-deploy-python-django.md)\.