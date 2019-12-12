# AWS Elastic Beanstalk Platforms Glossary<a name="platforms-glossary"></a>

Following are key terms related to Elastic Beanstalk platforms and their lifecycle\.

**Runtime**  
The programming language\-specific runtime software \(framework, libraries, interpreter, vm, etc\.\) required to run your application code\.

**Elastic Beanstalk Components**  
Software components that Elastic Beanstalk adds to a platform to enable Elastic Beanstalk functionality\. For example, the enhanced health agent is necessary for gathering and reporting health information\.

**Platform**  
A combination of an operating system \(OS\), runtime, web server, application server, and Elastic Beanstalk components\. Platforms provide components that are available to run your application\.

**Platform Version**  
A combination of specific versions of an operating system \(OS\), runtime, web server, application server, and Elastic Beanstalk components\. You create an Elastic Beanstalk environment based on a platform version and deploy your application to it\.  
A platform version has a semantic version number of the form *X\.Y\.Z*, where *X* is the major version, *Y* is the minor version, and *Z* is the patch version\.

**Platform Branch**  
A line of platform versions sharing specific \(typically major\) versions of some of their components, such as the operating system \(OS\), runtime, or Elastic Beanstalk components\. For example: *Java 8 with Tomcat 8\.5*; *Windows Server 2016 with IIS 10\.0 Version 2*\. Each successive platform version in the branch is an update to the previous one\.  
The latest platform version in each platform branch is available to you unconditionally for environment creation\. Previous platform versions in the branch are still supported—you can create an environment based on a previous platform version if you've used it in an environment in the last 30 days\. But these previous platform versions lack the most up\-to\-date components and aren't recommended for use\.

**Platform Update**  
A release of new platform versions that contain updates to some components of the platform—OS, runtime, web server, application server, and Elastic Beanstalk components\. Platform updates follow semantic version taxonomy, and can have several levels:  
+ *Major update* – An update that has changes that are incompatible with existing platform versions\. You might need to modify your application to run correctly on a new major version\. A major update has a new major platform version number\.
+ *Minor update* – An update that adds functionality that is backward compatible with an existing platform version\. You don't need to modify your application to run correctly on a new minor version\. A minor update has a new minor platform version number\.
+ *Patch update* – An update that consists of maintenance releases \(bug fixes, security updates, and performance improvements\) that are backward compatible with an existing platform version\. A patch update has a new patch platform version number\.

**Supported Platform Version**  
A platform version that has operating system \(OS\), runtime, application server, and web server versions that are still     supported by  their respective suppliers \(owners or communities\)\. Elastic Beanstalk makes supported platform versions available to you for environment creation\.

**Retired Platform Version**  
A platform version that contains an operating system \(OS\), runtime, application server, or web server version that has reached its End of Life \(EOL\), as designated by its supplier \(owner or community\)\. Retired platform versions aren't available for use in Elastic Beanstalk environments for either new or existing customers\.  
For details about platform version retirement, see [AWS Elastic Beanstalk Platform Support Policy](platforms-support-policy.md)\.

**Managed Updates**  
An Elastic Beanstalk feature that automatically applies patch and minor updates to the operating system \(OS\), runtime, web server, application server, and Elastic Beanstalk components for an Elastic Beanstalk supported platform version\. A managed update applies a newer platform version in the same platform branch to your environment\. You can configure managed updates to apply only patch updates, or minor and patch updates\. You can also disable managed updates completely\.  
For more information, see [Managed Platform Updates](environment-platform-update-managed.md)\.