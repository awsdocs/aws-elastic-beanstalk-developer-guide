# Migrating across major versions of the Elastic Beanstalk Windows server platform<a name="dotnet-v2migration"></a>

AWS Elastic Beanstalk has had several major versions of its Windows Server platform\. This page covers the main improvements for each major version, and what to consider before you migrate to a later version\.

The Windows Server platform is currently at version 2 \(v2\)\. If your application uses any Windows Server platform version earlier than v2, we recommend that you migrate to v2\.

## What's new in major versions of the Windows server platform<a name="dotnet-v2migration.diffs"></a>

### Windows server platform V2<a name="dotnet-v2migration.diffs.v2"></a>

Version 2 \(v2\) of the Elastic Beanstalk Windows Server platform was [released in February 2019](https://docs.aws.amazon.com/elasticbeanstalk/latest/relnotes/release-2019-02-21-windows-v2.html)\. V2 brings the behavior of the Windows Server platform closer to that of the Elastic Beanstalk Linux\-based platforms in several important ways\. V2 is fully backward compatible with v1, making migration from v1 easy\.

The Windows Server platform now supports the following:
+ *Versioning* – Each release gets a new version number, and you can refer to past versions \(that are still available to you\) when creating and managing environments\.
+ *Enhanced health* – For details, see [Enhanced health reporting and monitoring](health-enhanced.md)\.
+ *Immutable* and *Rolling with an Additional Batch* deployments – For details about deployment policies, see [Deploying applications to Elastic Beanstalk environments](using-features.deploy-existing-version.md)\.
+ *Immutable updates* – For details about update types, see [Configuration changes](environments-updating.md)\.
+ *Managed platform updates* – For details, see [Managed platform updates](environment-platform-update-managed.md)\.

**Note**  
The new deployment and update features depend on enhanced health\. Enable enhanced health to use them\. For details, see [Enabling Elastic Beanstalk enhanced health reporting](health-enhanced-enable.md)\.

### Windows server platform V1<a name="dotnet-v2migration.diffs.v1"></a>

Version 1\.0\.0 \(v1\) of the Elastic Beanstalk Windows Server platform was released in October 2015\. This version changes the order in which Elastic Beanstalk processes commands in [configuration files](ebextensions.md) during environment creation and updates\.

Previous platform versions don't have a version number in the solution stack name:
+ 64bit Windows Server 2012 R2 running IIS 8\.5
+ 64bit Windows Server Core 2012 R2 running IIS 8\.5
+ 64bit Windows Server 2012 running IIS 8
+ 64bit Windows Server 2008 R2 running IIS 7\.5

In earlier versions, the processing order for configuration files is inconsistent\. During environment creation, `Container Commands` run after the application source is deployed to IIS\. During a deployment to a running environment, container commands run before the new version is deployed\. During a scale up, configuration files are not processed at all\.

In addition to this, IIS starts up before container commands run\. This behavior has led some customers to implement workarounds in container commands, pausing the IIS server before commands run, and starting it again after they complete\.

Version 1 fixes the inconsistency and brings the behavior of the Windows Server platform closer to that of the Elastic Beanstalk Linux\-based platforms\. In the v1 platform, Elastic Beanstalk always runs container commands before starting the IIS server\.

The v1 platform solution stacks have a `v1` after the Windows Server version:
+ 64bit Windows Server 2012 R2 v1\.1\.0 running IIS 8\.5
+ 64bit Windows Server Core 2012 R2 v1\.1\.0 running IIS 8\.5
+ 64bit Windows Server 2012 v1\.1\.0 running IIS 8
+ 64bit Windows Server 2008 R2 v1\.1\.0 running IIS 7\.5

Additionally, the v1 platform extracts the contents of your application source bundle to `C:\staging\` before running container commands\. After container commands complete, the contents of this folder are compressed into a \.zip file and deployed to IIS\. This workflow allows you to modify the contents of your application source bundle with commands or a script before deployment\.

## Migrating from earlier major versions of the Windows server platform<a name="dotnet-v2migration.migration"></a>

Read this section for migration considerations before you update your environment\. To update your environment's platform to a newer version, see [Updating your Elastic Beanstalk environment's platform version](using-features.platform.upgrade.md)\.

### From V1 to V2<a name="dotnet-v2migration.migration.fromv1"></a>

The Windows Server platform v2 doesn't support \.NET Core 1\.x and 2\.0\. If you're migrating your application from Windows Server v1 to v2, and your application uses one of these \.NET Core versions, update your application to a \.NET Core version that v2 supports\. For a list of supported versions, see [\.NET on Windows Server with IIS](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html#platforms-supported.net) in the *AWS Elastic Beanstalk Platforms*\.

If your application uses a custom Amazon Machine Image \(AMI\), create a new custom AMI based on a Windows Server platform v2 AMI\. To learn more, see [Using a custom Amazon machine image \(AMI\)](using-features.customenv.md)\.

**Note**  
The deployment and update features that are new to Windows Server v2 depend on enhanced health\. When you migrate an environment to v2, enhanced health is disabled\. Enable it to use these features\. For details, see [Enabling Elastic Beanstalk enhanced health reporting](health-enhanced-enable.md)\.

### From pre\-V1<a name="dotnet-v2migration.migration.fromv0"></a>

In addition to considerations for migrating from v1, if you're migrating your application from a Windows Server solution stack that's earlier than v1, and you currently use container commands, remove any commands that you added to work around the processing inconsistencies when you migrate to a newer version\. Starting with v1, container commands are guaranteed to run completely before the application source that is deployed and before IIS starts\. This enables you to make any changes to the source in `C:\staging` and modify IIS configuration files during this step without issue\.

For example, you can use the AWS CLI to download a DLL file to your application source from Amazon S3:

`.ebextensions\copy-dll.config`

```
container_commands:
  copy-dll:
    command: aws s3 cp s3://my-bucket/dlls/large-dll.dll .\lib\
```

For more information on using configuration files, see [Advanced environment customization with configuration files \(`.ebextensions`\)](ebextensions.md)\.