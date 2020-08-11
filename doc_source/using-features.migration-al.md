# Migrating your Elastic Beanstalk Linux application to Amazon Linux 2<a name="using-features.migration-al"></a>

AWS provides two versions of Amazon Linux: [Amazon Linux 2](https://aws.amazon.com/amazon-linux-2/) and [Amazon Linux AMI](https://aws.amazon.com/amazon-linux-ami/)\. AWS Elastic Beanstalk maintains platform branches with both Amazon Linux versions\. For details about Linux platforms, see [Elastic Beanstalk Linux platforms](platforms-linux.md)\.

If your Elastic Beanstalk application is based on an Amazon Linux AMI platform branch, and the platform has newer generation Amazon Linux 2 platform branch\(es\), use this page to learn how to migrate your application's environments to Amazon Linux 2\. The two platform generations aren't guaranteed to be backward compatible with your existing application\. Furthermore, even if your application code successfully deploys to the new platform version, it might behave or perform differently due to operating system and run time differences\. Although Amazon Linux AMI and Amazon Linux 2 share the same Linux kernel, they differ in their initialization system, `libc` versions, the compiler tool chain, and various packages\. We've also updated platform specific versions of runtime, build tools, and other dependencies\. Therefore we recommend that you take your time, test your application thoroughly in a development environment, and make any necessary adjustments\.

When you're ready to go to production, Elastic Beanstalk requires a blue/green deployment to perform the upgrade\. For details about platform update strategies, see [Updating your Elastic Beanstalk environment's platform version](using-features.platform.upgrade.md)\.

## Considerations for all Linux platforms<a name="using-features.migration-al.generic"></a>

The following table discusses considerations you should be aware of when planning an application migration to Amazon Linux 2\. These considerations apply to any of the Elastic Beanstalk Linux platforms, regardless of specific programming languages or application servers\.


|  **Area**  |  **Changes and information**  | 
| --- | --- | 
|  Configuration Files  |  On Amazon Linux 2 platforms, you can use [configuration files](ebextensions.md) as before, and all sections work the same way\. However, specific settings might not work the same as they did on previous Amazon Linux AMI platforms\. For example: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.migration-al.html) We recommend using platform hooks to run custom code on your environment instances\. You can still use commands and container commands in `.ebextensions` configuration files, but they aren't as easy to work with\. For example, writing command scripts inside a YAML file can be cumbersome and difficult to test\. You still need to use `.ebextensions` configuration files for any script that needs a reference to an AWS CloudFormation resource\.  | 
|  Platform hooks  |  Amazon Linux 2 platforms introduce a new way to extend your environment's platform by adding executable files to hook directories on the environment's instances\. With previous Linux platform versions, you might have used [custom platform hooks](custom-platform-hooks.md)\. These hooks weren't designed for managed platforms and weren't supported, but could work in useful ways in some cases\. With Amazon Linux 2 platform versions, custom platform hooks don't work\. You should migrate any hooks to the new platform hooks\. For details, expand the *Platform Hooks* section in [Extending Elastic Beanstalk Linux platforms](platforms-linux-extend.md)\.  | 
|  Supported proxy servers  |  Amazon Linux 2 platform versions support the same reverse proxy servers as each platform supported in its Amazon Linux AMI platform versions\. All Amazon Linux 2 platform versions use nginx as their default reverse proxy server\. The Tomcat, Node\.js, PHP, and Python platform also support Apache HTTPD as an alternative\. All platforms enable proxy server configuration in a uniform way, as described in this section\. However, configuring the proxy server is slightly different than it was on Amazon Linux AMI\. These are the differences for all platforms: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.migration-al.html) For platform\-specific proxy configuration changes, see [Platform specific considerations](#using-features.migration-al.specific)\. For information about proxy configuration on Amazon Linux 2 platforms, expand the *Reverse Proxy Configuration* section in [Extending Elastic Beanstalk Linux platforms](platforms-linux-extend.md)\.  | 
|  Instance profile  |  Amazon Linux 2 platforms require an instance profile to be configured\. Environment creation might temporarily succeed without one, but the environment might show errors soon after creation when actions requiring an instance profile start failing\. For details, see [Managing Elastic Beanstalk instance profiles](iam-instanceprofile.md)\.  | 
|  Enhanced health  |  Amazon Linux 2 platform versions enable enhanced health by default\. This is a change if you don't use the Elastic Beanstalk console to create your environments\. The console enables enhanced health by default whenever possible, regardless of platform version\. For details, see [Enhanced health reporting and monitoring](health-enhanced.md)\.  | 
|  Custom AMI  |  If your environment uses a [custom AMI](using-features.customenv.md), create a new AMI based on Amazon Linux 2 for your new environment using an Elastic Beanstalk Amazon Linux 2 platform\.  | 
|  Custom platforms  |  The managed AMIs of Amazon Linux 2 platform versions don't support [custom platforms](custom-platforms.md)\.  | 

## Platform specific considerations<a name="using-features.migration-al.specific"></a>

This section discusses migration considerations specific to particular Elastic Beanstalk Linux platforms\.

### Docker<a name="using-features.migration-al.specific.docker"></a>

The following table lists migration information for the Amazon Linux 2 platform versions in the [Docker platform](create_deploy_docker.md)\.


|  **Area**  |  **Changes and information**  | 
| --- | --- | 
|  Storage  |  Elastic Beanstalk configures Docker to use [storage drivers](https://docs.docker.com/storage/storagedriver/) to store Docker images and container data\. On Amazon Linux AMI, Elastic Beanstalk used the [Device Mapper storage driver](https://docs.docker.com/storage/storagedriver/device-mapper-driver/)\. To improve performance, Elastic Beanstalk provisioned an extra Amazon EBS volume\. On Amazon Linux 2 Docker platform versions, Elastic Beanstalk uses the [OverlayFS storage driver](https://docs.docker.com/storage/storagedriver/overlayfs-driver/), and achieves even better performance while not requiring a separate volume anymore\. With Amazon Linux AMI, if you used the `BlockDeviceMappings` option of the `aws:autoscaling:launchconfiguration` namespace to add custom storage volumes to a Docker environment, we advised you to also add the `/dev/xvdcz` Amazon EBS volume that Elastic Beanstalk provisions\. Elastic Beanstalk doesn't provision this volume anymore, so you should remove it from your configuration files\. For details, see [Docker configuration on Amazon Linux AMI \(preceding Amazon Linux 2\)](create_deploy_docker.container.console.md#docker-alami)\.  | 
|  Private repository authentication  |  When you provide a Docker\-generated authentication file to connect to a private repository, you no longer need to convert it to the older format that Amazon Linux AMI Docker platform versions required\. Amazon Linux 2 Docker platform versions support the new format\. For details, see [Using images from a private repository](create_deploy_docker.container.console.md#docker-images-private)\.  | 
|  Proxy server  |  Amazon Linux 2 Docker platform versions don't support standalone containers that don't run behind a proxy server\. On Amazon Linux AMI Docker platform versions, this used to be possible through the `none` value of the `ProxyServer` option in the `aws:elasticbeanstalk:environment:proxy` namespace\.  | 

### Go<a name="using-features.migration-al.specific.go"></a>

The following table lists migration information for the Amazon Linux 2 platform versions in the [Go platform](go-environment.md)\.


|  **Area**  |  **Changes and information**  | 
| --- | --- | 
|  Port passing  |  On Amazon Linux 2 platforms, Elastic Beanstalk doesn't pass a port value to your application process through the `PORT` environment variable\. You can simulate this behavior for your process by configuring a `PORT` environment property yourself\. However, if you have multiple processes, and you're counting on Elastic Beanstalk passing incremental port values to your processes \(5000, 5100, 5200 etc\.\), you should modify your implementation\. For details, expand the *Reverse proxy configuration* section in [Extending Elastic Beanstalk Linux platforms](platforms-linux-extend.md)\.  | 

### Amazon Corretto<a name="using-features.migration-al.specific.corretto"></a>

The following table lists migration information for the Corretto platform branches in the [Java SE platform](java-se-platform.md)\.


|  **Area**  |  **Changes and information**  | 
| --- | --- | 
|  Corretto vs\. OpenJDK  |  To implement the Java Platform, Standard Edition \(Java SE\), Amazon Linux 2 platform branches use [Amazon Corretto](https://aws.amazon.com/corretto), an AWS distribution of the Open Java Development Kit \(OpenJDK\)\. Prior Elastic Beanstalk Java SE platform branches use the OpenJDK packages included with Amazon Linux AMI\.  | 
|  Build tools  |  Amazon Linux 2 platforms have newer versions of the build tools: `gradle`, `maven`, and `ant`\.  | 
|  JAR file handling  |  On Amazon Linux 2 platforms, if your source bundle \(ZIP file\) contains a single JAR file and no other files, Elastic Beanstalk no longer renames the JAR file to `application.jar`\. Renaming occurs only if you submit a JAR file on its own, not within a ZIP file\.  | 
|  Port passing  |  On Amazon Linux 2 platforms, Elastic Beanstalk doesn't pass a port value to your application process through the `PORT` environment variable\. You can simulate this behavior for your process by configuring a `PORT` environment property yourself\. However, if you have multiple processes, and you're counting on Elastic Beanstalk passing incremental port values to your processes \(5000, 5100, 5200 etc\.\), you should modify your implementation\. For details, expand the *Reverse proxy configuration* section in [Extending Elastic Beanstalk Linux platforms](platforms-linux-extend.md)\.  | 
|  Java 7  |  Elastic Beanstalk doesn't support an Amazon Linux 2 Java 7 platform branch\. If you have a Java 7 application, migrate it to Corretto 8 or Corretto 11\.  | 

### Tomcat<a name="using-features.migration-al.specific.tomcat"></a>

The following table lists migration information for the Amazon Linux 2 platform versions in the [Tomcat platform](java-tomcat-platform.md)\.


|  **Area**  |  **Changes and information**  | 
| --- | --- | 
|  **Option**  |  **Migration information**  | 
| --- | --- | 
|  Configuration options  |  On Amazon Linux 2 platform versions, Elastic Beanstalk supports only a subset of the configuration options and option values in the `aws:elasticbeanstalk:environment:proxy` namespace\. Here's the migration information for each option\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.migration-al.html) The `XX:MaxPermSize` option in the `aws:elasticbeanstalk:container:tomcat:jvmoptions` namespace isn't supported on Amazon Linux 2 platform versions\. The JVM setting to modify the size of the permanent generation applies only to Java 7 and earlier, and is therefore not applicable to Amazon Linux 2 platform versions\.  | 
|  Application path  |  On Amazon Linux 2 platforms, the path to the application's directory on Amazon EC2 instances of your environment is `/var/app/current`\. It was `/var/lib/tomcat8/webapps` on Amazon Linux AMI platforms\.  | 
|  `GzipCompression`  |  Unsupported on Amazon Linux 2 platform versions\.  | 
|  `ProxyServer`  |  Amazon Linux 2 Tomcat platform versions support both the nginx and the Apache HTTPD version 2\.4 proxy servers\. However, Apache version 2\.2 isn't supported\. On Amazon Linux AMI platform versions, the default proxy was Apache 2\.4\. If you used the default proxy setting and added custom proxy configuration files, your proxy configuration should still work on Amazon Linux 2\. However, if you used the `apache/2.2` option value, you now have to migrate your proxy configuration to Apache version 2\.4\.  | 

### Node\.js<a name="using-features.migration-al.specific.nodejs"></a>

The following table lists migration information for the Amazon Linux 2 platform versions in the [Node\.js platform](create_deploy_nodejs.container.md)\.


|  **Area**  |  **Changes and information**  | 
| --- | --- | 
|  **Option**  |  **Migration information**  | 
| --- | --- | 
|  Installed Node\.js versions  |  On Amazon Linux 2 platforms, Elastic Beanstalk maintains several Node\.js platform branches, and only installs the latest version of the Node\.js major version corresponding with the platform branch on each platform version\. For example, each platform version in the Node\.js 12 platform branch only has Node\.js 12\.x\.y installed by default\. On Amazon Linux AMI platform versions, we installed the multiple versions of multiple Node\.js versions on each platform version, and only maintained a single platform branch\. Choose the Node\.js platform branch that corresponds with the Node\.js major version that your application needs\.  | 
|  Apache HTTPD log file names  |  On Amazon Linux 2 platforms, if you use the Apache HTTPD proxy server, the HTTPD log file names are `access_log` and `error_log`, which is consistent with all other platforms that support Apache HTTPD\. On Amazon Linux AMI platform versions, these log files were named `access.log` and `error.log`, respectively\. For details about log file names and locations for all platforms, see [How Elastic Beanstalk sets up CloudWatch Logs](AWSHowTo.cloudwatchlogs.md#AWSHowTo.cloudwatchlogs.loggroups)\.  | 
|  Configuration options  |  On Amazon Linux 2 platforms, Elastic Beanstalk doesn't support the configuration options in the `aws:elasticbeanstalk:container:nodejs` namespace\. Some of the options have alternatives\. Here's the migration information for each option\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.migration-al.html)  | 
|  `NodeCommand`  |  Use a `Procfile` or the `scripts` keyword in a `package.json` file to specify the start script\.  | 
|  `NodeVersion`  |  Use the `engines` keyword in a `package.json` file to specify the Node\.js version\. Be aware that you can only specify a Node\.js version that correspondes with your platform branch\. For example, if you're using the Node\.js 12 platform branch, you can specify only a 12\.x\.y Node\.js version\. For details, see [Specifying Node\.js dependencies with a package\.json file](nodejs-platform-dependencies.md#nodejs-platform-packagejson)\.  | 
|  `GzipCompression`  |  Unsupported on Amazon Linux 2 platform versions\.  | 
|  `ProxyServer`  |  On Amazon Linux 2 Node\.js platform versions, this option moved to the `aws:elasticbeanstalk:environment:proxy` namespace\. You can choose between `nginx` \(the default\) and `apache`\. Amazon Linux 2 Node\.js platform versions don't support standalone applications that don't run behind a proxy server\. On Amazon Linux AMI Node\.js platform versions, this used to be possible through the `none` value of the `ProxyServer` option in the `aws:elasticbeanstalk:container:nodejs` namespace\. If your environment runs a standalone application, update your code to listen to the port that the proxy server \(nginx or Apache\) forwards traffic to\. <pre>var port = process.env.PORT || 8080;<br /><br />app.listen(port, function() {<br />  console.log('Server running at http://127.0.0.1:%s', port);<br />});</pre>  | 

### PHP<a name="using-features.migration-al.specific.php"></a>

The following table lists migration information for the Amazon Linux 2 platform versions in the [PHP platform](create_deploy_PHP.container.md)\.


|  **Area**  |  **Changes and information**  | 
| --- | --- | 
|  PHP file processing  |  On Amazon Linux 2 platforms, PHP files are processed using PHP\-FPM \(a CGI process manager\)\. On Amazon Linux AMI platforms we used mod\_php \(an Apache module\)\.  | 
|  Proxy server  |  Amazon Linux 2 PHP platform versions support both the nginx and the Apache HTTPD proxy servers\. The default is nginx\. Amazon Linux AMI PHP platform versions supported only Apache HTTPD\. If you added custom Apache configuration files, you can set the `ProxyServer` option in the `aws:elasticbeanstalk:environment:proxy` namespace to `apache`\.  | 

### Python<a name="using-features.migration-al.specific.python"></a>

The following table lists migration information for the Amazon Linux 2 platform versions in the [Python platform](create-deploy-python-container.md)\.


|  **Area**  |  **Changes and information**  | 
| --- | --- | 
|  WSGI server  |  On Amazon Linux 2 platforms, [Gunicorn](https://gunicorn.org/) is the default WSGI server\. By default, Gunicorn listens on port 8000\. The port might be different than what your application used on the Amazon Linux AMI platform\. If you're setting the `WSGIPath` option of the `[aws:elasticbeanstalk:container:python](command-options-specific.md#command-options-python)` namespace, replace the value with Gunicorn's syntax\. For details, see [Python configuration namespaces](create-deploy-python-container.md#python-namespaces)\. Alternatively, you can use a `Procfile` to specify and configure the WSGI server\. For details, see [Configuring the WSGI server with a Procfile](python-configuration-procfile.md)\.  | 
|  Application path  |  On Amazon Linux 2 platforms, the path to the application's directory on Amazon EC2 instances of your environment is `/var/app/current`\. It was `/opt/python/current/app` on Amazon Linux AMI platforms\.  | 
|  Proxy server  |  Amazon Linux 2 Python platform versions support both the nginx and the Apache HTTPD proxy servers\. The default is nginx\. Amazon Linux AMI Python platform versions supported only Apache HTTPD\. If you added custom Apache configuration files, you can set the `ProxyServer` option in the `aws:elasticbeanstalk:environment:proxy` namespace to `apache`\.  | 

### Ruby<a name="using-features.migration-al.specific.ruby"></a>

The following table lists migration information for the Amazon Linux 2 platform versions in the [Ruby platform](create_deploy_Ruby.container.md)\.


|  **Area**  |  **Changes and information**  | 
| --- | --- | 
|  Installed Ruby versions  |  On Amazon Linux 2 platforms, Elastic Beanstalk only installs the latest version of a single Ruby version, corresponding with the platform branch, on each platform version\. For example, each platform version in the Ruby 2\.6 platform branch only has Ruby 2\.6\.x installed\. On Amazon Linux AMI platform versions, we installed the latest versions of multiple Ruby versions, for example, 2\.4\.x, 2\.5\.x, and 2\.6\.x\. If your application uses a Ruby version that doesn't correspond to the platform branch you're using, we recommend that you switch to a platform branch that has the correct Ruby version for your application\.  | 
|  Application server  |  On Amazon Linux 2 platforms, Elastic Beanstalk only installs the Puma application server on all Ruby platform versions\. You can use a `Procfile` to start a different application server, and a `Gemfile` to install it\. On the Amazon Linux AMI platform, we supported two flavors of platform branches for each Ruby version—one with the Puma application server and the other with the Passenger application server\. If your application uses Passenger, you can configure your Ruby environment to install and use Passenger\. For more information and examples, see [Using the Elastic Beanstalk Ruby platform](create_deploy_Ruby.container.md)\.  | 