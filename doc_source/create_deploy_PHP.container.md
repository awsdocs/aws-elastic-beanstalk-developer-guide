# Using the Elastic Beanstalk PHP platform<a name="create_deploy_PHP.container"></a>

AWS Elastic Beanstalk supports a number of platforms for different versions of the PHP programming language\. These platforms support PHP web applications that can run alone or under Composer\. Learn more at [PHP](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.PHP) in the *AWS Elastic Beanstalk Platforms* document\.

Elastic Beanstalk provides [configuration options](command-options.md) that you can use to customize the software that runs on the EC2 instances in your Elastic Beanstalk environment\. You can configure environment variables needed by your application, enable log rotation to Amazon S3, and set common PHP initialization settings\.

Platform\-specific configuration options are available in the AWS Management Console for [modifying the configuration of a running environment](environment-configuration-methods-after.md)\. To avoid losing your environment's configuration when you terminate it, you can use [saved configurations](environment-configuration-savedconfig.md) to save your settings and later apply them to another environment\.

To save settings in your source code, you can include [configuration files](ebextensions.md)\. Settings in configuration files are applied every time you create an environment or deploy your application\. You can also use configuration files to install packages, run scripts, and perform other instance customization operations during deployments\.

If you use Composer, you can [include a `composer.json` file](php-configuration-composer.md) in your source bundle to install packages during deployment\.

For advanced PHP configuration and PHP settings that are not provided as configuration options, you can [use configuration files to provide an `INI` file](php-configuration-phpini.md) that can extend and override the default settings applied by Elastic Beanstalk, or install additional extensions\.

Settings applied in the AWS Management Console override the same settings in configuration files, if they exist\. This lets you have default settings in configuration files, and override them with environment\-specific settings in the console\. For more information about precedence, and other methods of changing settings, see [Configuration options](command-options.md)\.

## Configuring your PHP environment<a name="php-console"></a>

You can use the Elastic Beanstalk console to enable log rotation to Amazon S3, configure variables that your application can read from the environment, and change PHP settings\.

**To configure your PHP environment in the Elastic Beanstalk console**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and then, in the regions drop\-down list, select your region\.

1. In the navigation pane, choose **Environments**, and then choose your environment's name on the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Configuration**\.

1. In the **Software** configuration category, choose **Edit**\.

### PHP settings<a name="php-console-settings"></a>
+ **Document root** – The folder that contains your site's default page\. If your welcome page is not at the root of your source bundle, specify the folder that contains it relative to the root path\. For example, `/public` if the welcome page is in a folder named `public`\.
+ **Memory limit** – The maximum amount of memory that a script is allowed to allocate\. For example, `512M`\.
+ **Zlib output compression** – Set to `On` to compress responses\.
+ **Allow URL fopen** – Set to `Off` to prevent scripts from downloading files from remote locations\.
+ **Display errors** – Set to `On` to show internal error messages for debugging\.
+ **Max execution time** – The maximum time in seconds that a script is allowed to run before the environment terminates it\.

### Log options<a name="php-console-logs"></a>

The Log Options section has two settings:
+ **Instance profile**– Specifies the instance profile that has permission to access the Amazon S3 bucket associated with your application\.
+ **Enable log file rotation to Amazon S3** – Specifies whether log files for your application's Amazon EC2 instances should be copied to your Amazon S3 bucket associated with your application\.

### Environment properties<a name="php-console-properties"></a>

The **Environment Properties** section lets you specify environment configuration settings on the Amazon EC2 instances that are running your application\. These settings are passed in as key\-value pairs to the application\. 

Your application code can access environment properties by using `$_SERVER` or the `get_cfg_var` function\.

```
$endpoint = $_SERVER['API_ENDPOINT'];
```

See [Environment properties and other software settings](environments-cfg-softwaresettings.md) for more information\.

## The aws:elasticbeanstalk:container:php:phpini namespace<a name="php-namespaces"></a>

You can use a [configuration file](ebextensions.md) to set configuration options and perform other instance configuration tasks during deployments\. Configuration options can be defined by the Elastic Beanstalk service or the platform that you use and are organized into *namespaces*\.

The PHP platform defines options in the `aws:elasticbeanstalk:container:php:phpini` namespace, including one that is not available in the Elastic Beanstalk console\. `composer_options` sets custom options to use when installing dependencies using Composer through `composer.phar install`\. For more information including available options, go to [http://getcomposer\.org/doc/03\-cli\.md\#install](http://getcomposer.org/doc/03-cli.md#install)\.

The following example [configuration file](ebextensions.md) shows settings for each of the options available in this namespace:

**Example \.ebextensions/php\-settings\.config**  

```
option_settings:
  aws:elasticbeanstalk:container:php:phpini:
    document_root: /public
    memory_limit: 128M
    zlib.output_compression: "Off"
    allow_url_fopen: "On"
    display_errors: "Off"
    max_execution_time: 60
    composer_options: vendor/package
```

Elastic Beanstalk provides many configuration options for customizing your environment\. In addition to configuration files, you can also set configuration options using the console, saved configurations, the EB CLI, or the AWS CLI\. See [Configuration options](command-options.md) for more information\.

## Amazon Linux 2 considerations<a name="php-al2"></a>


|  | 
| --- |
| AWS Elastic Beanstalk support for Amazon Linux 2 is in beta release and is subject to change\. | 

Elastic Beanstalk provides Amazon Linux 2 PHP platform branches\. The platform versions in these branches are different than previous PHP platform versions based on Amazon Linux AMI in a few ways, both generic \(apply to all Amazon Linux 2 platforms\) and platform specific \(apply to Amazon Linux 2 PHP platform versions\)\. For details, see [Migrating your Elastic Beanstalk Linux application to Amazon Linux 2](using-features.migration-al.md)\.