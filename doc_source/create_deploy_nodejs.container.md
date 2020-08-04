# Using the Elastic Beanstalk Node\.js platform<a name="create_deploy_nodejs.container"></a>

**Important**  
Amazon Linux 2 platform versions are fundamentally different than Amazon Linux AMI platform versions \(preceding Amazon Linux 2\)\. These different platform generations are incompatible in several ways\. If you are migrating to an Amazon Linux 2 platform version, be sure to read the information in [Migrating your Elastic Beanstalk Linux application to Amazon Linux 2](using-features.migration-al.md)\.

The AWS Elastic Beanstalk Node\.js platform is a set of [platform versions](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.nodejs) for Node\.js web applications that  run behind an nginx proxy server\. 

Elastic Beanstalk provides [configuration options](command-options.md) that you can use to customize the software that runs on the EC2 instances in your Elastic Beanstalk environment\. You can configure environment variables needed by your application, enable log rotation to Amazon S3, and map folders in your application source that contain static files to paths served by the proxy server\.

Configuration options are available in the Elastic Beanstalk console for [modifying the configuration of a running environment](environment-configuration-methods-after.md)\. To avoid losing your environment's configuration when you terminate it, you can use [saved configurations](environment-configuration-savedconfig.md) to save your settings and later apply them to another environment\.

To save settings in your source code, you can include [configuration files](ebextensions.md)\. Settings in configuration files are applied every time you create an environment or deploy your application\. You can also use configuration files to install packages, run scripts, and perform other instance customization operations during deployments\.

You can [include a `Package.json` file](nodejs-platform-dependencies.md#nodejs-platform-packagejson) in your source bundle to install packages during deployment, to provide a start command, and to specify the Node\.js version you want your application to use\. You can include an [`npm-shrinkwrap.json` file](nodejs-platform-shrinkwrap.md) to lock down dependency versions\.

The Node\.js platform includes a proxy server to serve static assets, forward traffic to your application, and compress responses\. You can [extend or override the default proxy configuration](nodejs-platform-proxy.md) for advanced scenarios\.

You can add a `Procfile` to your source bundle to specify the command that starts your application, as the following example shows\. This feature replaces the legacy `NodeCommand` option in the `aws:elasticbeanstalk:container:nodejs` namespace\.

**Example Procfile**  

```
web: node index.js
```

For details about `Procfile` usage, expand the *Buildfile and Procfile* section in [Extending Elastic Beanstalk Linux platforms](platforms-linux-extend.md)\.

When you don't provide a `Procfile`, Elastic Beanstalk runs `npm start` if you provide a `package.json` file\. If you don't provide that either, Elastic Beanstalk looks for the file `app.js` or `server.js`, in this order, and runs it\.

Settings applied in the Elastic Beanstalk console override the same settings in configuration files, if they exist\. This lets you have default settings in configuration files, and override them with environment\-specific settings in the console\. For more information about precedence, and other methods of changing settings, see [Configuration options](command-options.md)\.

For details about the various ways you can extend an Elastic Beanstalk Linux\-based platform, see [Extending Elastic Beanstalk Linux platforms](platforms-linux-extend.md)\.

## Configuring your Node\.js environment<a name="nodejs-platform-console"></a>

The Node\.js platform settings let you fine\-tune the behavior of your Amazon EC2 instances\. You can edit the Elastic Beanstalk environment's Amazon EC2 instance configuration using the Elastic Beanstalk console\.

Use the Elastic Beanstalk console to enable log rotation to Amazon S3 and configure variables that your application can read from the environment\.

**To configure your Node\.js environment in the Elastic Beanstalk console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Configuration**\.

1. In the **Software** configuration category, choose **Edit**\.

### Container options<a name="nodejs-platform-console-settings"></a>

You can specify these platform\-specific options:
+ **Proxy server** – The proxy server to use on your environment instances\. By default, nginx is used\.

### Log options<a name="nodejs-platform-console-logging"></a>

The **Log Options** section has two settings:
+ **Instance profile** – Specifies the instance profile that has permission to access the Amazon S3 bucket associated with your application\.
+ **Enable log file rotation to Amazon S3** – Specifies whether log files for your application's Amazon EC2 instances should be copied to the Amazon S3 bucket associated with your application\.

### Static files<a name="nodejs-platform-console-staticfiles"></a>

To improve performance, the **Static files** section lets you configure the proxy server to serve static files \(for example, HTML or images\) from a set of directories inside your web application\. For each directory, you set the virtual path to directory mapping\. When the proxy server receives a request for a file under the specified path, it serves the file directly instead of routing the request to your application\.

For details about configuring static files using the Elastic Beanstalk console, see [Serving static files](environment-cfg-staticfiles.md)\.

### Environment properties<a name="nodejs-platform-console-envprops"></a>

The **Environment Properties** section lets you specify environment configuration settings on the Amazon EC2 instances that are running your application\. These settings are passed in as key\-value pairs to the application\.

Inside the Node\.js environment running in AWS Elastic Beanstalk, you can access the environment variables using `process.env.ENV_VARIABLE` similar to the following example\.

```
var endpoint = process.env.API_ENDPOINT
```

The Node\.js platform sets the PORT environment variable to the port to which the proxy server passes traffic\. See [Configuring the proxy server](nodejs-platform-proxy.md)\.

See [Environment properties and other software settings](environments-cfg-softwaresettings.md) for more information\.

### Configuring an Amazon Linux AMI \(preceding Amazon Linux 2\) Node\.js environment<a name="nodejs-platform-console.alami"></a>

The following console software configuration categories are supported only on an Elastic Beanstalk Node\.js environment that uses an Amazon Linux AMI platform version \(preceding Amazon Linux 2\)\.

#### Container options<a name="nodejs-platform-console-settings"></a>

On the configuration page, specify the following:
+ **Proxy server** – Specifies which web server to use to proxy connections to Node\.js\. By default, nginx is used\. If you select **none**, static file mappings do not take effect, and gzip compression is disabled\.
+ **Node\.js version** – Specifies the version of Node\.js\. For a list of supported Node\.js versions, see [Node\.js](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.nodejs) in the *AWS Elastic Beanstalk Platforms* guide\.
+ **Gzip compression** – Specifies whether gzip compression is enabled\. By default, gzip compression is enabled\.
+ **Node command** – Lets you enter the command used to start the Node\.js application\. An empty string \(the default\) means Elastic Beanstalk will use `app.js`, then `server.js`, and then `npm start` in that order\.

## Node\.js configuration namespace<a name="nodejs-namespaces"></a>

You can use a [configuration file](ebextensions.md) to set configuration options and perform other instance configuration tasks during deployments\. Configuration options can be defined by the Elastic Beanstalk service or the platform that you use and are organized into *namespaces*\.

You can choose the proxy to use on your environment's instances by using the `aws:elasticbeanstalk:environment:proxy` namespace\. The following example configures your environment to use the Apache HTTPD proxy server\.

**Example \.ebextensions/nodejs\-settings\.config**  

```
option_settings:
  aws:elasticbeanstalk:environment:proxy:
    ProxyServer: apache
```

You can configure the proxy to serve static files by using the `aws:elasticbeanstalk:environment:proxy:staticfiles` namespace\. For details and an example, see [Serving static files](environment-cfg-staticfiles.md)\.

Elastic Beanstalk provides many configuration options for customizing your environment\. In addition to configuration files, you can also set configuration options using the console, saved configurations, the EB CLI, or the AWS CLI\. See [Configuration options](command-options.md) for more information\.

## The Amazon Linux AMI \(preceding Amazon Linux 2\) Node\.js platform<a name="nodejs.alami"></a>

If your Elastic Beanstalk Node\.js environment uses an Amazon Linux AMI platform version \(preceding Amazon Linux 2\), read the additional information in this section\.

### Node\.js platform\-specific configuration options<a name="nodejs.alami.options"></a>

Elastic Beanstalk supports some platform\-specific configurations options for Amazon Linux AMI Node\.js platform versions\. You can choose which proxy server to run in front of your application, choose a specific version of Node\.js to run, and choose the command used to run your application\.

For proxy server, in addition to nginx, you can choose the Apache proxy server\. In addition, you can set the `none` value to the `ProxyServer` option\. In this case, Elastic Beanstalk runs your application as standalone, not behind any proxy server\. If your environment runs a standalone application, update your code to listen to the port that nginx forwards traffic to\.

```
var port = process.env.PORT || 8080;

app.listen(port, function() {
  console.log('Server running at http://127.0.0.1:%s', port);
});
```

### Node\.js language versions<a name="nodejs.alami.versions"></a>

In terms of supported language version, the Node\.js Amazon Linux AMI platform is slightly different than other Elastic Beanstalk managed platforms\. Each Node\.js platform version supports a few Node\.js language versions\. For a list of supported Node\.js versions, see [Node\.js](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.nodejs) in the *AWS Elastic Beanstalk Platforms* guide\.

You can use a platform\-specific configuration option to set the language version\. For details, see [Configuring your Node\.js environment](#nodejs-platform-console)\. Alternatively, you can use the Elastic Beanstalk console to update the Node\.js version that your environment uses as part of updating your platform version, as shown in the following procedure\.

**Note**  
When support for the version of Node\.js that you are using is removed from the platform, you must change or remove the version setting prior to doing a [platform update](using-features.platform.upgrade.md)\. This might occur when a security vulnerability is identified for one or more versions of Node\.js\.  
When this happens, attempting to update to a new version of the platform that doesn't support the configured [NodeVersion](command-options-specific.md#command-options-nodejs) fails\. To avoid needing to create a new environment, change the *NodeVersion* configuration option to a Node\.js version that is supported by both the old platform version and the new one, or [remove the option setting](environment-configuration-methods-after.md), and then perform the platform update\.

**To configure your environment's Node\.js version in the Elastic Beanstalk console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. On the environment overview page, under **Platform**, choose **Change**\.

1. On the **Update platform version** dialog, select a Node\.js version\.  
![\[Elastic Beanstalk update platform version confirmation\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/platform-nodejs-update-node-version.png)

1. Choose **Save**\.

### Node\.js configuration namespaces<a name="nodejs.alami.namespaces"></a>

The Node\.js Amazon Linux AMI platform defines additional options in the `aws:elasticbeanstalk:container:nodejs:staticfiles` and `aws:elasticbeanstalk:container:nodejs` namespaces\.

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