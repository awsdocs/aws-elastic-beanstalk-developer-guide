# Using the Elastic Beanstalk Ruby platform<a name="create_deploy_Ruby.container"></a>

**Important**  
Amazon Linux 2 platform versions are fundamentally different than Amazon Linux AMI platform versions \(preceding Amazon Linux 2\)\. These different platform generations are incompatible in several ways\. If you are migrating to an Amazon Linux 2 platform version, be sure to read the information in [Migrating your Elastic Beanstalk Linux application to Amazon Linux 2](using-features.migration-al.md)\.

The AWS Elastic Beanstalk Ruby platform is a set of [environment configurations](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.ruby) for Ruby web applications that can run behind an nginx proxy server under a Puma application server\. Each platform branch corresponds to a version of Ruby\.

If you use RubyGems, you can [include a `Gemfile` file](ruby-platform-gemfile.md) in your source bundle to install packages during deployment\.

Your application might run under a different application server, for example Passenger\. You can use a `Procfile` to start a different application server, and a `Gemfile` to install it\. For details, see [Configuring the application process with a Procfile](ruby-platform-procfile.md)\.

**Note**  
If you are using an Amazon Linux AMI Ruby platform branch \(preceding Amazon Linux 2\), be aware that Elastic Beanstalk provides two flavors of these branches \- with Puma and with Passenger\. If your application needs the Passenger application server, you can use the appropriate Passenger platform branch, and you don't need to do any additional configuration\.

Elastic Beanstalk provides [configuration options](command-options.md) that you can use to customize the software that runs on the Amazon Elastic Compute Cloud \(Amazon EC2\) instances in your Elastic Beanstalk environment\. You can configure environment variables needed by your application, enable log rotation to Amazon S3, and map folders in your application source that contain static files to paths served by the proxy server\. The platform also predefines some common environment variables related to Rails and Rack for ease of discovery and use\.

Configuration options are available in the Elastic Beanstalk console for [modifying the configuration of a running environment](environment-configuration-methods-after.md)\. To avoid losing your environment's configuration when you terminate it, you can use [saved configurations](environment-configuration-savedconfig.md) to save your settings and later apply them to another environment\.

To save settings in your source code, you can include [configuration files](ebextensions.md)\. Settings in configuration files are applied every time you create an environment or deploy your application\. You can also use configuration files to install packages, run scripts, and perform other instance customization operations during deployments\.

Settings applied in the Elastic Beanstalk console override the same settings in configuration files, if they exist\. This lets you have default settings in configuration files, and override them with environment\-specific settings in the console\. For more information about precedence, and other methods of changing settings, see [Configuration options](command-options.md)\.

For details about the various ways you can extend an Elastic Beanstalk Linux\-based platform, see [Extending Elastic Beanstalk Linux platforms](platforms-linux-extend.md)\.

## Configuring your Ruby environment<a name="create-deploy_Ruby.container.CON"></a>

You can use the Elastic Beanstalk console to enable log rotation to Amazon S3 and configure variables that your application can read from the environment\.

**To access the software configuration settings for your environment**

1. Open the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk), and in the **Regions** list, select your AWS Region\.

1. In the navigation pane, choose **Environments**, and then choose the name of your environment from the list\.
**Note**  
If you have many environments, use the search bar to filter the environment list\.

1. In the navigation pane, choose **Configuration**\.

1. In the **Software** configuration category, choose **Edit**\.

### Log options<a name="create_deploy_Ruby.container.console.logoptions"></a>

The **Log options** section has two settings:
+ **Instance profile**– Specifies the instance profile that has permission to access the Amazon S3 bucket associated with your application\.
+ **Enable log file rotation to Amazon S3** – Specifies whether log files for your application's Amazon EC2 instances should be copied to the Amazon S3 bucket associated with your application\.

### Static files<a name="create_deploy_Ruby.container.console.staticfiles"></a>

To improve performance, the **Static files** section lets you configure the proxy server to serve static files \(for example, HTML or images\) from a set of directories inside your web application\. For each directory, you set the virtual path to directory mapping\. When the proxy server receives a request for a file under the specified path, it serves the file directly instead of routing the request to your application\.

For details about configuring static files using the Elastic Beanstalk console, see [Serving static files](environment-cfg-staticfiles.md)\.

By default, the proxy server in a Ruby environment serves any files in the `public` folder at the `/public` path, and any files in the `public/assets` subfolder at the `/assets` path\. For example, if your application source contains a file named `logo.png` in a folder named `public`, the proxy server serves it to users at `subdomain.elasticbeanstalk.com/public/logo.png`\. If `logo.png` is in a folder named `assets` inside the `public` folder, the proxy server serves it at `subdomain.elasticbeanstalk.com/assets/logo.png`\. You can configure additional mappings as explained in this section\.

### Environment properties<a name="create_deploy_Ruby.env.console.ruby.envprops"></a>

The **Environment Properties** section lets you specify environment configuration settings on the Amazon EC2 instances that are running your application\. Environment properties are passed in as key\-value pairs to the application\.

The Ruby platform defines the following properties for environment configuration:
+  **BUNDLE\_WITHOUT** – A colon\-separated list of groups to ignore when [installing dependencies](http://bundler.io/bundle_install.html) from a [Gemfile](http://bundler.io/v1.15/man/gemfile.5.html)\.
+ **BUNDLER\_DEPLOYMENT\_MODE** – Set to `true` \(the default\) to install dependencies in [deployment mode](https://bundler.io/man/bundle-install.1.html#DEPLOYMENT-MODE) using Bundler\. Set to `false` to run `bundle install` in development mode\.
**Note**  
This environment property isn't defined on Amazon Linux AMI Ruby platform branches \(preceding Amazon Linux 2\)\.
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

You can use the `aws:elasticbeanstalk:environment:proxy:staticfiles` namespace to configure the environment proxy to serve static files\. You define mappings of virtual paths to application directories\.

The Ruby platform doesn't define any platform\-specific namespaces\. Instead, it defines environment properties for common Rails and Rack options\.

The following configuration file specifies a static files option that maps a directory named `staticimages` to the path `/images`, sets each of the platform defined environment properties, and sets an additional environment property named `LOGGING`\.

**Example \.ebextensions/ruby\-settings\.config**  

```
option_settings:
  aws:elasticbeanstalk:environment:proxy:staticfiles:
    /images: staticimages
  aws:elasticbeanstalk:application:environment:
    BUNDLE_WITHOUT: test
    BUNDLER_DEPLOYMENT_MODE: true
    RACK_ENV: development
    RAILS_SKIP_ASSET_COMPILATION: true
    RAILS_SKIP_MIGRATIONS: true
    LOGGING: debug
```

**Note**  
The `BUNDLER_DEPLOYMENT_MODE` environment property and the `aws:elasticbeanstalk:environment:proxy:staticfiles` namespace aren't defined on Amazon Linux AMI Ruby platform branches \(preceding Amazon Linux 2\)\.

Elastic Beanstalk provides many configuration options for customizing your environment\. In addition to configuration files, you can also set configuration options using the console, saved configurations, the EB CLI, or the AWS CLI\. See [Configuration options](command-options.md) for more information\.