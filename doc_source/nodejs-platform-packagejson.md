# Configuring Node\.js with a package\.json file<a name="nodejs-platform-packagejson"></a>

## Configuring your application<a name="nodejs-platform-packagejson-"></a>

Use a `package.json` file in the root of your project source to specify dependency packages and to provide a start command\.

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

## Amazon Linux 2 considerations<a name="nodejs-al2"></a>


|  | 
| --- |
| AWS Elastic Beanstalk support for Amazon Linux 2 is in beta release and is subject to change\. | 

On Amazon Linux 2 Node\.js platform versions, you can use the `package.json` file to specify the Node\.js version you want your application to use, as the following example shows\. This feature replaces the legacy `NodeVersion` option in the `aws:elasticbeanstalk:container:nodejs` namespace\. Be aware that you can only specify a Node\.js version that corresponds with your platform branch\. For example, if you're using the Node\.js 12 platform branch, you can only specify a 12\.x\.y Node\.js version\.

**Example package\.json – Node\.js version**  

```
{
  ...
  "engines": { "node" : "12.16.1" }
}
```