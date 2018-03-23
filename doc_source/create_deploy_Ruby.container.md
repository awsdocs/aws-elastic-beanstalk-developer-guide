# Using the AWS Elastic Beanstalk Ruby Platform<a name="create_deploy_Ruby.container"></a>

The AWS Elastic Beanstalk Ruby platform is a set of [environment configurations](concepts.platforms.md#concepts.platforms.ruby) for Ruby web applications that can run behind an nginx proxy server under a Puma or Passenger application server\. Each configuration corresponds to a version of Ruby.

Elastic Beanstalk provides [configuration options](command-options.md) that you can use to customize the software that runs on the EC2 instances in your Elastic Beanstalk environment\. You can configure environment variables needed by your application and enable log rotation to Amazon S3\. The platform also predefines some common environment variables related to Rails and Rack for ease of discovery and use\.

Platform\-specific configuration options are available in the AWS Management Console for [modifying the configuration of a running environment](environment-configuration-methods-after.md)\. To avoid losing your environment's configuration when you terminate it, you can use [saved configurations](environment-configuration-savedconfig.md) to save your settings and later apply them to another environment\.

To save settings in your source code, you can include [configuration files](ebextensions.md)\. Settings in configuration files are applied every time you create an environment or deploy your application\. You can also use configuration files to install packages, run scripts, and perform other instance customization operations during deployments\.

If you use RubyGems, you can [include a `Gemfile` file](ruby-platform-gemfile.md) in your source bundle to install packages during deployment\.

Settings applied in the AWS Management Console override the same settings in configuration files, if they exist\. This lets you have default settings in configuration files, and override them with environment specific settings in the console\. For more information about precedence, and other methods of changing settings, see [Configuration Options](command-options.md)\.

## Configuring Your Ruby Environment<a name="create-deploy_Ruby.container.CON"></a>

You can use the AWS Management Console to enable log rotation to Amazon S3 and configure variables that your application can read from the environment\.

**To access the software configuration settings for your environment**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk)\.

1. Navigate to the [management page](environments-console.md) for your environment\.

1. Choose **Configuration**\.

1. On the **Software** configuration card, choose **Modify**\.

### Log Options<a name="create_deploy_Ruby.container.console.logoptions"></a>

The Log Options section has two settings:

+ **Instance profile**– Specifies the instance profile that has permission to access the Amazon S3 bucket associated with your application\.

+ **Enable log file rotation to Amazon S3** – Specifies whether log files for your application's Amazon EC2 instances should be copied to your Amazon S3 bucket associated with your application\.

### Environment Properties<a name="create_deploy_Ruby.env.console.ruby.envprops"></a>

The **Environment Properties** section lets you specify environment configuration settings on the Amazon EC2 instances that are running your application\. Environment properties are passed in as key\-value pairs to the application\.

The Ruby platform defines the following properties for environment configuration:

+  **BUNDLE\_WITHOUT** – A colon\-separated list of groups to ignore when [installing dependencies](http://bundler.io/bundle_install.html) from a [Gemfile](http://bundler.io/v1.15/man/gemfile.5.html)\.

+  **RAILS\_SKIP\_ASSET\_COMPILATION** – Set to `true` to skip running [http://guides.rubyonrails.org/asset_pipeline.html#precompiling-assets](http://guides.rubyonrails.org/asset_pipeline.html#precompiling-assets) during deployment\.

+  **RAILS\_SKIP\_MIGRATIONS** – Set to `true` to skip running [http://guides.rubyonrails.org/active_record_migrations.html#running-migrations](http://guides.rubyonrails.org/active_record_migrations.html#running-migrations) during deployment\.

+  **RACK\_ENV** – Specify the environment stage for Rack\. For example, `development`, `production`, or `test`\.

Inside the Ruby environment running in Elastic Beanstalk, environment variables are accessible using the `ENV` object\. For example, you could read a property named `API_ENDPOINT` to a variable with the following code:

```
endpoint = ENV['API_ENDPOINT']
```

See [Environment Properties and Other Software Settings](environments-cfg-softwaresettings.md) for more information\.

## Ruby Configuration Namespaces<a name="ruby-namespaces"></a>

You can use a [configuration file](ebextensions.md) to set configuration options and perform other instance configuration tasks during deployments\. Configuration options can be defined by the Elastic Beanstalk service or the platform that you use and are organized into *namespaces*\.

The Ruby platform doesn't define any additional namespaces\. Instead, it defines environment properties for common Rails and Rack options\. The following configuration file sets each of the platform defined environment properties, sets an additional environment property named `LOGGING`\.

**Example \.ebextensions/ruby\-settings\.config**  

```
option_settings:
  aws:elasticbeanstalk:application:environment: 
    BUNDLE_WITHOUT: test
    RACK_ENV: development
    RAILS_SKIP_ASSET_COMPILATION: true
    RAILS_SKIP_MIGRATIONS: true
    LOGGING: debug
```

Elastic Beanstalk provides many configuration options for customizing your environment\. In addition to configuration files, you can also set configuration options using the console, saved configurations, the EB CLI, or the AWS CLI\. See [Configuration Options](command-options.md) for more information\.
