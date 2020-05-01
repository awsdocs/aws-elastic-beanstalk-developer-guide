# Setting up your Node\.js development environment<a name="nodejs-devenv"></a>

Set up a Node\.js development environment to test your application locally prior to deploying it to AWS Elastic Beanstalk\. This topic outlines development environment setup steps and links to installation pages for useful tools\.

For common setup steps and tools that apply to all languages, see [Configuring your development machine for use with Elastic Beanstalk](chapter-devenv.md)\.

**Topics**
+ [Installing Node\.js](#nodejs-devenv-nodejs)
+ [Installing npm](#nodejs-devenv-npm)
+ [Installing the AWS SDK for Node\.js](#nodejs-devenv-awssdk)
+ [Installing Express](#nodejs-devenv-express)

## Installing Node\.js<a name="nodejs-devenv-nodejs"></a>

Install Node\.js to run Node\.js applications locally\. If you don't have a preference, get the latest version supported by Elastic Beanstalk\. See [Node\.js](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.nodejs) in the *AWS Elastic Beanstalk Platforms* document for a list of supported versions\.

Download Node\.js at [nodejs\.org](https://nodejs.org/en/)\.

## Installing npm<a name="nodejs-devenv-npm"></a>

Node\.js uses the npm package manager to helps you install tools and frameworks for use in your application\. Download npm at [npmjs\.com](https://www.npmjs.com/)\.

## Installing the AWS SDK for Node\.js<a name="nodejs-devenv-awssdk"></a>

If you need to manage AWS resources from within your application, install the AWS SDK for JavaScript in Node\.js\. Install the SDK with npm:

```
$ npm install aws-sdk
```

Visit the [AWS SDK for JavaScript in Node\.js](https://aws.amazon.com/sdk-for-node-js/) homepage for more information\.

## Installing Express<a name="nodejs-devenv-express"></a>

Express is a web application framework that runs on Node\.js\. To use it, set up Express and create the project structure\. The following walks you through setting up Express on a Linux operating system\.

**Note**  
Depending on your permission level to system directories, you might need to prefix some of these commands with `sudo`\.

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

1. Run the `express` command\. This generates `package.json`, `app.js`, and a few directories\.

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

   The server runs on port 3000 by default\. To test it, run `curl http://localhost:3000` in another terminal, or open a browser on the local computer and go to `http://localhost:3000`\.

   Press **Ctrl\+C** to stop the server\.