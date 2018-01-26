# Using the AWS Elastic Beanstalk Node\.js Platform<a name="create_deploy_nodejs.container"></a>

The AWS Elastic Beanstalk Node\.js platform is a platform configuration for Node\.js web applications that can run behind an nginx proxy server, behind an Apache server, or standalone\.

Elastic Beanstalk provides configuration options that you can use to customize the software that runs on the EC2 instances in your Elastic Beanstalk environment\. You can choose which proxy server to run in front of your application, choose a specific version of Node\.js to run, and choose the command used to run your application\. You can also configure environment variables needed by your application and enable log rotation to Amazon S3\.

Platform\-specific configuration options are available in the AWS Management Console for modifying the configuration of a running environment\. To avoid losing your environment's configuration when you terminate it, you can use saved configurations to save your settings and later apply them to another environment\.

To save settings in your source code, you can include configuration files\. Settings in configuration files are applied every time you create an environment or deploy your application\. You can also use configuration files to install packages, run scripts, and perform other instance customization operations during deployments\.

You can include a `Package.json` file in your source bundle to install packages during deployment, and an `npm-shrinkwrap.json` file to lock down dependency versions\.

The Node\.js platform includes a proxy server to serve static assets, forward traffic to your application, and compress responses\. You can extend or override the default proxy configuration for advanced scenarios\.

Settings applied in the AWS Management Console override the same settings in configuration files, if they exist\. This lets you have default settings in configuration files, and override them with environment specific settings in the console\. For more information about precedence, and other methods of changing settings, see \.

## Configuring Your Node\.js Environment<a name="nodejs-platform-console"></a>

The Node\.js settings lets you fine\-tune the behavior of your Amazon EC2 instances and enable or disable Amazon S3 log rotation\. You can edit the Elastic Beanstalk environment's Amazon EC2 instance configuration using the AWS Management Console\.

**To configure your Node\.js environment in the Elastic Beanstalk console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the management page for your environment\.

1. Choose **Configuration**\.

1. On the **Software** configuration card, choose **Modify**\.

### Container Options<a name="nodejs-platform-console-settings"></a>

On the configuration page, specify the following:

+ **Proxy Server**– Specifies which web server to use to proxy connections to Node\.js\. By default, nginx is used\. If you select **none**, static file mappings do not take effect, and gzip compression is disabled\.

+ **Node Version**– Specifies the version of Node\.js\. For information about what versions are supported, see [Elastic Beanstalk Supported Platforms](concepts.platforms.md)\.
**Note**  
When support for the version of Node\.js that you are using is removed from the platform configuration, you must change or remove the version setting prior to doing a platform upgrade\. This may occur when a security vulnerability is identified for one or more versions of Node\.js  
When this occurs, attempting to upgrade to a new version of the platform that does not support the configured NodeVersion fails\. To avoid needing to create a new environment, change the *NodeVersion* configuration option to a version that is supported by both the old configuration version and the new one, or remove the option setting, and then perform the platform upgrade\.

+ **Gzip Compression**– Specifies whether gzip compression is enabled\. By default, gzip compression is enabled\.

+ **Node Command**–Lets you enter the command used to start the Node\.js application\. An empty string \(the default\) means Elastic Beanstalk will use `app.js`, then `server.js`, and then `npm start` in that order\.

### Log Options<a name="nodejs-platform-console-logging"></a>

The Log Options section has two settings:

+ **Instance profile**– Specifies the instance profile that has permission to access the Amazon S3 bucket associated with your application\.

+ **Enable log file rotation to Amazon S3**–Specifies whether log files for your application's Amazon EC2 instances should be copied to your Amazon S3 bucket associated with your application\.

### Static Files<a name="nodejs-platform-console-staticfiles"></a>

To improve performance, you may want to configure nginx or Apache to server static files \(for example, HTML or images\) from a set of directories inside your web application\. You can set the virtual path and directory mapping on the **Container** tab in the **Static Files** section\. To add multiple mappings, click **Add Path**\. To remove a mapping, click **Remove**\.

### Environment Properties<a name="nodejs-platform-console-envprops"></a>

The **Environment Properties** section lets you specify environment configuration settings on the Amazon EC2 instances that are running your application\. These settings are passed in as key\-value pairs to the application\.

Inside the Node\.js environment running in AWS Elastic Beanstalk, you can access the environment variables using `process.env.ENV_VARIABLE` similar to the following example\.

```
var endpoint = process.env.API_ENDPOINT
```

The Node\.js platform sets the PORT environment variable to the port to which the proxy server passes traffic\. See \.

See  for more information\.

## Node\.js Configuration Namespaces<a name="nodejs-namespaces"></a>

You can use a configuration file to set configuration options and perform other instance configuration tasks during deployments\. Configuration options can be defined by the Elastic Beanstalk service or the platform that you use and are organized into *namespaces*\.

The Node\.js platform defines options in the `aws:elasticbeanstalk:container:nodejs:staticfiles` and `aws:elasticbeanstalk:container:nodejs` namespaces\.

The following configuration file tells Elastic Beanstalk to use `npm start` to run the application, sets the proxy type to Apache, enables compression, and configures the proxy to serve static images from the `myimages` directory at the `/images` path\.

**Example \.ebextensions/node\-settings\.config**  

```
option_settings:
  aws:elasticbeanstalk:container:nodejs: 
    NodeCommand: "npm start"
    ProxyServer: apache
    GzipCompression: true
  aws:elasticbeanstalk:container:nodejs:staticfiles:
    /images: myimages
```

Elastic Beanstalk provides many configuration options for customizing your environment\. In addition to configuration files, you can also set configuration options using the console, saved configurations, the EB CLI, or the AWS CLI\. See  for more information\.