# Configuring Node\.js with a package\.json file<a name="nodejs-platform-packagejson"></a>

Include a `package.json` file in the root of your project source to specify dependency packages \(use the `dependencies` keyword\) and to provide a start command \(use the `scripts` keyword\)\. Using the `scripts` keyword can replace the legacy `NodeCommand` option in the `aws:elasticbeanstalk:container:nodejs` namespace\.

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

When a `package.json` file is present, Elastic Beanstalk runs `npm install` to install dependencies\. It also uses the `start` command to start your application\.

By default, Elastic Beanstalk installs dependencies in production mode \(`npm install --production`\)\. If you want to install development dependencies on your environment instances, set the `NPM_USE_PRODUCTION` [environment property](environments-cfg-softwaresettings.md#environments-cfg-softwaresettings-console) to `false`\.

You can also use the `engines` keyword in the `package.json` file to specify the Node\.js version you want your application to use, as the following example shows\. This feature replaces the legacy `NodeVersion` option in the `aws:elasticbeanstalk:container:nodejs` namespace\. Be aware that you can only specify a Node\.js version that corresponds with your platform branch\. For example, if you're using the Node\.js 12 platform branch, you can only specify a 12\.x\.y Node\.js version\. For valid Node\.js versions for each platform branch, see [Node\.js](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.nodejs) in the *AWS Elastic Beanstalk Platforms* guide\.

**Example package\.json – Node\.js version**  

```
{
  ...
  "engines": { "node" : "12.16.1" }
}
```

**Note**  
When support for the version of Node\.js that you are using is removed from the platform, you must change or remove the Node\.js version setting prior to doing a [platform update](using-features.platform.upgrade.md)\. This might occur when a security vulnerability is identified for one or more versions of Node\.js\.  
When this happens, attempting to update to a new version of the platform that doesn't support the configured Node\.js version fails\. To avoid needing to create a new environment, change the Node\.js version setting in `package.json` to a Node\.js version that is supported by both the old platform version and the new one, or remove the setting, and then deploy the new source bundle\. Only then perform the platform update\.