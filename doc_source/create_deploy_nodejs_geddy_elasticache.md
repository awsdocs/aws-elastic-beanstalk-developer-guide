# Deploying a Geddy Application with Clustering to Elastic Beanstalk<a name="create_deploy_nodejs_geddy_elasticache"></a>

This section walks you through deploying a sample application to Elastic Beanstalk using the Elastic Beanstalk Command Line Interface \(EB CLI\) and Git, and then updating the application to use the [Geddy](http://geddyjs.org/) framework and [Amazon ElastiCache](https://aws.amazon.com/elasticache/) for clustering\. Clustering enhances your web application's high availability, performance, and security\. To learn more about Amazon ElastiCache, go to [Introduction to ElastiCache](https://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/Introduction.html) in the *Amazon ElastiCache User Guide*\. 

**Note**  
This example creates AWS resources, which you might be charged for\. For more information about AWS pricing, see [https://aws.amazon.com/pricing/](https://aws.amazon.com/pricing/)\. Some services are part of the AWS Free Usage Tier\. If you are a new customer, you can test drive these services for free\. See [https://aws.amazon.com/free/](https://aws.amazon.com/free/) for more information\.

## Step 1: Set Up Your Git Repository<a name="create_deploy_nodejs_geddy_elasticache_gitinit"></a>

The EB CLI is a command line interface that you can use with Git to deploy applications quickly and more easily\. The EB CLI is available as part of the Elastic Beanstalk command line tools package\. For instructions to install the EB CLI, see [Install the Elastic Beanstalk Command Line Interface \(EB CLI\)](eb-cli3-install.md)\.

Initialize your Git repository\. After you run the following command, when you run `eb init`, the EB CLI recognizes that your application is set up with Git\.

```
git init .
```

## Step 2: Set Up Your Geddy Development Environment<a name="create_deploy_nodejs_geddy_elasticache_setup"></a>

Set up Geddy and create the project structure\. The following steps walk you through setting up Geddy on a Linux operating system\. 

**To set up your Geddy development environment on your local computer**

1. Install Node\.js\. For instructions, go to [http://nodejs\.org/](http://nodejs.org/)\. Verify you have a successful installation before proceeding to the next step\.

   ```
   $ node -v
   ```
**Note**  
For information about what Node\.js versions are supported, see [AWS Elastic Beanstalk Supported Platforms](concepts.platforms.md)\.

1. Create a directory for your Geddy application\.

   ```
   $ mkdir node-geddy
   $ cd node-geddy
   ```

1. Install npm\.

   ```
   node-geddy$ cd . && yum install npm
   ```

1. Install Geddy globally so that you have geddy generators or start the server\.

   ```
   node-geddy$ npm install -g geddy
   ```

1. Depending on your operating system, you may need to set your path to run the `geddy`code> command\. If you need to set your path, use the output from the previous step when you installed Geddy\. The following is an example\. 

   ```
   node-geddy$ export:PATH=$PATH:/usr/local/share/npm/bin/geddy
   ```

1. Create the directory for your application\.

   ```
   node-geddy$ geddy app myapp
   node-geddy$ cd myapp
   ```

1. Start the server\. Verify everything is working, and then stop the server\.

   ```
   myapp$ geddy
   myapp$ curl localhost:4000 (or use web browser)
   ```

   Press **Ctrl\+C** to stop the server\.

1. Initialize the Git repository\.

   ```
   myapp$ git init 
   ```

1. Exclude the following files from being added to the repository\. This step is not required, but it is recommended\.

   ```
   myapp$ cat > .gitignore <<EOT 
   log/
   .gitignore
   .elasticbeanstalk/
   EOT
   ```

## Step 3: Configure Elastic Beanstalk<a name="create_deploy_nodejs_geddy_eb_init-rds"></a>

The following instructions use the [Elastic Beanstalk command line interface](eb-cli3.md) \(EB CLI\) to configure an Elastic Beanstalk application repository in your local project directory\.

**To configure Elastic Beanstalk**

1. From the directory where you created your local repository, type the following command\.

   ```
   eb init
   ```

1. When you are prompted for the Elastic Beanstalk region, type the number of the region\. For information about this product's regions, see [Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html?r=1166) in the *Amazon Web Services General Reference*\. For this example, we'll use **US West \(Oregon\)**\.

1.  When you are prompted for the Elastic Beanstalk application to use, type the number corresponding to the option **Create New Application**\. Elastic Beanstalk generates an application name based on the current directory name, if an application name has not been previously configured\. In this example, we use geddyapp\. 

   ```
   Enter an AWS Elastic Beanstalk application name (auto-generated value is "myapp"): geddyapp
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

## Step 5: View the Application<a name="create_deploy_nodejs_geddy_elasticache_update-view"></a>

**To view the application in a browser window**

Type the following:

```
eb open
```

## Step 6: Update the Application<a name="create_deploy_nodejs_geddy_elasticache_update"></a>

After you have deployed a sample application, you can update it with your own application\. In this step, we update the sample application to use the Geddy framework\. You can download the final source code from [http://elasticbeanstalk\-samples\-us\-east\-2\.s3\.amazonaws\.com/nodejs\-example\-geddy\.zip](http://elasticbeanstalk-samples-us-east-2.s3.amazonaws.com/nodejs-example-geddy.zip)\.

**To update your application to use Geddy**

1. On your local computer, create a file called `node-geddy/myapp/package.json`\. This file contains the necessary dependencies\.

   ```
   {
       "name": "Elastic_Beanstalk_Geddy",
       "version": "0.0.1",
       "dependencies": {
           "geddy": "0.6.x"
       }
   }
   ```

1. On your local computer, create a file called `node-geddy/maypp/app.js` as an entry point to the program\. 

   ```
   var geddy = require('geddy');
   
   geddy.startCluster({
       hostname: '0.0.0.0',
       port: process.env.PORT || '3000',
       environment: process.env.NODE_ENV || 'development'
   });
   ```

   The preceding snippet uses an environment variable for the environment setting\. You can manually set the environment to production \(`environment: 'production'`\), or you can create an environment variable and use it like in the above example\. We'll create an environment variable and set the environment to production in the next procedure\.

1. Test locally\.

   ```
   myapp$ npm install
   myapp$ node app
   ```

   The server should start\. Press **Ctrl\+C** to stop the server\.

1. Deploy to Elastic Beanstalk\.

   ```
   myapp$ git add .
   myapp$ git commit -m "First Geddy app"
   myapp$ eb deploy
   ```

1. Your environment will be updated after a few minutes\. Once the environment is green and ready, refresh the URL to verify it worked\. You should see a web page that says "Hello, World\!"\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-nodejs-geddy.png)

You can access the logs for your EC2 instances running your application\. For instructions on accessing your logs, see [Viewing Logs from Your Elastic Beanstalk Environment's Amazon EC2 Instances](using-features.logging.md)\.

Next, let's create an environment variable and set the environment to production\.

**To create an environment variable**

1. On your local computer in your project directory \(e\.g\., `myapp/`\), create a directory called `.ebextensions`\. 

1. On your local computer, create a file called `node-geddy/myapp/.ebextensions/myapp.config` with the following snippet to set the environment to production\.
**Note**  
YAML relies on consistent indentation\. Match the indentation level when replacing content in an example configuration file and ensure that your text editor uses spaces, not tab characters, to indent\.

   ```
   option_settings:
     - option_name: NODE_ENV
       value: production
   ```

   For more information about the configuration file, see [Node\.js Configuration Namespaces](create_deploy_nodejs.container.md#nodejs-namespaces)

1. Run "geddy secret" to get the secret value\. You'll need the secret value to successfully deploy your application\.

   ```
   myapp$ geddy secret
   ```

   You can add `node-geddy/myapp/config/secrets.json` to `.gitignore`, or you can put the secret value in an environment variable and create a command to write out the contents\. For this example, we'll use a command\.

1. Add the secret value from `node-geddy/myapp/config/secrets.json` to the `node-geddy/myapp/.elasticbeanstalk/optionsettings.gettyapp-env` file\. \(The name of the `optionsettings` file contains the same extension as your environment name\)\. Your file should look similar to the following:

   ```
   [aws:elasticbeanstalk:application:environment]
   secret=your geddy secret
   PARAM1=
   ```

1. Update your Elastic Beanstalk environment with your updated option settings\.

   ```
   myapp$ eb update
   ```

   Verify that your environment is green and ready before proceeding to the next step\.

1. On your local computer, create a configuration file `node-geddy/myapp/.ebextensions/write-secret.config` with the following command\. 

   ```
   container_commands:
     01write:
       command: |
         cat > ./config/secrets.json << SEC_END
           { "secret":   "`{ "Fn::GetOptionSetting": {"OptionName": "secret", "Namespace":"aws:elasticbeanstalk:application:environment"}}`" }
         SEC_END
   ```

1. Add your files to the local repository, commit your changes, and deploy your updated application\.

   ```
   myapp$ git add .
   myapp$ git commit -m "added config files"
   myapp$ eb deploy
   ```

   Your environment will be updated after a few minutes\. After your environment is green and ready, refresh your browser to make sure it worked\. You should still see "Hello, World\!"\.

Next, let's update the Geddy application to use Amazon ElastiCache\.

**To updated your Geddy application to use Amazon ElastiCache**

1. On your local computer, create a configuration file `node-geddy/myapp/.ebextensions/elasticache-iam-with-script.config` with the following snippet\. This configuration file adds the elasticache resource to the environment and creates a listing of the nodes in the elasticache on disk at `/var/nodelist`\. You can also copy the file from [http://elasticbeanstalk\-samples\-us\-east\-2\.s3\.amazonaws\.com/nodejs\-example\-geddy\.zip](http://elasticbeanstalk-samples-us-east-2.s3.amazonaws.com/nodejs-example-geddy.zip)\. For more information on the ElastiCache properties, see [Example Snippets: ElastiCache](customize-environment-resources-elasticache.md)\.

   ```
   Resources:
     MyElastiCache:
       Type: AWS::ElastiCache::CacheCluster
       Properties:
         CacheNodeType: 
            Fn::GetOptionSetting:
                OptionName : CacheNodeType
                DefaultValue: cache.m1.small
         NumCacheNodes: 
              Fn::GetOptionSetting:
                OptionName : NumCacheNodes
                DefaultValue: 1
         Engine: 
              Fn::GetOptionSetting:
                OptionName : Engine
                DefaultValue: memcached
         CacheSecurityGroupNames:
           - Ref: MyCacheSecurityGroup
     MyCacheSecurityGroup:
       Type: AWS::ElastiCache::SecurityGroup
       Properties:
         Description: "Lock cache down to webserver access only"
     MyCacheSecurityGroupIngress:
       Type: AWS::ElastiCache::SecurityGroupIngress
       Properties:
         CacheSecurityGroupName: 
           Ref: MyCacheSecurityGroup
         EC2SecurityGroupName:
           Ref: AWSEBSecurityGroup
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
         cfn-list-stack-resources `{ "Ref" : "AWS::StackName" }` --region `{ "Ref" : "AWS::Region" }` | grep MyElastiCache | awk '{print $3}' | xargs -I {} elasticache-describe-cache-clusters {} --region `{ "Ref" : "AWS::Region" }` --show-cache-node-info | grep CACHENODE | awk '{print $4 ":" $6}' > `{ "Fn::GetOptionSetting" : { "OptionName" : "NodeListPath", "DefaultValue" : "/var/www/html/nodelist" } }`
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
     "/home/ec2-user/elasticache" : "https://s3.amazonaws.com/elasticache-downloads/AmazonElastiCacheCli-latest.zip"
   
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

1. On your local computer, create a configuration file `node-geddy/myapp/.ebextensions/elasticache_settings.config` with the following snippet\. 

   ```
   option_settings:
     "aws:elasticbeanstalk:customoption" :
        CacheNodeType : cache.m1.small
        NumCacheNodes : 1
        Engine : memcached
        NodeListPath : /var/nodelist
   ```

1. On your local computer, update `node-geddy/myapp/config/production.js`\. Add the following line to the top of the file \(just below the header\)\.

   ```
   var fs = require('fs')
   ```

   Then, add the following snippet just above `modules.exports`\. 

   ```
   var data = fs.readFileSync('/var/nodelist', 'UTF8', function(err) {
       if (err) throw err;
   });
   
   var nodeList = [];
   if (data) {
       var lines = data.split('\n');
       for (var i = 0 ; i < lines.length ; i++) {
           if (lines[i].length > 0) {
               nodeList.push(lines[i]);
           }
       }
   }
   
   if (nodeList) {
       config.sessions = {
         store: 'memcache',
         servers: nodeList,
         key: 'sid',
         expiry: 14*24*60*60
       }
   }
   ```

1. On your local computer, update `node-geddy/myapp/package.json` to include `memcached`\.

   ```
   {
      "name": "Elastic_Beanstalk_Geddy",
      "version": "0.0.1",
      "dependencies": {
         "geddy": "0.6.x",
         "memcached": "*"
      }
   }
   ```

1. Add your files to the local repository, commit your changes, and deploy your updated application\.

   ```
   myapp$ git add .
   myapp$ git commit -m "added elasticache functionality"
   myapp$ git aws.push
   ```

1. Your environment will be updated after a few minutes\. After your environment is green and ready, verify everything worked\. 

   1. Check the [Amazon CloudWatch console](https://console.aws.amazon.com/cloudwatch/home) to view your ElastiCache metrics\. To view your ElastiCache metrics, click **ElastiCache** in the left pane, and then select **ElastiCache: Cache Node Metrics** from the **Viewing** list\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-nodejs-geddy-cloudwatch.png)
**Note**  
Make sure you are looking at the same region that you deployed your application to\.

      If you copy and paste your application URL into another web browser, you should see your CurrItem count go up to 2 after 5 minutes\.

   1. Take a snapshot of your logs, and look in `/var/log/nodejs/nodejs.log`\. For more information about logs, see [Viewing Logs from Your Elastic Beanstalk Environment's Amazon EC2 Instances](using-features.logging.md)\. You should see something similar to the following:

      ```
      "sessions": {
          "key": "sid",
          "expiry": 1209600,
          "store": "memcache",
          "servers": [
            "aws-my-1awjsrz10lnxo.ypsz3t.0001.usw2.cache.amazonaws.com:11211"
          ]
        },
      ```

## Step 7: Clean Up<a name="create_deploy_nodejs_geddy_elasticache_delete"></a>

If you no longer want to run your application, you can clean up by terminating your environment and deleting your application\.

Use the `eb terminate` command to terminate your environment and the `eb delete` command to delete your application\. 

**To terminate your environment**

From the directory where you created your local repository, run `eb terminate`\.

```
$ eb terminate
```

This process can take a few minutes\. Elastic Beanstalk displays a message once the environment is successfully terminated\. 