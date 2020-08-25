# Getting started with Node\.js on Elastic Beanstalk<a name="nodejs-getstarted"></a>

To get started with Node\.js applications on AWS Elastic Beanstalk, all you need is an application [source bundle](applications-sourcebundle.md) to upload as your first application version and to deploy to an environment\. When you create an environment, Elastic Beanstalk allocates all of the AWS resources needed to run a highly scalable web application\.

## Launching an environment with a sample Node\.js application<a name="nodejs-getstarted-samples"></a>

Elastic Beanstalk provides single page sample applications for each platform as well as more complex examples that show the use of additional AWS resources such as Amazon RDS and language or platform\-specific features and APIs\.


**Samples**  

|  Environment type  |  Source bundle  |  Description  | 
| --- | --- | --- | 
|  Web Server  |   [nodejs\.zip](samples/nodejs.zip)   |  Single page application\. Use the procedure at [Create an Example Application](GettingStarted.CreateApp.md) to launch this example\.  | 
|  Web Server with Amazon RDS  |  [nodejs\-express\-hiking\-v1\.zip](samples/nodejs-express-hiking-v1.zip)  |  Hiking log application that uses the Express framework and an RDS database\. [Tutorial](create_deploy_nodejs_express.md)  | 
|  Web Server with Amazon ElastiCache  |  [nodejs\-example\-express\-elasticache\.zip](samples/nodejs-example-express-elasticache.zip)  |  Express web application that uses Amazon ElastiCache for clustering\. Clustering enhances your web application's high availability, performance, and security\. [Tutorial](nodejs-express-clustering.md)  | 
|  Web Server with DynamoDB, Amazon SNS and Amazon SQS  |  [eb\-node\-express\-sample\-v1\.0\.zip](https://github.com/awslabs/eb-node-express-sample/releases/download/v1.0/eb-node-express-sample-v1.0.zip) [Clone the repo at GitHub\.com](https://github.com/awslabs/eb-node-express-sample)  |  Express web site that collects user contact information for a new company's marketing campaign\. Uses the AWS SDK for JavaScript in Node\.js to write entries to a DynamoDB table, and Elastic Beanstalk configuration files to create resources in DynamoDB, Amazon SNS and Amazon SQS\. [Tutorial](nodejs-dynamodb-tutorial.md)  | 

## Next steps<a name="nodejs-getstarted-next"></a>

After you have an environment running an application, you can deploy a new version of the application or a completely different application at any time\. Deploying a new application version is very quick because it doesn't require provisioning or restarting EC2 instances\. For details about application deployment, see [Deploy a New Version of Your Application](GettingStarted.DeployApp.md)\.

After you've deployed a sample application or two and are ready to start developing and running Node\.js applications locally, see [the next section](nodejs-devenv.md) to set up a Node\.js development environment with all of the tools that you will need\.