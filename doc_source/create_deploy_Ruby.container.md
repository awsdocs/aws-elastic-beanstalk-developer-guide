# Using the Elastic Beanstalk Ruby platform<a name="create_deploy_Ruby.container"></a>

The AWS Elastic Beanstalk Ruby platform is a set of [environment configurations](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.ruby) for Ruby web applications that can run behind an nginx proxy server under a Puma or Passenger application server\. Each platform branch corresponds to a version of Ruby\.

Elastic Beanstalk provides [configuration options](command-options.md) that you can use to customize the software that runs on the Amazon Elastic Compute Cloud \(Amazon EC2\) instances in your Elastic Beanstalk environment\. You can configure environment variables needed by your application and enable log rotation to Amazon S3\. The platform also predefines some common environment variables related to Rails and Rack for ease of discovery and use\.

Platform\-specific configuration options are available in the AWS Management Console for [modifying the configuration of a running environment](environment-configuration-methods-after.md)\. To avoid losing your environment's configuration when you terminate it, you can use [saved configurations](environment-configuration-savedconfig.md) to save your settings and later apply them to another environment\.

To save settings in your source code, you can include [configuration files](ebextensions.md)\. Settings in configuration files are applied every time you create an environment or deploy your application\. You can also use configuration files to install packages, run scripts, and perform other instance customization operations during deployments\.

If you use RubyGems, you can [include a `Gemfile` file](ruby-platform-gemfile.md) in your source bundle to install packages during deployment\.

Settings applied in the AWS Management Console override the same settings in configuration files, if they exist\. This lets you have default settings in configuration files, and override them with environment\-specific settings in the console\. For more information about precedence, and other methods of changing settings, see [Configuration options](command-options.md)\.

## Configuring your Ruby environment<a name="create-deploy_Ruby.container.CON"></a>

You can use the Elastic Beanstalk console to enable log rotation to Amazon S3 and configure variables that your application can read from the environment\.

**To access the software configuration settings for your environment**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and then, in the regions drop\-down list, select your region\.

1. In the navigation pane, choose **Environments**, and then choose your environment's name on the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Configuration**\.

1. In the **Software** configuration category, choose **Edit**\.

### Log options<a name="create_deploy_Ruby.container.console.logoptions"></a>

The **Log options** section has two settings:
+ **Instance profile**– Specifies the instance profile that has permission to access the Amazon S3 bucket associated with your application\.
+ **Enable log file rotation to Amazon S3** – Specifies whether log files for your application's Amazon EC2 instances should be copied to your Amazon S3 bucket associated with your application\.

### Environment properties<a name="create_deploy_Ruby.env.console.ruby.envprops"></a>

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

See [Environment properties and other software settings](environments-cfg-softwaresettings.md) for more information\.

## Ruby configuration namespaces<a name="ruby-namespaces"></a>

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

Elastic Beanstalk provides many configuration options for customizing your environment\. In addition to configuration files, you can also set configuration options using the console, saved configurations, the EB CLI, or the AWS CLI\. See [Configuration options](command-options.md) for more information\.

## Amazon Linux 2 considerations<a name="ruby-al2"></a>


|  | 
| --- |
| AWS Elastic Beanstalk support for Amazon Linux 2 is in beta release and is subject to change\. | 

Elastic Beanstalk provides Amazon Linux 2 Ruby platform branches\. The platform versions in these branches are different than previous Ruby platform versions based on Amazon Linux AMI in a few ways, both generic \(apply to all Amazon Linux 2 platforms\) and platform specific \(apply to Amazon Linux 2 Ruby platform versions\)\. For details, see [Migrating your Elastic Beanstalk Linux application to Amazon Linux 2](using-features.migration-al.md)\.

Amazon Linux 2 Ruby platform versions have some new functionality\.
+ *Procfile support* – You can add a `Procfile` to your source bundle to specify the command that starts your application\. When you don't provide a `Procfile`, Elastic Beanstalk generates the following default file, which assumes you're using the pre\-installed Puma application server\.

  ```
  web: puma -C /opt/elasticbeanstalk/config/private/pumaconf.rb
  ```

  If you want to use your own provided Puma server, you can install it using a [Gemfile](ruby-platform-gemfile.md)\. The following example `Procfile` shows how to start it\.  
**Example Procfile**  

  ```
  web: bundle exec puma -C /opt/elasticbeanstalk/config/private/pumaconf.rb
  ```

  If you want to use the Passenger application server, use the following example files to configure your Ruby environment to install and use Passenger\.

  1. Use this example file to install Passenger\.  
**Example Gemfile**  

     ```
     source 'https://rubygems.org'
     gem 'passenger'
     ```

  1. Use this example file to instruct Elastic Beanstalk to start Passenger\.  
**Example Procfile**  

     ```
     web: bundle exec passenger start /var/app/current --socket /var/run/puma/my_app.sock
     ```
**Note**  
You don't have to change anything in the configuration of the nginx proxy server to use Passenger\. To use other application servers, you might need to customize the nginx configuration to properly forward requests to your application\.

  For details about `Procfile` usage, expand the *Buildfile and Procfile* section in [Extending Elastic Beanstalk Linux platforms](platforms-linux-extend.md)\.
+ *Bundler modes* – By default, Elastic Beanstalk uses Bundler to install dependencies in [deployment mode](https://bundler.io/man/bundle-install.1.html#DEPLOYMENT-MODE)\. The Ruby platforms supports an environment property called `BUNDLER_DEPLOYMENT_MODE`, which is set to `true` by default\. Set it to `false` to run `bundle install` in development mode\.