# Migrating to v1 Elastic Beanstalk Windows Server Platforms<a name="dotnet-v2migration"></a>

Version 1\.0\.0 of AWS Elastic Beanstalk's Windows Server based platforms was released in October 2015\. This version changes the order in which Elastic Beanstalk processes commands in \([configuration files](ebextensions.md)\) during environment creation and updates\.

Previous platform versions do not have a version number in the solution stack name:
+ 64bit Windows Server 2012 R2 running IIS 8\.5
+ 64bit Windows Server Core 2012 R2 running IIS 8\.5
+ 64bit Windows Server 2012 running IIS 8
+ 64bit Windows Server 2008 R2 running IIS 7\.5

In previous versions, the processing order for configuration files is inconsistent\. During environment creation, `Container Commands` run after the application source is deployed to IIS\. During a deployment to a running environment, container commands run before the new version is deployed\. During a scale up, configuration files are not processed at all\.

In addition to this, IIS starts up before container commands run\. This behavior has led some customers to implement workarounds in container commands, pausing the IIS server prior to commands running and starting it again after they complete\.

Version 1 fixes the inconsistency and brings the Windows Server platforms' behavior in line with Elastic Beanstalk's Linux\-based platforms\. In v1 platforms, Elastic Beanstalk always runs container commands prior to starting the IIS server\.

Version 1 platforms have a `v1` after the Windows Server version:
+ 64bit Windows Server 2012 R2 v1\.1\.0 running IIS 8\.5
+ 64bit Windows Server Core 2012 R2 v1\.1\.0 running IIS 8\.5
+ 64bit Windows Server 2012 v1\.1\.0 running IIS 8
+ 64bit Windows Server 2008 R2 v1\.1\.0 running IIS 7\.5

Additionally, v1 platforms extract the contents of your application source bundle to `C:\staging\` prior to running container commands\. After container commands complete, the contents of this folder are zipped up and deployed to IIS\. This workflow allows you to modify the contents of your application source bundle with commands or a script prior to deployment\.

If you currently use container commands on an older platform, remove any commands that you added to work around the processing inconsistencies when you move to v1\. In v1, container commands are guaranteed to run completely prior to the application source being deployed and IIS starting up, so you can make any changes to source in `C:\staging` and modify IIS configuration files during this step without issue\.

For example, you can use the AWS CLI to download a DLL file to your application source from Amazon S3:

`.ebextensions\copy-dll.config`

```
container_commands:
  copy-dll:
    command: aws s3 cp s3://my-bucket/dlls/large-dll.dll .\lib\
```

For more information on using configuration files, see [Advanced Environment Customization with Configuration Files \(`.ebextensions`\)](ebextensions.md)\.