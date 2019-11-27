# Migrating Your Elastic Beanstalk Linux Application to Amazon Linux 2<a name="using-features.migration-al"></a>


|  | 
| --- |
| AWS Elastic Beanstalk support for Amazon Linux 2 is in beta release and is subject to change\. | 

AWS provides two versions of Amazon Linux: [Amazon Linux 2](https://aws.amazon.com/amazon-linux-2/) and [Amazon Linux AMI](https://aws.amazon.com/amazon-linux-ami/)\. AWS Elastic Beanstalk maintains platform versions with both Amazon Linux versions\. For details about Linux platforms, see [Elastic Beanstalk Linux Platforms](platforms-linux.md)\.

If your Elastic Beanstalk application is based on a platform version with a previous version of Amazon Linux AMI, you can leverage our beta program and start exploring the migration of your application's environments to a newer Amazon Linux 2 platform version\. The two platform generations aren't guaranteed to be backward compatible with your existing application\. Furthermore, even if your application code successfully deploys to the new platform version, it might behave or perform differently due to operating system and run time differences\. Although Amazon Linux AMI and Amazon Linux 2 share the same Linux kernel, they differ in their initialization system, `libc` versions, the compiler tool chain, and various packages\. We've also updated platform specific versions of runtime, build tools, and other dependencies\. Therefore we recommend that you take your time, test your application thoroughly in a development environment, and make any necessary adjustments\.

When we release a generally available Amazon Linux 2 platform version, and when you're ready to go to production, Elastic Beanstalk requires a blue/green deployment to perform the upgrade\. For details about platform update strategies, see [Updating Your Elastic Beanstalk Environment's Platform Version](using-features.platform.upgrade.md)\.

## Considerations for All Linux Platforms<a name="using-features.migration-al.generic"></a>

The following table discusses considerations you should be aware of when planning an application migration to Amazon Linux 2\. These considerations apply to any of the Elastic Beanstalk Linux platforms, regardless of specific programming languages or application servers\.


|  **Area**  |  **Changes and information**  | 
| --- | --- | 
|  Configuration Files  |  On Amazon Linux 2 platforms, you can use [configuration files](ebextensions.md) as before, and all sections work the same way\. However, specific settings might not work the same as they did on previous Amazon Linux AMI platforms\. For example: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.migration-al.html) We recommend using platform hooks to run custom code on your environment instances\. You can still use commands and container commands in `.ebextensions` configuration files, but they aren't as easy to work with\. For example, writing command scripts inside a YAML file can be cumbersome and difficult to test\. You still need to use `.ebextensions` configuration files for any script that needs a reference to a AWS CloudFormation resource\.  | 
|  Platform hooks  |  Amazon Linux 2 platforms introduce a new way to extend your environment's platform by adding executable files to hook directories on the environment's instances\. With previous Linux platform versions, you might have used [custom platform hooks](custom-platform-hooks.md)\. These hooks weren't designed for managed platforms and weren't supported, but could work in useful ways in some cases\. With Amazon Linux 2 platform versions, custom platform hooks don't work\. You should migrate any hooks to the new platform hooks\. For details, expand the *Platform Hooks* section in [Extending Elastic Beanstalk Linux Platforms](platforms-linux-extend.md)\.  | 
|  Supported proxy servers  |  Amazon Linux 2 platforms no longer support Apache HTTPD\. They only support the nginx proxy server\. If you have any HTTP custom configuration, either files in the `.ebextensions/httpd` directory or commands in the `commands:` or `container_commands:` sections of an `.ebextensions` configuration file, modify them to use nginx instead\. For information about nginx proxy configuration, expand the *Reverse Proxy Configuration* section in [Extending Elastic Beanstalk Linux Platforms](platforms-linux-extend.md)\.  | 
|  Instance profile  |  Amazon Linux 2 platforms require an instance profile to be configured\. Environment creation might temporarily succeed without one, but the environment might show errors soon after creation when actions requiring an instance profile start failing\. For details, see [Managing Elastic Beanstalk Instance Profiles](iam-instanceprofile.md)\.  | 
|  Enhanced health  |  Amazon Linux 2 platforms enable enhanced health by default\. This is a change if you don't use the Elastic Beanstalk console to create your environments\. The console enables enhanced health by default whenever possible, regardless of platform version\. For details, see [Enhanced Health Reporting and Monitoring](health-enhanced.md)\.  | 
|  Custom AMI  |  If your environment uses a [custom AMI](using-features.customenv.md), create a new AMI based on Amazon Linux 2 for your new environment using an Elastic Beanstalk Amazon Linux 2 platform\.  | 
|  Beta limitations  |  Amazon Linux 2 platforms are missing some features in the beta versions\. We're working on adding support for these features\. Here's a list of features that aren't supported at this time\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.migration-al.html)  | 

## Platform Specific Considerations<a name="using-features.migration-al.specific"></a>

This section discusses migration considerations specific to particular Elastic Beanstalk Linux platforms\.

### Amazon Corretto<a name="using-features.migration-al.specific.corretto"></a>

The table below lists migration information for the Corretto platform versions in the [Java SE platform](java-se-platform.md)\.


|  **Area**  |  **Changes and information**  | 
| --- | --- | 
|  Corretto vs\. OpenJDK  |  To implement the Java Platform, Standard Edition \(Java SE\), Amazon Linux 2 platforms use [Amazon Corretto](https://aws.amazon.com/corretto), an AWS distribution of the Open Java Development Kit \(OpenJDK\)\. Prior Elastic Beanstalk Java SE platform versions use the OpenJDK packages included with Amazon Linux AMI\.  | 
|  Build tools  |  Amazon Linux 2 platforms have newer versions of the build tools: `gradle`, `maven`, and `ant`\.  | 
|  JAR file handling  |  On Amazon Linux 2 platforms, if your source bundle \(ZIP file\) contains a single JAR file and no other files, Elastic Beanstalk no longer renames the JAR file to `application.jar`\. Renaming occurs only if you submit a JAR file on its own, not within a ZIP file\.  | 
|  Port passing  |  On Amazon Linux 2 platforms, Elastic Beanstalk doesn't pass a port value to your application process through the `PORT` environment variable\. You can simulate this behavior for your process by configuring a `PORT` environment property yourself\. However, if you have multiple processes, and you're counting on Elastic Beanstalk passing incremental port values to your processes \(5000, 5100, 5200 etc\.\), you should modify your implementation\. For details, see [Configuring the Application Process with a Procfile](java-se-procfile.md)\.  | 
|  Java 7  |  Elastic Beanstalk doesn't support an Amazon Linux 2 Java 7 platform version\. If you have a Java 7 application, migrate it to Corretto 8 or Corretto 11\.  | 