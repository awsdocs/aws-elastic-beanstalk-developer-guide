# Configuring your application's dependencies<a name="nodejs-platform-dependencies"></a>

Your application might have dependencies on some Node\.js modules, like the ones you specify in `require()` statements\. You can specify these dependencies using a `package.json` file\. Alternatively, you can include your application's dependencies in the source bundle and deploy them with the application\. These two alternative methods are detailed in the following sections\.

## Specifying Node\.js dependencies with a package\.json file<a name="nodejs-platform-packagejson"></a>

Include a `package.json` file in the root of your project source to specify dependency packages and to provide a start command\. When a `package.json` file is present, Elastic Beanstalk runs `npm install` to install dependencies\. It also uses the `start` command to start your application\. For more information about the `package.json` file, see [the `package.json` guide](https://nodejs.dev/learn/the-package-json-guide) on the Node\.js website\.

Use the `scripts` keyword to provide a start command\. The `scripts` keyword is now used instead of the legacy `NodeCommand` option in the `aws:elasticbeanstalk:container:nodejs` namespace\.

**Example package\.json – Express**  

```
{
    "name": "my-app",
    "version": "0.0.1",
    "private": true,
    "dependencies": {
      "ejs": "latest",
      "aws-sdk": "latest",
      "express": "latest",
      "body-parser": "latest"
    },
    "scripts": {
      "start": "node app.js"
    }
  }
```

Use the `engines` keyword in the `package.json` file to specify the Node\.js version that you want your application to use\. You can also specify a version range using npm notation\. For more information about the syntax for version ranges, see [Semantic Versioning using npm](https://nodejs.dev/learn/semantic-versioning-using-npm) on the Node\.js website\. The `engines` keyword in the Node\.js `package.json` file replaces the legacy `NodeVersion` option in the `aws:elasticbeanstalk:container:nodejs` namespace\.

**Example `package.json` – Single Node\.js version**  

```
{
    ...
    "engines": { "node" : "14.16.0" }
  }
```

**Example `package.json` – Node\.js version range**  

```
{
    ...
    "engines": { "node" : ">=10 <11" }
  }
```

When a version range is indicated, Elastic Beanstalk installs the latest Node\.js version that the platform has available within the range\. In this example, the range indicates that the version must be greater than or equal to version 10, but less then version 11\. As a result, Elastic Beanstalk installs the latest Node\.js version 10\.x\.y, which is available on the [supported platform](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.nodejs)\. 

Be aware that you can only specify a Node\.js version that corresponds with your platform branch\. For example, if you're using the Node\.js 14 platform branch, you can only specify a 14\.x\.y Node\.js version\. You can use the version range options supported by npm to allow for more flexibility\. For valid Node\.js versions for each platform branch, see [Node\.js](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.nodejs) in the *AWS Elastic Beanstalk Platforms* guide\. 

By default, Elastic Beanstalk installs dependencies in production mode \(`npm install --production`\)\. If you want to install development dependencies on your environment instances, set the `NPM_USE_PRODUCTION` [environment property](environments-cfg-softwaresettings.md#environments-cfg-softwaresettings-console) to `false`\.

**Note**  
When support for the version of Node\.js that you are using is removed from the platform, you must change or remove the Node\.js version setting prior to doing a [platform update](using-features.platform.upgrade.md)\. This might occur when a security vulnerability is identified for one or more versions of Node\.js\.  
When this happens, attempting to update to a new version of the platform that doesn't support the configured Node\.js version fails\. To avoid needing to create a new environment, change the Node\.js version setting in `package.json` to a Node\.js version that is supported by both the old platform version and the new one\. You have the option to specify a Node\.js version range that includes a supported version, as described earlier in this topic\. You also have the option to remove the setting, and then deploy the new source bundle\. 

## Including Node\.js dependencies in a node\_modules directory<a name="nodejs-platform-nodemodules"></a>

To deploy dependency packages to environment instances together with your application code, include them in a directory that's named `node_modules` in the root of your project source\. Node\.js looks for dependencies in this directory\. For instructions, see [Loading from node\_modules Folders](https://nodejs.org/api/modules.html#modules_loading_from_node_modules_folders) in the Node\.js documentation\.

**Note**  
When you deploy a `node_modules` directory to an Amazon Linux 2 Node\.js platform version, Elastic Beanstalk assumes that you're providing your own dependency packages, and avoids installing dependencies specified in a [package\.json](#nodejs-platform-packagejson) file\.