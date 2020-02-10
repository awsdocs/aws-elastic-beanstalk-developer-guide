# Configuring Elastic Beanstalk environments \(advanced\)<a name="beanstalk-environment-configuration-advanced"></a>

When you create an AWS Elastic Beanstalk environment, Elastic Beanstalk provisions and configures all of the AWS resources required to run and support your application\. In addition to configuring your environment's metadata and update behavior, you can customize these resources by providing values for [configuration options](command-options.md)\. For example, you may want to add an Amazon SQS queue and an alarm on queue depth, or you might want to add an Amazon ElastiCache cluster\.

Most of the configuration options have default values that are applied automatically by Elastic Beanstalk\. You can override these defaults with configuration files, saved configurations, command line options, or by directly calling the Elastic Beanstalk API\. The EB CLI and Elastic Beanstalk console also apply recommended values for some options\.

You can easily customize your environment at the same time that you deploy your application version by including a configuration file with your source bundle\. When customizing the software on your instance, it is more advantageous to use a configuration file than to create a custom AMI because you do not need to maintain a set of AMIs\.

When deploying your applications, you may want to customize and configure the software that your application depends on\. These files could be either dependencies required by the application—for example, additional packages from the yum repository—or they could be configuration files such as a replacement for httpd\.conf to override specific settings that are defaulted by AWS Elastic Beanstalk\.

**Topics**
+ [Configuration options](command-options.md)
+ [Advanced environment customization with configuration files \(`.ebextensions`\)](ebextensions.md)
+ [Using Elastic Beanstalk saved configurations](environment-configuration-savedconfig.md)
+ [Environment manifest \(`env.yaml`\)](environment-cfg-manifest.md)
+ [Using a custom Amazon machine image \(AMI\)](using-features.customenv.md)
+ [Serving static files](environment-cfg-staticfiles.md)
+ [Configuring HTTPS for your Elastic Beanstalk environment](configuring-https.md)