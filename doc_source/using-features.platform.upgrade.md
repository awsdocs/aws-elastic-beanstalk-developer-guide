# Updating Your Elastic Beanstalk Environment's Platform Version<a name="using-features.platform.upgrade"></a>

Elastic Beanstalk regularly releases updates for the Linux and Windows Server based [platforms](concepts.platforms.md) that run applications on an Elastic Beanstalk environment\. A platform consists of a software component \(an AMI running a specific version of an OS, tools, and Elastic Beanstalk\-specific scripts\), and a configuration component \(the default [settings](command-options.md) applied to environments created with the platform\)\. New platform versions provide updates to existing software components and support for new features and configuration options\.

For platforms which support multiple incompatible major versions of the included web container, programming language, or framework, a separate *configuration* of the platform is provided for each\. For example, the [Java with Tomcat](java-tomcat-platform.md) platform supports separate configurations for Tomcat 7 and Tomcat 8\. Each configuration of a platform is versioned separately, but updates are generally released for all configurations of a given platform at the same time\.

When a new version of your environment's platform configuration is available, Elastic Beanstalk shows a message in the [environment management console](environments-console.md) and makes the **Change** button available\. If you've previously created an environment using an older version of the same configuration, or upgraded your environment from an older configuration, you can also use the **Change** button to revert to a previous configuration version\.

**To update your environment's platform**

1. Navigate to the [management page](environments-console.md) for your environment\.

1. In the **Overview** section, under **Configuration**, click **Change**\.  
![\[Elastic Beanstalk Newer Platform Available\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-env-dashboard-changetonewplatform.png)

1. Choose a **Platform Version**\. The newest platform version is selected automatically, but you can update to any version that you have used in the past if you choose\.  
![\[Elastic Beanstalk Update Platform Version Confirmation\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-env-updateplatform-rollingon.png)

1. Choose **Save**\.

Configurations are specific to major language and tool versions\. When a configuration is updated, the language and tools will not change major versions\. For example, *Java 7 with Tomcat 7* and *Java 8 with Tomcat 8* are two different configurations of the *Java with Tomcat* platform\. You cannot perform a platform update between configurations, only between versions of the same configuration, such as *Java 8 with Tomcat 8 version 1\.4\.5* and *Java 8 with Tomcat 8 version 2\.0\.0*\.

Linux\-based Elastic Beanstalk platforms are semantically versioned with three numbers, major, minor, and patch\. For example, *Java 8 with Tomcat 8 version 2\.1\.0* has a major version of 2, a minor version of 1, and a patch version of 0\. Major versions are only used for backwards incompatible changes\. Minor versions add support for new Elastic Beanstalk features, and patch versions fix bugs, update OS and software components, and provide access to updated packages in the Amazon Linux yum repository\.

You can configure your environment to apply minor and patch version updates automatically during a configurable weekly maintenance window with [Managed Platform Updates](environment-platform-update-managed.md)\. Elastic Beanstalk applies managed updates with no downtime or reduction in capacity, and cancels the update immediately if instances running your application on the new version fail health checks\.

Windows Server\-based platforms are not semantically versioned and do not support managed platform updates\. You can only launch the latest version of each Windows Server platform configuration and cannot roll back after an upgrade\.