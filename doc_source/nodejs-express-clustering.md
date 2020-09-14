# Deploying an Express application with clustering to Elastic Beanstalk<a name="nodejs-express-clustering"></a>

This tutorial walks you through deploying a sample application to Elastic Beanstalk using the Elastic Beanstalk Command Line Interface \(EB CLI\), and then updating the application to use the [Express](http://expressjs.com/) framework, [Amazon ElastiCache](https://aws.amazon.com/elasticache/), and clustering\. Clustering enhances your web application's high availability, performance, and security\. To learn more about Amazon ElastiCache, go to [What Is Amazon ElastiCache for Memcached?](https://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/Introduction.html) in the *Amazon ElastiCache for Memcached User Guide*\.

**Note**  
This example creates AWS resources, which you might be charged for\. For more information about AWS pricing, see [https://aws.amazon.com/pricing/](https://aws.amazon.com/pricing/)\. Some services are part of the AWS Free Usage Tier\. If you are a new customer, you can test drive these services for free\. See [https://aws.amazon.com/free/](https://aws.amazon.com/free/) for more information\.

## Prerequisites<a name="nodejs-express-clustering.prereq"></a>

This tutorial requires the Node\.js language, its package manager called npm, and the Express web application framework\. For details on installing these components and setting up your local development environment, see [Setting up your Node\.js development environment](nodejs-devenv.md)\.

**Note**  
For this tutorial, you don't need to install the AWS SDK for Node\.js, which is also mentioned in [Setting up your Node\.js development environment](nodejs-devenv.md)\.

The tutorial also requires the Elastic Beanstalk Command Line Interface \(EB CLI\)\. For details on installing and configuring the EB CLI, see [Install the EB CLI](eb-cli3-install.md) and [Configure the EB CLI](eb-cli3-configuration.md)\.

## Create an Elastic Beanstalk environment<a name="nodejs-express-clustering.create"></a>

Configure an EB CLI repository for your application and create an Elastic Beanstalk environment running the Node\.js platform\.

1. Create a repository with the eb init command\.

   ```
   ~/node-express$ eb init --platform node.js --region us-east-2
   Application node-express has been created.
   ```

   This command creates a configuration file in a folder named `.elasticbeanstalk` that specifies settings for creating environments for your application, and creates an Elastic Beanstalk application named after the current folder\.

1. Create an environment running a sample application with the eb create command\.

   ```
   ~/node-express$ eb create --sample node-express-env
   ```

   This command creates a load\-balanced environment with the default settings for the Node\.js platform and the following resources:
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

1. When environment creation completes, use the eb open command to open the environment's URL in the default browser\.

   ```
   ~/node-express$ eb open
   ```

## Update the application<a name="nodejs-express-clustering.update"></a>

Update the sample application in the Elastic Beanstalk environment to use the Express framework\.

You can download the final source code from [nodejs\-example\-express\-elasticache\.zip](samples/nodejs-example-express-elasticache.zip)\.

**Note**  
The prerequisite development environment setup results in an Express project structure in the `node-express` folder\. If you haven't already generated an Express project, run the following command\. For more details, see [Installing Express](nodejs-devenv.md#nodejs-devenv-express)\.  

```
~/node-express$ express && npm install
```

**To update your application to use Express**

1. Rename `node-express/app.js` to `node-express/express-app.js`\.

   ```
   node-express$ mv app.js express-app.js
   ```

1. Update the line `var app = express();` in `node-express/express-app.js` to the following:

   ```
   var app = module.exports = express();
   ```

1. On your local computer, create a file named `node-express/app.js` with the following code\.

   ```
   var cluster = require('cluster'),
       app = require('./express-app');
   
   var workers = {},
       count = require('os').cpus().length;
   
   function spawn(){
     var worker = cluster.fork();
     workers[worker.pid] = worker;
     return worker;
   }
   
   if (cluster.isMaster) {
     for (var i = 0; i < count; i++) {
       spawn();
     }
     cluster.on('death', function(worker) {
       console.log('worker ' + worker.pid + ' died. spawning a new process...');
       delete workers[worker.pid];
       spawn();
     });
   } else {
     app.listen(process.env.PORT || 5000);
   }
   ```

1. Deploy the updated application\.

   ```
   node-express$ eb deploy
   ```

1. Your environment will be updated after a few minutes\. Once the environment is green and ready, refresh the URL to verify it worked\. You should see a web page that says "Welcome to Express"\.

You can access the logs for your EC2 instances running your application\. For instructions on accessing your logs, see [Viewing logs from Amazon EC2 instances in your Elastic Beanstalk environment](using-features.logging.md)\.

Next, let's update the Express application to use Amazon ElastiCache\.

**To update your Express application to use Amazon ElastiCache**

1. On your local computer, create an `.ebextensions` directory in the top\-level directory of your source bundle\. In this example, we use `node-express/.ebextensions`\.

1. Create a configuration file `node-express/.ebextensions/elasticache-iam-with-script.config` with the following snippet\. For more information about the configuration file, see [Node\.js configuration namespace](create_deploy_nodejs.container.md#nodejs-namespaces)\. This creates an IAM user with the permissions required to discover the elasticache nodes and writes to a file anytime the cache changes\. You can also copy the file from [nodejs\-example\-express\-elasticache\.zip](samples/nodejs-example-express-elasticache.zip)\. For more information on the ElastiCache properties, see [Example: ElastiCache](customize-environment-resources-elasticache.md)\.
**Note**  
YAML relies on consistent indentation\. Match the indentation level when replacing content in an example configuration file and ensure that your text editor uses spaces, not tab characters, to indent\.

   ```
   Resources:
     MyCacheSecurityGroup:
       Type: 'AWS::EC2::SecurityGroup'
       Properties:
         GroupDescription: "Lock cache down to webserver access only"
         SecurityGroupIngress:
           - IpProtocol: tcp
             FromPort:
               Fn::GetOptionSetting:
                 OptionName: CachePort
                 DefaultValue: 11211
             ToPort:
               Fn::GetOptionSetting:
                 OptionName: CachePort
                 DefaultValue: 11211
             SourceSecurityGroupName:
               Ref: AWSEBSecurityGroup
     MyElastiCache:
       Type: 'AWS::ElastiCache::CacheCluster'
       Properties:
         CacheNodeType:
           Fn::GetOptionSetting:
             OptionName: CacheNodeType
             DefaultValue: cache.t2.micro
         NumCacheNodes:
           Fn::GetOptionSetting:
             OptionName: NumCacheNodes
             DefaultValue: 1
         Engine:
           Fn::GetOptionSetting:
             OptionName: Engine
             DefaultValue: redis
         VpcSecurityGroupIds:
           -
             Fn::GetAtt:
               - MyCacheSecurityGroup
               - GroupId
     AWSEBAutoScalingGroup :
       Metadata :
         ElastiCacheConfig :
           CacheName :
             Ref : MyElastiCache
           CacheSize :
              Fn::GetOptionSetting:
                OptionName : NumCacheNodes
                DefaultValue: 1
     WebServerUser : 
       Type : AWS::IAM::User
       Properties :
         Path : "/"
         Policies:
           -
             PolicyName: root
             PolicyDocument :
               Statement :
                 -
                   Effect : Allow
                   Action : 
                     - cloudformation:DescribeStackResource
                     - cloudformation:ListStackResources
                     - elasticache:DescribeCacheClusters
                   Resource : "*"
     WebServerKeys :
       Type : AWS::IAM::AccessKey
       Properties :
         UserName :
           Ref: WebServerUser
   
   Outputs:
     WebsiteURL:
       Description: sample output only here to show inline string function parsing
       Value: |
         http://`{ "Fn::GetAtt" : [ "AWSEBLoadBalancer", "DNSName" ] }`
     MyElastiCacheName:
       Description: Name of the elasticache
       Value:
         Ref : MyElastiCache
     NumCacheNodes:
       Description: Number of cache nodes in MyElastiCache
       Value:
         Fn::GetOptionSetting:
           OptionName : NumCacheNodes
           DefaultValue: 1
   
   files:
     "/etc/cfn/cfn-credentials" :
       content : |
         AWSAccessKeyId=`{ "Ref" : "WebServerKeys" }`
         AWSSecretKey=`{ "Fn::GetAtt" : ["WebServerKeys", "SecretAccessKey"] }`
       mode : "000400"
       owner : root
       group : root
   
     "/etc/cfn/get-cache-nodes" :
       content : |
         # Define environment variables for command line tools
         export AWS_ELASTICACHE_HOME="/home/ec2-user/elasticache/$(ls /home/ec2-user/elasticache/)"
         export AWS_CLOUDFORMATION_HOME=/opt/aws/apitools/cfn
         export PATH=$AWS_CLOUDFORMATION_HOME/bin:$AWS_ELASTICACHE_HOME/bin:$PATH
         export AWS_CREDENTIAL_FILE=/etc/cfn/cfn-credentials
         export JAVA_HOME=/usr/lib/jvm/jre
   
         # Grab the Cache node names and configure the PHP page
         aws cloudformation list-stack-resources --stack `{ "Ref" : "AWS::StackName" }` --region `{ "Ref" : "AWS::Region" }` --output text | grep MyElastiCache | awk '{print $4}' | xargs -I {} aws elasticache describe-cache-clusters --cache-cluster-id {} --region `{ "Ref" : "AWS::Region" }` --show-cache-node-info --output text | grep '^ENDPOINT' | awk '{print $2 ":" $3}' > `{ "Fn::GetOptionSetting" : { "OptionName" : "NodeListPath", "DefaultValue" : "/var/www/html/nodelist" } }`
       mode : "000500"
       owner : root
       group : root
   
     "/etc/cfn/hooks.d/cfn-cache-change.conf" :
       "content": |
         [cfn-cache-size-change]
         triggers=post.update
         path=Resources.AWSEBAutoScalingGroup.Metadata.ElastiCacheConfig
         action=/etc/cfn/get-cache-nodes
         runas=root
   
   sources :
     "/home/ec2-user/elasticache" : "https://elasticache-downloads.s3.amazonaws.com/AmazonElastiCacheCli-latest.zip"
   
   commands: 
     make-elasticache-executable:
       command: chmod -R ugo+x /home/ec2-user/elasticache/*/bin/*
   
   packages : 
     "yum" :
       "aws-apitools-cfn"  : []
   
   container_commands:
     initial_cache_nodes:
       command: /etc/cfn/get-cache-nodes
   ```

1. On your local computer, create a configuration file `node-express/.ebextensions/elasticache_settings.config` with the following snippet to configure ElastiCache\.

   ```
   option_settings:
     "aws:elasticbeanstalk:customoption":
        CacheNodeType: cache.t2.micro
        NumCacheNodes: 1
        Engine: memcached
        NodeListPath: /var/nodelist
   ```

1. On your local computer, replace `node-express/express-app.js` with the following snippet\. This file reads the nodes list from disk \(`/var/nodelist`\) and configures express to use `memcached` as a session store if nodes are present\. Your file should look like the following\.

   ```
   /**
    * Module dependencies.
    */
   
   var express = require('express'),
       session = require('express-session'),
       bodyParser = require('body-parser'),
       methodOverride = require('method-override'),
       cookieParser = require('cookie-parser'),
       fs = require('fs'),
       filename = '/var/nodelist',
       app = module.exports = express();
   
   var MemcachedStore = require('connect-memcached')(session);
   
   function setup(cacheNodes) {
     app.use(bodyParser.raw());
     app.use(methodOverride());
     if (cacheNodes) {
         app.use(cookieParser());
   
         console.log('Using memcached store nodes:');
         console.log(cacheNodes);
   
         app.use(session({
             secret: 'your secret here',
             resave: false,
             saveUninitialized: false,
             store: new MemcachedStore({'hosts': cacheNodes})
         }));
     } else {
       console.log('Not using memcached store.');
       app.use(cookieParser('your secret here'));
       app.use(session());
     }
   
     app.get('/', function(req, resp){
     if (req.session.views) {
         req.session.views++
         resp.setHeader('Content-Type', 'text/html')
         resp.write('Views: ' + req.session.views)
         resp.end()
      } else {
         req.session.views = 1
         resp.end('Refresh the page!')
       }
     });
   
     if (!module.parent) {
         console.log('Running express without cluster.');
         app.listen(process.env.PORT || 5000);
     }
   }
   
   // Load elasticache configuration.
   fs.readFile(filename, 'UTF8', function(err, data) {
       if (err) throw err;
   
       var cacheNodes = [];
       if (data) {
           var lines = data.split('\n');
           for (var i = 0 ; i < lines.length ; i++) {
               if (lines[i].length > 0) {
                   cacheNodes.push(lines[i]);
               }
           }
       }
       setup(cacheNodes);
   });
   ```

1. On your local computer, update `node-express/package.json` to add four dependencies\.

   ```
   {
     "name": "node-express",
     "version": "0.0.0",
     "private": true,
     "scripts": {
       "start": "node ./bin/www"
     },
     "dependencies": {
       "cookie-parser": "*",
       "debug": "~2.6.9",
       "express": "~4.16.0",
       "http-errors": "~1.6.2",
       "jade": "~1.11.0",
       "morgan": "~1.9.0",
       "connect-memcached": "*",
       "express-session": "*",
       "body-parser": "*",
       "method-override": "*"
     }
   }
   ```

1. Deploy the updated application\.

   ```
   node-express$ eb deploy
   ```

1. Your environment will be updated after a few minutes\. After your environment is green and ready, verify that the code worked\.

   1. Check the [Amazon CloudWatch console](https://console.aws.amazon.com/cloudwatch/home) to view your ElastiCache metrics\. To view your ElastiCache metrics, select **Metrics** in the left pane, and then search for **CurrItems**\. Select **ElastiCache > Cache Node Metrics**, and then select your cache node to view the number of items in the cache\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/elasticache-express.png)
**Note**  
Make sure you are looking at the same region that you deployed your application to\.

      If you copy and paste your application URL into another web browser and refresh the page, you should see your CurrItem count go up after 5 minutes\.

   1. Take a snapshot of your logs\. For more information about retrieving logs, see [Viewing logs from Amazon EC2 instances in your Elastic Beanstalk environment](using-features.logging.md)\.

   1. Check the file `/var/log/nodejs/nodejs.log` in the log bundle\. You should see something similar to the following:

      ```
      Using memcached store nodes:
      [ 'aws-my-1oys9co8zt1uo.1iwtrn.0001.use1.cache.amazonaws.com:11211' ]
      ```

## Clean up<a name="nodejs-express-clustering.delete"></a>

If you no longer want to run your application, you can clean up by terminating your environment and deleting your application\.

Use the `eb terminate` command to terminate your environment and the `eb delete` command to delete your application\. 

**To terminate your environment**

From the directory where you created your local repository, run `eb terminate`\.

```
$ eb terminate
```

This process can take a few minutes\. Elastic Beanstalk displays a message once the environment is successfully terminated\. 