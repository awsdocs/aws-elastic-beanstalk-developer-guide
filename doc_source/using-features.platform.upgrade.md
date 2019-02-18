# Updating Your Elastic Beanstalk Environment's Platform Version<a name="using-features.platform.upgrade"></a>

Elastic Beanstalk regularly releases updates for the Linux\-based and Windows Server\-based [platforms](concepts.platforms.md) that run applications on an Elastic Beanstalk environment\. A platform consists of a software component \(an AMI running a specific version of an operating system \(OS\), tools, and Elastic Beanstalk\-specific scripts\), and a configuration component \(the default [settings](command-options.md) applied to environments created with the platform\)\. New platform versions provide updates to existing software components and support for new features and configuration options\.

For platforms that support multiple incompatible major versions of the included web container, programming language, or framework, separate *platform branches* are concurrently supported, each with its own line of platform versions\. For example, the [Java with Tomcat](java-tomcat-platform.md) platform supports separate platform versions for Tomcat 7 and Tomcat 8\. Updates are generally released for all branches of a given platform at the same time\.

Linux\-based Elastic Beanstalk platforms are semantically versioned with three numbers, major, minor, and patch\. For example, *Java 8 with Tomcat 8 version 2\.1\.0* has a major version of 2, a minor version of 1, and a patch version of 0\. Major versions are only used for backward\-incompatible changes\. Minor versions add support for new Elastic Beanstalk features, and patch versions fix bugs, update OS and software components, and provide access to updated packages in the Amazon Linux yum repository\.

You can update your environment's platform version to another platform version with the same major number \(a minor or patch update\)\. You can't perform a platform update across major platform versions\.

Windows Server\-based platforms are not semantically versioned and do not support managed platform updates\. You can only launch the latest version of each Windows Server major platform version and can't roll back after an upgrade\.

**Warning**  
During managed platform updates with instance replacement enabled, immutable updates, and deployments with immutable updates enabled all instances are replaced\. This causes all acculumated [Amazon EC2 Burst Balances](https://docs.aws.amazon.com/AWSEC2/latest/DeveloperGuide/burstable-performance-instances.html) to be lost\.

## Updating an Environment's Platform Version<a name="using-features.platform.upgrade.config"></a>

When a new version of your environment's platform is available, Elastic Beanstalk shows a message in the [environment management console](environments-console.md) and makes the **Change** button available\. If you've previously created an environment using an older platform version, or upgraded your environment from an older version, you can also use the **Change** button to revert to a previous platform version\.

**To update your environment's platform**

1. Navigate to the [management page](environments-console.md) for your environment\.

1. In the **Overview** section, under **Configuration**, click **Change**\.  
![\[Elastic Beanstalk Newer Platform Available\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-env-dashboard-changetonewplatform.png)

1. Choose a **Platform Version**\. The newest platform version is selected automatically, but you can update to any version that you have used in the past if you choose\.  
![\[Elastic Beanstalk Update Platform Version Confirmation\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-env-updateplatform-rollingon.png)

1. Choose **Save**\.

You can configure your environment to apply minor and patch version updates automatically during a configurable weekly maintenance window with [managed platform updates](environment-platform-update-managed.md)\. Elastic Beanstalk applies managed updates with no downtime or reduction in capacity, and cancels the update immediately if instances running your application on the new version fail health checks\.

## Migrating an Environment to a Different Platform Branch<a name="using-features.platform.upgrade.cross-config"></a>

You might have reasons to migrate your application to a different platform branch\. Switching to the latest language version is a common reason\. For example, you might want to move your PHP application from version 7\.0 to version 7\.1\. Or move your Java with Tomcat application, from Java 7 with Tomcat 7 to Java 8 with Tomcat 8\.5\.

Elastic Beanstalk doesn't support automatic platform updates across platform branches\. When you want to change your environment's platform version to a version of a different branch, you can't use the procedure shown in the previous section\. The following procedure shows how to migrate your environment to a platform version of a different branch\.

**To migrate your environment's platform to a different platform branch**

1. [Create a new environment](using-features.environments.md), using the new target platform version, and deploy your application code to it\. The new environment should be in the Elastic Beanstalk application that contains the environment you're migrating\. Don't terminate the existing environment yet\.

1. Use the new environment to migrate your application code\. Find and fix any application compatibility issues resulting from migrating to the new language version\. Iterate on testing and deploying your fixes until you're satisfied with the application on the new environment\.

1. Turn the new environment into your production environment by swapping its CNAME with the existing production environment's CNAME\. For details, see [Blue/Green Deployments with AWS Elastic Beanstalk](using-features.CNAMESwap.md)\.

1. When you're satisfied with the state of your new environment in production, terminate the old environment\. For details, see [Terminate an Elastic Beanstalk Environment](using-features.terminating.md)\.