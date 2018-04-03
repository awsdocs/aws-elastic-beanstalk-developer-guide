# Installing Packages with a Package\.json File<a name="nodejs-platform-packagejson"></a>

Use a `package.json` file in the root of your project source to use npm to install packages that your application requires\.

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

When a `package.json` file is present, Elastic Beanstalk runs `npm install` to install dependencies\.

By default, Elastic Beanstalk installs dependencies in production mode \(`npm install --production`\)\. If you want to install development dependencies on your environment instances, set the `NPM_USE_PRODUCTION` [environment property](environments-cfg-softwaresettings.md#environments-cfg-softwaresettings-console) to `false`\.