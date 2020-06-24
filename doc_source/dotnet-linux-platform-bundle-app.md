# Bundling applications for the \.NET Core on Linux platform<a name="dotnet-linux-platform-bundle-app"></a>

You can run both *runtime\-dependent* and *self\-contained* \.NET Core applications on AWS Elastic Beanstalk\.

A runtime\-dependent application uses a \.NET Core runtime that Elastic Beanstalk provides to run your application\. Elastic Beanstalk uses the `runtimeconfig.json` file in your source bundle to determine the runtime to use for your application\. Elastic Beanstalk chooses the latest compatible runtime available for your application\.

A self\-contained application includes the \.NET Core runtime, your application, and its dependencies\. To use a version of the \.NET Core runtime that Elastic Beanstalk doesn't include in its platforms, provide a self\-contained application\.

## Examples<a name="dotnet-linux-platform-bundle-app-examples"></a>

You can compile both self\-contained and runtime\-dependent applications with the `dotnet publish` command\. To learn more about publishing \.NET Core apps, see [\.NET Core application publishing overview](https://docs.microsoft.com/en-us/dotnet/core/deploying) in the \.NET Core documentation\.

The following example file structure defines a single application that uses a \.NET Core runtime that Elastic Beanstalk provides\.

```
├── appsettings.Development.json
├── appsettings.json
├── dotnetcoreapp.deps.json
├── dotnetcoreapp.dll
├── dotnetcoreapp.pdb
├── dotnetcoreapp.runtimeconfig.json
├── web.config
├── Procfile
├── .ebextensions
├── .platform
```

You can include multiple applications in your source bundle\. The following example defines two applications to run on the same web server\. To run multiple applications, you must include a [Procfile](dotnet-linux-procfile.md) in your source bundle\. For a full example application, see [dotnet\-core\-linux\-multiple\-apps\.zip](samples/dotnet-core-linux-multiple-apps.zip)\.

```
├── DotnetMultipleApp1
│   ├── Amazon.Extensions.Configuration.SystemsManager.dll
│   ├── appsettings.Development.json
│   ├── appsettings.json
│   ├── AWSSDK.Core.dll
│   ├── AWSSDK.Extensions.NETCore.Setup.dll
│   ├── AWSSDK.SimpleSystemsManagement.dll
│   ├── DotnetMultipleApp1.deps.json
│   ├── DotnetMultipleApp1.dll
│   ├── DotnetMultipleApp1.pdb
│   ├── DotnetMultipleApp1.runtimeconfig.json
│   ├── Microsoft.Extensions.PlatformAbstractions.dll
│   ├── Newtonsoft.Json.dll
│   └── web.config
├── DotnetMultipleApp2
│   ├── Amazon.Extensions.Configuration.SystemsManager.dll
│   ├── appsettings.Development.json
│   ├── appsettings.json
│   ├── AWSSDK.Core.dll
│   ├── AWSSDK.Extensions.NETCore.Setup.dll
│   ├── AWSSDK.SimpleSystemsManagement.dll
│   ├── DotnetMultipleApp2.deps.json
│   ├── DotnetMultipleApp2.dll
│   ├── DotnetMultipleApp2.pdb
│   ├── DotnetMultipleApp2.runtimeconfig.json
│   ├── Microsoft.Extensions.PlatformAbstractions.dll
│   ├── Newtonsoft.Json.dll
│   └── web.config
├── Procfile
├── .ebextensions
├── .platform
```