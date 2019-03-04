# Deploying an Express Application to Elastic Beanstalk<a name="create_deploy_nodejs_express"></a>

This section walks you through deploying a sample application to Elastic Beanstalk using Elastic Beanstalk Command Line Interface \(EB CLI\) and git, and then updating the application to use the [Express](http://expressjs.com/) framework\. 

## Prerequisites<a name="nodejs-express-tutorial-prerequisites"></a>

This tutorial requires the Node\.js language and its package manager, NPM\. See [Setting Up your Node\.js Development Environment](nodejs-devenv.md) for details on setting up your local development environment\. This tutorial also requires the Elastic Beanstalk Command Line Interface \(EB CLI\)\. For details on installing and configuring the EB CLI, see [Install the Elastic Beanstalk Command Line Interface \(EB CLI\)](eb-cli3-install.md) and [Configure the EB CLI](eb-cli3-configuration.md)\.

## Install Express and Generate a Project<a name="create_deploy_nodejs_express_setup"></a>

Set up Express and create the project structure\. The following walks you through setting up Express on a Linux operating system\.

**To set up your Express development environment on your local computer**

1. Create a directory for your Express application\.

   ```
   ~$ mkdir node-express
   ~$ cd node-express
   ```

1. Install Express globally so that you have access to the `express` command\.

   ```
   ~/node-express$ npm install -g express-generator
   ```

1. Depending on your operating system, you may need to set your path to run the `express` command\. If you need to set your path, use the output from the previous step when you installed Express\. The following is an example\. 

   ```
   ~/node-express$ export PATH=$PATH:/usr/local/share/npm/bin/express
   ```

1. Run the `express` command\. This generates `package.json`\. 

   ```
   ~/node-express$ express
   ```

   When prompted if you want to continue, type **y**\.

1. Set up local dependencies\.

   ```
   ~/node-express$ npm install
   ```

1. Verify it works\.

   ```
   ~/node-express$ npm start
   ```

   You should see output similar to the following:

   ```
   > nodejs@0.0.0 start /home/local/user/node-express
   > node ./bin/www
   ```

   Press **Ctrl\+C** to stop the server\.

1. Initialize the Git repository\. If you don't have Git installed, download it from the [Git downloads site](https://git-scm.com/downloads)\.

   ```
   ~/node-express$ git init
   ```

1. Create a file named `.gitignore` and add the following files and directories to it\. These files will be excluded from being added to the repository\. This step is not required, but it is recommended\.

   **`node-express/.gitignore`**

   ```
   node_modules/
   .gitignore
   .elasticbeanstalk/
   ```

## Create an Elastic Beanstalk Environment<a name="create_deploy_nodejs_express_eb_init-rds"></a>

Configure an EB CLI repository for your application and create an Elastic Beanstalk environment running the Node\.js platform\.

1. Create a repository with the eb init command\.

   ```
   ~/node-express$ eb init --platform node.js --region us-west-2
   Application node-express has been created.
   ```

   This command creates a configuration file in a folder named `.elasticbeanstalk` that specifies settings for creating environments for your application, and creates an Elastic Beanstalk application named after the current folder\.

1. Create an environment running a sample application with the eb create command\.

   ```
   ~/node-express$ eb create --sample node-express-env
   ```

   This command creates a load balanced environment with the default settings for the Node\.js platform and the following resources:
   + **EC2 instance** – An Amazon Elastic Compute Cloud \(Amazon EC2\) virtual machine configured to run web apps on the platform that you choose\.

     Each platform runs a specific set of software, configuration files, and scripts to support a specific language version, framework, web container, or combination thereof\. Most platforms use either Apache or nginx as a reverse proxy that sits in front of your web app, forwards requests to it, serves static assets, and generates access and error logs\.
   + **Instance security group** – An Amazon EC2 security group configured to allow ingress on port 80\. This resource lets HTTP traffic from the load balancer reach the EC2 instance running your web app\. By default, traffic isn't allowed on other ports\.
   + **Load balancer** – An Elastic Load Balancing load balancer configured to distribute requests to the instances running your application\. A load balancer also eliminates the need to expose your instances directly to the internet\.
   + **Load balancer security group** – An Amazon EC2 security group configured to allow ingress on port 80\. This resource lets HTTP traffic from the internet reach the load balancer\. By default, traffic isn't allowed on other ports\.
   + **Auto Scaling group** – An Auto Scaling group configured to replace an instance if it is terminated or becomes unavailable\.
   + **Amazon S3 bucket** – A storage location for your source code, logs, and other artifacts that are created when you use Elastic Beanstalk\.
   + **Amazon CloudWatch alarms** – Two CloudWatch alarms that monitor the load on the instances in your environment and are triggered if the load is too high or too low\. When an alarm is triggered, your Auto Scaling group scales up or down in response\.
   + **AWS CloudFormation stack** – Elastic Beanstalk uses AWS CloudFormation to launch the resources in your environment and propagate configuration changes\. The resources are defined in a template that you can view in the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation)\.
   + **Domain name** – A domain name that routes to your web app in the form **subdomain*\.*region*\.elasticbeanstalk\.com*\.

1. When environment creation completes, use the eb open command to open the environment's URL in the default browser\.

   ```
   ~/node-express$ eb open
   ```

## Update the Application<a name="create_deploy_nodejs_express_update"></a>

After you have created an environment with a sample application, you can update it with your own application\. In this step, we update the sample application to use the Express framework\.

**To update your application to use Express**

1. Add a configuration file that sets the Node Command to "npm start":

   **`node-express/.ebextensions/nodecommand.config`**

   ```
   option_settings:
     aws:elasticbeanstalk:container:nodejs:
       NodeCommand: "npm start"
   ```

   For more information, see [Advanced Environment Customization with Configuration Files \(`.ebextensions`\)](ebextensions.md)\.

1. Stage the files:

   ```
   ~/node-express$ git add .
   ~/node-express$ git commit -m "First express app"
   ```

1. Deploy the changes:

   ```
   ~/node-express$ eb deploy
   ```

1. Once the environment is green and ready, refresh the URL to verify it worked\. You should see a web page that says **Welcome to Express**\.

Next, let's update the Express application to serve static files and add a new page\.

**To configure static files and add a new page to your Express application**

1. Add a second configuration file with the following content:

   **`node-express/.ebextensions/staticfiles.config`**

   ```
   option_settings:
     aws:elasticbeanstalk:container:nodejs:staticfiles:
       /public: /public
   ```

   This setting configures the proxy server to serve files in the `public` folder at the `/public` path of the application\. [Serving files statically](create_deploy_nodejs.container.md#nodejs-namespaces) from the proxy reduces the load on your application\.

1. Comment out the static mapping in `node-express/app.js`\. This step is not required, but it is a good test to confirm that static mappings are configured correctly\.

   ```
   //  app.use(express.static(path.join(__dirname, 'public'))); 
   ```

1. Add your updated files to your local repository and commit your changes\.

   ```
   ~/node-express$ git add .ebextensions/ app.js
   ~/node-express$ git commit -m "Serve stylesheets statically with nginx."
   ```

1. Add `node-express/routes/hike.js`\. Type the following:

   ```
   exports.index = function(req, res) {
    res.render('hike', {title: 'My Hiking Log'});
   };
   
   exports.add_hike = function(req, res) {
   };
   ```

1. Update `node-express/app.js` to include three new lines\.

   First, add the following line to add a `require` for this route:

   ```
   hike = require('./routes/hike');
   ```

   Your file should look similar to the following snippet:

   ```
   var express = require('express');
   var path = require('path');
   var hike = require('./routes/hike');
   ```

   Then, add the following two lines to `node-express/app.js` after `var app = express();`

   ```
   app.get('/hikes', hike.index);
   app.post('/add_hike', hike.add_hike);
   ```

   Your file should look similar to the following snippet:

   ```
   var app = express();
   app.get('/hikes', hike.index);
   app.post('/add_hike', hike.add_hike);
   ```

1. Copy `node-express/views/index.jade` to `node-express/views/hike.jade`\. 

   ```
   ~/node-express$ cp views/index.jade views/hike.jade
   ```

1. Add your files to the local repository, commit your changes, and deploy your updated application\.

   ```
   ~/node-express$ git add .
   ~/node-express$ git commit -m "Add hikes route and template."
   ~/node-express$ eb deploy
   ```

1. Your environment will be updated after a few minutes\. After your environment is green and ready, verify it worked by refreshing your browser and appending **hikes** at the end of the URL \(e\.g\., `http://node-express-env-syypntcz2q.elasticbeanstalk.com/hikes`\)\.

   You should see a web page titled **My Hiking Log**\.

## Clean Up<a name="create_deploy_nodejs_express_delete"></a>

If you are done working with Elastic Beanstalk, you can terminate your environment\.

Use the eb terminate command to terminate your environment and all of the resources that it contains\.

```
~/node-express$ eb terminate
The environment "node-express-env" and all associated instances will be terminated.
To confirm, type the environment name: node-express-env
INFO: terminateEnvironment is starting.
...
```