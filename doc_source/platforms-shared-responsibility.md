# Shared responsibility model for Elastic Beanstalk platform maintenance<a name="platforms-shared-responsibility"></a>

AWS and our customers share responsibility for achieving a high level of software component security and compliance\. This shared model reduces your operational burden\.

For details, see the AWS [Shared Responsibility Model](https://aws.amazon.com/compliance/shared-responsibility-model/)\.

AWS Elastic Beanstalk helps you perform your side of the shared responsibility model by providing a *managed updates* feature\. This feature automatically applies patch and minor updates for an Elastic Beanstalk supported platform version\. If a managed update fails, Elastic Beanstalk notifies you of the failure to ensure that you are aware of it and can take immediate action\.

For more information, see [Managed platform updates](environment-platform-update-managed.md)\.

In addition, Elastic Beanstalk does the following:
+ Publishes its [platform support policy](platforms-support-policy.md) and retirement schedule for the coming 12 months\.
+ Releases patch, minor, and major updates of operating system \(OS\), runtime, application server, and web server components typically within 30 days of their availability\. Elastic Beanstalk is responsible for creating updates to Elastic Beanstalk components that are present on its supported platform versions\. All other updates come directly from their suppliers \(owners or community\)\.

You are responsible to do the following:
+ Update all the components that you control \(identified as **Customer** in the AWS [Shared Responsibility Model](https://aws.amazon.com/compliance/shared-responsibility-model/)\)\. This includes ensuring the security of your application, your data, and any components that your application requires and that you downloaded\.
+ Ensure that your Elastic Beanstalk environments are running on a supported platform version, and migrate any environment running on a retired platform version to a supported version\.
+ Resolve all issues that come up in failed managed update attempts and retry the update\.
+ Patch the OS, runtime, application server, and web server yourself if you opted out of Elastic Beanstalk managed updates\. You can do this by [applying platform updates manually](using-features.platform.upgrade.md) or directly patching the components on all relevant environment resources\.
+ Manage the security and compliance of any AWS services that you use outside of Elastic Beanstalk according to the AWS [Shared Responsibility Model](https://aws.amazon.com/compliance/shared-responsibility-model/)\.