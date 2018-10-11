# Setting Up your Node\.js Development Environment<a name="nodejs-devenv"></a>

Set up a Node\.js development environment to test your application locally prior to deploying it to AWS Elastic Beanstalk\. This topic outlines development environment setup steps and links to installation pages for useful tools\.

For common setup steps and tools that apply to all languages, see [Configuring your development environment for use with AWS Elastic Beanstalk](chapter-devenv.md)

**Topics**
+ [Installing Node\.js](#nodejs-devenv-nodejs)
+ [Installing npm](#nodejs-devenv-npm)
+ [Installing the AWS SDK for Node\.js](#nodejs-devenv-awssdk)
+ [Installing Express](#nodejs-devenv-express)
+ [Installing Geddy](#nodejs-devenv-geddy)

## Installing Node\.js<a name="nodejs-devenv-nodejs"></a>

Install Node\.js to run Node\.js applications locally\. If you don't have a preference, get the latest version supported by Elastic Beanstalk\. See [Node\.js](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.nodejs) in the *AWS Elastic Beanstalk Platforms* document for a list of supported versions\.

Download Node\.js at [nodejs\.org](https://nodejs.org/en/)\.

**Note**  
When support for the version of Node\.js that you are using is removed from the platform configuration, you must change or remove the version setting prior to doing a [platform upgrade](using-features.platform.upgrade.md)\. This may occur when a security vulnerability is identified for one or more versions of Node\.js  
When this occurs, attempting to upgrade to a new version of the platform that does not support the configured [NodeVersion](command-options-specific.md#command-options-nodejs) fails\. To avoid needing to create a new environment, change the *NodeVersion* configuration option to a version that is supported by both the old configuration version and the new one, or [remove the option setting](environment-configuration-methods-after.md), and then perform the platform upgrade\.

## Installing npm<a name="nodejs-devenv-npm"></a>

Node\.js uses the npm package manager to helps you install tools and frameworks for use in your application\. Download npm at [npmjs\.com](https://www.npmjs.com/)\.

## Installing the AWS SDK for Node\.js<a name="nodejs-devenv-awssdk"></a>

If you need to manage AWS resources from within your application, install the AWS SDK for JavaScript in Node\.js\. Install the SDK with npm:

```
$ npm install aws-sdk
```

Visit the [AWS SDK for JavaScript in Node\.js](https://aws.amazon.com/sdk-for-node-js/) homepage for more information\.

## Installing Express<a name="nodejs-devenv-express"></a>

Express is a web application framework that runs on Node\.js\.

**To set up your Express development environment on your local computer**

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
   Express server listening on port 3000
   ```

   Press **Ctrl\+C** to stop the server\.

## Installing Geddy<a name="nodejs-devenv-geddy"></a>

Geddy is another web application framework that runs on Node\.js\.

**To set up your Geddy development environment on your local computer**

1. Install Geddy globally so that you have geddy generators or start the server\.

   ```
   ~/node-geddy$ npm install -g geddy
   ```

1. Depending on your operating system, you may need to set your path to run the `geddy`code> command\. If you need to set your path, use the output from the previous step when you installed Geddy\. The following is an example\. 

   ```
   ~/node-geddy$ export:PATH=$PATH:/usr/local/share/npm/bin/geddy
   ```

1. Create the directory for your application\.

   ```
   ~/node-geddy$ geddy app myapp
   ~/node-geddy$ cd myapp
   ```

1. Start the server\. Verify everything is working, and then stop the server\.

   ```
   ~/node-geddy/myapp$ geddy
   ~/node-geddy/myapp$ curl localhost:4000
   ```

   Press **Ctrl\+C** to stop the server\.