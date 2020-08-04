# Using the Elastic Beanstalk PHP platform<a name="create_deploy_PHP.container"></a>

**Important**  
Amazon Linux 2 platform versions are fundamentally different than Amazon Linux AMI platform versions \(preceding Amazon Linux 2\)\. These different platform generations are incompatible in several ways\. If you are migrating to an Amazon Linux 2 platform version, be sure to read the information in [Migrating your Elastic Beanstalk Linux application to Amazon Linux 2](using-features.migration-al.md)\.

AWS Elastic Beanstalk supports a number of platforms for different versions of the PHP programming language\. These platforms support PHP web applications that can run alone or under Composer\. Learn more at [PHP](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.PHP) in the *AWS Elastic Beanstalk Platforms* document\.

Elastic Beanstalk provides [configuration options](command-options.md) that you can use to customize the software that runs on the EC2 instances in your Elastic Beanstalk environment\. You can configure environment variables needed by your application, enable log rotation to Amazon S3, map folders in your application source that contain static files to paths served by the proxy server, and set common PHP initialization settings\.

Configuration options are available in the Elastic Beanstalk console for [modifying the configuration of a running environment](environment-configuration-methods-after.md)\. To avoid losing your environment's configuration when you terminate it, you can use [saved configurations](environment-configuration-savedconfig.md) to save your settings and later apply them to another environment\.

To save settings in your source code, you can include [configuration files](ebextensions.md)\. Settings in configuration files are applied every time you create an environment or deploy your application\. You can also use configuration files to install packages, run scripts, and perform other instance customization operations during deployments\.

If you use Composer, you can [include a `composer.json` file](php-configuration-composer.md) in your source bundle to install packages during deployment\.

For advanced PHP configuration and PHP settings that are not provided as configuration options, you can [use configuration files to provide an `INI` file](php-configuration-phpini.md) that can extend and override the default settings applied by Elastic Beanstalk, or install additional extensions\.

Settings applied in the Elastic Beanstalk console override the same settings in configuration files, if they exist\. This lets you have default settings in configuration files, and override them with environment\-specific settings in the console\. For more information about precedence, and other methods of changing settings, see [Configuration options](command-options.md)\.

For details about the various ways you can extend an Elastic Beanstalk Linux\-based platform, see [Extending Elastic Beanstalk Linux platforms](platforms-linux-extend.md)\.

## Configuring your PHP environment<a name="php-console"></a>

You can use the Elastic Beanstalk console to enable log rotation to Amazon S3, configure variables that your application can read from the environment, and change PHP settings\.

**To configure your PHP environment in the Elastic Beanstalk console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Configuration**\.

1. In the **Software** configuration category, choose **Edit**\.

### PHP settings<a name="php-console-settings"></a>
+ **Proxy server** – The proxy server to use on your environment instances\. By default, nginx is used\.
+ **Document root** – The folder that contains your site's default page\. If your welcome page is not at the root of your source bundle, specify the folder that contains it relative to the root path\. For example, `/public` if the welcome page is in a folder named `public`\.
+ **Memory limit** – The maximum amount of memory that a script is allowed to allocate\. For example, `512M`\.
+ **Zlib output compression** – Set to `On` to compress responses\.
+ **Allow URL fopen** – Set to `Off` to prevent scripts from downloading files from remote locations\.
+ **Display errors** – Set to `On` to show internal error messages for debugging\.
+ **Max execution time** – The maximum time in seconds that a script is allowed to run before the environment terminates it\.

### Log options<a name="php-console-logs"></a>

The Log Options section has two settings:
+ **Instance profile**– Specifies the instance profile that has permission to access the Amazon S3 bucket associated with your application\.
+ **Enable log file rotation to Amazon S3** – Specifies whether log files for your application's Amazon EC2 instances should be copied to the Amazon S3 bucket associated with your application\.

### Static files<a name="php-console-staticfiles"></a>

To improve performance, the **Static files** section lets you configure the proxy server to serve static files \(for example, HTML or images\) from a set of directories inside your web application\. For each directory, you set the virtual path to directory mapping\. When the proxy server receives a request for a file under the specified path, it serves the file directly instead of routing the request to your application\.

For details about configuring static files using the Elastic Beanstalk console, see [Serving static files](environment-cfg-staticfiles.md)\.

### Environment properties<a name="php-console-properties"></a>

The **Environment Properties** section lets you specify environment configuration settings on the Amazon EC2 instances that are running your application\. These settings are passed in as key\-value pairs to the application\. 

Your application code can access environment properties by using `$_SERVER` or the `get_cfg_var` function\.

```
$endpoint = $_SERVER['API_ENDPOINT'];
```

See [Environment properties and other software settings](environments-cfg-softwaresettings.md) for more information\.

## The aws:elasticbeanstalk:container:php:phpini namespace<a name="php-namespaces"></a>

You can use a [configuration file](ebextensions.md) to set configuration options and perform other instance configuration tasks during deployments\. Configuration options can be defined by the Elastic Beanstalk service or the platform that you use and are organized into *namespaces*\.

You can use the `aws:elasticbeanstalk:environment:proxy` namespace to choose the environment's proxy server\.

You can use the `aws:elasticbeanstalk:environment:proxy:staticfiles` namespace to configure the environment proxy to serve static files\. You define mappings of virtual paths to application directories\.

The PHP platform defines options in the `aws:elasticbeanstalk:container:php:phpini` namespace, including one that is not available in the Elastic Beanstalk console\. `composer_options` sets custom options to use when installing dependencies using Composer through `composer.phar install`\. For more information including available options, go to [http://getcomposer\.org/doc/03\-cli\.md\#install](http://getcomposer.org/doc/03-cli.md#install)\.

The following example [configuration file](ebextensions.md) specifies a static files option that maps a directory named `staticimages` to the path `/images`, and shows settings for each of the options available in the `aws:elasticbeanstalk:container:php:phpini` namespace:

**Example \.ebextensions/php\-settings\.config**  

```
option_settings:
  aws:elasticbeanstalk:environment:proxy:
    ProxyServer: apache
  aws:elasticbeanstalk:environment:proxy:staticfiles:
    /images: staticimages
  aws:elasticbeanstalk:container:php:phpini:
    document_root: /public
    memory_limit: 128M
    zlib.output_compression: "Off"
    allow_url_fopen: "On"
    display_errors: "Off"
    max_execution_time: 60
    composer_options: vendor/package
```

**Note**  
The `aws:elasticbeanstalk:environment:proxy:staticfiles` namespace isn't defined on Amazon Linux AMI PHP platform branches \(preceding Amazon Linux 2\)\.

Elastic Beanstalk provides many configuration options for customizing your environment\. In addition to configuration files, you can also set configuration options using the console, saved configurations, the EB CLI, or the AWS CLI\. See [Configuration options](command-options.md) for more information\.