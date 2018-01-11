# Deploying Node\.js Applications to AWS Elastic Beanstalk<a name="create_deploy_nodejs"></a>


+ [Getting Started with Node\.js on Elastic Beanstalk](nodejs-getstarted.md)
+ [Setting Up your Node\.js Development Environment](nodejs-devenv.md)
+ [Using the AWS Elastic Beanstalk Node\.js Platform](create_deploy_nodejs.container.md)
+ [Deploying an Express Application to Elastic Beanstalk](create_deploy_nodejs_express.md)
+ [Deploying a Node\.js Application with DynamoDB to Elastic Beanstalk](nodejs-dynamodb-tutorial.md)
+ [Deploying a Geddy Application with Clustering to Elastic Beanstalk](create_deploy_nodejs_geddy_elasticache.md)
+ [Adding an Amazon RDS DB Instance to your Node\.js Application Environment](create-deploy-nodejs.rds.md)
+ [Resources](create_deploy_nodejs.resources.md)

Elastic Beanstalk for Node\.js makes it easy to deploy, manage, and scale your Node\.js web applications using Amazon Web Services\. Elastic Beanstalk for Node\.js is available to anyone developing or hosting a web application using Node\.js\. This chapter provides step\-by\-step instructions for deploying your Node\.js web application to Elastic Beanstalk using the Elastic Beanstalk management console, and provides walkthroughs for common frameworks such as Express and Geddy\.

After you deploy your Elastic Beanstalk application, you can continue to use EB CLI to manage your application and environment, or you can use the Elastic Beanstalk console, AWS CLI, or the APIs\. 

**Note**  
When support for the version of Node\.js that you are using is removed from the platform configuration, you must change or remove the version setting prior to doing a platform upgrade\. This may occur when a security vulnerability is identified for one or more versions of Node\.js  
When this occurs, attempting to upgrade to a new version of the platform that does not support the configured NodeVersion fails\. To avoid needing to create a new environment, change the *NodeVersion* configuration option to a version that is supported by both the old configuration version and the new one, or remove the option setting, and then perform the platform upgrade\.

The topics in this chapter assume some knowledge of Elastic Beanstalk environments\. If you haven't used Elastic Beanstalk before, try the getting started tutorial to learn the basics\.