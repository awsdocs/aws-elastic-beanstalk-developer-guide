# Deploying a Rails Application to Elastic Beanstalk<a name="create_deploy_Ruby_rails"></a>

You can use the Elastic Beanstalk Command Line Interface \(EB CLI\) and Git to deploy a Rails application to Elastic Beanstalk\. This walkthrough shows you how\. You'll also learn how to set up a Rails installation from scratch in case you don't already have a development environment and application\.

**Software Versions**  
 Many of the technologies presented here are under active development\. For the best results, use the same versions of each tool when possible\. The versions used during the development of this tutorial are listed below\.  


****  

|  |  | 
| --- |--- |
| Ubuntu | 14\.04 | 
| RVM | 1\.26\.3 | 
| Ruby | 2\.1\.5p273 | 
| Rails | 4\.1\.8 | 
| Python | 2\.7\.6 | 
| Platform | 64bit Amazon Linux 2015\.09 v2\.0\.6 running Ruby 2\.1 \(Puma\) | 

For the typographic conventions used in this tutorial, see [Document Conventions](http://docs.aws.amazon.com/general/latest/gr/docconventions.html) in the General Reference\.

**Topics**
+ [Rails Development Environment Setup](#create_deploy_Ruby_rails-devenv)
+ [Install the EB CLI](#create_deploy_Ruby_rails-install)
+ [Set Up Your Git Repository](#create_deploy_Ruby_rails_gitinit)
+ [Configure the EB CLI](#create_deploy_Ruby_rails_ebinit)
+ [Create a Service Role and Instance Profile](#ruby-rails-tutorial-servicerole)
+ [Update the Gemfile](#ruby-rails-tutorial-gemfile)
+ [Deploy the Project](#ruby-rails-tutorial-deploy)
+ [Update the Application](#create_deploy_Ruby_rails_update)
+ [Clean Up](#create_deploy_Ruby_delete)

## Rails Development Environment Setup<a name="create_deploy_Ruby_rails-devenv"></a>

 Read this section if you are setting up a Rails development environment from scratch\. If you have a development environment configured with Rails, Git and a working app, you can [skip this section](#create_deploy_Ruby_rails-install)\. 

**Getting an Ubuntu EC2 Instance**  
 The following instructions were developed and tested using an Amazon EC2 instance running Ubuntu 14\.04\. For instructions on configuring and connecting to an EC2 instance using the AWS Management Console, read the [ Getting Started](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html) section of the *Amazon EC2 User Guide for Linux*\.   
 If you don't have access to the AWS Management Console or prefer to use the command line, check out the [AWS CLI User Guide](http://docs.aws.amazon.com/cli/latest/userguide/tutorial-ec2-ubuntu.html) for instructions on installing the AWS CLI and using it to configure security groups, create a key pair, and launch instances with the same credentials that you will use with the EB CLI\. 

### Install Rails<a name="w3ab1c47c11c11b6"></a>

 RVM, a popular version manager for Ruby, provides an option to install RVM, Ruby, and Rails with just a few commands: 

```
~$ gpg --keyserver hkp://keys.gnupg.net --recv-keys D39DC0E3
~$ curl -ssl https://raw.githubusercontent.com/rvm/rvm/master/binscripts/rvm-installer | bash -s stable --rails
~$ source /home/ubuntu/.rvm/scripts/rvm
```

Install nodejs to allow the Rails server to run locally: 

```
~$ sudo apt-get install nodejs
```

**Note**  
For help installing rails on other operating systems, try [http://installrails.com/](http://installrails.com/)\.

### Create a New Rails Project<a name="w3ab1c47c11c11b8"></a>

Use `rails new` with the name of the application to create a new Rails project\.

```
~$ rails new rails-beanstalk
```

Rails creates a directory with the name specified, generates all of the files needed to run a sample project locally, and then runs bundler to install all of the dependencies \(Gems\) defined in the project's Gemfile\. 

### Run the Project Locally<a name="w3ab1c47c11c11c10"></a>

 Test your Rails installation by running the default project locally\. 

```
$ cd rails-beanstalk
rails-beanstalk $ rails server -d
=> Booting WEBrick
=> Rails 4.2.0 application starting in development on http://localhost:3000
=> Run `rails server -h` for more startup options
rails-beanstalk $ curl http://localhost:3000
<!DOCTYPE html>
<html>
  <head>
    <title>Ruby on Rails: Welcome aboard</title>
...
```

**Note**  
Elastic Beanstalk precompiles Rails assets by default\. For Ruby 2\.1 container types, note the following:  
The nginx web server is preconfigured to serve assets from the `/public` and `/public/assets` folders\.
The Puma application requires that you add **gem "puma"** to your `Gemfile` for `bundle exec` to run correctly\.

## Install the EB CLI<a name="create_deploy_Ruby_rails-install"></a>

 In this section you'll install the EB CLI, a few dependencies, and Git\.

**Note**  
 Using Git or another form of revision control is recommended but also entirely optional when using the EB CLI\. Any of the steps in this tutorial that use Git can be skipped\. 

### Install Git, Python Development Libraries and Pip<a name="w3ab1c47c11c13b6"></a>

This tutorial uses Git for revision control and Pip to manage the EB CLI installation\. In your Ubuntu development environment, you can install all of them with the following sequence of commands: 

```
~$ sudo apt-get install git
~$ sudo apt-get install python-dev
~$ curl "https://bootstrap.pypa.io/get-pip.py" -o "get-pip.py"
~$ sudo python get-pip.py
```

**Windows Users**  
 Install [Python 3\.4](https://www.python.org/download/releases/3.4.0/), which includes pip\. 

### Install the EB CLI<a name="w3ab1c47c11c13b8"></a>

With Pip you can install the EB CLI with a single command:

**Linux, macOS, or Unix**

```
~$ sudo pip install awsebcli
```

**Windows**

```
> pip install awsebcli
```

## Set Up Your Git Repository<a name="create_deploy_Ruby_rails_gitinit"></a>

 If your Rails project is already in a local Git repository, continue to [Configure the EB CLI](#create_deploy_Ruby_rails_ebinit)\.

First, initiate the repository\. From within the Rails project directory, type `git init`\.

```
~/rails-beanstalk$ git init
Initialized empty Git repository in /home/ubuntu/rails-beanstalk/.git/
```

 Next, add all of the project's files to the staging area and commit the change\. 

```
~/rails-beanstalk$ git add .
~/rails-beanstalk$ git commit -m "default rails project"
 56 files changed, 896 insertions(+)
 create mode 100644 .gitignore
 create mode 100644 Gemfile
...
```

## Configure the EB CLI<a name="create_deploy_Ruby_rails_ebinit"></a>

With the Git repository configured and all necessary tools installed, configuring the EB CLI project is as simple as running `eb init` from within the project directory and following the prompts\. 

```
~/rails-beanstalk$ eb init
Select a default region
1) us-east-1 : US East (N. Virginia)
2) us-west-1 : US West (N. California)
3) us-west-2 : US West (Oregon)
...
```

The following values work for this tutorial, but feel free to use values that make sense for your requirements\. If you don't have access keys, see [ How Do I Get Security Credentials? ](http://docs.aws.amazon.com/general/latest/gr/getting-aws-sec-creds.html) in the AWS General Reference\.


**Values**  

|  |  | 
| --- |--- |
| Region | Enter \(keep default\) | 
| AWS Access Key ID | Your access key | 
| AWS Secret Access Key | Your secret key | 
| Application Name | Enter \(keep default\) | 
| Using Ruby? | y \(yes\) | 
| Platform Version | Ruby 2\.1 \(Puma\) | 
| Set up SSH? | n \(no\) | 

In addition to configuring the environment for deployment, `eb init` sets up some Git extensions and adds an entry to the `.gitignore` file in the project directory\. Commit the change to `.gitignore` before moving on\. 

```
~/rails-beanstalk$ git commit -am "updated .gitignore"
```

## Create a Service Role and Instance Profile<a name="ruby-rails-tutorial-servicerole"></a>

Newer platform versions require a service role and instance profile\. These roles allow Elastic Beanstalk to monitor the resources in your environment and the instances in your environment to upload log files to Amazon S3\. For more information, see [Service Roles, Instance Profiles, and User Policies](concepts-roles.md)\.

If you don't have a service role and instance profile already, use the Elastic Beanstalk Management Console to create them now\.

**To create a service role and instance profile**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Choose **Create New Application**\.

1. Proceed through the wizard until you reach the **Permissions** page\.

1. Choose **Next** to open the IAM console\.

1. Choose **Allow** to create the roles\.

## Update the Gemfile<a name="ruby-rails-tutorial-gemfile"></a>

Prior to deploying your application to Elastic Beanstalk, we need to make a small change to the default `Gemfile` generated by Rails\. Add Puma to the list of gems to make sure that it is installed properly:

**\~/rails\-beanstalk/Gemfile**

```
source 'https://rubygems.org'
gem 'puma'
gem 'rails', '4.1.8'
gem 'sqlite3'
...
```

Commit the change with `git commit`:

```
~/rails-beanstalk$ git commit -am "Add Puma to Gemfile"
```

## Deploy the Project<a name="ruby-rails-tutorial-deploy"></a>

 Next, create an Elastic Beanstalk environment and deploy your application to it with `eb create`:

```
~/rails-beanstalk$ eb create rails-beanstalk-env
Creating application version archive "app-150219_215138".
Uploading rails-beanstalk/app-150219_215138.zip to S3. This may take a while.
Upload Complete.
Environment details for: rails-beanstalk-env
  Application name: rails-beanstalk
  Region: us-west-2
  Deployed Version: app-150219_215138
  Environment ID: e-pi3immkys7
  Platform: 64bit Amazon Linux 2015.09 v2.0.6 running Ruby 2.1 (Puma)
  Tier: WebServer-Standard
  CNAME: UNKNOWN
  Updated: 2015-02-19 21:51:40.686000+00:00
Printing Status:
INFO: createEnvironment is starting.
...
```

**Note**  
If you see a "service role required" error message, run `eb create` interactively \(without specifying an environment name\) and the EB CLI creates the role for you\.

 With just one command, the EB CLI sets up all of the resources our application needs to run in AWS, including the following: 
+ An Amazon S3 bucket to store environment data
+ A load balancer to distribute traffic to the web server\(s\)
+ A security group to allow incoming web traffic
+ An Auto Scaling group to adjust the number of servers in response to load changes
+ Amazon CloudWatch alarms that notify the Auto Scaling group when load is low or high
+ An Amazon EC2 instance hosting our application

 When the process is complete, the EB CLI outputs the public DNS name of the application server\. Use `eb open` to open the website in the default browser\. In our Ubuntu environment the default browser is a text based browser called W3M\.

```
$ eb open
A really lowlevel plumbing error occured. Please contact your local Maytag(tm) repair man.
```

 This is Puma's way of telling us that something went wrong\. When an error like this occurs, you can check out the logs using the `eb logs` command\. 

```
rails-beanstalk $ eb logs
INFO: requestEnvironmentInfo is starting.
INFO: [Instance: i-8cdc6480] Successfully finished tailing 5 log(s)
================ i-8cdc6480 =================
-------------------------------------
/var/log/eb-version-deployment.log
-------------------------------------
...
```

 The error you're looking for is in the web container log, `/var/log/puma/puma.log`\. 

```
...
-------------------------------------
/var/log/puma/puma.log
-------------------------------------
=== puma startup: 2014-12-15 18:37:51 +0000 ===
=== puma startup: 2014-12-15 18:37:51 +0000 ===
[1982] + Gemfile in context: /var/app/current/Gemfile
[1979] - Worker 0 (pid: 1982) booted, phase: 0
2014-12-15 18:41:42 +0000: Rack app error: #<RuntimeError: Missing `secret_key_base` for 'production' environment, set this value in `config/secrets.yml`>
/opt/rubies/ruby-2.1.4/lib/ruby/gems/2.1.0/gems/railties-4.1.8/lib/rails/application.rb:462:in `validate_secret_key_config!'
/opt/rubies/ruby-2.1.4/lib/ruby/gems/2.1.0/gems/railties-4.1.8/lib/rails/application.rb:195:in `env_config'
...
```

To get the application working, you need to configure a few environment variables\. First is SECRET\_KEY\_BASE, which is referred to by `secrets.yml` in the `config` folder of our project\. 

This variable is used to create keys and should be a secret, as the name suggests\. This is why you don't want it stored in source control where other people might see it\. Set this to any value using `eb setenv`: 

```
rails-beanstalk $ eb setenv SECRET_KEY_BASE=23098520lkjsdlkjfsdf
INFO: Environment update is starting.
INFO: Updating environment rails-beanstalk-env's configuration settings.
INFO: Successfully deployed new configuration to environment.
INFO: Environment update completed successfully.
```

 The EB CLI automatically restarts the web server whenever you update configuration or deploy new code\. Try loading the site again\. 

```
$ eb open
The page you were looking for doesn't exist (404)
```

 A 404 error may not look like much of an improvement, but it shows that the web container is working and couldn't find a route to the page you're looking for\. 

 So what happened to the welcome page you saw earlier? In this case the environment variable you need is RACK\_ENV\. Right now it's set to production, suppressing the display of debug features as well as the Welcome to Rails page\. 

 View the current value of all environment variables using the `eb printenv` command\. 

```
rails-beanstalk $ eb printenv
 Environment Variables:
     AWS_SECRET_KEY = None
     RAILS_SKIP_ASSET_COMPILATION = false
     SECRET_KEY_BASE = 23098520lkjsdlkjfsdf
     RACK_ENV = production
     PARAM5 = None
     PARAM4 = None
     PARAM3 = None
     PARAM2 = None
     PARAM1 = None
     BUNDLE_WITHOUT = test:development
     RAILS_SKIP_MIGRATIONS = false
     AWS_ACCESS_KEY_ID = None
```

The proper way to fix this is to add content and routes to the project\. For the moment, though, we just want to see our project working, so we'll set RACK\_ENV to development\. 

```
rails-beanstalk $ eb setenv RACK_ENV=development
```

The next time you load the site it should succeed\. 

```
$ eb open
Ruby on Rails: Welcome aboard
...
```

 Now that you know it works, you can set RACK\_ENV back to production and see about adding that content\. 

```
rails-beanstalk $ eb setenv RACK_ENV=production
```

## Update the Application<a name="create_deploy_Ruby_rails_update"></a>

Now it's time to add some content to the front page to avoid the 404 error you saw in production mode\. 

 First you'll use `rails generate` to create a controller, route, and view for your welcome page\. 

```
$ rails generate controller WelcomePage welcome
      create  app/controllers/welcome_page_controller.rb
       route  get 'welcome_page/welcome'
      invoke  erb
      create    app/views/welcome_page
      create    app/views/welcome_page/welcome.html.erb
...
```

 This gives you all you need to access the page at *rails\-beanstalk\-env\-kpvmmmqpbr*\.elasticbeanstalk\.com/welcome\_page/welcome\. Before you publish the changes, however, change the content in the view and add a route to make this page appear at the top level of the site\. 

 Use your favorite text editor to edit the content in `app/views/welcome_page/welcome.html.erb`\. Nano and Vim are popular command line editors\. For this example, you'll use `cat` to simply overwrite the content of the existing file\. 

```
rails-beanstalk $ cat > app/views/welcome_page/welcome.html.erb
> <h1>Welcome!</h1>
> <p>This is the front page of my first Rails application on Elastic Beanstalk.</p>
Ctrl+D
```

 Finally, add the following route to `config/routes.rb`: 

```
Rails.application.routes.draw do
  get 'welcome_page/welcome'
  root 'welcome_page#welcome'
end
```

 This tells Rails to route requests to the root of the website to the welcome page controller's welcome method, which renders the content in the welcome view \(`welcome.html.erb`\)\. Now we're ready to commit the changes and update our environment using `eb deploy`\. 

```
rails-beanstalk $ git add .
rails-beanstalk $ git commit -m "welcome page controller, view and route"
rails-beanstalk $ eb deploy
INFO: Environment update is starting.
INFO: Deploying new version to instance(s).
INFO: New application version was deployed to running EC2 instances.
INFO: Environment update completed successfully.
```

The update process is fairly quick\. Read the front page at the command line using Curl or navigate to the type `eb open` to open the site in a web browser to see the results\.

```
$ eb open
Welcome

This is the front page of my first Rails application on Elastic Beanstalk.
```

 Now you're ready to continue work on your Rails site\. Whenever you have new commits to push, use `eb deploy` to update your environment\. 

## Clean Up<a name="create_deploy_Ruby_delete"></a>

If you no longer want to run your application, you can clean up by terminating your environment and deleting your application\.

Use the `terminate` command to terminate your environment and the `delete` command to delete your application\. 

**To terminate your environment and delete the application**
+ From the directory where you created your local repository, run `eb terminate`\.

  ```
  $ eb terminate
  ```

  This process can take a few minutes\. Elastic Beanstalk displays a message once the environment is successfully terminated\. 

 Don't hesitate to terminate an environment to save on resources while you continue to develop your site\. You can always recreate your Beanstalk environment using `eb create`\. 