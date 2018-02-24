# Deploying a Symfony2 Application to Elastic Beanstalk<a name="create_deploy_PHP_symfony2"></a>

This section walks you through deploying a sample application to Elastic Beanstalk using the Elastic Beanstalk Command Line Interface \(EB CLI\) and Git, and then updating the application to use the [Symfony2](http://symfony.com/) framework\. 


+ [Set Up Your Symfony2 Development Environment](#create_deploy_PHP_symfony2_setup)
+ [Configure Elastic Beanstalk](#create_deploy_PHP_eb_init)
+ [View the Application](#create_deploy_PHP_symfony2_update-view)
+ [Update the Application](#create_deploy_PHP_symfony2_update)
+ [Clean Up](#create_deploy_PHP_symfony2_delete)

## Set Up Your Symfony2 Development Environment<a name="create_deploy_PHP_symfony2_setup"></a>

Set up Symfony2 and create the project structure\. The following walks you through setting up Symfony2 on a Linux operating system\. For more information, go to [http://symfony\.com/download](http://symfony.com/download)\. For information on running Symfony2 behind a load balancer, see [How to Configure Symfony to Work behind a Load Balancer or a Reverse Proxy](http://symfony.com/doc/2.8/request/load_balancer_reverse_proxy.html)\.

**To set up your PHP development environment on your local computer**

1. Download and install composer from getcomposer\.org\. For more information, go to [http://getcomposer\.org/download/](http://getcomposer.org/download/)\.

   ```
   curl -s https://getcomposer.org/installer | php
   ```

1. Install Symfony2 Standard Edition with Composer\. Check [http://symfony\.com/download](http://symfony.com/download) for the latest available version\. Using the following command, composer will install the vendor libraries for you\.

   ```
   php composer.phar create-project symfony/framework-standard-edition symfony2_example/ <version number>
   cd symfony2_example
   ```
**Note**  
You may need to set the date\.timezone in the php\.ini to successfully complete installation\. Also provide parameters for Composer, as needed\.

1. Initialize the Git repository\.

   ```
   git init 
   ```

1. Update the **\.gitignore** file to ignore vendor, cache, logs, and composer\.phar\. These files do not need to get pushed to the remote server\.

   ```
   cat > .gitignore <<EOT
   app/bootstrap.php.cache
   app/cache/*
   app/logs/*
   vendor
   composer.phar
   EOT
   ```

1. Generate the hello bundle\. 

   ```
   php app/console generate:bundle --namespace=Acme/HelloBundle --format=yml
   ```

   When prompted, accept all defaults\. For more information, go to [Creating Pages in Symfony2](http://symfony.com/doc/current/book/page_creation.html)\.

Next, set some configuration options\. Composer dependencies require that you set the HOME or COMPOSER\_HOME environment variable\. Also configure Composer to self\-update so that you always use the latest version\.

In addition, set the root directory for your application\.

**To configure Composer and the root directory**

1. Create a configuration file with the extension **\.config** \(e\.g\., `composer.config`\) and place it in an `.ebextensions` directory at the top level of your source bundle\. You can have multiple configuration files in your `.ebextensions` directory\. For information about the file format of configuration files, see [Advanced Environment Customization with Configuration Files \(`.ebextensions`\)](ebextensions.md)\.
**Note**  
Configuration files should conform to YAML or JSON formatting standards\. For more information, go to [http://www\.yaml\.org/start\.html](http://www.yaml.org/start.html) or [http://www\.json\.org](http://www.json.org), respectively\.

1. In the **\.config** file, type the following\.

   ```
   commands:
     01updateComposer:
       command: export COMPOSER_HOME=/root && /usr/bin/composer.phar self-update 1.0.0-alpha11
   
   option_settings:
     - namespace: aws:elasticbeanstalk:application:environment
       option_name: COMPOSER_HOME
       value: /root
     - namespace: aws:elasticbeanstalk:container:php:phpini
       option_name: document_root
       value: /web
   ```

   Replace *1\.0\.0\-alpha11* with your preferred version of composer\. See [getcomposer\.org/download](https://getcomposer.org/download/) for a list of available versions\.

## Configure Elastic Beanstalk<a name="create_deploy_PHP_eb_init"></a>

The following instructions use the [Elastic Beanstalk command line interface](eb-cli3.md) \(EB CLI\) to configure an Elastic Beanstalk application repository in your local project directory\.

**To configure Elastic Beanstalk**

1. From the directory where you created your local repository, type the following command\.

   ```
   eb init
   ```

1. When you are prompted for the Elastic Beanstalk region, type the number of the region\. For information about this product's regions, see [Regions and Endpoints](http://docs.aws.amazon.com/general/latest/gr/rande.html?r=1166) in the *Amazon Web Services General Reference*\. For this example, we'll use **US West \(Oregon\)**\.

1.  When you are prompted for the Elastic Beanstalk application to use, type the number corresponding to the option **Create New Application**\. Elastic Beanstalk generates an application name based on the current directory name, if an application name has not been previously configured\. In this example, we use symfony2app\. 

   ```
   Enter an AWS Elastic Beanstalk application name (auto-generated value is "windows"): symfony2app
   ```
**Note**  
If you have a space in your application name, be sure you do not use quotation marks\. 

1. Type **y** if Elastic Beanstalk correctly detected the correct platform you are using\. Type **n** if not, and then specify the correct platform\.

1.  When prompted, type **y** if you want to set up Secure Shell \(SSH\) to connect to your instances\. Type **n** if you do not want to set up SSH\. In this example, we will type **n**\.

   ```
   Do you want to set up SSH for your instances?
   (y/n): n
   ```

1. Create your running environment\.

   ```
   eb create
   ```

1. When you are prompted for the Elastic Beanstalk environment name, type the name of the environment\. Elastic Beanstalk automatically creates an environment name based on your application name\. To accept the default, press **Enter**\.

   ```
   Enter Environment Name
   (default is HelloWorld-env):
   ```
**Note**  
If you have a space in your application name, be sure you do not have a space in your environment name\. 

1. When you are prompted to provide a CNAME prefix, type the CNAME prefix you want to use\. Elastic Beanstalk automatically creates a CNAME prefix based on the environment name\. To accept the default, press **Enter**\.

   ```
   Enter DNS CNAME prefix
   (default is HelloWorld):
   ```

After configuring Elastic Beanstalk, you are ready to deploy a sample application\. 

To update your Elastic Beanstalk configuration, you can use the **init** command again\. When prompted, you can update your configuration options\. To keep any previous settings, press the **Enter** key\. 

**To deploy a sample application**

+ From the directory where you created your local repository, type the following command\.

  ```
  eb deploy
  ```

  This process can take several minutes to complete\. Elastic Beanstalk provides status updates during the process\. If at any time you want to stop polling for status updates, press **Ctrl\+C**\. Once the environment status is Green, Elastic Beanstalk outputs a URL for the application\. Copy and paste the URL into your web browser to view the application\.

## View the Application<a name="create_deploy_PHP_symfony2_update-view"></a>

**To view the application**

+ To open your application in a browser window, type the following:

  ```
  eb open
  ```

## Update the Application<a name="create_deploy_PHP_symfony2_update"></a>

After you have deployed a sample application, you can update it with your own application\. In this step, we update the sample application with a simple "Hello World" Symfony2 application\. 

**To update the sample application**

1. Add your files to your local Git repository, and then commit your change\.

   ```
   git add -A && git commit -m "Initial commit"
   ```
**Note**  
For information about Git commands, go to [Git \- Fast Version Control System](http://git-scm.com/)\.

1. Create an application version matching your local repository and deploy to the Elastic Beanstalk environment if specified\.

   ```
   eb deploy
   ```

   You can also configure Git to push from a specific branch to a specific environment\. For more information, see "Using Git with EB CLI" in the topic [Managing Elastic Beanstalk Environments with the EB CLI](eb-cli3-getting-started.md)\.

1. After your environment is Green and Ready, append **/web/hello/AWS** to the URL of your application\. The application should write out "Hello AWS\!" 

You can access the logs for your EC2 instances running your application\. For instructions on accessing your logs, see [Viewing Logs from Your Elastic Beanstalk Environment's Amazon EC2 Instances](using-features.logging.md)\.

## Clean Up<a name="create_deploy_PHP_symfony2_delete"></a>

If you no longer want to run your application, you can clean up by terminating your environment and deleting your application\.

Use the `terminate` command to terminate your environment and the `delete` command to delete your application\. 

**To terminate your environment and delete the application**

+ From the directory where you created your local repository, run `eb terminate`\.

  ```
  $ eb terminate
  ```

  This process can take a few minutes\. Elastic Beanstalk displays a message once the environment is successfully terminated\. 