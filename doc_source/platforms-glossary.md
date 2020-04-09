# Elastic Beanstalk platforms glossary<a name="platforms-glossary"></a>

Following are key terms related to AWS Elastic Beanstalk platforms and their lifecycle\.

**Runtime**  
The programming language\-specific runtime software \(framework, libraries, interpreter, vm, etc\.\) required to run your application code\.

**Elastic Beanstalk Components**  
Software components that Elastic Beanstalk adds to a platform to enable Elastic Beanstalk functionality\. For example, the enhanced health agent is necessary for gathering and reporting health information\.

**Platform**  
A combination of an operating system \(OS\), runtime, web server, application server, and Elastic Beanstalk components\. Platforms provide components that are available to run your application\.

**Platform Version**  
A combination of specific versions of an operating system \(OS\), runtime, web server, application server, and Elastic Beanstalk components\. You create an Elastic Beanstalk environment based on a platform version and deploy your application to it\.  
A platform version has a semantic version number of the form *X\.Y\.Z*, where *X* is the major version, *Y* is the minor version, and *Z* is the patch version\.  
A platform version can be in one of the following states:  
+ *Supported* – A platform version that consists entirely of *supported components*\. All components have not reached their End of Life \(EOL\), as designated by their respective suppliers \(owners—AWS or third parties—or communities\)\. They receive regular patch or minor updates from their suppliers \. Elastic Beanstalk makes supported platform versions available to you for environment creation\.
+ *Retired* – A platform version with one or more *retired components*, which have reached their End of Life \(EOL\), as designated by their suppliers\. Retired platform versions aren't available for use in Elastic Beanstalk environments for either new or existing customers\.

  For details about retired components, see [Elastic Beanstalk platform support policy](platforms-support-policy.md)\.

**Platform Branch**  
A line of platform versions sharing specific \(typically major\) versions of some of their components, such as the operating system \(OS\), runtime, or Elastic Beanstalk components\. For example: *Python 3\.6 running on 64bit Amazon Linux*; *IIS 10\.0 running on 64bit Windows Server 2016*\. Each successive platform version in the branch is an update to the previous one\.  
The latest platform version in each platform branch is available to you unconditionally for environment creation\. Previous platform versions in the branch are still supported—you can create an environment based on a previous platform version if you've used it in an environment in the last 30 days\. But these previous platform versions lack the most up\-to\-date components and aren't recommended for use\.  
A platform branch can be in one of the following states:  
+ *Supported* – A current platform branch\. It consists entirely of *supported components*\. It receives ongoing platform updates, and is recommended for use in production environments\. For a list of supported platform branches, see [Elastic Beanstalk supported platforms](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html) in the *AWS Elastic Beanstalk Platforms* guide\.
+ *Beta* – A preview, pre\-release platform branch\. It's experimental in nature\. It may receive ongoing platform updates for a while, but has no long\-term support\. A beta platform branch isn't recommended for use in production environments\. Use it only for evaluation\. For a list of beta platform branches, see [Elastic Beanstalk Platform Versions in Public Beta](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-beta.html) in the *AWS Elastic Beanstalk Platforms* guide\.
+ *Deprecated* – A platform branch with one or more *deprecated components*\. It receives ongoing platform updates, but isn't recommended for use in production environments\. For a list of deprecated platform branches, see [Elastic Beanstalk Platform Versions Scheduled for Retirement](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-retiring.html) in the *AWS Elastic Beanstalk Platforms* guide\.
+ *Retired* – A platform branch with one or more *retired components*\. It doesn't receive platform updates anymore, and isn't recommended for use in production environments\. Retired platform branches aren't listed in the *AWS Elastic Beanstalk Platforms* guide\. Elastic Beanstalk doesn't make platform versions of retired platform branches available to you for environment creation\.
A *supported component* has no retirement date scheduled by its supplier \(owner or community\)\. The supplier might be AWS or a third party\. A *deprecated component* has a retirement date scheduled by its supplier\. A *retired component* has reached End of Life \(EOL\) and is no longer supported by its supplier\. For details about retired components, see [Elastic Beanstalk platform support policy](platforms-support-policy.md)\.  
If your environment uses a deprecated or retired platform branch, we recommend that you update it to a platform version in a supported platform branch\. For details, see [Updating your Elastic Beanstalk environment's platform version](using-features.platform.upgrade.md)\.

**Platform Update**  
A release of new platform versions that contain updates to some components of the platform—OS, runtime, web server, application server, and Elastic Beanstalk components\. Platform updates follow semantic version taxonomy, and can have several levels:  
+ *Major update* – An update that has changes that are incompatible with existing platform versions\. You might need to modify your application to run correctly on a new major version\. A major update has a new major platform version number\.
+ *Minor update* – An update that adds functionality that is backward compatible with an existing platform version\. You don't need to modify your application to run correctly on a new minor version\. A minor update has a new minor platform version number\.
+ *Patch update* – An update that consists of maintenance releases \(bug fixes, security updates, and performance improvements\) that are backward compatible with an existing platform version\. A patch update has a new patch platform version number\.

**Managed Updates**  
An Elastic Beanstalk feature that automatically applies patch and minor updates to the operating system \(OS\), runtime, web server, application server, and Elastic Beanstalk components for an Elastic Beanstalk supported platform version\. A managed update applies a newer platform version in the same platform branch to your environment\. You can configure managed updates to apply only patch updates, or minor and patch updates\. You can also disable managed updates completely\.  
For more information, see [Managed platform updates](environment-platform-update-managed.md)\.