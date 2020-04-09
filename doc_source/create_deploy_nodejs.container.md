# Using the Elastic Beanstalk Node\.js platform<a name="create_deploy_nodejs.container"></a>

The AWS Elastic Beanstalk Node\.js platform is a [platform version](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.nodejs) for Node\.js web applications that can run behind an nginx proxy server, behind an Apache server, or standalone\.

Elastic Beanstalk provides [configuration options](command-options.md) that you can use to customize the software that runs on the EC2 instances in your Elastic Beanstalk environment\. You can choose which proxy server to run in front of your application, choose a specific version of Node\.js to run, and choose the command used to run your application\. You can also configure environment variables needed by your application and enable log rotation to Amazon S3\.

Platform\-specific configuration options are available in the AWS Management Console for [modifying the configuration of a running environment](environment-configuration-methods-after.md)\. To avoid losing your environment's configuration when you terminate it, you can use [saved configurations](environment-configuration-savedconfig.md) to save your settings and later apply them to another environment\.

To save settings in your source code, you can include [configuration files](ebextensions.md)\. Settings in configuration files are applied every time you create an environment or deploy your application\. You can also use configuration files to install packages, run scripts, and perform other instance customization operations during deployments\.

You can [include a `Package.json` file](nodejs-platform-packagejson.md) in your source bundle to install packages during deployment, and an [`npm-shrinkwrap.json` file](nodejs-platform-shrinkwrap.md) to lock down dependency versions\.

The Node\.js platform includes a proxy server to serve static assets, forward traffic to your application, and compress responses\. You can [extend or override the default proxy configuration](nodejs-platform-proxy.md) for advanced scenarios\.

In terms of supported language version, the Node\.js platform is slightly different than other Elastic Beanstalk managed platforms\. Each Node\.js platform version supports a few Node\.js language versions\. You can use the Elastic Beanstalk console to update the Node\.js version that your environment uses\. The following sections discuss two ways for doing so—using a platform\-specific configuration option, or as part of updating your platform version\.

**Note**  
When support for the version of Node\.js that you are using is removed from the platform, you must change or remove the version setting prior to doing a [platform update](using-features.platform.upgrade.md)\. This might occur when a security vulnerability is identified for one or more versions of Node\.js\.  
When this happens, attempting to update to a new version of the platform that doesn't support the configured [NodeVersion](command-options-specific.md#command-options-nodejs) fails\. To avoid needing to create a new environment, change the *NodeVersion* configuration option to a Node\.js version that is supported by both the old platform version and the new one, or [remove the option setting](environment-configuration-methods-after.md), and then perform the platform update\.

Settings applied in the AWS Management Console override the same settings in configuration files, if they exist\. This lets you have default settings in configuration files, and override them with environment\-specific settings in the console\. For more information about precedence, and other methods of changing settings, see [Configuration options](command-options.md)\.

## Configuring your Node\.js environment<a name="nodejs-platform-console"></a>

The Node\.js settings lets you fine\-tune the behavior of your Amazon EC2 instances and enable or disable Amazon S3 log rotation\. You can edit the Elastic Beanstalk environment's Amazon EC2 instance configuration using the Elastic Beanstalk console\.

**To configure your Node\.js environment in the Elastic Beanstalk console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and then, in the regions drop\-down list, select your region\.

1. In the navigation pane, choose **Environments**, and then choose your environment's name on the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Configuration**\.

1. In the **Software** configuration category, choose **Edit**\.

### Container options<a name="nodejs-platform-console-settings"></a>

On the configuration page, specify the following:
+ **Proxy server**– Specifies which web server to use to proxy connections to Node\.js\. By default, nginx is used\. If you select **none**, static file mappings do not take effect, and gzip compression is disabled\.
+ **Node\.js version**– Specifies the version of Node\.js\. For a list of supported Node\.js versions, see [Node\.js](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.nodejs) in the *AWS Elastic Beanstalk Platforms* guide\.
+ **Gzip compression**– Specifies whether gzip compression is enabled\. By default, gzip compression is enabled\.
+ **Node command**–Lets you enter the command used to start the Node\.js application\. An empty string \(the default\) means Elastic Beanstalk will use `app.js`, then `server.js`, and then `npm start` in that order\.

### Log options<a name="nodejs-platform-console-logging"></a>

The Log Options section has two settings:
+ **Instance profile**– Specifies the instance profile that has permission to access the Amazon S3 bucket associated with your application\.
+ **Enable log file rotation to Amazon S3**–Specifies whether log files for your application's Amazon EC2 instances should be copied to your Amazon S3 bucket associated with your application\.

### Static files<a name="nodejs-platform-console-staticfiles"></a>

To improve performance, you can configure the proxy server to serve static files \(for example, HTML or images\) from a set of directories inside your web application\. When the proxy server receives a request for a file under the specified path, it serves the file directly instead of routing the request to your application\. You can set the virtual path and directory mappings in the **Static Files** section of the **Modify software** configuration page\.

For details about configuring static files using the Elastic Beanstalk console, see [Serving static files](environment-cfg-staticfiles.md)\.

### Environment properties<a name="nodejs-platform-console-envprops"></a>

The **Environment Properties** section lets you specify environment configuration settings on the Amazon EC2 instances that are running your application\. These settings are passed in as key\-value pairs to the application\.

Inside the Node\.js environment running in AWS Elastic Beanstalk, you can access the environment variables using `process.env.ENV_VARIABLE` similar to the following example\.

```
var endpoint = process.env.API_ENDPOINT
```

The Node\.js platform sets the PORT environment variable to the port to which the proxy server passes traffic\. See [Configuring the proxy server](nodejs-platform-proxy.md)\.

See [Environment properties and other software settings](environments-cfg-softwaresettings.md) for more information\.

## Update your Node\.js version<a name="nodejs-platform-version"></a>

You can use the Elastic Beanstalk console to update the Node\.js version that your environment uses as part of updating the platform version\. For a list of supported Node\.js versions, see [Node\.js](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.nodejs) in the *AWS Elastic Beanstalk Platforms* guide\.

**To configure your environment's Node\.js version in the Elastic Beanstalk console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and then, in the regions drop\-down list, select your region\.

1. In the navigation pane, choose **Environments**, and then choose your environment's name on the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. On the environment overview page, under **Platform**, choose **Change**\.

1. On the **Update platform version** dialog, select a Node\.js version\.  
![\[Elastic Beanstalk update platform version confirmation\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/platform-nodejs-update-node-version.png)

1. Choose **Save**\.

## Node\.js configuration namespaces<a name="nodejs-namespaces"></a>

You can use a [configuration file](ebextensions.md) to set configuration options and perform other instance configuration tasks during deployments\. Configuration options can be defined by the Elastic Beanstalk service or the platform that you use and are organized into *namespaces*\.

The Node\.js platform defines options in the `aws:elasticbeanstalk:container:nodejs:staticfiles` and `aws:elasticbeanstalk:container:nodejs` namespaces\.

The following configuration file tells Elastic Beanstalk to use `npm start` to run the application, sets the proxy type to Apache, enables compression, and configures the proxy to serve static files from two source directories: HTML files at the `html` path under the website's root from the `statichtml` source directory, and image files at the `images` path under the website's root from the `staticimages` source directory\.

**Example \.ebextensions/node\-settings\.config**  

```
option_settings:
  aws:elasticbeanstalk:container:nodejs: 
    NodeCommand: "npm start"
    ProxyServer: apache
    GzipCompression: true
  aws:elasticbeanstalk:container:nodejs:staticfiles:
    /html: statichtml
    /images: staticimages
```

Elastic Beanstalk provides many configuration options for customizing your environment\. In addition to configuration files, you can also set configuration options using the console, saved configurations, the EB CLI, or the AWS CLI\. See [Configuration options](command-options.md) for more information\.

## Amazon Linux 2 considerations<a name="nodejs-al2"></a>


|  | 
| --- |
| AWS Elastic Beanstalk support for Amazon Linux 2 is in beta release and is subject to change\. | 

Elastic Beanstalk provides Amazon Linux 2 Node\.js platform branches\. The platform versions in these branches are different than previous Node\.js platform versions based on Amazon Linux AMI in a few ways, both generic \(apply to all Amazon Linux 2 platforms\) and platform specific \(apply to Amazon Linux 2 Node\.js platform versions\)\. For details, see [Migrating your Elastic Beanstalk Linux application to Amazon Linux 2](using-features.migration-al.md)\.

Amazon Linux 2 Node\.js platform versions have some new functionality\.

You can add a `Procfile` to your source bundle to specify the command that starts your application, as the following example shows\. This feature replaces the legacy `NodeCommand` option in the `aws:elasticbeanstalk:container:nodejs` namespace\.

**Example Procfile**  

```
web: node index.js
```

For details about `Procfile` usage, expand the *Buildfile and Procfile* section in [Extending Elastic Beanstalk Linux platforms](platforms-linux-extend.md)\.

When you don't provide a `Procfile`, Elastic Beanstalk behaves as before\. It runs `npm start` if you provide a `package.json` file\. If you don't provide that either, Elastic Beanstalk looks for the file `app.js` or `server.js`, in this order, and runs it\.