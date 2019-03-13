# Updating Your Elastic Beanstalk Environment's Platform Version<a name="using-features.platform.upgrade"></a>

Elastic Beanstalk regularly releases new platform versions to update all Linux\-based and Windows Server\-based [platforms](concepts.platforms.md)\. New platform versions provide updates to existing software components and support for new features and configuration options\. To learn about platforms and platform versions, see [AWS Elastic Beanstalk Platforms Glossary](platforms-glossary.md)\.

You can use the Elastic Beanstalk console or the EB CLI to update your environment's platform version\. Depending on the platform version you'd like to update to, Elastic Beanstalk recommends one of two methods for performing platform updates\.
+ [Method 1 – Update your Environment's Platform Version](#using-features.platform.upgrade.config)\. We recommend this method when you're updating to the latest platform version, without a change in runtime, web server, or application server versions, and without a change in the major platform version\. This is the most common and routine platform update\.
+ [Method 2 – Perform a Blue/Green Deployment](#using-features.platform.upgrade.bluegreen)\. We recommend this method when you're updating to a different runtime, web server, or application server versions, or to a different major platform version\. This is a good approach when you want to take advantage of new runtime capabilities or the latest Elastic Beanstalk functionality\.

For more help with choosing the best platform update method, expand the section for your environment's platform\.

## Single Container Docker<a name="using-features.platform.upgrade.docker-single"></a>

Use [Method 1](#using-features.platform.upgrade.config) to perform platform updates\.

## Multicontainer Docker<a name="using-features.platform.upgrade.docker-multi"></a>

Use [Method 1](#using-features.platform.upgrade.config) to perform platform updates\.

## Preconfigured Docker<a name="using-features.platform.upgrade.docker-preconfigured"></a>

Consider the following cases:
+ If you're migrating your application to another platform, for example from *Go 1\.4 \(Docker\)* to *Go 1\.11* or from *Python 3\.4 \(Docker\)* to *Python 3\.6*, use [Method 2](#using-features.platform.upgrade.bluegreen)\.
+ If you're migrating your application to a different Docker container version, for example from *Glassfish 4\.1 \(Docker\)* to *Glassfish 5\.0 \(Docker\)*, use [Method 2](#using-features.platform.upgrade.bluegreen)\.
+ If you're updating to a latest platform version with no change in container version or major version, use [Method 1](#using-features.platform.upgrade.config)\.

## Go<a name="using-features.platform.upgrade.go"></a>

Use [Method 1](#using-features.platform.upgrade.config) to perform platform updates\.

## Java SE<a name="using-features.platform.upgrade.java-se"></a>

Consider the following cases:
+ If you're migrating your application to a different Java runtime version, for example from *Java 7* to *Java 8*, use [Method 2](#using-features.platform.upgrade.bluegreen)\.
+ If you're updating to a latest platform version with no change in runtime version, use [Method 1](#using-features.platform.upgrade.config)\.

## Java with Tomcat<a name="using-features.platform.upgrade.java-tomcat"></a>

Consider the following cases:
+ If you're migrating your application to a different Java runtime version or Tomcat application server version, for example from *Java 7 with Tomcat 7* to *Java 8 with Tomcat 8\.5*, use [Method 2](#using-features.platform.upgrade.bluegreen)\.
+ If you're migrating your application across major Java with Tomcat platform versions \(v1\.x\.x, v2\.x\.x, and v3\.x\.x\), use [Method 2](#using-features.platform.upgrade.bluegreen)\.
+ If you're updating to a latest platform version with no change in runtime version, application server version, or major version, use [Method 1](#using-features.platform.upgrade.config)\.

## \.NET on Windows Server with IIS<a name="using-features.platform.upgrade.dotnet"></a>

Consider the following cases:
+ If you're migrating your application to a different Windows operating system version, for example from *Windows Server 2008 R2* to *Windows Server 2016*, use [Method 2](#using-features.platform.upgrade.bluegreen)\.
+ If you're migrating your application across major Windows Server platform versions, see [Migrating from Earlier Major Versions of the Windows Server Platform](dotnet-v2migration.md#dotnet-v2migration.migration), and use [Method 2](#using-features.platform.upgrade.bluegreen)\.
+ If your application is currently running on a Windows Server platform V2\.x\.x and you're updating to a latest platform version, use [Method 1](#using-features.platform.upgrade.config)\.

**Note**  
[Windows Server platform versions](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.net) earlier than v2 aren't semantically versioned\. You can only launch the latest version of each of these Windows Server major platform versions and can't roll back after an upgrade\.

## Node\.js<a name="using-features.platform.upgrade.nodejs"></a>

Use [Method 2](#using-features.platform.upgrade.bluegreen) to perform platform updates\.

## PHP<a name="using-features.platform.upgrade.php"></a>

Consider the following cases:
+ If you're migrating your application to a different PHP runtime version, for example from *PHP 5\.6* to *PHP 7\.2*, use [Method 2](#using-features.platform.upgrade.bluegreen)\.
+ If you're migrating your application across major PHP platform versions \(v1\.x\.x and v2\.x\.x\), use [Method 2](#using-features.platform.upgrade.bluegreen)\.
+ If you're updating to a latest platform version with no change in runtime version or major version, use [Method 1](#using-features.platform.upgrade.config)\.

## Python<a name="using-features.platform.upgrade.python"></a>

Consider the following cases:
+ If you're migrating your application to a different Python runtime version, for example from *Python 2\.7* to *Python 3\.6*, use [Method 2](#using-features.platform.upgrade.bluegreen)\.
+ If you're migrating your application across major Python platform versions \(v1\.x\.x and v2\.x\.x\), use [Method 2](#using-features.platform.upgrade.bluegreen)\.
+ If you're updating to a latest platform version with no change in runtime version or major version, use [Method 1](#using-features.platform.upgrade.config)\.

## Ruby<a name="using-features.platform.upgrade.ruby"></a>

Consider the following cases:
+ If you're migrating your application to a different Ruby runtime version or application server version, for example from *Ruby 2\.3 with Puma* to *Ruby 2\.6 with Puma*, use [Method 2](#using-features.platform.upgrade.bluegreen)\.
+ If you're migrating your application across major Ruby platform versions \(v1\.x\.x and v2\.x\.x\), use [Method 2](#using-features.platform.upgrade.bluegreen)\.
+ If you're updating to a latest platform version with no change in runtime version, application server version, or major version, use [Method 1](#using-features.platform.upgrade.config)\.

## Method 1 – Update your Environment's Platform Version<a name="using-features.platform.upgrade.config"></a>

Use this method to update to the latest version of your environment's platform\. If you've previously created an environment using an older platform version, or upgraded your environment from an older version, you can also use this method to revert to a previous platform version\.

**To update your environment's platform version**

1. Navigate to the [management page](environments-console.md) for your environment\.

1. In the **Overview** section, under **Configuration**, click **Change**\.  
![\[Elastic Beanstalk Newer Platform Available\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-env-dashboard-changetonewplatform.png)

1. Choose a **Platform Version**\. The newest platform version is selected automatically, but you can update to any version that you've used in the past\.  
![\[Elastic Beanstalk Update Platform Version Confirmation\]](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/images/aeb-env-updateplatform-rollingon.png)

1. Choose **Save**\.

To further simplify platform updates, Elastic Beanstalk can manage them for you\. You can configure your environment to apply minor and patch version updates automatically during a configurable weekly maintenance window\. Elastic Beanstalk applies managed updates with no downtime or reduction in capacity, and cancels the update immediately if instances running your application on the new version fail health checks\. For details, see [Managed Platform Updates](environment-platform-update-managed.md)\.

## Method 2 – Perform a Blue/Green Deployment<a name="using-features.platform.upgrade.bluegreen"></a>

Use this method to update to a different runtime, web server, or application server versions, or to a different major platform version\. This is typically necessary when you want to take advantage of new runtime capabilities or the latest Elastic Beanstalk functionality\.

When you migrate across major platform versions or to platform versions with major component updates, there's a greater likelihood that your application, or some aspects of it, might not function as expected on the new platform version, and might require changes\.

Before performing the migration, update your local development machine to the newer runtime versions and other components of the platform you plan on migrating to\. Verify that your application still works as expected, and make any necessary code fixes and changes\. Then use the following best practice procedure to safely migrate your environment to the new platform version\.

**To migrate your environment to a platform version with major updates**

1. [Create a new environment](using-features.environments.md), using the new target platform version, and deploy your application code to it\. The new environment should be in the Elastic Beanstalk application that contains the environment you're migrating\. Don't terminate the existing environment yet\.

1. Use the new environment to migrate your application\. In particular:
   + Find and fix any application compatibility issues that you couldn't discover during the development phase\.
   + Ensure that any customizations that your application makes using [configuration files](ebextensions.md) work correctly in the new environment\. These might include option settings, additional installed packages, custom security policies, and script or configuration files installed on environment instances\.
   + If your application uses a custom Amazon Machine Image \(AMI\), create a new custom AMI based on the AMI of the new platform version\. To learn more, see [Using a Custom Amazon Machine Image \(AMI\)](using-features.customenv.md)\. Specifically, this is required if your application uses the Windows Server platform with a custom AMI, and you're migrating to a Windows Server V2 platform version\. In this case, see also [Migrating from Earlier Major Versions of the Windows Server Platform](dotnet-v2migration.md#dotnet-v2migration.migration)\.

   Iterate on testing and deploying your fixes until you're satisfied with the application on the new environment\.

1. Turn the new environment into your production environment by swapping its CNAME with the existing production environment's CNAME\. For details, see [Blue/Green Deployments with Elastic Beanstalk](using-features.CNAMESwap.md)\.

1. When you're satisfied with the state of your new environment in production, terminate the old environment\. For details, see [Terminate an Elastic Beanstalk Environment](using-features.terminating.md)\.