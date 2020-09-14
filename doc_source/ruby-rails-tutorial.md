# Deploying a rails application to Elastic Beanstalk<a name="ruby-rails-tutorial"></a>

Rails is an open source, model\-view\-controller \(MVC\) framework for Ruby\. This tutorial walks you through the process of generating a Rails application and deploying it to an AWS Elastic Beanstalk environment\.

**Topics**
+ [Prerequisites](#ruby-rails-tutorial-prereqs)
+ [Launch an Elastic Beanstalk environment](#ruby-rails-tutorial-launch)
+ [Install rails and generate a website](#ruby-rails-tutorial-generate)
+ [Configure rails settings](#ruby-rails-tutorial-configure)
+ [Deploy your application](#ruby-rails-tutorial-deploy)
+ [Cleanup](#ruby-rails-tutorial-cleanup)
+ [Next steps](#ruby-rails-tutorial-nextsteps)

## Prerequisites<a name="ruby-rails-tutorial-prereqs"></a>

### Basic Elastic Beanstalk knowledge<a name="ruby-rails-tutorial-prereqs-basic"></a>

This tutorial assumes you have knowledge of the basic Elastic Beanstalk operations and the Elastic Beanstalk console\. If you haven't already, follow the instructions in [Getting started using Elastic Beanstalk](GettingStarted.md) to launch your first Elastic Beanstalk environment\.

### Command line<a name="ruby-rails-tutorial-prereqs-cmdline"></a>

To follow the procedures in this guide, you will need a command line terminal or shell to run commands\. Commands are shown in listings preceded by a prompt symbol \($\) and the name of the current directory, when appropriate\.

```
~/eb-project$ this is a command
this is output
```

On Linux and macOS, use your preferred shell and package manager\. On Windows 10, you can [install the Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/install-win10) to get a Windows\-integrated version of Ubuntu and Bash\.

### Rails dependencies<a name="ruby-rails-tutorial-prereqs-railsreqs"></a>

The Rails framework has the following dependencies\. Be sure you have all of them installed\.
+ **Ruby 2\.2\.2 or newer** – For installation instructions, see [Setting up your Ruby development environment](ruby-development-environment.md)\.

  In this tutorial we use Ruby 2\.5\.1 and the corresponding Elastic Beanstalk platform version\.
+ **Node\.js** – For installation instructions, see [Installing Node\.js via package manager](https://nodejs.org/en/download/package-manager/)\.
+ **Yarn** – For installation instructions, see [Installation](https://yarnpkg.com/lang/en/docs/install/) on the *Yarn* website\.

## Launch an Elastic Beanstalk environment<a name="ruby-rails-tutorial-launch"></a>

Use the Elastic Beanstalk console to create an Elastic Beanstalk environment\. Choose the **Ruby** platform and accept the default settings and sample code\.

**To launch an environment \(console\)**

1. Open the Elastic Beanstalk console using this preconfigured link: [console\.aws\.amazon\.com/elasticbeanstalk/home\#/newApplication?applicationName=tutorials&environmentType=LoadBalanced](https://console.aws.amazon.com/elasticbeanstalk/home#/newApplication?applicationName=tutorials&environmentType=LoadBalanced)

1. For **Platform**, select the platform and platform branch that match the language used by your application\.

1. For **Application code**, choose **Sample application**\.

1. Choose **Review and launch**\.

1. Review the available options\. Choose the available option you want to use, and when you're ready, choose **Create app**\.

Environment creation takes about 5 minutes and creates the following resources:
+ **EC2 instance** – An Amazon Elastic Compute Cloud \(Amazon EC2\) virtual machine configured to run web apps on the platform that you choose\.

  Each platform runs a specific set of software, configuration files, and scripts to support a specific language version, framework, web container, or combination of these\. Most platforms use either Apache or NGINX as a reverse proxy that sits in front of your web app, forwards requests to it, serves static assets, and generates access and error logs\.
+ **Instance security group** – An Amazon EC2 security group configured to allow inbound traffic on port 80\. This resource lets HTTP traffic from the load balancer reach the EC2 instance running your web app\. By default, traffic isn't allowed on other ports\.
+ **Load balancer** – An Elastic Load Balancing load balancer configured to distribute requests to the instances running your application\. A load balancer also eliminates the need to expose your instances directly to the internet\.
+ **Load balancer security group** – An Amazon EC2 security group configured to allow inbound traffic on port 80\. This resource lets HTTP traffic from the internet reach the load balancer\. By default, traffic isn't allowed on other ports\.
+ **Auto Scaling group** – An Auto Scaling group configured to replace an instance if it is terminated or becomes unavailable\.
+ **Amazon S3 bucket** – A storage location for your source code, logs, and other artifacts that are created when you use Elastic Beanstalk\.
+ **Amazon CloudWatch alarms** – Two CloudWatch alarms that monitor the load on the instances in your environment and that are triggered if the load is too high or too low\. When an alarm is triggered, your Auto Scaling group scales up or down in response\.
+ **AWS CloudFormation stack** – Elastic Beanstalk uses AWS CloudFormation to launch the resources in your environment and propagate configuration changes\. The resources are defined in a template that you can view in the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation)\.
+ **Domain name** – A domain name that routes to your web app in the form **subdomain*\.*region*\.elasticbeanstalk\.com*\.

All of these resources are managed by Elastic Beanstalk\. When you terminate your environment, Elastic Beanstalk terminates all the resources that it contains\.

**Note**  
The Amazon S3 bucket that Elastic Beanstalk creates is shared between environments and is not deleted during environment termination\. For more information, see [Using Elastic Beanstalk with Amazon S3](AWSHowTo.S3.md)\.

## Install rails and generate a website<a name="ruby-rails-tutorial-generate"></a>

Install Rails and its dependencies with the `gem` command\.

```
~$ gem install rails
Fetching: concurrent-ruby-1.0.5.gem (100%)
Successfully installed concurrent-ruby-1.0.5
Fetching: i18n-1.0.1.gem (100%)
Successfully installed i18n-1.0.1
...
```

Test your Rails installation\.

```
~$ rails --version
Rails 5.2.0
```

Use `rails new` with the name of the application to create a new Rails project\.

```
~$ rails new ~/eb-rails
```

Rails creates a directory with the name specified, generates all of the files needed to run a sample project locally, and then runs bundler to install all of the dependencies \(Gems\) defined in the project's Gemfile\. 

Test your Rails installation by running the default project locally\.

```
~$ cd eb-rails
eb-rails $ rails server
=> Booting Puma
=> Rails 5.2.0 application starting in development
=> Run `rails server -h` for more startup options
Puma starting in single mode...
* Version 3.11.4 (ruby 2.5.1-p57), codename: Love Song
* Min threads: 5, max threads: 5
* Environment: development
* Listening on tcp://0.0.0.0:3000
Use Ctrl+C to stop
...
```

Open `http://localhost:3000` in a web browser to see the default project in action\.

![\[The default rails site development page.\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/ruby-rails-default.png)

This page is only visible in development mode\. Add some content to the front page of the application to support production deployment to Elastic Beanstalk\. Use `rails generate` to create a controller, route, and view for your welcome page\. 

```
~/eb-rails$ rails generate controller WelcomePage welcome
      create  app/controllers/welcome_page_controller.rb
       route  get 'welcome_page/welcome'
      invoke  erb
      create    app/views/welcome_page
      create    app/views/welcome_page/welcome.html.erb
      invoke  test_unit
      create    test/controllers/welcome_page_controller_test.rb
      invoke  helper
      create    app/helpers/welcome_page_helper.rb
      invoke    test_unit
      invoke  assets
      invoke    coffee
      create      app/assets/javascripts/welcome_page.coffee
      invoke    scss
      create      app/assets/stylesheets/welcome_page.scss.
```

This gives you all you need to access the page at `/welcome_page/welcome`\. Before you publish the changes, however, change the content in the view and add a route to make this page appear at the top level of the site\. 

Use a text editor to edit the content in `app/views/welcome_page/welcome.html.erb`\. For this example, you'll use `cat` to simply overwrite the content of the existing file\. 

**Example app/views/welcome\_page/welcome\.html\.erb**  

```
<h1>Welcome!</h1>
<p>This is the front page of my first Rails application on Elastic Beanstalk.</p>
```

 Finally, add the following route to `config/routes.rb`: 

**Example config/routes\.rb**  

```
Rails.application.routes.draw do
  get 'welcome_page/welcome'
  root 'welcome_page#welcome'
```

This tells Rails to route requests to the root of the website to the welcome page controller's welcome method, which renders the content in the welcome view \(`welcome.html.erb`\)\.

## Configure rails settings<a name="ruby-rails-tutorial-configure"></a>

Use the Elastic Beanstalk console to configure Rails with environment properties\. Set the `SECRET_KEY_BASE` environment property to a string of up to 256 alphanumeric characters\.

Rails uses this property to create keys\. Therefore you should keep it a secret and not store it in source control in plain text\. Instead, you provide it to Rails code on your environment through an environment property\.

**To configure environment properties in the Elastic Beanstalk console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Configuration**\.

1. In the **Software** configuration category, choose **Edit**\.

1. Under **Environment properties**, enter key\-value pairs\.  
![\[Environment properties section in the Modify software configuration page\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/wizard-software-environment.png)

1. Choose **Apply**\.

Now you're ready to deploy the site to your environment\.

## Deploy your application<a name="ruby-rails-tutorial-deploy"></a>

Create a [source bundle](applications-sourcebundle.md) containing the files created by Rails\. The following command creates a source bundle named `rails-default.zip`\.

```
~/eb-rails$ zip ../rails-default.zip -r * .[^.]*
```

Upload the source bundle to Elastic Beanstalk to deploy Rails to your environment\.

**To deploy a source bundle**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. On the environment overview page, choose **Upload and deploy**\.

1. Use the on\-screen dialog box to upload the source bundle\.

1. Choose **Deploy**\.

1. When the deployment completes, you can choose the site URL to open your website in a new tab\.

## Cleanup<a name="ruby-rails-tutorial-cleanup"></a>

When you finish working with Elastic Beanstalk, you can terminate your environment\. Elastic Beanstalk terminates all AWS resources associated with your environment, such as [Amazon EC2 instances](using-features.managing.ec2.md), [database instances](using-features.managing.db.md), [load balancers](using-features.managing.elb.md), security groups, and [alarms](using-features.alarms.md#using-features.alarms.title)\. 

**To terminate your Elastic Beanstalk environment**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. Choose **Environment actions**, and then choose **Terminate environment**\.

1. Use the on\-screen dialog box to confirm environment termination\.

With Elastic Beanstalk, you can easily create a new environment for your application at any time\.

## Next steps<a name="ruby-rails-tutorial-nextsteps"></a>

For more information about Rails, visit [rubyonrails\.org](https://rubyonrails.org/)\.

As you continue to develop your application, you'll probably want a way to manage environments and deploy your application without manually creating a \.zip file and uploading it to the Elastic Beanstalk console\. The [Elastic Beanstalk Command Line Interface](eb-cli3.md) \(EB CLI\) provides easy\-to\-use commands for creating, configuring, and deploying applications to Elastic Beanstalk environments from the command line\.

Finally, if you plan on using your application in a production environment, you will want to [configure a custom domain name](customdomains.md) for your environment and [enable HTTPS](configuring-https.md) for secure connections\.